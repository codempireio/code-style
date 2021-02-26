# React Native Module (Android)

[Return to Table of Contents](../README.md)

## Intro

This guide is an introduction to React Native Modules (Android). In the learning process, we will create a small project that will use the native module. Guide structure and navigation:

0. [General Information](#general-information)
1. [ReactContextBaseJavaModule](#reactcontextbasejavamodule)
2. [ReactPackage](#reactpackage)
3. [ReactMethod](#reactmethod)
4. [Callbacks](#callbacks)
5. [EventEmitter](#eventemitter)
6. [Exporting Constants](#exporting-constants)
7. [TypeScript](#typescript)

## General Information

Sometimes a React Native app needs to access a native platform API that is not available by default in JavaScript, for example, the native APIs. Maybe you want to reuse some existing Objective-C, Swift, Java, or C++ libraries without having to reimplement them in JavaScript.

> The NativeModule system exposes instances of Java/Objective-C/C++ (native) classes to JavaScript (JS) as JS objects, thereby allowing you to execute arbitrary native code from within JS.

In this guide, we will create a module to get device battery info. We recommend using Android Studio to create Native Modules for Java.

## ReactContextBaseJavaModule

The first step is to create `BatteryInfo.java` object inside of your java code. To create Java class go `File -> New -> Java Class`. Then we should extend our class with ReactContextBaseJavaModule. To do so we implement the default method `getName`:

```java
public class BatteryInfo extends ReactContextBaseJavaModule {

    BatteryInfo(ReactApplicationContext context) {
        super(context);
    }

    @NonNull
    @Override
    public String getName() {
        return "BatteryInfo";
    }
}
```

This method should return the name of NativeModule that will be accessed from JS code. In this example, we could access our module using `NativeModule.BatteryInfo`.

## ReactPackage

Once a native module is written, it needs to be registered with React Native. To do so we should create `ReactPackage`.

> During initialization, React Native will loop over all packages, and for each `ReactPackage`, register each native module within.

Add new Java class `BatteryPackage` to the project. This class should implement the  `ReactPackage` interface. To do so we should implement two methods: `createNativeModules` and `createViewManagers`.

React Native invokes the method `createNativeModules` on a ReactPackage to get the list of native modules to register. Also, it invokes `createViewManagers` to get the list of native UI components. This guide covers only NativeModules. To learn more about NativeUIComponents read the [docs](https://reactnative.dev/docs/native-components-android)

```java
public class BatteryPackage implements ReactPackage {
    @NonNull
    @Override
    public List<NativeModule> createNativeModules(@NonNull ReactApplicationContext reactContext) {
        List<NativeModule> modules = new ArrayList<>();

        // Here we can add multiple modules
        modules.add(new BatteryInfo(reactContext));
        // module.add(...)

        return modules;
    }

    @NonNull
    @Override
    public List<ViewManager> createViewManagers(@NonNull ReactApplicationContext reactContext) {
        return Collections.emptyList();
    }
}
```

After creating `BatteryPackage` we should add it to package list. Find `getPackages` method in `MainApplication` and add our package to package list:

```java
@Override
protected List<ReactPackage> getPackages() {
  @SuppressWarnings("UnnecessaryLocalVariable")
  List<ReactPackage> packages = new PackageList(this).getPackages();

  // Here we can add multiple packages
  packages.add(new BatteryPackage());
  // package.add(...)

  return packages;
}
```

Now we connect our NativeModule into an application and can use it from JS code as:

```js
import { NativeModules } from 'react-native';

export const { BatteryInfo } from NativeModules;
```

## ReactMethod

For now, our module does nothing. We should add some methods for using them from JS. Use annotation `@ReactMethod` to create the method.

> To get info about the battery in Android we should use a broadcast receiver for the ACTION_BATTERY_CHANGED intent. This receiver should be declared and registered.

Let's add a broadcast receiver to our module. To do so we should override native method `onReceive`. This method occurs every time when battery status changed. We will use state of the battery later. For now we can leave it blank.

Also, we will use ReactMethod to register this module after the JS UI part will be initialized:

```java
// this variable will contain the battery status
private float batteryPct;

private BroadcastReceiver mBatInfoReceiver = new BroadcastReceiver(){
    @Override
    public void onReceive(Context ctxt, Intent intent) {
        int level = intent.getIntExtra(BatteryManager.EXTRA_LEVEL, -1);
        int scale = intent.getIntExtra(BatteryManager.EXTRA_SCALE, -1);
        batteryPct = level * 100 / (float) scale;
    }
};

@ReactMethod
public void register() {
    getCurrentActivity().registerReceiver(
            mBatInfoReceiver,
            new IntentFilter(Intent.ACTION_BATTERY_CHANGED));
}
```

Now we can use this method as `BatteryInfo.register()`. We should make this call on the first launch. This project is an example and we could use it directly in `App` component in `useEffect` hook. But in the real project, we should use `AppService` or another high-level service to do so.

```js
const { BatteryInfo } = NativeModules;

export const App = () => {
  useEffect(() => {
    BatteryInfo.register();
  }, []);

  return <View />;
};
```

## Callbacks

Sometimes we should use callbacks in `ReactMethod`. E.g. we want to see the success of executing and path some parameters back to the JS part. To do so we may pass as parameter `Callback`. This is a class that has the `invoke()` method. We could pass any type of argument.

In our example project, we would like to see when the broadcast receiver is registered. Lets modify our `register()` method and add `Callback` parameter. This callback will be executed when a broadcast receiver is registered. Also, we may pass a parameter to this `Callback`. In our case, we will pass the timestamp.

```java
@ReactMethod
public void register(Callback callback) {
    getCurrentActivity().registerReceiver(
            mBatInfoReceiver,
            new IntentFilter(Intent.ACTION_BATTERY_CHANGED));

    // Get currect timestamp
    Long timestampLong = System.currentTimeMillis()/1000;
    String timestamp = timestampLong.toString();

    callback.invoke(timestamp);
}
```

Now, in JS part, we could provide as a parameter a callback. Just for example we make an `ToastAndroid` with message that reciver is registred:

```js
const registerCallback = useCallback((timestamp: string) => {
  ToastAndroid.show(
    `Broadcast receiver is registred.\nTimestamp: ${timestamp}`,
    500
  );
}, []);

useEffect(() => {
  BatteryInfo.register(registerCallback);
}, [registerCallback]);

return <View />;
```

> There is similar to `Callback` parameter - `Promise`. We will not use this parameter in this guide. But it is important to mention that this method has 2 methods - `resolve()` and `reject()` and you can call any of these methods to manipulate with `Promise`

## EventEmitter

Sometimes we would like to not call every time for every event. But we would like to handle events directly from the module. For this task, we should use `RCTDeviceEventEmitter` class in the Android part. This is part of DeviceEventManagerModule and has the `emit()` method. This method takes 2 parameters:

- `String` - `eventName`. Use constant for a specific event and handle it from JS code.
- `WritableMap` - `params`. WritableMap is similar to `Object` in JS and contains key-value pairs.

Let's add an event emitter to our `BroadcastReceiver` to send events about battery status changed. To do so we should add a new method `sendBatteryStatus` and call it from the `onReceive` method in the receiver:

```java
private static final String BATTERY_INFO_EVENT = "batteryInfo";
private static final String BATTERY_INFO_PARAM = "battery";

private BroadcastReceiver mBatInfoReceiver = new BroadcastReceiver(){
    // This method will occurs every time when os updates battery status
    @Override
    public void onReceive(Context ctxt, Intent intent) {
        int level = intent.getIntExtra(BatteryManager.EXTRA_LEVEL, -1);
        int scale = intent.getIntExtra(BatteryManager.EXTRA_SCALE, -1);
        batteryPct = level * 100 / (float) scale;
        
        // Call event sender after battery status is updated
        sendBatteryStatus();
    }
};

private void sendBatteryStatus() {
    // Create params WritableMap
    WritableMap params = Arguments.createMap();
    params.putString(BATTERY_INFO_PARAM, String.valueOf(batteryPct));

    ReactContext ctx = getReactApplicationContext();

    // Emit event
    ctx.getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)
            .emit(BATTERY_INFO_EVENT, params);
}
```

Now we could handle this event from JS code. To do so we should use `NativeEventEmitter`. This class may use to create `eventListener` for a specific event.

Let's add the `eventListener` and print the battery status for every event that we will handle:

```js
const BATTERY_INFO_EVENT = 'batteryInfo';

interface IBatteryEvent {
  battery: string;
}


export const App = () => {
  // ...

  // state variable for battery status
  const [battery, setBattery] = useState('');

  useEffect(() => {
    // create eventListener
    const eventEmmiter = new NativeEventEmitter(BatteryInfo);
    const eventListener = eventEmmiter.addListener(BATTERY_INFO_EVENT, (event: IBatteryEvent) => {
      setBattery(event.battery);
    });

    // cleanup listener
    return () => eventListener.remove();
  });

  return (
    <View>
      <Text>{battery}</Text>
    </View>
  );
}
```

## Exporting Constants

A native module can export constants by implementing the native method `getConstants()`, which is available in JS. The most common use is to send event types. But you can use it for any type of constants.

Lets pass our event constant to JS part. First we should implement method `getConstants()` in our module:

```java
@Nullable
@Override
public Map<String, Object> getConstants() {
    final Map<String, Object> constants = new HashMap<>();
    constants.put("BATTERY_INFO_EVENT", BATTERY_INFO_EVENT);
    return constants;
}
```

Then we should retrieve this constant in JS part and use it for `EventEmmiter`:

```js
useEffect(() => {
  // get event constant
  const { BATTERY_INFO_EVENT } = BatteryInfo.getConstants();
  const eventEmmiter = new NativeEventEmitter(BatteryInfo);

  // use it in event listener
  const eventListener = eventEmmiter.addListener(BATTERY_INFO_EVENT, (event: IBatteryEvent) => {
    setBattery(event.battery);
  });

  return () => eventListener.remove();
});
```

## TypeScript

In our example, we have used our `NativeModule` directly. To create some typing for this module we should cast NativeModules to a union type that contains a definition for our module. Here is an example for our `BatteryInfo` module:

```js
type TRegisterCallback = (timestamp: string) => void;

interface INativeModules extends NativeModulesStatic {
  BatteryInfo: {
    // ReactMethod that we have created
    register: (callback: TRegisterCallback) => void;

    // Constants that module returns
    getConstants: () => {
      BATTERY_INFO_EVENT: string;
    };

    // Should be default. Uses to support NativeEventEmitter
    addListener: (event: string) => void;
    removeListeners: () => void;
  };
  // May pass other modules in case of multiple modules
}

const { BatteryInfo } = NativeModules as INativeModules;
```

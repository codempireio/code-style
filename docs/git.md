# Codempire: Git Flow ⭐

[Return to Table of Contents](../README.md)

## Table of Contents 🏆

  - [**General Guideline**](#general)
  - [**Pull Request**](#pull-request)

## **General**

### **Goal**

> Our approach is follow understandable git system where easy to understand what is on every branch and what changes will be in next PR. We wan to keep our git log clean and understandable where we can easily revert, checkout and remove commits. 👍

**Rules** ❗

1. One feature or bugfix = One commit;
2. One task or issue = One PR;
3. Clear branch and commit message to keep clean history.

### **Setup** 🐸

> Before you start working with any repo you need to create git config with next commands

1. `git config --global user.name "Dev Name"`
2. `git config --global user.email dev@codempire.io`
3. `git config --global core.editor nano` instead of nano you can choose whatever you like

### **Branches**

**Stable branches** 😎

`master` - main development branch. Developers will open their PR here

`production` - release branch with actual release changes. PR on this branch usually created from master or this branch is automatically updates from CI/CD

`test` | `stage` | `demo` - others environment branches which are deployed to some servers

**Temporary (PR's branches)** 😈

`feature/feature-name` - developer branch for PR to `master` branch with new functionality. Instead of *feature-name* here can be next examples *feature/login*, *feature/user-search*, *feature/table-layout*, etc.

`bugfix/feature-fixes` - developer branch for PR to `master` branch with some bug fixing. For example there could be *bugfix/page-reload*, *bugfix/user-session*.

`production/what-deploying` - developer branch from `master` to `production` where will be set of several commits which we need to deploy to any environment such as *production*, *test*, *stage*, etc.

### **Commits**

> Usually we're using 2 approaches for commit message

1. Repeat branch name to commit message. In this case commits should be very short and change only named part of project. Examples: `feature/date-picker`, `bugfix/time-slot-crash`, `feature/navigation-setup`
2. Create clear commit message which will explain more clear what was changed. Should be written in past tense. Examples: `Added datepicker to user panel`, `Fixed user session problem on ios`, `Updated profile screen layout`

## **Pull Request**

### **Check List**

> Before open a PR you need to check next rules

1. You are up to date with `master` branch to avoid conflicts;
2. You are following all our rules: 1 commit with clear message and all inside you commit related to certain issue.
3. The application doesn't crash and doesn't put any errors to console.
4. You have no `any` types inside your typescript code

### **Pulling `master` branch**

> Our goal is to pull `master` branch and update dev branch to avoid further conflicts and do this without ugly *merge commit*. To do this we need to do next steps:

1. Fetch all origin branches with  `git fetch` command

2. Rebase master branch from origin with `git rebase origin/master` command if you have any conflicts you'll resolve them and after it type `git rebase --continue` command

### **Rename commit message**

> If you have a typo or not clear commit message you could update it in several ways:

1. Using `git commit --amend` command 😌 After it text editor will open previous commit message and you could change it. Also you could change author of previous commit `git commit --amend --author="Codempire <dev@codempire.io>"` and other stuff. Con of this approach is limitation of only previous commit

2. More complex way is to use interactive rebase `git rebase -i HEAD~n` 😅 after command you'll see text editor and there you could put `r` or `reword` string before commit and in next step you will rename it. Main pro of this method is ability to reword any commit in git history (depends on your `n`).

> After any of this method you need to push you changes with ability to rewrite previous name `git push -f` 🚀

### **Combining commits**

> To keep our git flow clean it's necessary to combine commits in PR into one. We're doing this using `rebase magic` 😎 Let's imaging situation with 4 commits and we need to combine them into 1.

1. All starts with `git rebase --interactive HEAD~4`  (instead of 4 you will put your amount of commits)

2. In opened text editor `pick` commit what you want to save and put `squash` near unwanted commits

   ```sh
      pick ab30a62 feature/date-filter
      squash 8bca4d3 updated review commit
      squash dh12gs2 date bug fix
      s dh12gs2 updated typescript

   ```

3. If there're any conflicts between those commits you'll be asked to resolve them. And after you finish you need to type `git rebase --continue`

4. After git finish this rebase you'll be asked to type commit message for all of this commits. Usually we're commenting unwanted commits with `#`

   ```sh
   # This is a combination of 4 commits.
   # This is the 1st commit message:

   feature/date-filter
   # This is the commit message #2:

   # updated review commit

   # This is the commit message #3:

   # date bug fix

   # This is the commit message #3:

   # updated typescript

   ```

5. And the final step will be push updated version of git history, do this with `git push -f` to rewrite current history 🍺

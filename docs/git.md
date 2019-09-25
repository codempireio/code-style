# Git Flow

[Return to Table of Contents](../README.md)

> Our goal is keep our git log clean and understandable where we can easy revert, checkout and remove commits
> For this approach we have next rules:
>
> - 1 feature/1 bugfix = 1 commit;
> - 1 issue = 1 pull request;
> - clear commit message
>   `git rebase` is our main tool to keep that rules
>

## Our Branches

`master` - our main codebase branch

`release` - release branch with latest changes for production

`feature/feature-name` | `bugfix/what-fixed` - developer will open PR to `master` from these branches

## How to be up to date with `master`

> To merge master changes into current branch without creating annoying `merge commit` we are doing next steps

### 1. fetching repo with `git fetch`

### 2. rebasing unmerged commit from master branch with `git rebase origin/master`

### 3. resolving conflicts _if needed_ than `git rebase --continue`

## How to squash several commits into 1

> To keep our git flow clean we're using rebase to squash several commit into one

### 1. rebasing `n` of commits that we need to squash/rename with `git rebase --interactive HEAD~n` o

> For example:

```
git rebase -i HEAD~3
```

### 2. setup commands for each commit that need to be handle in opened text editor

> For example:

```
   pick ab30a62 feature/date-filter
   squash 8bca4d3 fixed support
   squash dh12gs2 refactoring
```

> After this operating we will keep only first (picked) commit and squash 2 into it

### 3. type commit message for selected squashed commit

> For example:

```
# This is a combination of 3 commits.
# This is the 1st commit message:

feature/date-filter
# This is the commit message #2:

# fixed support

# This is the commit message #3:

# refactoring

```

> Where we commenting with `#` unnecessary commit messages

### 4. resolving conflicts _if needed_ than `git rebase --continue`

### 5. after we rebase commit history we should update origin `git push --force`

## How to add changes to previous commit for single feature

> When you need to update already pushed commit after code review for example

### 1. add your changes to previous commit `git commit --amend`

### 2. update commit message of previous commit in text edior

### 3. force update your branch `git push -f`

## commit message

> Usually commit messages differs from project to project. But commonly we use commit message as our `issue branch`.
> For example:
>
> - `feature/date-picker`
> - `bugfix/time-slot-crash`
> - `feature/navigation-setup`
>

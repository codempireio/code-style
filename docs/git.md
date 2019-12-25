# Git Flow

[Return to Table of Contents](../README.md)

> Our goal is to keep our git log clean and understandable where we can easily revert, checkout and remove commits
> For this approach we have next rules:
>
> - 1 feature/1 bugfix = 1 commit;
> - 1 issue = 1 pull request;
> - clear commit message
>   `git rebase` is our main tool to keep that rules

## Our Branches

`master` - our main codebase branch

`release` - release branch with latest changes for production

`feature/feature-name` | `bugfix/what-fixed` - developer will open PR to `master` from these branches

## Pull Request check-list

> Before every pull request check next rules

1. You are up to date with `master` branch
2. You are following `1 feature = 1 commit` rule
3. You wrote the correct commit message
4. The application doesn't crash
5. There are no errors in the console
6. You have no `any` type in your typescript code

## How to be up to date with `master` branch

> To merge master changes into current branch without creating annoying `merge commit` we are doing next steps

1. fetching repo with `git fetch`

2. rebasing unmerged commit from the master branch with `git rebase origin/master`

3. resolving conflicts _if needed_ than `git rebase --continue`

## How to squash several commits into 1

> To keep our git flow clean we're using rebase to squash several commits into one

1. rebasing `n` of commits that we need to squash/rename with `git rebase --interactive HEAD~n` o

> For example:

```sh
git rebase -i HEAD~3
```

1. setup commands for each commit that need to be handled in opened text editor

> For example:

```sh
   pick ab30a62 feature/date-filter
   squash 8bca4d3 fixed support
   squash dh12gs2 refactoring
```

> After this operating we will keep only first (picked) commit and squash 2 into it

1. type commit message for selected squashed commit

> For example:

```sh
# This is a combination of 3 commits.
# This is the 1st commit message:

feature/date-filter
# This is the commit message #2:

# fixed support

# This is the commit message #3:

# refactoring

```

> Where we commenting with `#` unnecessary commit messages

1. resolving conflicts _if needed_ than `git rebase --continue`

2. after we rebase commit history we should update origin `git push --force`

## How to add changes to the previous commit for a single feature

> When you need to update already pushed commit after code review for example

1. add your changes to previous commit `git commit --amend`

2. update commit message of the previous commit in the text editor

3. force update your branch `git push -f`

## How to write correct commit message

> Usually commit messages differs from project to project. But commonly we use commit message as our `issue branch`.
> For example:
>
> - `feature/date-picker`
> - `bugfix/time-slot-crash`
> - `feature/navigation-setup`

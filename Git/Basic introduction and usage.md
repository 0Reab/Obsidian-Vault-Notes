## Introduction
Git is for version control of your code repository.
There are topics and concepts:
1. Repository - place where you store code
2. Branch - Copy of main branch which you can change safely
3. Issue - Tickets for the code repository, bug reports, feature requests...
4. Pull request - Address the issue with changes in repo 
5. Pull - Sync your local code with code in the online repo
6. Merge request - Approve merging of 'development' branch to the 'main'
7. Commit - Small repo change with message to track progress.
8. Push - Update the branch with new commits - push onto the branch
9. Clone - Copy the repo locally
10. Fork - Copy the repo from someone else


## Clone

```shell
git clone 'url_of_repository'
```

Copy code repository locally.

## Commit

```shell
git commit -m 'this is the message for my commit'
```

So for example you refactor a function in 'development' branch, now you can commit those changes with the message.
If the change is related to an issue in 'issues' of the repository you can use keywords in the message to reference the issue and automagically close it when making a pull request. Example 'Closes #4'.

## Checkout

```shell
git checkout -b 'new_branch_name'
```

Create a new branch of repo and switch to it.

## Branch

```shell
git branch 'branch_name'
git branch -l # list branches
```

Change to different branch.

## Push

```shell
git push origin 'branch_name'
```

This will push the previous commits to the branch you choose.
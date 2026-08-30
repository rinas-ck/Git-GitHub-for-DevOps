Git & GitHub --- Complete DevOps Study Notes, Lab Practice & Interview Questions

Goal: Build strong practical Git/GitHub skills for Cloud/DevOps
interviews and day-to-day DevOps work.

This guide includes the topics from my handwritten notes, plus
important missing topics, hands-on labs, real-world workflows,
troubleshooting, and interview questions.

1. What is Version Control?

Version Control System (VCS) is a tool used to track changes to
files over time.

It helps developers:

Track who changed what and when.

Compare different versions.

Restore previous versions.

Work safely with other developers.

Create branches for separate features.

Merge completed work.

Types of VCS

Centralized Version Control System --- CVCS

Examples:

SVN

CVS

Characteristics:

One central server contains the repository.

Developers connect to the central server to commit/update.

If the server is unavailable, collaboration can be affected.

The central repository is the main source of truth.

Distributed Version Control System --- DVCS

Examples:

Git

Mercurial

Characteristics:

Every developer normally has a complete local repository.

Commits can be created locally.

Work can continue without network access.

Changes can later be pushed/pulled between repositories.

2. Git

Git is a distributed version control system used to track source-code
changes and collaborate on software projects.

Git was created by Linus Torvalds in 2005, initially for Linux
kernel development.

Why Git is important for DevOps

Git is commonly used to:

Store application/source code.

Manage infrastructure code.

Manage Terraform configurations.

Store Dockerfiles.

Store Kubernetes manifests.

Trigger CI/CD pipelines.

Review changes through pull requests.

Maintain release history.

Roll back changes.

3. Git Architecture

A basic Git workflow can be understood as:

Working Directory
       |
       | git add
       v
Staging Area
       |
       | git commit
       v
Local Repository
       |
       | git push
       v
Remote Repository
   (GitHub/GitLab/etc.)

Working Directory

The actual files you are currently editing.

Staging Area

A temporary area where you select changes that should go into the next
commit.

Local Repository

The .git directory containing Git's history, objects, references,
configuration, etc.

Remote Repository

A repository hosted on a remote service such as GitHub.

4. Git Installation & Initial Configuration

Check Git:

git --version

Configure username:

git config --global user.name "Your Name"

Configure email:

git config --global user.email "your-email@example.com"

View configuration:

git config --list

Check a specific setting:

git config user.name
git config user.email

Set default branch:

git config --global init.defaultBranch main

5. Create a Local Git Repository

Create a project:

mkdir git-demo
cd git-demo

Initialize Git:

git init

Check status:

git status

This creates a hidden .git directory.

ls -la

6. Basic Git Workflow

Create a file:

echo "Hello Git" > app.txt

Check status:

git status

Add one file:

git add app.txt

Add all changed files:

git add .

Commit:

git commit -m "Add app file"

View commits:

git log

Compact log:

git log --oneline

View current changes:

git diff

View staged changes:

git diff --staged

7. Git Commit

A commit is a snapshot of staged changes.

Good commit messages are:

Add login validation
Fix Docker build issue
Update Terraform variables
Add Kubernetes deployment
Fix CI pipeline

Avoid vague messages such as:

changes
update
test
done

A commit should ideally represent one logical change.

8. Git Add

Add one file:

git add file.txt

Add multiple files:

git add file1.txt file2.txt

Add everything:

git add .

Interactive staging:

git add -p

git add -p is useful when only part of a file should be included in a
commit.

9. Git Status

git status

It helps identify:

Untracked files.

Modified files.

Staged files.

Current branch.

Changes ready for commit.

Run git status frequently.

10. Git Restore

Discard changes in a working-tree file:

git restore file.txt

Unstage a file:

git restore --staged file.txt

Be careful: restoring a file can permanently remove uncommitted changes.

11. Git Reset

Soft reset

Moves HEAD but keeps changes staged:

git reset --soft HEAD~1

Useful when you want to redo the previous commit.

Mixed reset

Default reset. Moves HEAD and unstages changes while keeping file
contents:

git reset HEAD~1

Hard reset

Moves HEAD and discards tracked working-tree changes:

git reset --hard HEAD~1

Warning: --hard can destroy uncommitted work.

12. Git Revert

git revert creates a new commit that reverses an earlier commit.

git revert <commit-id>

This is generally safer for changes that have already been pushed to a
shared branch.

Reset vs Revert

Command        Main purpose

git reset    Move branch/HEAD backward
git revert   Create a new commit undoing an old commit

Interview point:

Prefer git revert for undoing changes already shared with other
developers because it preserves public history.

13. Git Reflog

Reflog records movements of HEAD and branch references.

git reflog

It can help recover from mistakes such as:

Accidental reset.

Lost commit.

Wrong rebase.

Deleted branch.

Example:

git reset --hard HEAD@{1}

Use the appropriate reflog entry after checking it carefully.

14. Git Branches

A branch is an independent line of development.

List branches:

git branch

Create branch:

git branch feature-login

Create and switch:

git switch -c feature-login

Switch branch:

git switch feature-login

Older equivalent:

git checkout feature-login

Delete local branch:

git branch -d feature-login

Force delete:

git branch -D feature-login

15. Why Use Branches?

Example:

main
 |
 +--- feature/login
 |
 +--- feature/payment
 |
 +--- bugfix/api

Branches allow developers to work independently without directly
changing main.

Typical branches:

main

develop

feature/*

bugfix/*

hotfix/*

release/*

16. Branch Merge

Suppose:

main
 |
 A---B
      \
       C---D   feature

Switch to main:

git switch main

Merge:

git merge feature

After merge:

A---B-------E
     \     /
      C---D

A merge commit may be created depending on the history and merge
strategy.

17. Fast-Forward Merge

If the target branch has not moved:

A---B---C main
         \
          D---E feature

After fast-forward:

A---B---C---D---E main

No separate merge commit is required.

18. Merge Conflict

A conflict happens when Git cannot automatically combine changes.

Typical process:

git merge feature

Git reports a conflict.

Check:

git status

Open the conflicted file:

<<<<<<< HEAD
current branch changes
=======
incoming branch changes
>>>>>>> feature

Edit the file and keep the correct content.

Then:

git add <file>
git commit

Abort the merge if needed:

git merge --abort

Conflict resolution habit

Understand both changes.

Decide the correct final code.

Remove conflict markers.

Test the application.

Stage the resolved file.

Complete the merge.

19. Git Rebase

Rebase moves/replays commits onto another base.

Example:

A---B---C main
     \
      D---E feature

Run:

git switch feature
git rebase main

Result:

A---B---C---D'---E' feature

Merge vs Rebase

Merge:

Preserves branch history.

Creates merge commit when needed.

Safer for shared history.

Rebase:

Creates a cleaner linear history.

Rewrites commit history.

Should be used carefully on already-published shared commits.

Interview rule:

Do not casually rebase commits that other people are already depending
on.

20. Interactive Rebase

Useful for cleaning local commit history:

git rebase -i HEAD~3

Common operations:

pick
reword
edit
squash
fixup
drop

Use it before opening a PR when you need to clean up your own local
commits.

21. Cherry-Pick

Apply a specific commit from another branch:

git cherry-pick <commit-id>

Useful when:

One bug fix is needed on another branch.

You do not want to merge the entire branch.

A hotfix needs to be copied to a release branch.

22. Git Stash

git stash temporarily saves uncommitted work.

Use case:

You are working on feature A, but suddenly need to switch to another
branch. Your current changes are not ready to commit.

Save changes:

git stash

Include untracked files:

git stash -u

List stashes:

git stash list

Apply a stash:

git stash apply

Apply a specific stash:

git stash apply stash@{0}

Apply and remove it:

git stash pop

Delete a stash:

git stash drop stash@{0}

Delete all stashes:

git stash clear

Create a stash with a message:

git stash push -m "login work"

23. Git Tag

Tags identify important commits, usually releases.

List tags:

git tag

Create tag:

git tag v1.0.0

Annotated tag:

git tag -a v1.0.0 -m "Release version 1.0.0"

Push tag:

git push origin v1.0.0

Push all tags:

git push origin --tags

Delete local tag:

git tag -d v1.0.0

24. Git Show

Show commit/tag information:

git show <commit-id>

Show a tag:

git show v1.0.0

25. Git Log --- Important Options

git log
git log --oneline
git log --graph --oneline --all
git log --author="Name"
git log --since="1 week ago"
git log -- file.txt

A very useful visual command:

git log --oneline --graph --decorate --all

26. Git Diff

Working directory vs staging area:

git diff

Staging area vs last commit:

git diff --staged

Compare branches:

git diff main..feature

Compare commits:

git diff <commit1> <commit2>

27. Delete / Rename Files

Delete a tracked file:

git rm file.txt

Rename:

git mv old.txt new.txt

Then commit:

git commit -m "Rename file"

28. .gitignore

.gitignore tells Git which files should not normally be tracked.

Example:

.env
*.log
node_modules/
__pycache__/
.terraform/
*.tfstate
*.tfstate.*
.vscode/
.idea/
.DS_Store

Important DevOps rule:

Never commit passwords, API keys, private keys, cloud credentials, or
other secrets.

If a secret was committed, simply adding it to .gitignore does not
remove it from Git history.

29. Git Remote

A remote is a named reference to another repository.

List remotes:

git remote -v

Add remote:

git remote add origin <repository-url>

Change remote URL:

git remote set-url origin <repository-url>

Remove remote:

git remote remove origin

Show remote details:

git remote show origin

origin is only a conventional name. It is not a special requirement.

30. GitHub

GitHub is a cloud platform for hosting Git repositories and
collaborating on software projects.

Git is the version-control tool.

GitHub is a hosting/collaboration platform built around Git.

Simple difference

Git       = Version control system
GitHub    = Remote hosting + collaboration platform

Other Git hosting platforms include GitLab and Bitbucket.

31. Create a GitHub Repository

Typical workflow:

Create a repository on GitHub.

Create or initialize the local project.

Add GitHub as the remote.

Commit the code.

Push to GitHub.

Example:

git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin <repository-url>
git push -u origin main

32. Clone a GitHub Repository

git clone <repository-url>

Clone into a specific directory:

git clone <repository-url> project-name

After cloning:

cd project-name
git remote -v

A clone normally gives you the working files plus the local Git
repository/history.

33. Git Fetch

Download remote updates without changing your current working branch:

git fetch origin

Fetch everything:

git fetch --all

Fetch is useful for reviewing remote changes before integrating them.

34. Git Pull

git pull normally performs a fetch followed by integration of the
fetched changes.

git pull

Explicitly:

git pull origin main

A pull can use merge or rebase depending on configuration/options.

Useful safer workflow:

git fetch origin
git log --oneline --graph --all
git merge origin/main

35. Git Push

Push a local branch:

git push origin main

First push and set upstream:

git push -u origin main

After upstream is configured:

git push

Push a new branch:

git push -u origin feature/login

Delete a remote branch:

git push origin --delete feature/login

36. Push & Pull Operation --- Practical Flow

Developer A
    |
    | git push
    v
GitHub Repository
    ^
    | git pull/fetch
    |
Developer B

If another developer pushes first:

Your local branch
       |
       | git push
       X  rejected
       |
Remote contains newer commits

Then:

git pull --rebase

or fetch and integrate manually:

git fetch origin
git rebase origin/main
git push

Resolve conflicts if necessary.

37. Remote Tracking Branches

Examples:

main
origin/main
origin/feature/login

origin/main is your local reference to the remote-tracking state of
the main branch on origin.

Update remote-tracking references:

git fetch origin

38. GitHub Authentication

HTTPS

HTTPS repositories use authentication through GitHub-supported
credentials/tokens rather than your normal GitHub account password.

Personal Access Token --- PAT

A PAT can be used for HTTPS Git authentication.

Important principles:

Give the token only required permissions.

Keep it secret.

Never commit it.

Never paste it into public repositories.

Revoke compromised tokens immediately.

Prefer short-lived/fine-grained credentials where appropriate.

Typical remote:

git remote -v

Do not store a real token in scripts or source code.

39. SSH Authentication

SSH is a common way to authenticate Git operations without repeatedly
entering credentials.

Generate a key:

ssh-keygen -t ed25519 -C "your-email@example.com"

Start agent:

eval "$(ssh-agent -s)"

Add key:

ssh-add ~/.ssh/id_ed25519

Copy the public key:

cat ~/.ssh/id_ed25519.pub

Add the public key to GitHub.

Test:

ssh -T git@github.com

Use SSH remote:

git remote set-url origin git@github.com:USERNAME/REPOSITORY.git

Private vs Public Key

Private key -> Keep secret
Public key  -> Upload to GitHub

Never share the private key.

40. GitHub Pull Request --- PR

A Pull Request is a request to review and merge changes from one branch
into another.

Typical workflow:

main
 |
 +--- feature/login
          |
          | commits
          v
       Push branch
          |
          v
     Open Pull Request
          |
       Code Review
          |
       CI Checks
          |
       Approval
          |
        Merge

A PR can include:

Description.

Code changes.

Review comments.

Automated checks.

Approvals.

Discussion.

Linked issues.

41. Fork

A fork is your own GitHub copy of another user's repository under
your account.

Typical open-source workflow:

Original Repository
       |
      Fork
       v
Your GitHub Repository
       |
     Clone
       |
     Modify
       |
      Push
       |
    Pull Request
       |
Original Repository

A fork is different from simply cloning a repository.

42. Fork vs Clone

Fork                                Clone

Creates a GitHub-side copy under    Downloads a repository to your
your account                        computer

Common in open-source contribution  Used for local development

Happens on GitHub                   Happens locally

You can fork first and then clone your fork.

43. Upstream Remote

When working from a fork:

git remote add upstream <original-repository-url>

Check:

git remote -v

Typical setup:

origin   -> your fork
upstream -> original repository

Get latest original changes:

git fetch upstream

Update your branch as required:

git switch main
git merge upstream/main

Then push to your fork:

git push origin main

44. GitHub Issues

Issues can be used for:

Bugs.

Feature requests.

Tasks.

Improvements.

Discussions/work tracking.

A DevOps team may use issues to track:

Bug -> Fix -> Branch -> PR -> CI -> Review -> Merge -> Close issue

45. GitHub Actions

GitHub Actions provides CI/CD automation.

Example workflow:

Developer pushes code
        |
        v
GitHub Actions
        |
   +----+----+
   |         |
  Build     Test
   |         |
   +----+----+
        |
      Deploy

Workflow files are normally stored under:

.github/workflows/

Example:

name: CI

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Run tests
        run: echo "Run tests here"

46. GitHub Actions and DevOps

GitHub can become the trigger/source for a CI/CD pipeline.

Example:

Git Push
   |
   v
GitHub
   |
   v
GitHub Actions
   |
   +--> Build
   |
   +--> Test
   |
   +--> Docker Build
   |
   +--> Push Image
   |
   +--> Deploy

GitHub Actions can also interact with:

AWS

Docker

Kubernetes

Terraform

Azure

GCP

47. GitHub Secrets

Never hard-code credentials inside workflow files.

Use:

Repository/Environment Secrets

Examples:

AWS credentials
Docker registry credentials
API tokens
SSH keys

Access secrets in Actions using GitHub's secrets context.

Example:

env:
  API_TOKEN: ${{ secrets.API_TOKEN }}

Do not print secrets in logs.

48. GitHub Repository Best Practices

A professional repository may contain:

project/
├── README.md
├── .gitignore
├── LICENSE
├── src/
├── tests/
├── Dockerfile
├── .github/
│   └── workflows/
│       └── ci.yml
└── docs/

For infrastructure:

terraform/
├── main.tf
├── variables.tf
├── outputs.tf
├── providers.tf
└── README.md

49. README.md

A good project README should explain:

Project name.

Project purpose.

Architecture.

Technologies.

Prerequisites.

Installation.

Configuration.

Usage.

How to run tests.

Deployment.

CI/CD.

Screenshots/diagrams if useful.

Troubleshooting.

Author/contact information.

50. Git Branching Strategies

Feature Branch Workflow

main
 |
 +--- feature/A
 |
 +--- feature/B

Developers create feature branches and merge through PRs.

Git Flow

Common branch types:

main
develop
feature/*
release/*
hotfix/*

Trunk-Based Development

Developers integrate small changes frequently into a main/trunk branch,
often using short-lived branches and feature flags.

For modern CI/CD, trunk-based approaches are often preferred where the
team/process supports them.

51. Feature Flags

Feature flags allow functionality to be enabled/disabled without
deploying completely different code.

Concept:

if feature_enabled:
    new_feature()
else:
    old_feature()

Useful for:

Gradual rollout.

Testing in production.

Quick rollback of functionality.

Separating deployment from release.

52. Git Release

Typical release flow:

Feature branches
       |
       v
Pull Requests
       |
       v
main
       |
       v
Tag v1.0.0
       |
       v
CI/CD
       |
       v
Production

Example:

git tag -a v1.0.0 -m "Release 1.0.0"
git push origin v1.0.0

53. Git Hooks

Git hooks allow scripts to run at Git events.

Examples:

pre-commit

commit-msg

pre-push

post-merge

Use cases:

Run linting.

Run tests.

Validate commit messages.

Prevent accidental commits containing secrets.

Do not rely only on local hooks for security because developers can
bypass them. Important checks should also run in CI.

54. Git Internals

Git stores objects inside:

.git/objects/

Important Git objects:

Blob --- file contents.

Tree --- directory structure.

Commit --- snapshot metadata and parent relationship.

Tag --- annotated tag object.

Important references:

HEAD
branches
tags
refs

HEAD

HEAD points to the currently checked-out commit/reference.

Example:

git show HEAD

55. HEAD, Working Tree & Index

Think of:

Working Tree
     |
 git add
     v
Index / Staging Area
     |
 git commit
     v
Repository

HEAD points to the current checked-out commit.

56. Detached HEAD

Detached HEAD means HEAD points directly to a commit rather than a
branch.

Example:

git checkout <commit-id>

or:

git switch --detach <commit-id>

You can inspect/test old code.

If you create useful commits there, create a branch:

git switch -c recovery-branch

57. Git Blame

Shows which commit/author last modified each line:

git blame file.txt

Useful for understanding the history of a specific line.

58. Git Bisect

Used to find the commit that introduced a bug.

Start:

git bisect start

Mark current version as bad:

git bisect bad

Mark a known good version:

git bisect good <commit-id>

Git checks out a middle commit.

Test it:

git bisect good

or:

git bisect bad

Repeat until Git identifies the problematic commit.

Finish:

git bisect reset

59. Git Clean

Shows untracked files that would be removed:

git clean -n

Remove untracked files:

git clean -f

Remove untracked directories too:

git clean -fd

Be careful: this can permanently delete untracked files.

60. Git Maintenance Commands to Know

git status
git log
git diff
git show
git branch
git switch
git merge
git rebase
git cherry-pick
git stash
git tag
git remote
git fetch
git pull
git push
git reset
git revert
git reflog
git blame
git bisect

61. Important Difference: Fetch vs Pull

git fetch

Downloads remote changes but does not normally integrate them into your
current branch.

git fetch origin

git pull

Fetches and then integrates the changes.

git pull

Interview answer:

Fetch is safer when I want to inspect remote changes first. Pull is
convenient when I am ready to integrate them.

62. Important Difference: Pull vs Clone

Clone

Used when you do not yet have the repository locally:

git clone <url>

Pull

Used after the repository already exists locally:

git pull

63. Important Difference: Git vs GitHub

Git:

Distributed VCS.

Runs locally.

Tracks history.

GitHub:

Hosted Git platform.

Remote repositories.

Pull Requests.

Issues.

Actions.

Reviews.

Collaboration.

64. Important Difference: Merge vs Rebase

Merge

git merge feature

Pros:

Preserves existing history.

Does not rewrite existing commits.

Cons:

Can create a more complex graph.

Rebase

git rebase main

Pros:

Linear history.

Cleaner commit graph.

Cons:

Rewrites commit history.

Dangerous if rewriting commits already shared by others.

65. Important Difference: Reset vs Revert

reset  -> changes branch history
revert -> creates a new undo commit

For a public/shared branch, revert is generally the safer choice.

66. Important Difference: Stash vs Commit

Stash:

Temporary.

Useful for unfinished work.

Not a replacement for normal commits.

Commit:

Permanent history entry.

Represents a logical change.

Can be pushed/shared.

67. Lab 1 --- Basic Git Repository

Objective

Create a repository and make your first commits.

Steps

mkdir git-lab-01
cd git-lab-01
git init

echo "Version 1" > app.txt
git add app.txt
git commit -m "Add version 1"

echo "Version 2" >> app.txt
git status
git diff

git add app.txt
git commit -m "Update app"

git log --oneline

Verify

git status
git log --oneline

Expected:

Working tree clean

68. Lab 2 --- Branching & Merge

mkdir git-branch-lab
cd git-branch-lab

git init
echo "main" > app.txt
git add .
git commit -m "Initial commit"

git switch -c feature/login
echo "login feature" >> app.txt
git add .
git commit -m "Add login feature"

git switch main
git merge feature/login

git log --oneline --graph --all

Practice:

Create another branch.

Make two commits.

Merge it into main.

Delete the feature branch.

69. Lab 3 --- Merge Conflict

Create a branch:

git switch -c feature-a

Change the same line in a file and commit.

Switch back:

git switch main

Change the same line differently and commit.

Merge:

git merge feature-a

Resolve the conflict manually.

Then:

git add .
git commit

Practice this until you can resolve conflicts confidently.

70. Lab 4 --- Stash

git switch -c feature-a
echo "unfinished work" >> app.txt

git stash
git status

git switch main

git stash list
git stash pop

Practice:

git stash push -m "feature-a unfinished"
git stash list
git stash apply stash@{0}
git stash drop stash@{0}

71. Lab 5 --- Reset, Revert & Reflog

Create several commits:

echo "A" > file.txt
git add .
git commit -m "A"

echo "B" >> file.txt
git add .
git commit -m "B"

echo "C" >> file.txt
git add .
git commit -m "C"

View history:

git log --oneline

Practice:

git reset --soft HEAD~1

Then inspect:

git status

Also practice:

git reset --hard HEAD~1
git reflog

Then recover a previous state using the appropriate reflog entry.

Do this only in a disposable lab repository.

72. Lab 6 --- GitHub Remote

Create a new empty GitHub repository.

Then:

git init
echo "# GitHub Lab" > README.md
git add .
git commit -m "Initial commit"

git branch -M main
git remote add origin <your-repository-url>
git push -u origin main

Verify:

git remote -v
git branch -a

73. Lab 7 --- Clone, Fetch & Pull

Clone:

git clone <repository-url>
cd <repository>

Create a change from another location/account if possible.

Then:

git fetch origin
git log --oneline --all
git pull

Understand exactly what changed before and after each command.

74. Lab 8 --- GitHub Feature Branch & PR

Clone your repository.

Create a feature branch.

Add a change.

Commit.

Push the branch.

Open a Pull Request on GitHub.

Review the diff.

Add another commit.

Observe the PR update.

Merge the PR.

Delete the branch.

Commands:

git switch -c feature/readme-update
git add .
git commit -m "Improve README"
git push -u origin feature/readme-update

75. Lab 9 --- Fork & Upstream

Use a public repository where you have permission to contribute.

Workflow:

git clone <your-fork-url>
cd repository

git remote add upstream <original-repository-url>

git remote -v

git fetch upstream
git switch main
git merge upstream/main

git push origin main

Then create a feature branch and open a PR to the original repository.

76. Lab 10 --- GitHub Actions CI

Create:

.github/workflows/ci.yml

Example:

name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Run test
        run: |
          echo "Running tests"
          echo "CI passed"

Push it and inspect the Actions tab on GitHub.

77. Lab 11 --- Git + Docker

Create a simple project:

git-docker-lab/
├── Dockerfile
├── app.py
├── requirements.txt
└── README.md

Practice:

git init
git add .
git commit -m "Add Docker application"

Create a feature branch and modify the Dockerfile.

Push and create a PR.

78. Lab 12 --- Git + Terraform

Create:

terraform-lab/
├── main.tf
├── variables.tf
├── outputs.tf
├── .gitignore
└── README.md

Important:

.terraform/
*.tfstate
*.tfstate.*
*.tfvars

Do not commit cloud credentials or sensitive Terraform variable values.

Practice:

git add .
git commit -m "Add Terraform infrastructure"
git push

79. Lab 13 --- Real DevOps Git Workflow

Simulate a company workflow:

main
 |
 +--- feature/vpc
 |
 +--- feature/docker
 |
 +--- feature/ci

For each feature:

Create branch.

Make changes.

Commit.

Push.

Open PR.

Review.

Run CI.

Merge.

Delete branch.

Pull latest main.

Repeat until the process becomes natural.

80. Lab 14 --- GitHub Actions + Docker

Build a workflow:

Git push
   |
   v
GitHub Actions
   |
   +--> Checkout
   |
   +--> Test
   |
   +--> Docker Build
   |
   +--> Docker Image

Extend the workflow to:

Build a Docker image.

Tag it.

Push it to a container registry.

Use GitHub Secrets for credentials.

Never hard-code credentials.

81. Lab 15 --- Git Troubleshooting

Practice solving these deliberately:

Scenario 1

git push rejected

Investigate:

git fetch origin
git log --oneline --graph --all

Then decide whether to merge or rebase.

Scenario 2

Accidentally committed a secret.

Practice:

Rotate/revoke the secret.

Remove it from the working tree.

Understand that removing the latest copy does not automatically
erase history.

Learn history-rewriting tools for appropriate cases.

Force-push only with careful coordination when history has to be
rewritten.

Scenario 3

Accidentally reset a branch.

Use:

git reflog

Recover the correct commit.

Scenario 4

Merge conflict.

Resolve it and complete the merge.

82. Professional Git Commit Workflow

A strong daily workflow:

git status
git switch main
git pull --rebase
git switch -c feature/my-change

# edit files

git diff
git add -p
git diff --staged
git commit -m "Add my change"
git push -u origin feature/my-change

Then:

Pull Request
   ↓
Review
   ↓
CI
   ↓
Approval
   ↓
Merge

83. Before Every Push

Check:

git status
git diff
git diff --staged
git log --oneline -5

Look for:

Secrets.

Debug code.

Large unnecessary files.

Passwords.

Tokens.

Private keys.

Terraform state.

.env files.

Build artifacts.

84. Common Git Mistakes

Mistake 1 --- Committing secrets

Bad:

AWS_ACCESS_KEY_ID=...
AWS_SECRET_ACCESS_KEY=...

Fix:

Revoke/rotate credentials immediately.

Remove the secret.

Investigate whether history must be cleaned.

Mistake 2 --- Using force push carelessly

Avoid:

git push --force

Prefer:

git push --force-with-lease

when history rewriting is intentionally required and the team allows it.

Mistake 3 --- Huge commits

Prefer small logical commits.

Mistake 4 --- Working directly on main

Use the team's approved branching/PR process.

Mistake 5 --- Pulling without understanding local changes

Check:

git status

first.

85. Interview Questions --- Basic

Q1. What is Git?

Git is a distributed version control system used to track changes and
manage source-code history.

Q2. What is GitHub?

GitHub is a hosted platform for Git repositories and software
collaboration.

Q3. Difference between Git and GitHub?

Git provides version control. GitHub provides remote hosting and
collaboration features such as PRs, Issues and Actions.

Q4. What is a repository?

A repository is a Git project containing files and version history.

Q5. What is a commit?

A commit is a recorded snapshot of staged changes.

Q6. What is a branch?

A branch is a movable reference used to develop changes independently.

Q7. What is the staging area?

The staging area contains changes selected for the next commit.

Q8. What is git init?

It initializes a Git repository in a directory.

Q9. What is git clone?

It creates a local copy of a remote repository.

Q10. What is git status?

It displays the state of the working tree and staging area.

86. Interview Questions --- Intermediate

Q11. Difference between git fetch and git pull?

Fetch downloads remote updates without normally integrating them into
the current branch. Pull fetches and then integrates.

Q12. Difference between merge and rebase?

Merge combines histories and can create a merge commit. Rebase replays
commits onto a new base and rewrites commit IDs.

Q13. What is a merge conflict?

It occurs when Git cannot automatically determine how to combine
conflicting changes.

Q14. How do you resolve a merge conflict?

Check status, inspect conflict markers, edit the file, test, stage the
resolved file, and complete the merge.

Q15. Difference between reset and revert?

Reset moves branch history. Revert creates a new commit that reverses an
earlier commit.

Q16. What is git stash?

It temporarily stores uncommitted changes so the working tree can be
changed safely.

Q17. What is cherry-pick?

It applies the changes introduced by a specific commit onto the current
branch.

Q18. What is git reflog?

It records local updates to references such as HEAD and can help recover
lost commits.

Q19. What is .gitignore?

A file specifying patterns for files Git should normally ignore.

Q20. What is a remote?

A named reference to another Git repository, often hosted on GitHub.

87. Interview Questions --- GitHub

Q21. What is a Pull Request?

A request to review and integrate changes from one branch into another.

Q22. What is a fork?

A GitHub-side copy of another repository under your account.

Q23. Fork vs clone?

Fork creates a remote copy on GitHub. Clone creates a local working
copy.

Q24. What is origin?

A conventional name for the default remote repository created/configured
for a clone or added manually.

Q25. What is upstream?

A conventional name for the original repository when working from a
fork.

Q26. How do you create a PR?

Push a feature branch to GitHub and open a PR from that branch to the
target branch.

Q27. How do you protect main?

Use branch protection/rulesets, required PR reviews, required status
checks, and restricted direct pushes according to team policy.

Q28. How are secrets handled in GitHub Actions?

Store them in GitHub Secrets or appropriate environment/organization
secret stores and reference them securely in workflows.

88. Interview Questions --- Scenario Based

Q29. Your push is rejected. What do you do?

First inspect:

git fetch origin
git status
git log --oneline --graph --all

Then integrate the remote changes using the team's preferred
merge/rebase strategy and push again.

Q30. You accidentally committed a password. What do you do?

Immediately revoke/rotate the credential. Then remove the secret from
the repository and, if necessary, clean it from Git history using an
appropriate history-rewriting method. Coordinate any force push with the
team.

Q31. You deleted a commit accidentally. Can it be recovered?

Often yes, if the commit is still reachable through the reflog or
another reference:

git reflog

Q32. Someone changed the same line you changed. What happens?

A merge/rebase may produce a conflict. Resolve the conflict, test the
result, stage it, and continue.

Q33. You have unfinished work but must switch branches.

Use:

git stash
git switch <branch>

Later:

git stash pop

Q34. How do you undo a bad production commit?

For a shared branch, generally use:

git revert <commit>

Then test and deploy through the normal CI/CD process.

Q35. Why should you avoid force-pushing main?

It can rewrite shared history and disrupt other developers and
automation.

89. DevOps Interview Scenario

Question

A developer pushes code to GitHub. Explain what can happen in a modern
DevOps pipeline.

Answer

Developer
   |
   | git push
   v
GitHub
   |
   v
Pull Request
   |
   v
CI Pipeline
   |
   +--> Checkout
   +--> Lint
   +--> Unit Test
   +--> Security Scan
   +--> Build
   +--> Docker Build
   |
   v
Artifact / Container Registry
   |
   v
Deployment
   |
   +--> Dev
   +--> Staging
   +--> Production

Git is therefore an important foundation for CI/CD.

90. Must-Know Commands Cheat Sheet

Setup

git --version
git config --global user.name "Name"
git config --global user.email "email"
git config --list

Repository

git init
git clone <url>
git status

Changes

git diff
git add .
git add -p
git diff --staged
git commit -m "message"

History

git log
git log --oneline
git log --graph --all
git show <commit>
git reflog

Branches

git branch
git switch -c feature/name
git switch main
git branch -d feature/name

Integration

git merge branch
git rebase main
git cherry-pick <commit>

Undo

git restore file
git restore --staged file
git reset --soft HEAD~1
git reset HEAD~1
git reset --hard HEAD~1
git revert <commit>

Stash

git stash
git stash -u
git stash list
git stash apply
git stash pop
git stash drop
git stash clear

Remote

git remote -v
git remote add origin <url>
git fetch
git pull
git push

Tags

git tag
git tag v1.0.0
git push origin v1.0.0

Troubleshooting

git status
git log --oneline --graph --all
git reflog
git diff
git remote -v

91. 7-Day Git & GitHub Practice Plan

Day 1 --- Fundamentals

Study:

VCS.

CVCS vs DVCS.

Git vs GitHub.

Working directory.

Staging area.

Local repository.

Remote repository.

Practice:

git init
git add
git commit
git status
git log
git diff

Day 2 --- Branching

Practice:

Create branches.

Switch branches.

Merge.

Fast-forward merge.

Merge conflict.

Day 3 --- Undo & Recovery

Practice:

restore.

reset.

revert.

reflog.

clean.

Day 4 --- Stash & Advanced History

Practice:

stash.

rebase.

interactive rebase.

cherry-pick.

tags.

Day 5 --- GitHub

Practice:

Create repository.

Clone.

Remote.

Push.

Pull.

Fetch.

Fork.

PR.

Issues.

Branch rules.

Day 6 --- DevOps Integration

Practice:

GitHub Actions.

Secrets.

Docker + Git.

Terraform + Git.

CI workflow.

Day 7 --- Interview Simulation

Without looking at notes, explain:

Git vs GitHub.

Git architecture.

Git add/commit/push.

Fetch vs pull.

Merge vs rebase.

Reset vs revert.

Stash.

Cherry-pick.

Reflog.

Merge conflicts.

Fork vs clone.

PR workflow.

GitHub Actions.

Git secrets.

Git in a DevOps CI/CD pipeline.

Then perform the full workflow from memory:

clone
  ↓
branch
  ↓
edit
  ↓
diff
  ↓
add
  ↓
commit
  ↓
push
  ↓
PR
  ↓
CI
  ↓
review
  ↓
merge
  ↓
delete branch
  ↓
pull latest main

92. Final Interview Checklist

Before a Git/GitHub interview, make sure you can explain and perform
these without copying commands from the internet:

What is VCS?

CVCS vs DVCS.

What is Git?

Git vs GitHub.

Working tree vs staging area vs repository.

git init.

git add.

git commit.

git status.

git diff.

git log.

Branch creation/switching.

Merge.

Merge conflicts.

Rebase.

Interactive rebase.

Cherry-pick.

Stash.

Tag.

Reset.

Revert.

Reflog.

Fetch.

Pull.

Push.

Remote.

PAT.

SSH.

.gitignore.

Fork.

Clone.

Origin.

Upstream.

Pull Request.

Code review.

Branch protection.

GitHub Actions.

GitHub Secrets.

Git + Docker.

Git + Terraform.

Git + CI/CD.

Secret/credential handling.

Production rollback using revert.

Recovering lost commits with reflog.

93. The One Workflow to Remember

                    GITHUB
                       |
                 Pull Request
                       |
                 Code Review + CI
                       |
Developer ---> feature branch
   |                   |
   | git add           |
   | git commit        |
   | git push          |
   |                   v
   +--------------> Remote Branch
                       |
                       v
                     MERGE
                       |
                       v
                      MAIN
                       |
                       v
                    RELEASE
                       |
                       v
                 DEPLOYMENT

Core mental model

EDIT
 ↓
git status
 ↓
git diff
 ↓
git add
 ↓
git diff --staged
 ↓
git commit
 ↓
git fetch / git pull
 ↓
git push
 ↓
PULL REQUEST
 ↓
CI + REVIEW
 ↓
MERGE
 ↓
DEPLOY

94. Important Security Rules

Never commit passwords.

Never commit AWS access keys.

Never commit private SSH keys.

Never commit .env files containing secrets.

Use GitHub Secrets for CI/CD credentials.

Use least privilege.

Rotate credentials immediately after accidental exposure.

Review .gitignore.

Scan repositories for secrets.

Do not assume deleting a secret from the latest commit removes it
from history.

95. Final Goal

For a Cloud/DevOps Engineer, Git should not be treated as only a
list of commands.

You should be able to explain the complete engineering workflow:

Source Code
    ↓
Git
    ↓
GitHub
    ↓
Branch
    ↓
Commit
    ↓
Push
    ↓
Pull Request
    ↓
Code Review
    ↓
CI
    ↓
Build/Test/Security Scan
    ↓
Artifact/Docker Image
    ↓
CD
    ↓
Cloud/Kubernetes/Production

The target is:

Understand the concept → run the command → troubleshoot the failure
→ explain it in an interview → use it in a real DevOps project.

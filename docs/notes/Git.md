## Introduction to git

### Story of git
[[Linux]] Operating being Open Source maintains its kernel code accessible to the public. During the early years of maintenance (1991 - 2002), the changes to the kernel were passed around as patches and archived files.

In 2002, the Linux Kernel Project began using a proprietary [[Version Control System#Distributed Version Control Systems (DVCS)|DVCS]] software by the name *BitKeeper*. In 2005 the relationship between the Linux community and the BitKeeper software company broke and the tool's free-of-charge status was revoked.

This prompted the Linux Community and primarily [[Linux Torvalds]] to develop their own tool based on the learnings and short comings of the previous system. On conception, Git had a few goals
- Speed of operation
- Simplicity in design
- Strong support for non-linear development (parallel branching)
- Fully distributed
- Handle large projects

Since 2005, when Git saw its initial release, the system has always stuck to these goals that it set out with initially. Today Git is one of the most popular distributed version control systems and it is free to use and open source.

### Where it differs
git fundamentally differs from the other VCS tools available in each of the following ways.

1. **Snapshots**
	- Other VCS tools generally look for the differences in data between each file and store data on a per file basis (referred to as *delta-based version control*).
	- Git on the other hand, takes complete snapshots of the overall state of the files and stores the information only on files that have changed. This enables Git to be mindful of what data needs to be updated in its tracking. If a file did not change, it merely refers to the last tracked version that was changed. In a way, it appears as a stream of snapshots.
2. **Operations done Locally**
	- Almost all the operations that Git does is done locally on the machine. This makes Git really fast when compared to CVCS systems which are network latency based for the speed of operation.
	- It also gives the freedom to work on the files without being connected to a network. All changes can be stored offline and committed to the local copy of the repository.
3. **Integrity**
	1. Every action in Git is checksummed before it is stored and it can be referred to by that checksum. This means that every action that occurs on the files is tracked by Git, either success or failure of action. Thus, information cannot be lost or corrupted without git knowing it.
	2. Git internally uses SHA-1 hash which is a 40 character hexadecimal value computed based on the contents of a file or directory structure in Git. Git stores everything in its database by hash value of its contents. A SHA-1 hash looks like  `24b9da6552252987aa493b52f8696cd6d3b00373`.
4. **Data Addition**
	- Git always adds data to its database and it is very difficult to mess things up that is not undoable.
	- If commits are frequently and promptly made, data can be recovered to any state as required.

### Git states
By default, git does not track a file unless explicitly asked to. Thus all files in a git repository can be either *tracked* or *untracked*
1. **Tracked** - Git was explicitly asked to track changes to the file.
2. **Untracked** - Git was not asked to track changes to the file.

Files that are being actively tracked by git can have 4 states
1. **Unmodified** - Files are are not changed from the last snapshot or checkout from the original source.
1. **Modified** - Files have been changed/modified, but not stored in the git database.
2. **Staged** - Files that have are set to be saved in the git database in the next snapshot.
3. **Committed** - Files that have been saved in the git database.

These states are cyclic in nature. When a file is committed, it is sent back to the unmodified state, as there are not changes from the last snapshot or checkout. But subsequently when changes are made to the file, it goes into the modified state, which can then be staged and eventually committed, thus performing the cycle all over again.

With this in mind, all git projects have 3 sections namely,
1. **Working Tree/Directory** - Where the tracked and untracked files reside.
2. **Staging Area** - Where the files that are set to be saved in the next snapshot reside. Physically, it is present in the `.git` directory. In git terminology, it is referred to as `index`.
3. **`.git` directory** - This is where git stores all the metadata and object database for the git project.

### Git and command line
The Power of Git comes with its support on the command-line. Although Git contains several open-source and paid GUI clients available, most of them implement a subset of git's capability. This makes the command line the only place where git shines with its full capability.

---
## Up and running with git

### Git installation
Git is available on Windows, Linux and MacOS Operating Systems. The instructions to download and setup Git varies based on the operating system and further information on the same can be found at [Git - Downloads](https://git-scm.com/downloads).

### Git setup
Git comes with a tool called as `git config` that allows to set up git as per preferences. Configurations can be set on 3 levels, namely
1. **System Level at `[path]/etc/gitconfig`** - Applied to every user in the system.
2. User Level at  `/.gitconfig` or `/.config/git/config` - Applies to the current user in the system.
3. Repo Level at `config` file at `.git` directory - Applies to the currently repository the use is currently working on.

Each level of config overrides the previous level configuration. To get a list of all git configurations, run the following command.

```PowerShell
# Show configurations
git config --list

# Show configurations and their origin
git config --list --show-origin
```

The following are some of the most common configuration settings that are done when setting up a local git environment.
1. **Setting up identity** - Used to authenticate the user and their actions.
2. **Setting up default editor** - Sets up default code editor for the actions that git launches. Options include Emacs, Vim, Notepad++, VS Code, Sublime Text or Atom.
3. **Setting up default branch name** - Git and other platforms are moving from the terminology of `master` to other names to be more inclusive. Most users prefer the term `main` to be their default branch name.

```PowerShell
# Identity Configurations
git config --global user.name "John Doe" 
git config --global user.email johndoe@example.com

# Editor Configurations
git config --global core.editor vscode

# Default Branch Configurations
git config --global init.defaultBranch main

```

### Getting help
Manuals and help in git can be accessed in a number of ways. These are showcased below.

```PowerShell
# For full-blown help/manual page
git help <verb>
git <verb> --help
man git-<verb>

# For compressed, available options for commands
git <verb> -h
```

### Git repositories
In the context of Git, a *repository* or a *repo* is a central location where code and its related files are stored and managed. It is basically a directory or folder where git tracks and manages directories, subdirectories and files.

These git repositories can be hosted on various platforms such as  [[GitHub]], GitLab, Bitbucket or even self-hosted.

The tracking made by Git in these repositories are what allows git to facilitate code rollbacks, comparisons and so on.

### Initializing git repositories
In order to make git track a particular directory of interest (create a git repo), git needs to be specifically informed to track the directory. Git does not globally track all changes. This process is called as *initialization* and it is done to create new git repositories. Initialization of git repositories is done via the `git init` command

```bash title="Initializing a git repository"
# Initialize git repository in the current directory
git init .

# Initialize git repository in another directory
git init "C:\Users\UniqueUser\Learning Git\RealRepo"

# Initialize a git repository with a different main branch name
git init -b primary

# Initialize a git repository - less verbose
git init -q
```

he command creates a `.git` directory with subdirectories for `objects`, `refs/heads`, `refs/tags` and template files. It also creates an initial branch without any commits (name based on the settings - global preference or local options via flags). 

**Note:** If a destination directory path is not provided, git initializes a repository in the current location.

> [!success] `git init` twice?
> Running `git init` on a repository more than once is not an issue. The command is commonly rerun when new templates are added or to move the repository to another place if separate git directory is given.

> [!caution] Just initializing does not track files
> It is to be noted that just initializing a repository does not track the files in the directory. In order to track the files, the files have to be added to the git tracking and then committed. This is covered later.

> [!danger] Gitception - Do not initialize git repos inside other repositories
> It is not good practice to have git tracked repositories inside git repositories making a nested git repository case. Some conflicts might arise and is generally discouraged.
> 
> However there are ways to deal with managing existing git repositories that are used as a part of a current project. These ways are dealt later.

### Getting the status of a repo
The status of a git repository states the current condition of the repository under question. It is used to know the following things about a directory.

1. **Branch information** - It shows the name of the current branch that the repository is on.
2. **Staging area information** - It shows the status of files in the staging area, including which files have been modified and which files are ready to be committed.
3. **Untracked files** - It shows a list of files that are in the repository's working directory but have not yet been added to the staging area.
4. **Upstream branch information** - It shows whether the current branch has an upstream branch and whether the local branch is ahead or behind the upstream branch.
5.  **Conflicts** - It shows if there are any conflicts in the repository that need to be resolved before changes can be committed.

```bash title="Getting the status of a git repository"
git status
```

```txt title="Output from git status command"
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   README.md
```

**Note:** It can also state if a git repository exists in a particular directory. The following snippet shows how git status reports if there is no git repository in the directory of interest.

```txt title="Output from git status command"
fatal: not a git repository (or any of the parent directories): .git
```

### Saving changes to a repo
Saving changes to a repository is done in two steps, each with a specific purpose.

1. **Staging** - Move the changes to be committed to the staging area.
2. **Committing** - Actually writing the state to the version history

### Staging changes in a repo
Staging in the context of git refers to the process of preparing changes made to a file or files for commit. Staging facilitates the users to selectively choose which changes must be committed, allowing the commits to be focused and organized.

> [!info] Keeping the commits atomic
> Care must be taken to make sure that a commit should encompass a single change or feature or a fix. In simpler terms, each commit must be focussed on one things only. This is done to facilitate easier undo and rollbacks and makes the code easier to follow and review.

To stage changes in a git repository, the `git add`  command is used.

```bash title="Staging changes in a git repository"
# Staging a specific file
git add myfile.txt

# Staging multiple files
git add myfile.txt README.md 

# Staging all changes
git add .
```

Upon running the command, `git status` command can be run to check and see if the changes are staged.

```txt title="git status after staging changes"
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   README.md
```

### Committing changes
Committing in the context of git means to save a snapshot of the state of a file or multiple files in the version history. Commits are like save points to which the code base can be reverted to. The command `git commit`  can be used to commit changes.

```bash title="Committing changes to a repository"
# Commit message provided via the command
git commit -m "Commit Title" -m "Commit description"

# Commit message provided via the default git text editor*
git commit 
```

Upon running the command, git saves the changes to the version history and produces a message similar to the one showcased below. However, this can also be silenced with the `-q` flag or make it more detailed with the `-v` flag.

**Note:** During the installation of git or via the global config options, it is possible to set the default text editor for git. This text editor is used when input from the user is required such as in case of supplying a commit message.

> [!info] Writing good commit messages
> Committing is at the core of the git workflow, and good commit messages are very important in communicating the changes made by the code to the existing project. This makes it easier to follow the changes for future reference or to aid in troubleshooting or bug hunting.
> 
> The following are some of the guidelines for writing a good commit message
> 
> 1. **Keep it short and concise** - A good commit message should be short and to the point. It should describe the changes made in the commit in a clear and concise manner.
> 2. **Use the imperative mood** - A commit message should be written in the imperative mood. This means using command-like language, such as "Fix bug" or "Add new feature".
> 3. **Provide context** - A commit message should provide context for the changes made in the commit. It should answer questions such as "Why was this change made?" and "What problem does it solve?"
> 4. **Use the body for more detail** - If necessary, provide more detail in the body of the commit message. This can include a description of the changes made, any relevant information about the code or files modified, and any necessary instructions for other developers.
> 5. **Follow any guidelines set by the project** - Some projects may have specific guidelines or conventions for writing commit messages. Make sure to follow any guidelines set by the project to ensure consistency and clarity.
> 

> [!info] Fixing (or) amending the last commit
> Sometimes, a change needs to be done on the last commit. This could be due to accidentally missing file(s) or typos in the commit messages and so on. The amend flag to the `git commit` command performs the redo of the previous commit. Before running a commit with the `amend` flag, changes can be made to the codebase to correct the mistakes performed in the previous commit if required.
> 
> After the changes are made, when ready to perform the commit, the command, `git commit` is run with `amend` flag as `git commit --amend`. Now git opens the commit message for the commit in the default text editor configured. By default, git uses `vim` as the text editor of choice. Now, the necessary changes can be made to the commit messages. By default, the previous commit messages are taken as the base, allowing changes to be made when committing.

### Getting the history (log) of previous commits
To know the commit history of a git repository, the `git log` command can be used. By default, a git log command returns the following 4 pieces of information.

1. **Commit hash and branch information** - Hash (digital uniquely identifying string of characters) uniquely identifies the commit and the branch information lets the user know which repository, branch and location the commit was made to.
2. **Author** - Shows the author who made the commit.
3. **Timestamp** - Shows when the commit was performed.
4. **Commit Message** - Shows the message provided by the commit author when performing the commit.

**Command:**

```bash
git log
```

**Command Output (Example)**

```txt
commit e5a5f55a235c5d2f9a4e4de4d41fa71a1a07c51f (HEAD -> master, origin/master)
Author: John Doe <johndoe@example.com>
Date:   Fri May 7 14:30:15 2021 -0400

    Add new feature

commit 0123456789abcdef0123456789abcdef01234567
Author: Jane Smith <janesmith@example.com>
Date:   Wed May 5 10:45:32 2021 -0400

    Update documentation

commit 9876543210fedcba9876543210fedcba98765432
Author: John Doe <johndoe@example.com>
Date:   Mon May 3 16:20:49 2021 -0400

    Initial commit

```

### Ignoring files and directories
A `gitignore` is a configuration file used to specify git to ignore files and folder when tracking changes in a repository. 

This is useful in cases where these files and folders are log files, secrets, build artifacts, lock files or binary files. These files might be changing frequently or can be generated by the end-user whenever required, and hence hold no value to be tracked by git.

Setting up an appropriate `gitignore` file aids in maintenance of a cleaner, more manageable and less cluttered. 

Following are some of the important questions and their corresponding workflows to keep in mind when working with `.gitignore` files. 

1. **What file and file format is the `.gitignore` file** - `gitignore` files are created as `.gitignore` (without extension) files.
2. **Where to place the file?** - `gitignore` files are usually created at the root of the repository.
3. **What if I place the `.gitignore` file in a sub-directory?** - If a `.gitignore` file is placed in a child directory to the root directory of the git repository, git uses that `.gitignore` file to ignore files in that particular directory specifically. Tt is to be noted that a `gitignore` file at a directory is applicable only to that directory and its children and not to its parents or siblings.
4. **Okay, so what is the best practice for `.gitignore` files?** - It is recommended to not have multiple `.gitignore` files per repository to avoid conflicts and complexity. Also, when a repository is collaboratively maintained, it is important that all the collaborators  have the same `.gitignore` file, to avoid ignore conflicts.
5. **Is the `.gitignore` file tracked by git?** - Yes, `.gitignore` files must be tracked by git in order for git to ignore files and folders as per the instructions stated.
6. **What if a new file/folder to be ignored is added to the `.gitignore` file after it is being tracked by git?** - Once git starts tracking a particular file/folder, git will continue tracking the file even after the `.gitignore` file is modified to ignore the file/folder. In order to remove the file from git actively tracking the file/folder, there are two options
	- *Remove the file/folder from the repository* - This process completely deletes the file or folder that needs to be stopped being tracked. This can be done using `git rm <file>` command. Post this command, the file will be removed from git's tracking as well from the repository.
	- *Remove the file/folder from git tracking only* - This removes the file/folder that needs to be stopped being tracked from git's tracking. This can be done using `git rm --cached <file>` command. Post this command, the file will be removed from git's tracking, but will remain in the repository.
7. **How to specify files/folders for ignoring?** - `.gitignore` accepts files and folders as individual entities as well as with wildcard characters. The following snippet captures some of the common formats and sequences followed when ignoring files and folders in git repositories. 

```gitignore title="Commonly used gitignore formats"
# ignore a specific file
some-file.txt

# ignore all files with a specific extension
*.log 

# ignore a specific directory
my-directory/

# ignore directories with a certain name
**/log-files

# ignore all files and sub-directories in a directory
logs/**

# ignore specific files in a directory and its sub-directories
logs/*.log

# ignore files that match a pattern (rule negation)
*.log
!important.log
```

### Branching
In git, branching refers to the creation of a new line of development from the existing line. Here, the line of development is referred to as branch, so called because it resembles the branches of a tree. 

Essentially, a branch is a pointer to a specific commit made in history, pointing to the state of the codebase at that point in time. Any file or folder in each branch, once created, can be changed, deleted, modified or added without affecting the parent branch it was created from as long as the author of those commits wishes to keep them separate. Essentially, commits in each branch are independent of other branches.

Once the author decides to carry over the changes to the parent branch, an operation called as merging can be done.

Git always operates in a branch. Unless changed, git creates a default branch by the name `master` or `main` whenever a new repository is created with `git init`. 

> [!info] Master v Main
> `master` and `main` are both used as names for the default branch created by git when initializing a repository. While any name is allowed to be set as the default name, these two are in common use.
> 
> Over time, the name of the default branch has evolved. Several community members found the term `master` being associated with slavery and racism. Hence they advocated and started using a better term, `main` as the default branch name. 
> 
> Git works no matter what name is set as the default branch name, and the preference ultimately needs to be set by the owner of the repository or governing principles in case of an organization or an open-source project team.
> 
> To set or change the default branch name, use the `git config` command by supplying the new name for the branch. Reference snippet is provided below.
> 
> ```bash title="set/change global branch name configuration"
> git config --global init.defaultBranch main
> ```

> [!info] What is the HEAD?
> In git, **HEAD** is a reference to the current commit in a repository, pointing to the tip of the current branch where the most recent commit was made on that branch. In simpler terms, HEAD is a pointer to the last commit of the current branch.  
> 

The `git branch` command is extensively used to manipulate branches. Following are some of the most common implementations of the `git branch` command.

```bash title="git branch command implementations"
# list all local branches
git branch

# list all remote branches
git branch -r

# list both local and remote branches
git branch -a

# show SHA-1 commit ID 
git branch -v

# rename a branch (HEAD on the branch)
git branch -m bug-fixes

# rename a branch - forcefully (HEAD on the branch)
git branch -M bug-fixes

# delete a branch (HEAD on another branch)
git branch -d bug-fixed

# delete a branch - Forcefully (HEAD on another branch)
git branch -D bug-fixed
```

### Switching branches
The command `git switch` can be used to switch from one branch to another. Both `git switch` and `git checkout` are commands that can be used to switch to new branches. 

`git checkout` used to be the only way to change branches in older versions of git (prior to version 2.23). In newer versions of git (since version 2.23), the command `git switch` was introduced. 

`git switch` is aimed at providing an intuitive and streamlined way of changing branches in git repositories in branching and committing.

Following are some of the differences between `git checkout` and `git switch` commands.

1. **Simplistic** - `git switch` command is simpler as it takes only one argument, where `git checkout`, being able to do more complex things, is inherently more complex.
2. **Less error prone** - Due to its complex nature, `git checkout` command is more error-prone to the simplistic git switch command
3. **Data loss protection** - The `git switch` command has built-in checks to prevent data loss, when trying to switch to a different branch with uncommitted changes in the working branch, which git checkout lacks, which may lead to data loss. When trying to switch branches with uncommitted changes, the `git switch` command prompts to `commit` or `stash` the changes.

**Commands:**

```bash title="git switch and git checkout to change branches"
# switching to a new branch with git switch
git switch bugfix

# create and switch to a new branch
git switch -c bugfix

# switching to a new branch with git checkout
git checkout bugfix
```

### Merging branches
In git, merging is the process of combining changes from different branches and integrating them into a single branch. When two branches are merged, git makes a new commit that combines the changes from both the branches. 

By default, git manages conflicts when merging two branches and performs a new commit if necessary. To merge branches, first switch to the branch into which the changes need to be recorded (target branch) using `git switch` or `git checkout` commands. Next, merge the source branch using the git merge command.

```bash title="merging branches"
# merging a branch to the main branch (HEAD not on bug-fixed)
git merge bug-fixed
```

The following are some of the most commonly used types of merges in git.

1. **Fast-forward merge** - A fast-forward merge occurs when the branch you are merging into has not diverged from the branch you are merging in any way. In this case, Git can simply move the pointer of the branch you are on to the latest commit on the other branch. This type of merge is fast because it does not create a new merge commit.
2. **Recursive merge** - A recursive merge is the default merge strategy used by Git. It is used when the branches you are merging have diverged and cannot be fast-forwarded. Git creates a new merge commit that combines the changes from both branches. In this case, git prompts the merge author for a commit message for the new commit that arises from merging the two branches.
3. **Octopus merge** - An octopus merge is a type of recursive merge that allows you to merge multiple branches into a single branch at the same time. This is useful when you have several branches that you want to integrate into a single codebase.
4. **Squash merge** - A squash merge allows you to merge changes from a feature branch into another branch as a single commit. This can be useful if you want to keep the commit history of your main branch clean and concise.
5. **Rebase merge** - A rebase merge is a way of integrating changes from one branch into another by replaying the changes made in the source branch on top of the target branch. This can be useful when you want to keep the commit history linear and avoid creating merge commits.

**Merge Conflicts** occur in git when two branches are merged and have changes in the same file(s), that git cannot automatically merge them. This is because, git might not know which changes to keep and which changes to discard. Merge conflicts typically occur, when the same section of a file is modified in both the branches.

Merge conflicts need to be manually resolved before a commit can be made. Changes in both the branches are reviewed and the appropriate content is kept, discarding the one that is not required.

The following snippet shows how a merge conflict is presented to the merge author to be manually resolved.

```txt
<<<<<<< HEAD (Current Change) 
Some changes  in the existing branch
=======
Some changes that are incoming to the existing branch
>>>>>>> NEW-BRANCH (Incoming Change)
```

The following are the steps to resolve a merge conflict

1. Make the essential changes on the file.
2. Remove the conflict markers
3. Stage the file with `git add` 
4. Commit the file with `git commit`

It is important to note that merge conflicts are very time-consuming and complex especially when a lot of changes are conflicting or when multiple files are in conflict.

Here are some steps to take to avoid merge conflicts

1. **Keep the branches small** - One of the main causes of merge conflicts is when multiple developers are working on the same file or section of code. To minimize the risk of conflicts, it's a good idea to break up the work into small, focused branches that only modify a few files or lines of code.
2. **Pull frequently** - Before starting to work on a branch, make sure to pull the latest changes from the remote repository (if applicable) to ensure that the branch is up-to-date with the latest changes made by other developers. This can help prevent conflicts that arise from changes made by other developers.
3. **Communicate with the team** - If changes to the same section is required and known, it's a good idea to communicate with the other developers beforehand to coordinate your efforts and avoid conflicts.
4. **Use descriptive commit messages** - When making a commit, make sure to use a descriptive commit message that explains what changes were made. This can help other developers understand the changes and can help prevent conflicts that arise from misunderstandings.
5. **Test the changes** - Before merging branches, make sure to test the changes thoroughly to ensure that they work as expected. This can help prevent conflicts that arise from bugs or errors in the code.
6. **Use merge tools** - There are several merge tools available that can help resolve conflicts more easily. These tools can highlight conflicting changes and allow comparing and merging the changes more easily.

> [!info] Conflict Resolution - IDE Tools
> Most IDEs come with built-in tools to handle and resolve merge conflicts. These could include, but limited to accepting the current change (from the target branch), accepting changes from the incoming branch, accepting both changes and so on. 
> 
> It is important to learn to use these tools that are built into these IDEs to make the process easier.

### Finding the difference
The `git diff` command displays the differences between the two versions of a file using a unified diff format, which shows the added and removed lines in the file. The output of the command can be customized using various options and flags to change the format, scope, and level of detail of the output. `git diff` just provides information and does not alter the repository in any form (similar to `git log` and `git status`).

Following section explains the general output of the git diff command and how to make sense of what each part means.

```txt
diff --git a/Shopping-List.md b/Shopping-List.md
index 47e4552..08c9a76 100644
--- a/Shopping-List.md
+++ b/Shopping-List.md
@@ -5,3 +5,10 @@ Store: David's Store
 ### Monthly Shopping Items
 Get the following things each month
+Corn Flour
+Jam
+Chocolate Spread
+Noodles
+Rice
+Carrots
+Tomatoes
 Do not forget to take a bag to carry the grocery items. 
```


Some of the most common implementations of git diff are given below for reference

```bash
# Differences between working directory and staging area
# These are changes not staged for commit
git diff

# Differences between staging area and last commit
# These are changes that staged and are yet to be committed
git diff --staged
git diff --cached

# Differences between working directory and a particular commit
# This shows the changes since a particular commit
git diff <commit>
git diff HEAD~2
git diff 4a23590..2c63590

# Differences between the current branch and another branch
# This shows what changes have been made across files
git diff <branch>
git diff bugfix # diff between current branch and bugfix branch
git diff bigfix..main
git diff bigfix main

# NOTE: All these changes can be directed towards a specific file
git diff HEAD feature.js
git diff HEAD feature.js app.js
```

### Stashing the changes
Stashing is the process of temporarily saving changes in a repository into a stash, without committing those changes. This returns a clean working directory, where the it leaves room for other operations such as branch switching, committing other changes and so on. 

The `git stash` command is used to perform stashing. Following are some of the most commonly used stashing operations.

1. **Stash Changes** - To stash current changes, run `git stash save` or simply `git stash`. This will save both the modified and staged files in a new stash.
2. **View Stashes** - To view a list of all available stashes, run `git stash list`. It will display the stash index number, description, and the branch where the stash was created.
3. **Apply Stash** - To apply the most recent stash and restore the changes to the working directory, run `git stash apply`. This will merge the changes from the stash into the current branch.
4. **Apply Specific Stash** - If multiple stashes exist, a specific stash can be applied by specifying its index number, such as `git stash apply stash@{2}`. The stash number can be obtained via the `git stash list` command.
5. **Drop Stash** - When a stash is no longer needed, it can be removed from the stash stack using `git stash drop stash@{1}`. This action cannot be undone.
6. **Pop Stash** - To apply the most recent stash and remove it from the stash stack in one step, run `git stash pop`.
7. **Clear the stash** - To clear out the entire stash, run `git stash clear`.

```bash
# Save a stash with description
git stash save "Boss asked me to switch branches"

# View all stashes
git stash list

# Apply the most recent stash to the working directory
git stash apply

# Apply a specific stash to the working directory
git stash apply stash@{2}

# Remove/delete a specific stash
git stash drop stash@{3}

# Apply and remove the most recent stash
git stash pop

# Apply and remove a specific stash
git stash pop stash@{2}


```

**Note** - It is possible to save a stash in one branch and then `git stash apply` or `git stash pop`  them in another branch. This is done by making changes in a branch, stashing them, switching to another branch with `git switch <branch>`, then running `git stash apply` or `git stash pop` .

### Checking out an old commit
To checkout the state of the repository at a specific commit's point in time, the `git checkout` command can be used along with the commit identifier such as the commit hash.

When an older commit is checked out, git enters what is called as a **detached HEAD** state. In this state, git allows the user to explore the codebase of the repository at that point in time and facilitates making changes if needed. This basically moves the HEAD to that specific commit, where usually HEAD refers to a branch and not a specific commit.

The following outline specifies the workflow for working with older commits.

1. **Checking out an old commit** - To checkout an older commit, a commit identifier such as the commit hash (at least first 7 characters) must be known. With the identifier information, use the `git checkout <commit_identifier>` command. Now, git moves the HEAD to that specific commit and enters the detached HEAD state.
2. **Detached HEAD state** - The state so called, as HEAD usually refers to an entire branch and not a single commit is when the HEAD points to the commit of interest. Any changes made in this state will not be associated with a branch.
3. **Inspecting commit and making changes** - Any changes made at this point will not be associated to a branch and will be lost unless saved specifically. To save the changes, a new branch can be created at the current commit level.
4. **Returning to a branch** - To return to a branch or create a new branch, the `git checkout` command can be used with the reference to the branch name.
5. **Re-attaching the HEAD** - Once the HEAD moves to a specific branch, the HEAD is said to be in attached state once again as it goes back to referencing a branch rather than a commit when it first detached while checking out an older commit.

```bash
# Checking out an older commit
git checkout 4e237h5

# Detached HEAD state

# Inspecting commit and making changes

# Attaching HEAD / Returning to a branch
git checkout main
```

### Discarding changes
If changes made after a commit is to be discarded, the command `git checkout` can be used. This reverts the state of the repository to the commit specified in the command. Doing this will remove all the changes that were made after the last commit.

```bash
# Discarding changes from a specific file (Method 1)
git checkout HEAD fruits.txt

# Discarding changes from all files (Method 1)
git checkout HEAD .

# Discarding changes from a specific file (Method 2)
git checkout -- fruits.txt veggies.txt

# Discarding changes from all files (Method 2)
git checkout -- .
```

Another command that can be used to perform the discarding of the changes since a commit is `git restore`

```bash
# Discarding changes from a specific file
git restore fruits.txt

# Discarding changes from all files
git restore .

# Reverting state to a specific commit for specific files
git restore --source HEAD~2 fruits.txt

# Reverting state to a specific commit for all files
git restore --source HEAD~2 .
```

Note that following the `git checkout` and `git restore` commands to discard changes removes the uncommitted changes from the repository, thus using them with caution is recommended.

### Un-staging changes
If the aim is just to un-stage the changes made in a repository, there-by keeping all the files in which the changes were made, again the `git restore` command can be used with the `--staged` flag.

```bash
# Unstaging changes to a specific file
git restore --staged fruits.txt

# Unstaging changes to all files
git restore --staged .
```

The same functionality, of removing the staged files can be done with the command `git reset` as well. The `git reset` command with the flag `--` un-stages the changes made to the repository that is not committed. 

```bash
# Un-staging all changes and revert to the previous commit
git reset --
```

It is to be notes that the git reset command works at the scope of a commit history, and thus will not allow to reset only one file. To un-stage the changes in one single file, use the `git restore` command. 

Note that running the git reset command without the -- flag behaves differently, and performs an undo on the last commit.

### Undoing commits
Sometimes, the commits made to a branch might have to be undone for various reasons. For this purpose, the `git reset` command can be used.

The `git reset`  command is commonly specified with one of 3 flags, namely

1. **Soft Reset with `--soft`** - Reverts back the branch pointer to a commit, but preserves the changes in the staging area. Changes in the working directory and staging area are preserved. 
2. **Mixed Reset with `--mixed`** - This is the default option, when no flag is specified. Reverts back the branch pointer to a commit, and matches the staging area to the commit. Changes in the working directory are preserved.
3. **Hard Reset with `--hard`** - Reverts back the branch pointer to a commit. This discards any and all changes to the working directory and the staging area. Needless to say, care must be taken before performing this action, as all changes will be lost.

```bash
# Soft Reset - Preserve changes in working directory & staging area
git reset --soft 423a145

# Mixed Reset - Preserve changes in working directory
git reset --mixed 423a145

# Mixed Reset - DOES NOT PRESERVE CHANGES
git reset --hard 423a145

# NOTE: Also accepts dynamic HEAD pointers
git reset 423a145 HEAD~2
```

### Reverting commits
The command `git revert` is used to create a new commit that sets the state of the repository as per a previous commit. This allows to safely revert/undo a commit while preserving the commit history.

Here, a new commit is made that reverses the changes made in the specified commit. This ensures that the commit history remains intact and that the original commit is not removed or modified. 

Here are some key pointers when it comes to git revert

- It creates a new commit, so the commit history is preserved.
- It's a safe way to undo changes, especially in shared repositories where you don't want to modify the existing history.
- It can revert a single commit or a range of commits

```bash
# Revert to a previous commit
git revert 423a145
```

It is important to note that the command `git revert` functions at the commit level and will not be able to revert a specific file. To revert specific changes within a commit, other git commands such as `git cherry-pick`, `git reset` or interactive rebase via `git rebase -i` shall be used.

However it is to be noted that, `git revert` can introduce conflicts if the changes being reverted conflict with the subsequent changes. Thus, it is recommended to review the changes and test the reverted state before sharing the code to a shared repository. 

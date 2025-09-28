# Commands
# Ultimate Command Reference 

An exhaustive and actively maintained reference guide for Git, Bash, Networking, and other command-line tools. This is my personal knowledge base.

---

## 1. The Golden Rules: How to Discover *Any* Command

Before the lists, here are the master keys. These three commands allow you to learn about **any** other command without leaving the terminal.

* `man <command>`: Shows the official **man**ual page for a command. This is the most detailed documentation. (Press `q` to quit).
* `<command> --help`: Shows a quick usage summary and a list of common options.
* `apropos <keyword>`: Searches manual pages for a keyword to help you find a command when you don't know its name.

---

## 2. Git Commands

The complete version control toolkit, from basic commits to advanced history manipulation.

#### **Repository Setup & Configuration**
* `git config --global user.name "Name"`: Sets your name for all commits.
* `git config --global user.email "email@example.com"`: Sets your email for all commits.
* `git config --global --edit`: Opens the global config file for manual editing.
* `git init`: Initializes a new Git repository in the current folder.
* `git clone <url>`: Creates a local copy of a remote repository.

#### **Core Workflow (The Daily Cycle)**
* `git status`: Shows the current status of your project.
* `git add <file>`: Stages a specific file for commit.
* `git add .`: Stages all modified and new files.
* `git reset <file>`: Unstages a file.
* `git commit -m "Message"`: Saves staged changes with a message.
* `git commit --amend`: Modifies your most recent (unpushed) commit.
* `git log`: Shows the commit history.
* `git log --oneline --graph`: Shows a compact, visual graph of the history.
* `git diff`: Shows unstaged changes.
* `git diff --staged`: Shows staged changes waiting to be committed.
* `git show <commit>`: Shows the changes made in a specific commit.

#### **Branching & Merging**
* `git branch`: Lists all local branches.
* `git branch <name>`: Creates a new branch.
* `git checkout <branch>`: Switches to an existing branch.
* `git checkout -b <name>`: Creates a new branch and switches to it.
* `git merge <branch>`: Merges the specified branch into your current branch.
* `git branch -d <name>`: Deletes a local branch.

#### **Remote Repositories (Collaboration)**
* `git remote -v`: Lists your configured remote repositories.
* `git remote add <name> <url>`: Adds a new remote.
* `git fetch <remote>`: Downloads history from a remote but does not merge it.
* `git pull`: Fetches and merges changes from the default remote (`origin`).
* `git push`: Pushes your committed changes to the default remote (`origin`).
* `git push -u origin <branch>`: Pushes a new branch to the remote and sets it to track.

#### **Advanced & Undoing**
* `git stash`: Temporarily saves uncommitted changes.
* `git stash pop`: Re-applies the last stashed changes.
* `git revert <commit>`: Creates a new commit that undoes a previous commit.
* `git reset --soft HEAD~1`: Undoes the last commit but keeps the changes staged.
* `git reset --hard <commit>`: **(Dangerous)** Discards all changes back to a specific commit.
* `git rebase -i HEAD~N`: Interactively rewrites the last N commits (squash, reword, etc.).
* `git cherry-pick <commit>`: Applies a specific commit from another branch to your current branch.
* `git bisect start`: Begins a binary search through commit history to find a bug.
* `git blame <file>`: Shows who last modified each line of a file.

---

## 3. Bash & Linux Commands

The toolkit for navigating and controlling a Linux-based operating system.

#### **File System Navigation**
* `pwd`: Print Working Directory (your current location).
* `ls`: List files and directories.
* `ls -al`: List all files (including hidden) in long format.
* `cd <dir>`: Change directory.
* `cd ..`: Move up one directory.
* `cd ~`: Go to your home directory.

#### **File & Directory Manipulation**
* `touch <file>`: Create an empty file or update its timestamp.
* `cat <file>`: Display file content.
* `cp <src> <dest>`: Copy a file.
* `mv <src> <dest>`: Move or rename a file.
* `rm <file>`: Remove a file.
* `mkdir <dir>`: Create a new directory.
* `rmdir <dir>`: Remove an empty directory.
* `ln -s <target> <link>`: Create a symbolic link.

#### **Text Processing**
* `echo "text"`: Print text to the terminal.
* `grep "pattern" <file>`: Search for a pattern in a file.
* `sed 's/old/new/' <file>`: Search and replace text in a stream or file.
* `awk '{print $1}'`: A powerful text-processing language.
* `head`: Show the beginning of a file.
* `tail`: Show the end of a file.
* `sort`: Sort lines of text.
* `uniq`: Report or omit repeated lines.
* `wc`: Count lines, words, and characters.

#### **System Information**
* `uname -a`: Print all system information.
* `df -h`: Show disk space usage in human-readable format.
* `du -sh <dir>`: Show the total size of a directory.
* `free -h`: Show memory usage.
* `uptime`: Show how long the system has been running.
* `whoami`: Show your current user.
* `id`: Display user and group information.

#### **Process Management**
* `ps aux`: List all running processes.
* `top` / `htop`: Interactive process viewer.
* `kill <PID>`: Terminate a process by its ID.
* `killall <name>`: Kill all processes by name.
* `bg` / `fg`: Send processes to the background or bring them to the foreground.

#### **Permissions**
* `chmod <permissions> <file>`: Change file permissions (e.g., `chmod +x script.sh`).
* `chown <user>:<group> <file>`: Change file ownership.
* `sudo <command>`: Run a command with superuser privileges.

#### **Archiving**
* `tar -czvf archive.tar.gz <dir>`: Create a compressed tar archive.
* `tar -xzvf archive.tar.gz`: Extract a tar archive.
* `zip` / `unzip`: Create and extract `.zip` files.

---

## 4. Networking Commands

The essential tools for inspecting and troubleshooting network connections.

* `ping <host>`: Check connectivity to a host.
* `ip addr show` / `ifconfig`: Display network interface and IP address information.
* `traceroute <host>`: Trace the network path to a host.
* `ss -tulpn` / `netstat -tulpn`: Show listening network ports and services.
* `dig <domain>`: Perform DNS lookups.
* `host <domain>`: Simple DNS lookup utility.
* `curl <url>`: Transfer data from or to a server (powerful tool for API testing).
* `wget <url>`: Download files from the internet.
* `ssh <user>@<host>`: Securely connect to a remote machine.
* `scp <file> <user>@<host>:<dest>`: Securely copy files over the network.
* `rsync -avh <src> <dest>`: Synchronize files and directories between locations (locally or remotely).

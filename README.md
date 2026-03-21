```
(async () => {
    const API_KEY = "YOUR_API_KEY_HERE"; 
    const MODEL = "gemini-pro"; 

    // --- PHASE 0: MEMORY CHECK (The Persistence Fix) ---
    // If answers are already saved, just print them and stop.
    const savedData = sessionStorage.getItem("LMS_ANSWERS");
    if (savedData) {
        console.clear();
        console.log("%c💾 RECOVERED ANSWERS FROM MEMORY", "color: #00ffcc; font-size: 16px; font-weight: bold;");
        const answers = JSON.parse(savedData);
        for (const [qNum, answer] of Object.entries(answers)) {
            console.log(`%c${qNum}: %c${answer}`, "color: yellow; font-weight: bold;", "color: white;");
        }
        console.log("%c\n💡 (To clear this memory for a brand new quiz, type this and hit enter: sessionStorage.clear() )", "color: gray; font-style: italic;");
        return; // Exits the script so it doesn't re-scrape the whole test
    }

    const navButtons = document.querySelectorAll('.qnbutton');
    const totalQuestions = navButtons.length;
    
    if (totalQuestions === 0) {
        console.error("❌ No questions found. Are you on the quiz page?");
        return;
    }

    const baseUrl = window.location.href.split('&page=')[0];
    console.log(`🚀 Phase 1: Scraping ${totalQuestions} questions silently...`);

    let scrapedData = "";

    // --- PHASE 1: SCRAPING ---
    for (let i = 0; i < totalQuestions; i++) {
        try {
            const response = await fetch(`${baseUrl}&page=${i}`);
            const html = await response.text();
            const doc = new DOMParser().parseFromString(html, 'text/html');
            
            const qContainer = doc.querySelector('.que');
            if (qContainer) {
                const qText = qContainer.querySelector('.qtext').innerText.trim();
                const options = Array.from(qContainer.querySelectorAll('.flex-fill.ml-1'))
                                     .map(opt => opt.innerText.trim());
                
                scrapedData += `Q${i + 1}: ${qText}\n`;
                scrapedData += options.map((o, idx) => `  ${String.fromCharCode(97 + idx)}) ${o}`).join('\n') + "\n\n";
            }
        } catch (e) {
            scrapedData += `Q${i + 1}: Error loading question\n\n`;
        }
        await new Promise(r => setTimeout(r, 150)); 
    }

    console.log("✅ Phase 1 Complete. Extracted data.");
    console.log(`🧠 Phase 2: Querying ${MODEL} API...`);

    // --- PHASE 2: FETCHING ANSWERS ---
    try {
        // The prompt is updated to enforce the "letter + text" format
        const prompt = `You are an expert solver. Analyze these multiple-choice questions and provide the correct answers. 
        Return strictly a valid JSON object where keys are the question numbers (e.g., "Q1", "Q2") and values include BOTH the option letter and the exact text (e.g., "c) 45" or "a) Compile Time"). Do not include markdown formatting.
        
        Questions:
        ${scrapedData}`;

        const res = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/${MODEL}:generateContent?key=${API_KEY}`, {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({
                contents: [{ parts: [{ text: prompt }] }],
                generationConfig: { temperature: 0.1 } 
            })
        });

        if (!res.ok) {
            const errText = await res.text();
            console.error(`❌ Google API Error (${res.status}):`, errText);
            return; 
        }

        const data = await res.json();
        let aiText = data.candidates[0].content.parts[0].text;
        
        // Sanitize response
        aiText = aiText.replace(/```json/gi, '').replace(/```/gi, '').trim();
        const answers = JSON.parse(aiText);

        // --- DISPLAY & STORE (The Backpack) ---
        console.clear();
        console.log("%c🎯 SECURED ANSWERS", "color: #00ffcc; font-size: 16px; font-weight: bold;");
        
        for (const [qNum, answer] of Object.entries(answers)) {
            console.log(`%c${qNum}: %c${answer}`, "color: yellow; font-weight: bold;", "color: white;");
        }
        
        // Saves the data to the browser tab's local storage
        sessionStorage.setItem("LMS_ANSWERS", JSON.stringify(answers));
        console.log("%c\n💾 Answers cached in sessionStorage. They will survive page reloads.", "color: gray; font-style: italic;");

    } catch (error) {
        console.error("❌ Pipeline crashed during Phase 2 processing:", error);
    }
})();
```




















## IPTV Git link
```
https://iptv-org.github.io/iptv/index.m3u
```


## To Clear chrome Incognito Catche

1. Close all Incognito Windows (It just clears the session anthe)
2. Clear DNS Catche
   Open chrome and type:
   ```
   chrome://net-internals/#dns
   ```
3. Click on "clear host cache"

### Bonus: In windows
  ```
ipconfig/flushdns
  ```



# Commands
## For Command line interface

---

## 1. The Golden Rules: How to Discover *Any* Command

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

## 3. Bash Commands

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

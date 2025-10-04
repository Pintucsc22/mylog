**Date**: 2025-09-30 
---

## 🔹 What is GitHub?

* A platform to **store**, **track**, and **collaborate** on code.
* Built on **Git**, a version control system.

---

## 🔹 Key Concepts

| Term       | Meaning                                    |
| ---------- | ------------------------------------------ |
| **Repo**   | A project folder tracked with Git          |
| **Commit** | A saved snapshot of code changes           |
| **Branch** | A separate line of work from `main`        |
| **Merge**  | Combine changes from one branch to another |
| **Remote** | Online version of your repo (on GitHub)    |
| **Pull**   | Download changes from GitHub               |
| **Push**   | Upload your changes to GitHub              |

---

## 🔹 Most Used Commands

```bash
# Start a repo
git init

# Clone a repo
git clone <URL>

# Check status
git status

# Stage + commit changes
git add .
git commit -m "message"

# Push to GitHub
git push origin <branch>

# Pull latest from GitHub
git pull origin <branch>

# Create + switch to a branch
git checkout -b feature-xyz
```

---


## 🧑‍🏫 What is "Global Git Setup"?

Before using Git, you need to tell it **who you are**. This info is attached to every commit you make — like signing your work.

You only do this **once per machine**.

---

## ✍️ Step-by-Step: Set Your Identity

### 1. **Set your name**

```bash
git config --global user.name "Your Name"
```

* Replace `"Your Name"` with your actual name.
* This shows up as the author of commits.

---

### 2. **Set your email**

```bash
git config --global user.email "you@example.com"
```

* Use the same email you use on GitHub (important for linking commits to your GitHub account).

---

### 3. ✅ Check if it worked

```bash
git config --global --list
```

You should see:

```
user.name=Your Name
user.email=you@example.com
```

---

## 🔄 Bonus (Optional, but smart)

### Set default branch name to `main`

```bash
git config --global init.defaultBranch main
```

Git used to default to `master`. Now, `main` is the standard.

---


## 🧑‍🏫 Global Setup: One-Time, Per Machine

* You only need to run the **global setup once** on your **local machine** (e.g., laptop or desktop).
* It applies to **all your Git projects** on that machine.

---
## Q:Why after global setup need to authentication every time?


👉 Global Setup only sets your **identity** (who made the commit).
It does **not** store your GitHub login credentials.

That’s why when you push a repo (`git push`), Git asks for **GitHub username & password (or token)**.

---

## 🔹 Why this happens

1. **Global setup ≠ authentication**

   * Git knows your name/email but not your GitHub account password.

2. **GitHub disabled password authentication**

   * Since 2021, GitHub no longer allows normal passwords for `git push`.
   * You must use a **Personal Access Token (PAT)** or **SSH keys**.

---

## ✅ Solutions

### Option 1: Use HTTPS with Personal Access Token

1. Generate a token in GitHub:

   * Go to **GitHub → Settings → Developer Settings → Personal Access Tokens → Tokens (classic)**
   * Click **Generate new token**
   * Select scopes: `repo`, `workflow` (minimum for pushing code)
   * Copy the token

2. When Git asks for **username & password**:

   * Username → your GitHub username
   * Password → paste the **token** (not your real password)

3. To avoid typing it every time, **cache credentials**:

```bash
git config --global credential.helper store
```

Now Git will remember your token.

---
👍 This is an important difference to understand:

---

# 🔹 `credential.helper store` vs `credential.helper cache`

Both are ways for Git to **remember your credentials**, but they work differently.

---

## 🧑‍🏫 1. `store`

```bash
git config --global credential.helper store
```

* Saves your credentials in a **file**:

  * Linux/Mac → `~/.git-credentials`
  * Windows → `%USERPROFILE%\.git-credentials`
* Stored in **plain text** (⚠️ not encrypted).
* **Permanent** → credentials are reused every time, until you manually remove them.
* Good if you don’t want to type every time, but less secure.

✅ Example entry in the file:

```
https://username:PERSONAL_ACCESS_TOKEN@github.com
```

---

## 🧑‍🏫 2. `cache`

```bash
git config --global credential.helper cache
```

* Stores your credentials in **memory (RAM)**, not in a file.
* Only lasts for a **default timeout of 15 minutes**.
* After that, Git will ask again.
* Safer because it’s temporary, but less convenient.

👉 You can change the timeout (in seconds):

```bash
git config --global credential.helper 'cache --timeout=3600'
```

This keeps credentials for **1 hour**.

---

## 🔍 Quick Comparison

| Feature          | `store` 🗂️                 | `cache` ⏱️                               |
| ---------------- | --------------------------- | ---------------------------------------- |
| **Where stored** | File (`.git-credentials`)   | In memory (RAM)                          |
| **Security**     | ❌ Plain text (unsafe)       | ✅ Safer (not saved permanently)          |
| **Duration**     | Permanent                   | Temporary (default 15 min, configurable) |
| **Use case**     | Convenience, one-time setup | Short-term use, shared machines          |

---

✅ **Best Practice**

* Use **SSH keys** or **Personal Access Token (PAT)** with `store` on **personal machine**.
* Use **cache** on **shared/public computers** where permanent storage is risky.

---
### Option 2: Use SSH Keys (Recommended 🔑)

1. Generate SSH key:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

2. Start SSH agent & add key:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

3. Copy your key to GitHub:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the output → paste into **GitHub → Settings → SSH and GPG Keys → Add new SSH key**.

4. Test connection:

```bash
ssh -T git@github.com
```

5. Change remote from HTTPS → SSH:

```bash
git remote set-url origin git@github.com:username/repo.git
```

Now no password needed 🎉

---

👉 So:

* **Global setup = author identity**
* **Token/SSH = authentication to GitHub**

---

## 🧑‍🏫 What is a Repository (Repo)?

A **repository** is just a **folder** tracked by Git.

It stores:

* Your code
* The history of your changes
* Branches, commits, and more

You can have:

* **Local repo** (on your machine)
* **Remote repo** (on GitHub)

---

## 🔧 Common Repository Management Commands

### 1. **Start a new repo in an existing folder**

```bash
git init
```

* Creates a local Git repo (starts tracking it).

---

### 2. **Clone an existing repo from GitHub**

```bash
git clone <repo-url>
```

Example:

```bash
git clone https://github.com/user/repo.git
```

* This creates a copy of the GitHub repo on your machine.

---

### 3. **Check the status of your repo**

```bash
git status
```

* Shows which files are:

  * Changed
  * Staged
  * Untracked

Use this often.

---

### 4. **View the commit history**

```bash
git log
```

or short version:

```bash
git log --oneline
```

* Shows the history of changes (commits).

---

### 5. **Check which repo you're connected to (remote)**

```bash
git remote -v
```

* Shows where Git will push/pull your code.

---

### 6. **Add a remote repo **

```bash
git remote add origin <repo-url>
```

---

### 7. **Remove a remote **

```bash
git remote remove origin
```

---

### 8. **Rename the remote **

```bash
git remote rename old-name new-name
```

---




Great 👍 Let’s continue in the **same style**.
Here’s **Part 3: Branching in Git/GitHub** 🚀

---

**Date**: 2025-10-04

---

## 🧑‍🏫 Part 3: Branching

A **branch** is like a **separate workspace** where you can experiment, fix bugs, or add features **without affecting the main code**.

* The default branch is usually **`main`**.
* You can create as many branches as you need.
* When finished, you **merge** a branch back into `main`.

---

## 🔹 Why Use Branches?

* Work on **new features** without breaking main code
* Fix bugs independently
* Multiple developers can work in **parallel**
* Keeps code history organized

---

## 🔧 Branch Commands

### 1. **List all branches**

```bash
git branch
```

* Shows current branch (marked with `*`)

---

### 2. **Create a new branch**

```bash
git branch feature-login
```

* Creates branch `feature-login`

---

### 3. **Switch to a branch**

```bash
git checkout feature-login
```

* Moves to branch `feature-login`

---

### 4. **Create & switch (shortcut)**

```bash
git checkout -b feature-login
```

* Creates AND switches in one step

---

### 5. **Merge a branch into main**

```bash
git checkout main
git merge feature-login
```

* Brings `feature-login` changes into `main`

---

### 6. **Delete a branch**

```bash
git branch -d feature-login
```

* Deletes local branch (safe delete)

Force delete (if branch not merged yet):

```bash
git branch -D feature-login
```

---

### 7. **Push a branch to GitHub**

```bash
git push origin feature-login
```

---

### 8. **Pull a remote branch**

```bash
git checkout -b feature-login origin/feature-login
```

* Creates local branch from remote

---
# 🧰 Git Installation and Setup Guide

## 1. Installation and Setup

### 🐧 Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install git
```

---

## 2. Initial Configuration

After installation, configure your Git identity:

```bash
# Set your name
git config --global user.name "Your Name"

# Set your email
git config --global user.email "your.email@example.com"
```

View all configurations:

```bash
git config --list
```

Tell Git to save your credentials permanently so you don’t have to input them every time for push/pull:

```bash
git config --global credential.helper store
```

---

## 3. Creating a New Repository

```bash
# Create a new repository
mkdir my-project
cd my-project

# Initialize Git
git init
```

Expected output:
```
Initialized empty Git repository in /path/to/my-project/.git/
```

---

## 4. Link Local Repository to GitHub Repository

Link your local repository to GitHub via HTTPS:

```bash
git remote add origin https://github.com/username/my-project.git 
```

Confirm that it worked:

```bash
git remote -v
```

---

## 5. Basic Git Workflow

### 1️⃣ Check Status

See which files have changed:

```bash
git status
```

---

### 2️⃣ Stage Changes

Stage a specific file:

```bash
git add filename.txt
```

Stage multiple files:

```bash
git add file1.txt file2.txt
```

Stage all files in the current directory:

```bash
git add .
```

---

### 3️⃣ Change Branch to Main

Ensure your local branch is the same as the remote repository’s branch:

```bash
git branch -M main
```

---

### 4️⃣ Commit Changes

Commit with a message:

```bash
git commit -m "Added feature X"
```

> 💡 You might get an error the first time if Git needs your name and email — make sure you’ve configured them.

---

### 5️⃣ Push Changes

Push changes to the GitHub repository for the first time:

```bash
git push -u origin main
```

The `-u origin main` option sets the local branch to always push to the "main" branch on GitHub.

On first push, Git will ask:

```
Username for 'https://github.com': your_github_username
Password for 'https://github.com': your_PAT_here
```

Subsequent pushes can simply be done with:

```bash
git push
```

---

✅ **You’re all set!**
Your Git is now properly installed, configured, and linked to GitHub.


# HOW TO CONNECT LOCAL REPOSITORY TO GITHUB THROUGH SSH

##Connect with SSH

###1. Generate SSH private and public keys

ssh-keygen -t ed25519 -C lewekies@gmail.com


###2. Start the SSH agent

eval "$(ssh-agent -s)"


###3. Add the SSH Key to your SSH Agent

ssh-add /home/username/.ssh/id_ed25519


###4. Copy the Public SSH key to your clipboard

cat /home/username/.ssh/id_ed25519.pub


###5. Go to your GitHub account → SSH Keys section → Paste and add the key


###6. Clone a repository using SSH

git clone git@github.com:username/repository

###OR, if already cloned, connect to the repository via SSH:

git remote set-url origin git@github.com:username/repository_name


###7. Verify the change (optional)

git remote -v


###8. Test the new SSH key (optional)

ssh -T git@github.com


###9. (Optional) Removing SSH Keys from Your Account
rm /home/lerowi45/.ssh/id_ed25519
rm /home/lerowi45/.ssh/id_ed25519.pub
ssh-add -d /home/lerowi45/.ssh/id_ed25519

Public and Private Key Location
/home/username/.ssh/id_ed25519



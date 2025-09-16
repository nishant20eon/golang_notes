# Daily Git & GitHub Commands

## ✅ **Daily Git & GitHub Commands for a Java Developer**

### 🔹 **Starting Work**

1. **Clone a repository:**
    
    ```bash
    git clone <repository-url>
    
    ```
    
    Example:
    
    ```bash
    git clone https://github.com/username/project.git
    
    ```
    
2. **Check the current branch and status:**
    
    ```bash
    git status
    git branch
    
    ```
    
3. **Pull the latest changes from the remote:**
    
    ```bash
    git pull origin main
    
    ```
    
    or for another branch:
    
    ```bash
    git pull origin feature-branch
    
    ```
    

---

### 🔹 **Making Changes**

1. **Stage changes:**
    
    ```bash
    git add .
    
    ```
    
    or stage specific files:
    
    ```bash
    git add src/Main.java
    
    ```
    
2. **Commit changes with a message:**
    
    ```bash
    git commit -m "Implement feature X or fix bug Y"
    
    ```
    
3. **Push changes to GitHub:**
    
    ```bash
    git push origin main
    
    ```
    

---

### 🔹 **Working with Branches**

1. **Create a new branch:**
    
    ```bash
    git checkout -b feature-branch
    
    ```
    
2. **Switch to an existing branch:**
    
    ```bash
    git checkout feature-branch
    
    ```
    
3. **Push a new branch and set the upstream:**
    
    ```bash
    git push -u origin feature-branch
    
    ```
    

---

### 🔹 **Collaborating**

1. **Fetch all branches and changes without merging:**
    
    ```bash
    git fetch
    
    ```
    
2. **Merge changes from another branch:**
    
    ```bash
    git merge main
    
    ```
    
3. **Resolve conflicts if they appear, then commit:**
    
    ```bash
    git add .
    git commit -m "Resolved merge conflicts"
    
    ```
    
4. **Create a pull request (usually via GitHub website):**
    - Push your branch.
    - Go to GitHub → compare and create a pull request.

---

### 🔹 **Viewing History & Logs**

1. **Show commit history:**
    
    ```bash
    git log
    
    ```
    
    For a more concise view:
    
    ```bash
    git log --oneline
    
    ```
    
2. **See changes before committing:**
    
    ```bash
    git diff
    
    ```
    

---

### 🔹 **Undo / Fix Mistakes**

1. **Unstage a file:**
    
    ```bash
    git reset HEAD <file>
    
    ```
    
2. **Discard changes in a file:**
    
    ```bash
    git checkout -- <file>
    
    ```
    
3. **Amend the last commit (after fixing or adding files):**
    
    ```bash
    git commit --amend
    
    ```
    

---

### 🔹 **Tagging (Optional)**

1. **Create a new tag:**
    
    ```bash
    git tag v1.0
    
    ```
    
2. **Push tags to GitHub:**
    
    ```bash
    git push origin --tags
    
    ```
    

---

## ✅ **Helpful Git Configurations**

- Set your name and email:
    
    ```bash
    git config --global user.name "Your Name"
    git config --global user.email "you@example.com"
    
    ```
    
- Check the config settings:
    
    ```bash
    git config --list
    
    ```
    

---

## 📦 **Typical Daily Workflow Example for a Java Developer**

```bash
git pull origin main
git checkout -b feature/new-login
# Make changes in Java files
git add .
git commit -m "Add login validation logic"
git push -u origin feature/new-login

```

After this, the developer would open a pull request on GitHub to merge the feature branch into `main`.
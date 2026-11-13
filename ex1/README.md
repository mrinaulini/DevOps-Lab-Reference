# Experiment 1 – Git Essentials

**Tools:** VS Code, Git, GitHub  
**Aim:** To understand and perform Git operations such as repository initialization, staging, committing, branching, pushing, creating Pull Requests and merging changes.

## Files

| File | Description |
|------|-------------|
| `ex.py` | Prints welcome messages about Git |
| `ex1.py` | Prints welcome messages about GitHub |

## Source Code

### ex.py
```python
print("Welcome to Git")
print("Git tracks code changes")
```

### ex1.py
```python
print("Welcome to GitHub")
print("My first DevOps experiment")
```

## Git Commands

```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@gmail.com"
git init
git status
git add .
git commit -m "initial commit"
git log
git branch -M main
git remote add origin https://github.com/USERNAME/ex1.git
git remote -v
git push -u origin main
git checkout -b main1
# Modify ex.py, then run:
git add .
git commit -m "updated file"
git push origin main1
```

## Procedure

1. Create a folder named `ex1` on the Desktop and open it in VS Code.
2. Create two files named `ex.py` and `ex1.py` (already provided).
3. Configure the Git username and email in the VS Code Terminal.
4. Initialize the repository using `git init` and check it using `git status`.
5. Add the files using `git add .` and save them using `git commit`.
6. Create a new repository named `ex1` on GitHub.
7. Connect the local repository to GitHub using `git remote add origin`.
8. Push the main branch to GitHub.
9. Create a new branch named `main1` and modify `ex.py`.
10. Add and commit the modification, then push `main1` to GitHub.
11. On GitHub, create a Pull Request from `main1` into `main` and merge it.

> **Note:** Replace `USERNAME` and the email with your own details. On GitHub, choose **Compare & pull request**, create the Pull Request, and select **Merge pull request**.

## Result

Git operations including initialization, staging, committing, branching, pushing, Pull Request creation and merging were performed and verified.
````md
- git init
- git add
- git commit
- git log
- git revert
- git restore
- git status
````
---

## 1. Initialize Repository

Create a project folder and initialize Git:

```bash
mkdir git-demo
cd git-demo
git init
```

Check repository status:

```bash
git status
```

---

## 2. Create a File

```bash
echo "Hello DevOps" > app.txt
```

Check status:

```bash
git status
```

Output shows Untracked files.

---

## 3. Add File to Staging (git add)

```bash
git add app.txt
```

Verify:

```bash
git status
```

Now file is in staging area.

---

## 4. Commit Changes (git commit)

```bash
git commit -m "Initial commit - added app.txt"
```

This creates the first snapshot of the project.

---

## 5. View Commit History (git log)

```bash
git log
```

Short format:

```bash
git log --oneline
```

---

## 6. Modify File and Check Status

```bash
echo "New line added" >> app.txt
git status
```

Git shows file as modified.

---

## 7. Restore Changes (git restore)

If you want to discard local changes:

```bash
git restore app.txt
```

File returns to last committed version.

---

## 8. Create Another Commit

```bash
echo "Version 2" >> app.txt
git add .
git commit -m "Added version 2"
```

Check logs:

```bash
git log --oneline
```

---

## 9. Revert a Commit (git revert)

To undo a commit safely without deleting history:

```bash
git revert HEAD
```

A new commit will be created that reverses previous changes.

---


## git restore

* Discards local working directory changes
* Does not affect commit history

## git revert

* Creates a new commit to reverse changes
* Safe for shared repositories and production


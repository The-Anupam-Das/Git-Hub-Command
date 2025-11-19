# Selective Merge: Moving Changes Except Specific Files

## Scenario

I have ~50 changed files in `test`, and I want to move everything to `main` **except 2 files** that I know by name.

Here's the clean, professional, and safe way to do it 👇

## 🎯 Goal

Push everything from `test` → `main`, except:

- `src/components/DocumentSettings/TeacherSignature.jsx`
- `src/components/Event/EventList.jsx`

## 🧭 Step-by-Step: "Everything Except 2 Files" Approach

### 1️⃣ Get the latest branches

- `git fetch --all`

### 2️⃣ Switch to main and pull latest

- `git checkout main`
- `git pull origin main`

### 3️⃣ Create a temporary branch from main

- `git checkout -b selective/test-to-main`

### 4️⃣ Bring all changes from test into your temp branch

- `git checkout test -- .`
- OR
- `git merge dev`

- This command copies all files that exist in test into your working directory.

### 5️⃣ Now revert the two files you don’t want back to the main version

- `git checkout main -- src/components/DocumentSettings/TeacherSignature.jsx`
- `git checkout main -- src/components/Event/EventList.jsx`

💡 What this does:

- The first command resets TeacherSignature.jsx to whatever version exists in main (ignoring test changes).

- The second does the same for EventList.jsx.

- So now your working directory has:
✅ All 48 desired files from test
🚫 Those 2 files reverted back to main’s state (excluded).

### 6️⃣ Stage and commit everything

- `git add .`
- `git commit -m "Merge all test changes into main except TeacherSignature.jsx and EventList.jsx"`

### 7️⃣ Push and create a PR

- `git push origin selective/test-to-main`

- Then open a PR → main.
Your diff will show all test changes except those 2 files.

🧠 How this works (behind the scenes)

- git checkout test -- . brings all files from the test branch into your new working branch.

- git checkout main -- <file> restores those specific files to their main version — effectively undoing the test changes for those files.

- git add . stages everything that remains (48 files in your case).

- git commit then locks in exactly those changes.

⚠️ Tips

- Always run git status after step 4 and step 5 to verify exactly which files are being committed.

- You can confirm excluded files didn’t change by:  
 `git diff main -- src/components/DocumentSettings/TeacherSignature.jsx`
 `git diff main -- src/components/Event/EventList.jsx`

🧹 Optional cleanup after merge

- Once your PR is merged into main:

- `git checkout main`
- `git pull origin main`
- `git branch -d selective/test-to-main`
- `git push origin --delete selective/test-to-main`

🔥 Abort the broken merge
- `git merge --abort`

🔥 Discard all local changes in dev-to-test
- `git reset --hard`
- `git clean -fd`

1️⃣ Go to the repository
cd /usr/src/kodekloudrepos

2️⃣ Fetch latest changes

Always do this first to ensure your local repo is up to date.

git fetch origin

3️⃣ Switch to the feature branch
git checkout feature


This is the branch that needs to be rebased.

4️⃣ Rebase feature branch onto master
git rebase origin/master

What this does

Takes all feature branch commits

Re-applies them on top of the latest master

Keeps history linear

No merge commit is created

Feature branch work is preserved

5️⃣ Resolve conflicts (if any)

If Git stops due to conflicts:

Fix the conflicted files

Stage the fixes:

git add <file-name>


Continue rebase:

git rebase --continue


If you need to abort (rare):

git rebase --abort

6️⃣ Push the rebased feature branch

Since rebase rewrites commit history, you must force push.

git push origin feature --force


✅ This updates the remote feature branch safely.

🔑 Important Notes (Why this is correct)
Requirement	How it’s satisfied
No data loss	Rebase reapplies feature commits
No merge commit	Rebase keeps linear history
Feature updated with master	Rebasing onto master
Changes pushed	Force push after rebase

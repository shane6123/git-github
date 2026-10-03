Phase 6 — Stash & Advanced Local Workflow 📦
Phase 5 taught:
How to combine branches using merge/rebase and resolve conflicts.

Now we move into something developers face constantly:
“I'm in the middle of my work, but I suddenly need to switch context.”

Maybe:
You're working on:
feature/payment

Then suddenly:
🚨 Production bug!

"Please fix this immediately."

But your current work is incomplete.
You don't want:
git commit -m "half done stuff"

So what do you do?
git stash

This phase should go beyond just git stash. It should teach the viewer how to manage messy, interrupted local work professionally.
🎯 Phase 6 Goal
By the end of this phase, viewers should understand:
📦 Stash
🎯 Selective staging
🏷️ Tags
🧭 Reflog in practical workflows
⚡ Git aliases
🧹 Clean working directory
🛠️ Useful local Git techniques

The core mental model:
             WORKING ON FEATURE
                    │
                    ▼
             🚨 INTERRUPTION
                    │
          ┌─────────┴─────────┐
          │                   │
      Can commit?          Not ready?
          │                   │
          ▼                   ▼
       Commit               Stash
                              │
                              ▼
                        Fix urgent work
                              │
                              ▼
                        Return to feature
                              │
                              ▼
                         Stash pop

1. Start with the real-world scenario
You're working on:
feature/payment

You changed:
payment.js
checkout.js
payment.css

But it's not finished.
Your working tree:
feature/payment

payment.js     ✏️
checkout.js    ✏️
payment.css    ✏️

Then:
🚨 Production bug

"Fix login immediately!"

You need to switch:
git switch main

Git may stop you because your local changes could be overwritten.
2. Your options
Explain that developers have several options.
Uncommitted work
      │
      ├── Ready to commit?
      │       │
      │       └── Commit
      │
      ├── Not ready?
      │       │
      │       └── Stash
      │
      └── Don't need changes?
              │
              └── Restore

This is a much better mental model than:
"git stash temporarily stores changes."

3. git stash
Command:
git stash

Visual:
feature/payment
       │
       │
       ▼
Uncommitted changes
       │
   git stash
       │
       ▼
   📦 Stash
       │
       ▼
Clean working tree

Now:
Working Directory

payment.js      clean
checkout.js     clean
payment.css     clean

You can switch branches.
4. What actually happened?
This is important.
Before:
Working Directory
      │
      ├── payment.js ✏️
      ├── checkout.js ✏️
      └── payment.css ✏️

After stash:
Working Directory
      │
      └── Clean
           
             +

           Stash
             │
             ├── payment.js changes
             ├── checkout.js changes
             └── payment.css changes

Think:
Working Tree
     │
     │ git stash
     ▼
📦 Temporary shelf

5. git stash list
Now suppose you've stashed multiple times.
git stash list

Example:
stash@{0}: WIP on feature/payment
stash@{1}: WIP on feature/login
stash@{2}: WIP on feature/dashboard

Visual:
             STASH
               │
       ┌───────┼────────┐
       ▼       ▼        ▼
    stash@0  stash@1  stash@2
    payment   login   dashboard

Important:
The latest stash is normally stash@{0}.

6. git stash pop
Once your urgent work is finished:
git switch feature/payment

Then:
git stash pop

Visual:
       📦 stash
          │
          │ pop
          ▼
   Working Directory
          │
          ▼
   Your unfinished work
       returns

Important distinction:
stash pop
   ↓
Apply stash
   +
Remove stash

7. git stash apply
Now:
git stash apply

Difference:
stash apply
    ↓
Restore changes
    ↓
Keep stash


stash pop
    ↓
Restore changes
    ↓
Remove stash if successful

This is a very useful distinction.
8. git stash drop
If you don't need a stash:
git stash drop stash@{0}

Visual:
stash@{0}
    │
    ▼
  🗑️ Delete

And:
git stash clear

removes all stash entries.
⚠️ Make the warning visible:
git stash clear removes all stashes.

9. Stash with a message
Instead of:
git stash

teach:
git stash push -m "WIP payment validation"

Then:
git stash list

You'll see:
stash@{0}: On feature/payment: WIP payment validation

This becomes much easier to understand later.
10. Stash only specific changes
Now go one level deeper.
Suppose:
payment.js        → payment work
README.md         → documentation work
debug.log         → temporary debug

You only want to stash some work.
Git supports path-specific stashing.
Example:
git stash push -m "Payment WIP" -- payment.js

The idea:
Working Directory

payment.js ───────► 📦 Stash

README.md ───────── stays

debug.log ───────── stays

This is a useful advanced technique.
11. Stash untracked files
By default, people often forget:
New file
   ↓
Untracked

You can stash untracked files with:
git stash -u

or:
git stash --include-untracked

Visual:
Modified files       → stash
Untracked files      → stash with -u
Ignored files        → normally not included

You don't need to go deeply into ignored files here.
12. The git stash trap
Important teaching point:
Stash is not a replacement for commits.

Don't encourage this:
Day 1 → stash
Day 2 → stash
Day 3 → stash
Day 4 → stash
Day 5 → "Where was my code?" 😵

Instead:
Work is meaningful
      │
      ▼
Make a commit

Work is temporary/incomplete
      │
      ▼
Use stash

A useful rule:
Commit meaningful work; stash temporary interruptions.

13. Selective staging
Now move beyond stash.
Suppose you changed one file in three different ways:
app.js

Change 1 → Login
Change 2 → Debugging
Change 3 → Payment

You only want to commit Change 1.
Instead of:
git add app.js

you can use:
git add -p

Git will interactively ask which hunks to stage.
Conceptually:
             app.js
                │
       ┌────────┼────────┐
       ▼        ▼        ▼
    Login     Debug    Payment
      │
      ▼
   Stage only this

This is a powerful developer workflow.
14. git add -p
Show the idea:
git add -p

Git presents a chunk of changes.
You can choose:
y → stage this hunk
n → don't stage
s → split
q → quit

Don't overwhelm viewers with every option.
The main concept:
You can build a commit from selected pieces of your changes.

15. Why selective staging matters
Imagine:
Your changes:

✅ Feature
✅ Bug fix
❌ Debug console.log
❌ Temporary code

You can create a clean commit:
git add -p
git commit -m "Add payment validation"

Instead of:
git add .
git commit -m "some changes"

This prepares the viewer for professional code review later.
16. git clean
Now introduce another useful cleanup tool.
Suppose:
git status

Untracked files:
    test.txt
    debug.log
    temp.json

If you truly don't need them:
git clean

But do not immediately demonstrate deletion.
First:
git clean -n

or:
git clean --dry-run

This shows what would be removed.
Then:
git clean -f

Strong warning
⚠️ git clean removes untracked files.
Always inspect with -n first.

This is an excellent practical lesson.
17. Git tags
Now shift from temporary work to marking important history.
Suppose:
A ─── B ─── C ─── D ─── E
                  ↑
             Production

You want to mark that release:
v1.0.0
  │
  ▼
D

Use:
git tag v1.0.0

Visual:
A ─── B ─── C ─── D ─── E
                  ↑
                v1.0.0

18. Why tags?
Explain:
Branches usually move. Tags generally identify a specific point in history.

Visual:
Branch:

A ─── B ─── C ─── D
                  ↑
                main
                  ↓
               later moves


Tag:

A ─── B ─── C ─── D
                  ↑
                v1.0.0
                  │
             stays attached
             to that release

This makes tags very useful for releases.
19. Annotated tags
Introduce:
git tag -a v1.0.0 -m "Release version 1.0.0"

Then:
git tag

And:
git show v1.0.0

Explain:
Lightweight tag
     ↓
Simple reference

Annotated tag
     ↓
Metadata
     ├── tagger
     ├── date
     └── message

For releases, annotated tags are worth understanding.
20. Git aliases
Now add a small productivity section.
Suppose you repeatedly type:
git log --oneline --graph --decorate --all

Create an alias:
git config --global alias.lg "log --oneline --graph --decorate --all"

Now:
git lg

Visual:
Long command
     │
     ▼
Git alias
     │
     ▼
Short command

Useful examples:
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch

But since modern Git has switch, I'd teach:
git switch

instead of encouraging old checkout patterns for branch switching.
21. Reflog in practical use
You already introduced reflog in Phase 3.
Now connect it to everyday advanced workflow.
Imagine:
You accidentally:

git reset --hard HEAD~3

Then:
git reflog

Find:
HEAD@{0} → old position
HEAD@{1} → previous position

Then inspect:
git show <commit>

And recover if appropriate.
The lesson:
reflog
  ↓
Local reference movement history
  ↓
Find previous HEAD/branch positions
  ↓
Potential recovery

22. A useful local Git workflow
Now bring all of Phase 6 together.
Imagine:
feature/payment
       │
       ▼
Working on feature
       │
       ▼
🚨 Urgent bug
       │
       ▼
git stash
       │
       ▼
switch to bugfix branch
       │
       ▼
fix + commit
       │
       ▼
return to feature
       │
       ▼
git stash pop
       │
       ▼
continue feature
       │
       ▼
git add -p
       │
       ▼
clean commit
       │
       ▼
git tag later for release

That's a real developer workflow.
23. Phase 6 decision tree
This is an excellent README diagram:
                What should I do?
                       │
          ┌────────────┼─────────────┐
          ▼            ▼             ▼
      Meaningful     Temporary      Unwanted
         work          work          work
          │             │             │
          ▼             ▼             ▼
       Commit         Stash         Restore
                                      │
                                      ▼
                                Untracked files?
                                      │
                                      ▼
                                  git clean

Then:
Need a clean commit?
       │
       ▼
    git add -p
       │
       ▼
Stage only what belongs

And:
Need to mark a release?
       │
       ▼
     git tag

24. Phase 6 command set
Stash
git stash
git stash push -m "message"
git stash list
git stash pop
git stash apply
git stash drop
git stash clear
git stash -u

Selective staging
git add -p

Cleanup
git clean -n
git clean -f

Tags
git tag
git tag v1.0.0
git tag -a v1.0.0 -m "Release 1.0.0"
git show v1.0.0

Aliases
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --decorate --all"

Recovery
git reflog

25. What NOT to overload Phase 6 with
Keep these for later:
❌ Pull Requests
❌ GitHub Actions
❌ GitHub Secrets
❌ Team permissions
❌ Organization
❌ Advanced CI/CD
❌ Cherry-pick in depth
❌ Submodules
❌ Git internals

You want Phase 6 to remain:
"How do I become comfortable managing my local Git workflow?"

🎬 Phase 6 Video Flow
00:00  📦 Why do we need git stash?
          ↓
02:00  Real-world interruption scenario
          ↓
05:00  git stash
          ↓
08:00  git stash list
          ↓
10:00  git stash pop
          ↓
12:00  git stash apply
          ↓
14:00  stash messages
          ↓
16:00  stash untracked files
          ↓
18:00  stash specific changes
          ↓
20:00  Why stash ≠ commit
          ↓
22:00  🎯 git add -p
          ↓
26:00  Selective commits
          ↓
29:00  🧹 git clean
          ↓
32:00  🏷️ Git tags
          ↓
35:00  Lightweight vs annotated tags
          ↓
38:00  ⚡ Git aliases
          ↓
40:00  🛟 Practical reflog
          ↓
43:00  Complete local workflow
          ↓
46:00  Practice exercise
          ↓
48:00  Summary

📄 Phase 6 README
# Phase 6 — Stash & Advanced Local Workflow

## 🎯 Goal

Learn how to manage interrupted work,
create clean commits, mark releases,
and recover from mistakes.

## 📦 Git Stash

### git stash
### git stash list
### git stash pop
### git stash apply
### git stash drop
### git stash clear

## 📝 Stash Messages

## 📁 Stash Untracked Files

## 🎯 Selective Staging

### git add -p

## 🧹 Cleaning Untracked Files

### git clean -n
### git clean -f

## 🏷️ Git Tags

### Lightweight Tags
### Annotated Tags

## ⚡ Git Aliases

## 🛟 Reflog in Practice

## 🧠 Local Git Workflow

## 🚨 Common Mistakes

## 🧪 Practice Exercise

## 📌 Key Takeaways

🧪 Phase 6 Practice Project
Give the viewer a scenario rather than a command checklist.
START

main
 │
 └── feature/payment

Task 1
Make changes to:
payment.js
checkout.js

Don't commit yet.
Task 2
Pretend an urgent bug arrives.
git stash push -m "Payment WIP"

Task 3
Create a bugfix branch:
git switch -c bugfix/login

Fix it and commit.
Task 4
Return:
git switch feature/payment

Restore:
git stash pop

Task 5
Make multiple changes to one file and use:
git add -p

Create a clean commit.
Task 6
Mark a release:
git tag -a v1.0.0 -m "Release v1.0.0"

Finally:
git log --oneline --graph --decorate --all

The viewer should now see something like:
* commit payment feature
* bugfix login
* feature/payment
* main

🧠 Five things viewers must remember
1️⃣ Stash
   → Temporarily put unfinished work aside.

2️⃣ Commit
   → Save meaningful work into Git history.

3️⃣ git add -p
   → Build clean commits from selected changes.

4️⃣ Tag
   → Mark an important point such as a release.

5️⃣ Reflog
   → Find previous local reference positions
     when recovery is needed.

And the transition to the next phase becomes very natural:
PHASE 4
🌿 Branching
     │
     ▼
Work independently
     │
     ▼
PHASE 5
🔀 Merge / Rebase
     │
     ▼
Combine work
     │
     ▼
PHASE 6
📦 Local Workflow
     │
     ├── stash
     ├── clean commits
     ├── tags
     └── recovery
     │
     ▼
PHASE 7
🌐 GitHub Collaboration
     │
     ├── Remote repositories
     ├── Pull Requests
     ├── Code Reviews
     ├── Issues
     └── GitHub workflow

Phase 7 is the major transition from “Git on my machine” → “GitHub and real team collaboration.” That is where we can build the complete multi-developer workflow with Alice/Bob/you, remote branches, Pull Requests, reviews, branch protection, and GitHub Issues.
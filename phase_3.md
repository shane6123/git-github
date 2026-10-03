# Phase 3 — Git History & Recovery

Phase 2 taught:

> **Edit → Stage → Commit → Push**

Now Phase 3 answers the next question every developer eventually asks:

> **“I made a mistake. What exactly happened, and how do I safely go back?”**

This phase should teach viewers to **understand Git history before manipulating it**.

* * *

# 🎯 Phase 3 Goal

By the end, viewers should understand:

```
              Git History

A ─── B ─── C ─── D
              │
              ▼
            HEAD
```

And know how to:

```
🔍 Inspect history
↩️ Undo local changes
⏪ Move commits back
↩️ Safely undo a published commit
🧭 Find lost commits
```

The key distinction of this phase:

> **Discarding file changes, moving Git history, and creating an inverse commit are three different operations.**

* * *

# 1\. Start with the real-world problem

Don't start with git reset.

Start with this:

```
Monday
  ↓
Commit A
"Create login page"

Tuesday
  ↓
Commit B
"Add authentication"

Wednesday
  ↓
Commit C
"Add payment"

Thursday
  ↓
💥 Something broke
```

Developer asks:

> "What changed?"

Git history gives us the answer.

```
A ─── B ─── C
│     │     │
│     │     └── Add payment
│     └──────── Add authentication
└────────────── Create login page
```


This establishes why history matters.

* * *

# 2\. Git commit = checkpoint

'''
Build on Phase 2.

```
                    Git History

       Commit A       Commit B       Commit C
          ●─────────────●─────────────●
          │             │             │
       Initial       Login          Payment
       project       feature         feature
```

Think of commits as **checkpoints in your project's history**.

But be careful with the wording:

> A commit is not simply a "backup file."

It represents a snapshot/state of the project and its parent relationship within Git's history.

* * *

# 3\. git log

Start with the basic history command.

```
git log
```

Then

```
git log --oneline
```

Example:

```
a82f31c Add payment validation
7bd21aa Add authentication
32ca901 Create login page
91ab342 Initial commit
```
Visual:

```
91ab342 → 32ca901 → 7bd21aa → a82f31c
 Initial     Login      Auth      Payment
```

* * *

# 4\. Understanding HEAD

This deserves a dedicated visual.

```
91ab342 → 32ca901 → 7bd21aa → a82f31c
                              ↑
                             HEAD  
```

Explain:

> **HEAD tells Git where your current position is in the history.**

Usually:
```
HEAD
 │
 ▼
main
 │
 ▼
Latest commit
```

More accurately:

```
HEAD
 │
 ▼
main
 │
 ▼
Commit 
```

* * *

# 5\. HEAD, branch and commit

Show the relationship:

```

              HEAD
                │
                ▼
              main
                │
                ▼
A ───── B ───── C
```

Three different things:

```
HEAD   → where you are
main   → branch pointer
C      → commit
```

This is one of the most useful mental models in Git.

* * *

# 6\. git show

Now answer:

> "What exactly is inside this commit?"

```
git show <commit-id>
```

Visual:

```
Commit C
   │
   ├── Author
   ├── Date
   ├── Message
   └── Changes
```

For example:

```
git show a82f31c
```

This lets developers inspect a particular commit.

* * *

# 7\. git diff

Now connect history with differences.

```
git diff
```

means:

```
Working Directory
        │
        ▼
"What changed?"
```

And:

```
git diff HEAD


conceptually asks:

```
Current files
      │
      ▼
Compared with HEAD
```

Also:

Bash

```
git diff <commit1> <commit2>
```

Visual:

```
Commit A                    Commit B
   ●──────────────────────────●
              │
              ▼
        What changed?
```     

This is useful for debugging.

* * *

# 8\. Recovery starts: "I changed a file and regret it"

Scenario:

```
Last commit
    │
    ▼
index.js ✅
    │
    │ developer edits
    ▼
index.js ❌
```

The change hasn't been committed.

Use:

```
git restore index.js
```

Visual:

```
Last committed version
          │
          ▼
     git restore
          │
          ▼
Working file returns
to committed state
```

### Important warning

This discards the uncommitted changes to that file.

So teach:

> **Check what you're discarding before using restore.**

* * *

# 9\. Restore staged changes

Now introduce a slightly more advanced scenario.

```
Working Directory
       │
    git add
       ▼
Staging Area
```

You staged something accidentally.

```
git restore --staged index.js
```

Visual:

```
              git restore --staged
                      │
                      ▼
Staging Area ───────────────► Working Directory
```

The file is **unstaged**, but its working-directory changes remain.

This distinction is very important.

* * *

# 10\. The big topic: git reset

Now transition carefully.

Say:

> "git restore mainly deals with files and staging. git reset can also move where the current branch points in history."

Visual:

```
A ─── B ─── C ─── D
                  ↑
                 main
```

Suppose we reset one commit back:

```
A ─── B ─── C ─── D
          ↑
         main
```        

The branch pointer moved.

* * *

# 11\. Reset — three modes

This deserves a dedicated diagram.

```
                git reset
                    │
          ┌─────────┼─────────┐
          │         │         │
        --soft    --mixed    --hard
          │         │         │
          ▼         ▼         ▼
       Commit     Commit     Commit
       pointer    pointer    pointer
          │         │         │
       Staging    Staging    Staging
       remains    resets     resets
          │         │         │
       Working    Working    Working
       remains    remains    resets`
```       

More concretely:

### \--soft

```
git reset --soft HEAD~1

```
Commit       ← moved back
Staging      ← changes remain staged
Working      ← changes remain
```

### \--mixed

Bash

```
git reset HEAD~1
```

or:

Bash

```
git reset --mixed HEAD~1
```

```
Commit       ← moved back
Staging      ← changes unstaged
Working      ← changes remain
```

### \--hard

Bash

```
git reset --hard HEAD~1
```

```
Commit       ← moved back
Staging      ← reset
Working      ← reset
```

### Important warning

\--hard can discard changes.

For the video, make this very visible:

```
⚠️ git reset --hard

Potentially destroys uncommitted work.
Use it intentionally.
```

* * *

# 12\. Reset vs Restore

This is a common beginner confusion.

Use this table in your README:

| Command | Main purpose |
| --- | --- |
| git restore file | Restore working-file content |
| git restore --staged file | Remove file from staging |
| git reset | Move/reset history or staging depending on mode |
| git revert | Create a new commit that undoes an earlier commit |

Visual:

```
              Recovery Tools

                   Git
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
     restore      reset       revert
        │           │           │
      Files      History      New commit
      /stage     pointer      undoing old
                              commit
```                              

* * *

# 13\. git revert — extremely important

This is where you introduce the concept of **safe undoing of published history**.

Suppose:

```
A ─── B ─── C
          ↑
       Bad commit
```

Instead of deleting C:

```
A ─── B
```

git revert C creates a new commit:

```
A ─── B ─── C ─── D
          │       │
        Bad      Undo C
```

Visual:

```
Original commit
      C
      │
      ▼
   Bad change
      │
      │ git revert
      ▼
New commit D
      │
      ▼
Change is undone
```

The key sentence:

> **git revert does not erase the old commit; it creates a new commit that reverses its changes.**

* * *

# 14\. Reset vs Revert

This should be one of the strongest comparisons in the phase.

```
RESET

A ─── B ─── C
          ↑
        remove/move pointer
```        

versus:

```
REVERT

A ─── B ─── C ─── D
          │       │
        bad     undo
```

### General mental model

```
reset
  ↓
"Move my branch/history pointer."

revert
  ↓
"Create a new commit that undoes an old commit."
```

For commits that have already been shared with others, revert is generally safer because it preserves the existing shared history rather than rewriting it.

* * *

# 15\. What happens if I already pushed?

This is a great real-world scenario.

```
Developer
    │
    ▼
Commit C
    │
    ▼
git push
    │
    ▼
GitHub
    │
    ▼
Team members already have C
```

Then:

```
❌ Don't casually rewrite shared history

```
Instead:

git revert <commit>
git push
```

Result:

```
A ─── B ─── C ─── D
                  ↑
              Undo commit
```              

This naturally prepares the viewer for your later **team workflow phase**.

* * *

# 16\. git reflog — the safety net

Now give viewers the "wow" moment.

Imagine:

```
A ─── B ─── C ─── D
```

You accidentally reset:

```
A ─── B
      ↑
     main
```     

You think:

> "I lost C and D!"

But Git may still know where your branch pointer was.

Bash

```
git reflog
```

Visual:

```
                REFLOG

HEAD@{0} → B
HEAD@{1} → D
HEAD@{2} → C
HEAD@{3} → B
```

Then potentially recover by moving the branch back to the appropriate commit.

Plain text

`reflog │ ▼ Find old commit │ ▼ Recover`

Explain an important nuance:

> reflog records movements of references such as HEAD locally; it is not a permanent backup system and isn't the same thing as the shared GitHub history.

* * *

# 17\. The "Oops!" demonstration

This could be one of the best demos in the entire beginner series.

Create:

```
A ─── B ─── C ─── D
```

Then accidentally:

Bash

```
git reset --hard HEAD~2
```

Now:

```
A ─── B
      ↑
     HEAD
```     

Viewer thinks:

> 😱 "The commits disappeared!"

Then:

Bash

```
git reflog
```

Find:

```
D
```

Then demonstrate recovery carefully.

The lesson:

> **Git often has more history than you can currently see through your branch pointer.**

* * *

# 18\. Recovery decision tree

Give viewers this cheat sheet.

```
                I made a mistake
                       │
          ┌────────────┼─────────────┐
          │            │             │
       File change   Staged       Committed
          │            │             │
          ▼            ▼             ▼
       restore      restore       Was it
                    --staged       pushed?
                                      │
                            ┌─────────┴─────────┐
                            │                   │
                           NO                  YES
                            │                   │
                            ▼                   ▼
                         reset             revert
                            │
                            ▼
                         reflog
                       if needed
```                       

This is excellent material for your README.

* * *

# 19\. Phase 3 command set

Keep the command list organized.

### Inspect

Bash

```
git log
git log --oneline
git show
git diff
git diff HEAD
git diff <commit1> <commit2>
```

### Working-directory recovery

Bash

```
git restore <file>
git restore --staged <file>
```

### History manipulation

Bash

```
git reset --soft HEAD~1
git reset HEAD~1
git reset --hard HEAD~1
```

### Safe undo

Bash

```
git revert <commit>
```

### Recovery

Bash

```
git reflog
```

* * *

# 20\. Don't teach everything about reset yet

This is important for your course structure.

Don't go deeply into:

```
❌ Interactive rebase
❌ Cherry-pick
❌ Squashing
❌ Rewriting remote history
❌ Force push
```

Those concepts fit better after branching and collaboration.

Otherwise beginners will see:

```
restore
reset
revert
rebase
cherry-pick
```

and think:

> "Why does Git have 47 ways to undo one file?" 😄

* * *

# 21\. Phase 3 complete mental model

End the video with this:

```
                         GIT HISTORY
                              │
                              ▼

        A ───── B ───── C ───── D
        │       │       │       │
        │       │       │       └── Current work
        │       │       └────────── Feature
        │       └────────────────── Authentication
        └────────────────────────── Initial project

                              │
                     Something goes wrong
                              │
             ┌────────────────┼────────────────┐
             ▼                ▼                ▼
          restore           reset            revert
             │                │                │
        File/staging      Move pointer      New commit
             │                │                │
             └────────────────┼────────────────┘
                              ▼
                           reflog
                              │
                              ▼
                       Recover history
```                       
                       
* * *

# 🎬 Phase 3 Video Flow

```
00:00  Why Git History Matters
         ↓
01:30  Commits as Checkpoints
         ↓
03:00  git log
         ↓
05:00  Understanding HEAD
         ↓
07:00  git show
         ↓
08:30  git diff
         ↓
10:00  git restore
         ↓
12:00  Restore staged changes
         ↓
14:00  git reset
         ↓
16:00  --soft / --mixed / --hard
         ↓
20:00  git revert
         ↓
23:00  Reset vs Revert
         ↓
25:00  What if I already pushed?
         ↓
27:00  git reflog
         ↓
30:00  Recovery demonstration
         ↓
34:00  Recovery decision tree
         ↓
36:00  Summary
```

* * *

# 📄 Phase 3 README Structure

```
# Phase 3 — Git History & Recovery

## 🎯 Goal

Understand Git history and safely recover
from common mistakes.

## 🧠 Git History Mental Model

A → B → C → D

## 📜 git log

## 🔍 git show

## 🔎 git diff

## 🧭 Understanding HEAD

## ↩️ git restore

## 📦 Restore Staged Changes

## ⏪ git reset

### --soft
### --mixed
### --hard

## ↩️ git revert

## ⚔️ reset vs revert

## 🚨 What If I Already Pushed?

## 🛟 git reflog

## 🔄 Recovery Decision Tree

## ⚠️ Common Mistakes

## 🧪 Practice Exercises

## 📌 Key Takeaways
```

### The 4 sentences I want viewers to leave with

```
1. git restore → "Undo file/staging changes."

2. git reset → "Move/reset my current history pointer
                 depending on the mode."

3. git revert → "Create a new commit that undoes
                 an earlier commit."

4. git reflog → "Help me find where my local references
                 used to point."
```                 

And the phase's central visual:

```
             "I made a mistake!"
                       │
                       ▼
                 Don't panic 😄
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
       restore       reset        revert
          │            │            │
        files       history      new commit
                       │
                       ▼
                    reflog
                       │
                       ▼
                    recover
```                    
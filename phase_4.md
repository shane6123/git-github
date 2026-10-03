Phase 4 — Git Branching 🌿
Phase 3 taught:
How Git stores history and how to recover from mistakes.

Now we introduce one of Git's most important ideas:
How can I work on a new feature without disturbing the existing code?

That is the problem branches solve.
🎯 Phase 4 Goal
By the end of this phase, viewers should understand:
Why branches exist
       ↓
What a branch actually is
       ↓
How branches work internally
       ↓
Create / switch / delete branches
       ↓
Feature branching
       ↓
Multiple features in parallel
       ↓
How branches eventually come back together

The most important mental model:

```
                 main
                  │
                  ▼
A ───── B ─────── C
                  │
                  ├───────────────┐
                  │               │
                  ▼               ▼
           feature/login    feature/payment
                │               │
                ▼               ▼
                D               E
                │               │
                ▼               ▼
                F               G
```

A branch is not a separate copy of the entire project.
That's an important misconception to correct.
1. Start with the real-world problem
Imagine you're working on production code:
main
 │
 A
 │
 B
 │
 C

The application is working.
Now your manager says:
"Add Google login."

You start modifying the production code directly:
main
 │
 A
 │
 B
 │
 C
 │
 D ← Google login
 │
 E ← half finished
 │
 F ← broken 😵

Meanwhile:
"We also need payment."

Now you're mixing:
Google Login
Payment
Bug Fix
Production Code

That's dangerous.
2. Branches solve this problem
Instead:
                    main
                      │
A ───── B ───── C ────●
                      │
             ┌────────┴────────┐
             ▼                 ▼
      feature/login      feature/payment
             │                 │
             ▼                 ▼
             D                 E
             │                 │
             ▼                 ▼
             F                 G

Now:
main
 ↓
Stable code

feature/login
 ↓
Google login work

feature/payment
 ↓
Payment work

Each feature can progress independently.
3. What is a branch actually?
This is where you should go slightly deeper than a typical beginner tutorial.
Many people think:
"A branch is a copy of my project."

Not exactly.
A useful mental model is:
A branch is a movable pointer/reference to a commit.

Start with:
A ─── B ─── C
            ↑
           main

Then:
git switch -c feature/login

You now have:
A ─── B ─── C
            ↑
        main
            ↑
     feature/login

Both branches currently point to the same commit.
Then make a commit:
A ─── B ─── C ─── D
            ↑     ↑
           main  feature/login

Actually, more precisely:
A ─── B ─── C ─── D
            │       ↑
            │       feature/login
            ↑
           main

main stays at C.
feature/login moves to D.
That's the key insight.
4. Branch + HEAD
Connect this with Phase 3.
You previously learned:
HEAD
 │
 ▼
main
 │
 ▼
C

After switching:
HEAD
 │
 ▼
feature/login
 │
 ▼
C

After committing:
HEAD
 │
 ▼
feature/login
 │
 ▼
D

main
 │
 ▼
C

So:
HEAD → current branch → current commit

This relationship is extremely important.
5. Create a branch
Introduce:
git branch

to see branches.
Then:
git branch feature/login

Visual:
A ─── B ─── C
            ↑
        main
            ↑
      feature/login

But you're still on main.
This is a good opportunity to explain:
Creating a branch and switching to a branch are two separate operations.

6. Switch branches
Modern Git:
git switch feature/login

Now:
HEAD
 │
 ▼
feature/login
 │
 ▼
C

And:
main
 │
 ▼
C

Both point to C, but your current branch is feature/login.
7. Create + switch in one command
This is what developers usually do:
git switch -c feature/login

Visual:
Current branch
      │
      ▼
      main
       │
       │ git switch -c
       ▼
feature/login

This:
1. Creates the branch.
2. Switches to it.
8. Make the first feature commit
Now modify:
login.html

Then:
git add login.html
git commit -m "Add login page"

History:
                 feature/login
                       │
                       ▼
A ─── B ─── C ──────── D
             ↑
            main

Then another commit:
A ─── B ─── C ──────── D ─── E
             ↑               ↑
            main       feature/login

This is the first time viewers should see:
The branch pointer moves when we create commits.

9. Switching between branches
Suppose you're on:
feature/login

and need to inspect main.
git switch main

Now:
HEAD
 │
 ▼
main
 │
 ▼
C

Switch back:
git switch feature/login

Now:
HEAD
 │
 ▼
feature/login
 │
 ▼
E

10. What happens to my files?
This is an excellent demonstration.
Imagine:
main
 └── index.js
      console.log("production")

Feature branch:
feature/login
 └── index.js
      console.log("login feature")

When you switch:
git switch main

Git updates your working directory to match the selected branch.
main
 ↓
production code

Then:
git switch feature/login

feature/login
 ↓
login code

So viewers understand:
Switching branches changes the working tree to reflect the selected branch's state.

11. The feature-branch workflow
Now introduce the workflow they'll use repeatedly.
                main
                  │
                  ▼
             Create branch
                  │
                  ▼
          feature/login
                  │
          ┌───────┴───────┐
          ▼               ▼
       edit code       edit code
          │               │
          ▼               ▼
       commit          commit
          │               │
          └───────┬───────┘
                  ▼
             Pull Request
                  │
                  ▼
                main

At this stage, don't deeply teach Pull Requests.
Tell viewers:
"We'll learn how this branch gets back into the team code through Pull Requests and merging in later phases."

That creates anticipation.
12. Multiple features at the same time
Now make the demo realistic.
                         main
                          │
                          C
                       /     \
                      /       \
                     ▼         ▼
             feature/login   feature/payment
                  │               │
                  D               E
                  │               │
                  F               G

Three developers:
Alice
 └── feature/login

Bob
 └── feature/payment

Charlie
 └── feature/dashboard

Visual:
                         main
                           │
                           C
                ┌──────────┼──────────┐
                ▼          ▼          ▼
             login      payment    dashboard
                │          │          │
                ▼          ▼          ▼
               D            E          F

This is the beginning of parallel development.
13. Why not create everything from main?
Suppose:
main
 │
 C

You create:
feature/login

But someone else has already added important work to main.
main
 │
 C ─── D

Your branch:
C
 \
  L1 ─── L2

Now:
main
 │
 C ─── D

feature/login
     \
      L1 ─── L2

This leads naturally to:
"How do I bring the latest main changes into my feature?"

That's where merge and rebase come later.
Don't solve it fully in Phase 4.
Just introduce the problem.
14. Branch naming
Teach practical naming conventions.
Good:
feature/login
feature/payment
feature/user-profile

bugfix/login-validation
bugfix/payment-error

hotfix/payment-production

chore/update-dependencies

Avoid:
test
new
branch1
shane
mybranch
final
new-feature

A useful pattern:
<type>/<short-description>

Example:
feature/google-login
bugfix/token-expiry
hotfix/payment-failure
chore/node-version

15. Delete a branch
After the work is complete:
git branch -d feature/login

Visual:
Before:

main ─── C ─── D
              ↑
         feature/login


After:

main ─── C ─── D

Important:
Deleting a branch doesn't necessarily delete the commits themselves.

You're deleting the branch reference.
This connects nicely to the pointer mental model.
16. Local vs remote branches
Introduce this briefly.
So far we've had:
Your computer

main
feature/login
feature/payment

GitHub has its own branches:
GitHub

main
feature/login
feature/payment

Visual:
             Your Computer
                  │
        ┌─────────┼─────────┐
        │         │         │
       main     login     payment
        │         │         │
        └─────────┼─────────┘
                  │
                GitHub
                  │
        ┌─────────┼─────────┐
        │         │         │
       main     login     payment

We'll properly cover remote branches and tracking branches when we move into GitHub collaboration.
17. Branch vs folder
This misconception is worth explicitly correcting.
❌ Don't think:
project/
├── main/
├── feature-login/
└── feature-payment/

✅ Think:
             Git Repository
                   │
             Commit History
                   │
       ┌───────────┼───────────┐
       ▼           ▼           ▼
      main      login       payment
       │           │           │
       └────── pointers ───────┘

Branches are references into the commit graph, not ordinary project folders.
18. What happens when branches diverge?
This is an important visual before Phase 5.
                 main
                  │
A ─── B ─── C ─── D
           \
            E ─── F
                 ↑
          feature/login

Now the histories have diverged.
main       → A B C D

feature    → A B C E F

Both contain:
A → B → C

but then they have different commits.
This creates the next question:
How do we combine them?

And that becomes Phase 5 — Merge, Rebase & Conflicts.
19. Detached HEAD — optional but useful
I would introduce this briefly, not make it a major topic.
Normally:
HEAD
 │
 ▼
main
 │
 ▼
C

But if you checkout a specific commit:
git switch --detach <commit>

you can get:
        HEAD
         │
         ▼
         C

main ───────────────► D

Now HEAD points directly to a commit instead of a branch.
Explain:
Detached HEAD means you're not currently on a branch.

Don't go deep into recovery here; Phase 3 already introduced reflog, and advanced branch workflows can revisit this later.
20. Phase 4 practical demo
I recommend one continuous demo:
Starting point
main
 │
A ─── B

Create login
git switch -c feature/login

A ─── B
      ↑
      └── main
      └── feature/login

Commit login
A ─── B ─── C
      ↑     ↑
     main  login

Create another feature from main
git switch main
git switch -c feature/payment

Then:
                  feature/login
                       │
A ─── B ─── C ──────── D
      │
     main
      │
      └──── E
            ↑
       feature/payment

Now you have:
login
  ↓
C → D

payment
  ↓
B → E

This makes the parallel development concept very easy to see.
21. Phase 4 command set
# List branches
git branch

# Create branch
git branch feature/login

# Switch branch
git switch feature/login

# Create + switch
git switch -c feature/login

# Switch back
git switch main

# Delete local branch
git branch -d feature/login

# Force delete local branch
git branch -D feature/login

# Show branches with commit information
git branch -v

# Show all local + remote branches
git branch -a

For beginners, emphasize these four:
git branch
git switch
git switch -c
git branch -d

22. Important safety point: -d vs -D
Don't spend too much time here, but explain:
git branch -d
       ↓
Safe deletion check

versus:
git branch -D
       ↓
Force deletion

Visual:
-d
 │
 └── "Is it safe to delete?"
           │
          YES
           ↓
        delete


-D
 │
 └── "Delete it anyway."

Tell viewers:
Use -D intentionally; don't make it your default.

23. Phase 4 complete mental model
This should be your final diagram:
                         GIT BRANCHING
                              │
                              ▼
                    One history / many paths
                              │
                              ▼

A ─── B ─── C ────────────────●──────── main
                 \             \
                  \             \
                   D ─── E ─────●── feature/login
                    \
                     F ─── G ───●── feature/payment

And:
Branch
  ↓
A movable pointer to a commit

HEAD
  ↓
Where I am currently working

Commit
  ↓
A point in project history

🎬 Phase 4 Video Flow
00:00  🌿 Why do we need branches?
          ↓
02:00  Real-world feature development problem
          ↓
04:00  What is a branch?
          ↓
07:00  Branch = pointer to commit
          ↓
09:00  HEAD + branch relationship
          ↓
11:00  git branch
          ↓
13:00  git switch
          ↓
15:00  git switch -c
          ↓
17:00  Making commits on a feature branch
          ↓
19:00  Switching between branches
          ↓
21:00  Multiple features in parallel
          ↓
24:00  Branch naming
          ↓
25:00  Delete branches
          ↓
27:00  Local vs remote branches
          ↓
29:00  Branch divergence
          ↓
31:00  Detached HEAD — quick introduction
          ↓
33:00  Complete branching workflow
          ↓
35:00  Summary

📄 Phase 4 README Structure
# Phase 4 — Git Branching

## 🎯 Goal

Understand how Git branches allow developers
to work on features independently.

## 🧠 What Problem Do Branches Solve?

## 🌿 What Is a Branch?

## 📌 Branch = Pointer to a Commit

## 🧭 HEAD and Branches

## 🌱 Create a Branch

## 🔀 Switch Branches

## 🚀 Create + Switch

## 👥 Multiple Features

## 🏷️ Branch Naming Convention

## 🗑️ Delete a Branch

## 🌐 Local vs Remote Branches

## 🌳 Branch Divergence

## ⚠️ Detached HEAD

## 🛠️ Commands

## 🚨 Common Mistakes

## 🧪 Practice Exercise

## 📌 Key Takeaways

🧪 Practice Exercise for Viewers
Give them one exercise instead of just asking them to copy commands.
Start:

main
 │
 A

Task
Create:
feature/login
feature/payment
feature/profile

Make at least two commits on each feature.
They should eventually have:
                         main
                          │
                          A
                       /  │  \
                      /   │   \
                     /    │    \
                  login payment profile
                    │      │      │
                    ●      ●      ●
                    │      │      │
                    ●      ●      ●

Then ask them to run:
git branch
git log --oneline --all --graph

That last command is particularly useful here:
git log --oneline --graph --all

It lets them see the branch structure visually in the terminal.
🧠 The 5 things viewers must remember
1️⃣ Branch
   → A movable pointer to a commit.

2️⃣ HEAD
   → Where you are currently working.

3️⃣ Feature branch
   → Isolate work from the main line.

4️⃣ Multiple branches
   → Allow multiple features to progress independently.

5️⃣ Diverged branches
   → Eventually need to be combined.

And that gives us the perfect bridge:
                  PHASE 4
              🌿 Branching
                    │
                    ▼
        "Now I have multiple branches."
                    │
                    ▼
            ┌───────┴────────┐
            │                │
          main          feature/login
            │                │
            └───────┬────────┘
                    ▼
             ❓ How combine?
                    │
                    ▼
       PHASE 5 — Merge / Rebase
                    │
                    ▼
Phase 5 — Merge, Rebase & Merge Conflicts 🔀
Phase 4 taught:
Branches let us work independently.

Now we have a problem:
main
 │
 A ─── B ─── C ─── D
          \
           E ─── F
                ↑
           feature/login

We now need to answer:
How do we bring the feature back into the main code?

This phase should teach three things deeply:
                    Branches
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
        Merge        Rebase      Conflicts
          │            │            │
       Combine      Re-write      Resolve
       histories     history       changes

🎯 Phase 5 Goal
By the end, viewers should understand:
- What merge does
- Fast-forward merge
- Three-way merge
- What a merge commit is
- What rebase does
- Merge vs rebase
- Why conflicts happen
- How to resolve conflicts
- How to continue/abort a merge
- How to continue/abort a rebase
- Why force-pushing after rebase needs care
- A practical team workflow
The most important rule:
Don't teach merge and rebase as commands to memorize. Teach them as two different ways of combining histories.

1. Start with the problem
Use the project from Phase 4.
main
 │
 A ─── B ─── C
          \
           D ─── E
                ↑
          feature/login

The feature is complete.
But main has moved:
main
 │
 A ─── B ─── C ─── F
          \
           D ─── E
                ↑
          feature/login

Now:
main       → A B C F

feature    → A B C D E

We have two histories.
Question:
How do we combine them?

Answer:
Merge
Rebase

2. Merge — the simplest mental model
The first thing to teach:
Merge combines two histories.

git switch main
git merge feature/login

Visual:
             feature/login
                  │
                  ▼
A ─── B ─── C ─── D ─── E
          \             /
           F ──────────
                 ↑
                main

More accurately, when histories have diverged:
                D ─── E
               /       \
A ─── B ─── C           M
               \       /
                F ────

Where:
M = merge commit

3. Fast-forward merge
Before teaching the complicated merge, show the simplest case.
Start:
A ─── B
      ↑
     main

Create feature:
A ─── B ─── C ─── D
      ↑           ↑
     main       feature

Notice:
main hasn't changed since the feature branch was created.

So Git can simply move main forward:
A ─── B ─── C ─── D
                  ↑
                main
                feature

No merge commit is needed.
This is a:
Fast-forward merge

4. Fast-forward visual
Before:

main
 ↓
A ─── B ─── C ─── D
                  ↑
               feature


After merge:

A ─── B ─── C ─── D
                  ↑
            main + feature

The important idea:
Git didn't have to combine two divergent histories. It only moved the branch pointer forward.

5. Three-way merge
Now create the realistic scenario.
                 feature
                    │
                    D
                   /
A ─── B ─── C
             \
              F
              ↑
             main

Both branches changed after C.
Git identifies:
Base = C

Main changes:
C → F

Feature changes:
C → D

Then Git combines those changes.
                 D
                / \
               /   \
A ─── B ─── C       M
               \   /
                F

This is called a:
Three-way merge

Because Git considers:
             Common Ancestor
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
       Feature               Main
          │                   │
          └─────────┬─────────┘
                    ▼
                  Merge

6. Merge commit
If both branches have diverged, Git may create:
M

The merge commit has two parents:
        D
       / \
      /   \
     C     M
      \   /
       F

Conceptually:
M
├── Parent 1 → feature
└── Parent 2 → main

This is useful because the history records:
"These two lines of development were combined here."

7. Now introduce Rebase
Don't start with:
git rebase main

Start with the history.
Before:
                 D ─── E
                /
A ─── B ─── C
             \
              F ─── G
                   ↑
                  main

Actually, feature is based on C while main has moved to F/G.
The goal of rebase:
Take the feature commits and replay them on top of the latest main.

Conceptually:
Before:

A ─── B ─── C ─── F ─── G
          \
           D ─── E

After rebase:
A ─── B ─── C ─── F ─── G ─── D' ─── E'
                              ↑
                         feature/login

The commits become new commits:
D → D'
E → E'

because their parent relationship changed.
8. Rebase visual
This is the image I would emphasize heavily:
             BEFORE

main
 │
 A ─── B ─── C ─── F ─── G
          \
           D ─── E
                ↑
             feature


             AFTER REBASE

A ─── B ─── C ─── F ─── G ─── D' ─── E'
                                     ↑
                                  feature

Say:
Rebase changes the base of your branch.

That is much easier to remember than:
"Rebase moves commits."

9. Merge vs Rebase
Now give the viewer the side-by-side picture.
Merge
A ─── B ─── C ─── F
          \       /
           D ─ E
                \
                 M

Preserves the branching structure.
Rebase
A ─── B ─── C ─── F ─── D' ─── E'

Creates a more linear history.
10. The conceptual difference
Use this table in the README:
Merge	Rebase
Combines histories	Replays commits on a new base
Preserves divergence	Produces a linear-looking history
Can create merge commit	Doesn't create a merge commit for the rebase itself
Doesn't rewrite existing commits	Recreates/re-writes commits
Usually straightforward for shared branches	Requires more care when commits are already shared


The key:
Merge
→ "Combine these histories."

Rebase
→ "Put my work on top of this newer base."

11. Why rebase can be dangerous
This is extremely important.
Suppose you pushed:
A ─── B ─── C
          \
           D ─── E

Other developers may already have D and E.
Then you rebase:
A ─── B ─── C ─── F ─── G ─── D' ─── E'

D and E are replaced by different commit objects.
Now the shared history becomes complicated.
Therefore:
Avoid rebasing commits that other people are already depending on unless your team has explicitly agreed on that workflow.

Simple rule for beginners:
Private/local work
        ↓
Rebase is usually easier to use

Shared/public history
        ↓
Be very careful with rebase

12. Merge conflict — the important part
Now create a conflict intentionally.
This is where the video becomes highly practical.
Start:
main
 │
 A

Two branches:
feature/login
feature/profile

Both modify the same line.
Original:
const message = "Hello";

Developer A:
const message = "Hello User!";

Developer B:
const message = "Welcome Back!";

Now Git cannot automatically decide which one should win.
13. What Git shows
During the conflict:
<<<<<<< HEAD
const message = "Welcome Back!";
=======
const message = "Hello User!";
>>>>>>> feature/login

Explain each marker:
<<<<<<< HEAD
     ↓
Current branch version

======= 
     ↓
Separator

>>>>>>> feature/login
     ↓
Incoming branch version

This is not valid final code.
The developer must decide what the final code should be.
14. Conflict resolution
Suppose the desired final result is:
const message = "Hello User! Welcome Back!";

Remove the conflict markers:
<<<<<<<
=======
>>>>>>>

Then:
git add .

And:
git commit

The merge is complete.
Visual:
Conflict
   ↓
Inspect both changes
   ↓
Choose/combine correct code
   ↓
Remove markers
   ↓
git add
   ↓
git commit
   ↓
Merge complete

15. Important: Git doesn't decide business logic
This is a great teaching point.
Git can detect:
"Two people changed the same area."

But Git cannot necessarily know:
"Which behavior is correct for the application?"

So:
Git
 ↓
Detect conflict
 ↓
Developer
 ↓
Understand code
 ↓
Choose correct result

This is why merge conflicts require developer judgment, not just a command.
16. git status during conflict
Teach viewers to immediately run:
git status

Git will tell them what is happening.
Typical mental model:
Merge in progress
       │
       ▼
git status
       │
       ▼
Which files are conflicted?
       │
       ▼
Resolve them

Again:
When confused, git status is your friend.

17. Abort a merge
What if you realize:
"I don't want to do this merge."

You can abort:
git merge --abort

Visual:
Merge started
     │
     ▼
Conflict
     │
     ├─────────────┐
     │             │
   Resolve       Abort
     │             │
     ▼             ▼
 Complete       Return to
 merge          pre-merge state

This is an important safety tool.
18. Rebase conflicts
Rebase can also create conflicts.
Example:
git rebase main

If conflict occurs:
Conflict
   │
   ▼
Resolve file
   │
   ▼
git add .
   │
   ▼
git rebase --continue

If you decide:
"Stop this rebase."

Use:
git rebase --abort

So viewers learn:
Merge conflict:
git merge --abort

Rebase conflict:
git rebase --abort

And:
Rebase resolved:
git rebase --continue

19. The complete merge workflow
             feature/login
                  │
                  ▼
             git switch main
                  │
                  ▼
       git merge feature/login
                  │
           ┌──────┴──────┐
           │             │
        No conflict    Conflict
           │             │
           ▼             ▼
       Merge done     git status
                         │
                         ▼
                    Fix conflicts
                         │
                         ▼
                       git add
                         │
                         ▼
                      git commit
                         │
                         ▼
                    Merge complete

20. The complete rebase workflow
           feature/login
                │
                ▼
          git rebase main
                │
         ┌──────┴──────┐
         │             │
      No conflict    Conflict
         │             │
         ▼             ▼
       Done         Fix code
                       │
                       ▼
                    git add
                       │
                       ▼
                git rebase --continue
                       │
                 ┌─────┴─────┐
                 │           │
              Continue      Abort
                 │           │
                 ▼           ▼
               Done    git rebase --abort

21. Merge vs Rebase — practical scenario
This is where I would make your teaching more realistic.
Imagine:
main
 │
 A ─── B ─── C ─── D
          \
           E ─── F
                ↑
             feature

Option 1 — Merge
git switch main
git merge feature

Result:
A ─── B ─── C ─── D ───── M
          \             /
           E ─── F ────

Option 2 — Rebase
First:
git switch feature
git rebase main

Result:
A ─── B ─── C ─── D ─── E' ─── F'

Then potentially fast-forward main:
git switch main
git merge feature

Result:
A ─── B ─── C ─── D ─── E' ─── F'

This gives viewers a very clear picture of why teams sometimes use rebase.
22. Don't say "rebase is better"
This is important for your course.
Don't teach:
"Rebase is better because history is clean."

Instead:
"Merge and rebase have different tradeoffs. Teams choose based on their workflow, history preferences, and whether commits are already shared."

Then show the factual differences.
That keeps the tutorial technically responsible.
23. Force push after rebase
This is an advanced but important connection.
After rebasing a branch that was previously pushed:
Remote:
A ─ B ─ C ─ D ─ E

Local:
A ─ B ─ C ─ D' ─ E'

Normal push may be rejected because the histories diverged.
Developers may encounter:
git push --force-with-lease

Prefer teaching:
--force-with-lease

rather than casually teaching:
git push --force

Explain:
--force-with-lease provides a safety check that the remote branch hasn't changed unexpectedly since your last fetch.

But don't encourage force-pushing shared branches.
24. The complete Phase 5 mental model
This should be your final big diagram:
                         BRANCHES
                            │
                            ▼
                 Multiple lines of work
                            │
                            ▼
                    ┌───────┴────────┐
                    │                │
                  MERGE            REBASE
                    │                │
             Combine histories   Replay commits
                    │                │
                    ▼                ▼
             Preserve graph       New commit IDs
                    │                │
                    └───────┬────────┘
                            ▼
                       CONFLICT?
                            │
                    ┌───────┴───────┐
                    │               │
                    NO              YES
                    │               │
                    ▼               ▼
                  Done         Resolve manually
                                   │
                                   ▼
                              git add
                                   │
                         ┌─────────┴─────────┐
                         ▼                   ▼
                       merge              rebase
                     git commit       git rebase --continue

🎬 Phase 5 Video Flow
00:00  🔀 Why do we need merge?
          ↓
02:00  Branches have diverged
          ↓
04:00  What does merge do?
          ↓
06:00  Fast-forward merge
          ↓
09:00  Three-way merge
          ↓
12:00  Merge commit
          ↓
14:00  What is rebase?
          ↓
17:00  Rebase visual demonstration
          ↓
20:00  Merge vs Rebase
          ↓
23:00  Why rebase rewrites history
          ↓
25:00  When shared history matters
          ↓
27:00  Create intentional merge conflict
          ↓
30:00  Understand conflict markers
          ↓
33:00  Resolve conflict
          ↓
35:00  git merge --abort
          ↓
36:00  Rebase conflict
          ↓
38:00  git rebase --continue
          ↓
39:00  git rebase --abort
          ↓
40:00  force-with-lease
          ↓
42:00  Complete workflow
          ↓
44:00  Summary

📄 Phase 5 README
# Phase 5 — Merge, Rebase & Conflicts

## 🎯 Goal

Understand how Git combines divergent branches
and how to resolve conflicts.

## 🌳 Branch Divergence

## 🔀 Git Merge

### Fast-Forward Merge

### Three-Way Merge

### Merge Commit

## 🔄 Git Rebase

## ⚔️ Merge vs Rebase

## 💥 Merge Conflicts

### Why Conflicts Happen

### Conflict Markers

### Resolving Conflicts

## 🛑 Abort a Merge

git merge --abort

## 🔄 Rebase Conflicts

git rebase --continue
git rebase --abort

## 🚨 Rewriting History

## 🔐 Force Push

### git push --force
### git push --force-with-lease

## 🧠 Practical Decision Guide

## 🧪 Practice Exercises

## 📌 Key Takeaways

🧪 Practice Exercise
Give viewers a conflict challenge, not just commands.
Step 1
Create:
main
 │
 A

Step 2
Create:
feature/login

Change the same line in the feature branch.
Step 3
Switch to main.
Change the same line differently.
main:
"Hello from main"

feature:
"Hello from login"

Step 4
Merge:
git merge feature/login

💥 Conflict.
Step 5
Resolve it manually.
Then:
git add .
git commit

Finally inspect:
git log --oneline --graph --all

The viewer should see:
*   Merge branch 'feature/login'
|\
| * Login change
* | Main change
|/
* Previous commit

That terminal graph is the payoff of the entire phase.
🧠 Five things viewers must remember
1️⃣ Merge
   → Combines two histories.

2️⃣ Fast-forward
   → Git only moves the branch pointer forward.

3️⃣ Rebase
   → Replays commits onto a new base.

4️⃣ Conflict
   → Git cannot automatically determine
     the correct combined result.

5️⃣ Shared history
   → Be careful when rewriting commits
     other developers already have.

And the perfect bridge to Phase 6:
Phase 4
🌿 Branching
      │
      ▼
"I can work independently."
      │
      ▼
Phase 5
🔀 Merge / Rebase
      │
      ▼
"I can combine branches."
      │
      ▼
"But what if I need to
temporarily stop my work?"
      │
      ▼
Phase 6
📦 Stash & Advanced Local Workflow
      │
      ├── git stash
      ├── partial staging
      ├── tags
      ├── aliases
      └── reflog in practical workflows

This keeps the learning progression natural: individual work → branching → combining work → managing interruptions and advanced local workflows.
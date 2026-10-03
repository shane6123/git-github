PPhase 7 — GitHub Collaboration 🌐👥
This is a major milestone in the course.
Phases 1–6 were mostly about:
              YOUR COMPUTER
                    │
                    ▼
                   Git
                    │
       ┌────────────┼────────────┐
       ▼            ▼            ▼
    commits      branches      history
       │            │            │
       └────────────┼────────────┘
                    ▼
               local workflow

Now we move to:
                    GitHub
                      ☁️
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
   👨‍💻 Developer A  👨‍💻 Developer B  👨‍💻 Developer C
        │              │              │
        └──────────────┼──────────────┘
                       ▼
                  Collaboration

The viewer should finally understand:
Git manages version history. GitHub gives the team a place to collaborate around that Git history.

🎯 Phase 7 Goal
By the end of this phase, viewers should understand:
GitHub Repository
      ↓
Remote
      ↓
Push / Pull / Fetch
      ↓
Remote Branches
      ↓
Pull Requests
      ↓
Code Review
      ↓
Issues
      ↓
Team Collaboration

And most importantly:
What actually happens when multiple developers work on the same GitHub repository?

1. Start with the problem
Phase 6 ended with a developer working locally.
Now imagine:
                    E-Commerce Project

                          Git
                           │
                       Your laptop

Everything is fine.
But then:
👨‍💻 Alice
👨‍💻 Bob
👨‍💻 Shane
👨‍💻 Charlie

all need to work on the same project.
You can't simply pass around:
project.zip
project-final.zip
project-final-new.zip

😂
You need a shared remote repository.
2. GitHub Repository
Introduce:
                    GitHub
                       ☁️
                       │
                       ▼
                ┌──────────────┐
                │  Repository  │
                │              │
                │  Source Code │
                │  History     │
                │  Branches    │
                │  Issues      │
                │  PRs         │
                └──────────────┘

Then connect the local repository:
             YOUR COMPUTER
                   │
                   │ Git
                   ▼
            Local Repository
                   │
                 remote
                   │
                   ▼
                GitHub
                   │
                   ▼
           Remote Repository

3. Local vs Remote
This distinction must be crystal clear.
LOCAL
────────────────────

Your Mac
   │
   └── Git repository
        │
        ├── commits
        ├── branches
        └── history

versus:
REMOTE
────────────────────

GitHub
   │
   └── Remote repository
        │
        ├── commits
        ├── branches
        └── history

Together:
             LOCAL                       REMOTE

        Your Computer                  GitHub
             │                            │
             ▼                            ▼
       Local Repository  ◄────────► Remote Repository
                              │
                         push / pull

4. origin
Bring back the concept from Phase 2.
git remote -v

Example:
origin  git@github.com:shane/shop.git

Explain:
origin
  │
  └── conventional name
      for the remote repository

Visual:
Your Repository
      │
      │ origin
      ▼
GitHub Repository

And importantly:
origin is just a remote name. It isn't the name of GitHub.

5. Push
The local developer makes commits:
A ─── B ─── C

Then:
git push

GitHub becomes:
A ─── B ─── C

Visual:
LOCAL                         GITHUB

A ─── B ─── C   ───────────► A ─── B ─── C
                   push

6. Pull
Now another developer joins.
                 GitHub
                    │
                    │
                    ▼
              Developer B

Developer B runs:
git pull

Conceptually:
GitHub
  │
  ▼
Fetch changes
  │
  ▼
Integrate changes
  │
  ▼
Local repository

Important:
git pull is essentially a fetch followed by integration, commonly merge or rebase depending on configuration/options.

Don't oversimplify it as merely "download code."
7. Fetch vs Pull
Use a very clear visual:
                 GitHub
                    │
            ┌───────┴───────┐
            │               │
          fetch            pull
            │               │
            ▼               ▼
    Update remote refs   Fetch + integrate
            │               │
            ▼               ▼
       You inspect       Working branch
       first

Example:
git fetch origin

Then inspect:
git log main..origin/main

Or simply:
git log --oneline --all

Then decide how to integrate.
This is where viewers start understanding why fetch can be safer for inspection than blindly pulling.
8. Remote branches
Now introduce:
main
feature/login
feature/payment

on GitHub.
Visual:
                  GitHub
                     │
            ┌────────┼────────┐
            ▼        ▼        ▼
          main     login    payment

Locally:
Your Computer
      │
      ├── main
      ├── feature/login
      └── feature/payment

And remote-tracking references:
origin/main
origin/feature/login
origin/feature/payment

9. Local branch vs remote-tracking branch
This is a very important concept.
main

is your local branch.
origin/main

is your local reference to the state of the main branch on the remote.
Visual:
LOCAL

main
 │
 ▼
C


origin/main
 │
 ▼
C

Suppose someone pushes:
GitHub:

A ─── B ─── C ─── D

You haven't fetched yet:
Your machine:

main        → C
origin/main → C

After:
git fetch

you might have:
main        → C
origin/main → D

Now you can see:
The remote has moved ahead of my local branch.

This is an excellent visual lesson.
10. Tracking branches
Now introduce:
main
  │
  └── tracks origin/main

Visual:
Local branch
     │
     │ tracks
     ▼
Remote-tracking branch
     │
     ▼
GitHub branch

This is why:
git push

or:
git pull

can often work without specifying the remote and branch every time.
Don't go too deeply into upstream configuration yet.
11. Multiple developers
Now start the main practical story.
                         GitHub
                            │
                     ┌──────┴──────┐
                     │ Repository  │
                     └──────┬──────┘
                            │
           ┌────────────────┼────────────────┐
           │                │                │
           ▼                ▼                ▼
        👨‍💻 Alice        👨‍💻 Bob         👨‍💻 Shane
           │                │                │
        login            payment          dashboard
           │                │                │
        branch            branch           branch

Each developer:
pull/fetch
    ↓
create feature branch
    ↓
work
    ↓
commit
    ↓
push

12. Pull Request — the biggest new concept
Now introduce PRs.
Suppose:
main
 │
 A ─── B ─── C
          \
           D ─── E
                ↑
           feature/login

Developer pushes:
git push -u origin feature/login

GitHub now has:
feature/login
       │
       ▼
D ─── E

Then the developer opens:
feature/login
      │
      ▼
Pull Request
      │
      ▼
main

13. What is a Pull Request?
Don't define it as:
"A request to pull code."

That's confusing.
Teach:
A Pull Request is a collaboration and review mechanism for proposing changes from one branch into another.

Visual:
feature/login
      │
      │  "I want to merge my changes"
      ▼
Pull Request
      │
      ├── Changed files
      ├── Commits
      ├── Description
      ├── Review comments
      └── Checks
      │
      ▼
main

This is the moment GitHub becomes much more than just "cloud Git."
14. Pull Request lifecycle
This should be a major diagram.
Developer
    │
    ▼
Feature Branch
    │
    ▼
git push
    │
    ▼
GitHub
    │
    ▼
Create Pull Request
    │
    ▼
Code Review
    │
    ├──────────────┐
    │              │
  Changes       Approved
  requested        │
    │              ▼
    │           Merge
    │              │
    └──► Update ◄──┘
                   │
                   ▼
                  main

15. Pull Request review
Show a realistic example:
PR #42
Add Google Authentication

Files changed: 8
Commits: 4
Checks: ✅

Reviewer:
👨‍💻 Bob

"Can we move this token validation
into middleware?"

Developer responds:
"Updated. Thanks."

Then:
Code review
    ↓
Changes requested
    ↓
Developer pushes changes
    ↓
PR updates automatically

This is the real collaboration loop.
16. PR is not just a merge button
Very important.
A PR can contain:
                Pull Request
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
   Discussion      Review       Checks
        │            │            │
        ▼            ▼            ▼
   Requirements    Quality       Tests

That's why PRs are valuable to engineering teams.
17. PR checks
Now introduce CI concept lightly.
A PR can show:
Checks

✅ Build
✅ Unit Tests
✅ Lint
✅ Security Scan

Or:
❌ Unit Tests

Then:
PR
 │
 ├── Code review
 └── Automated checks

Don't teach GitHub Actions deeply yet.
That belongs to Phase 11.
Here you're only showing:
GitHub can enforce checks before code is merged.

18. Merge methods on GitHub
Now introduce the three common merge options.
Pull Request
     │
     ├── Merge commit
     │
     ├── Squash and merge
     │
     └── Rebase and merge

Merge commit
Preserves the PR's branch history and creates a merge commit.
Squash
Combines the PR's commits into one commit before adding it to the target branch.
Before:

A ─── B ─── C
          \
           D ─ E ─ F

After squash:

A ─── B ─── C ─── S

Rebase and merge
Replays the PR commits onto the target branch to produce a linear history.
Don't tell viewers one is universally "best."
Explain the tradeoffs and tell them teams choose policies intentionally.
19. Code review
Now explain what reviewers actually look for.
                    CODE REVIEW
                         │
       ┌─────────────────┼─────────────────┐
       ▼                 ▼                 ▼
   Correctness        Security         Maintainability
       │                 │                 │
       ▼                 ▼                 ▼
    Logic             Secrets           Structure
    Tests              Auth              Readability
    Edge cases         Input             Duplication

This makes the PR process meaningful rather than:
"Click Approve."

20. GitHub Issues
Now introduce another collaboration feature.
              GitHub Repository
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
      Pull Requests           Issues
          │                     │
     Code changes          Work tracking

Example:
Issue #102
────────────────────
Login fails after token expiry

Labels:
🐛 bug
🔴 high priority

Assigned:
Shane

Then connect it:
Issue
  │
  ▼
Branch
  │
  ▼
Pull Request
  │
  ▼
Merge

That's a powerful workflow.
21. Issues vs Pull Requests
Make this distinction clear.
Issue
 ↓
"What work/problem needs to be addressed?"

Pull Request
 ↓
"Here is my proposed code change."

Visual:
Issue
  │
  │ developer works
  ▼
Feature Branch
  │
  ▼
Pull Request
  │
  ▼
Review
  │
  ▼
Merge

22. Linking Issue + PR
Show the concept:
Issue #102
   │
   ▼
Fix login expiration
   │
   ▼
PR #110
   │
   ▼
Merge
   │
   ▼
Issue closed

GitHub supports keywords such as:
Fixes #102
Closes #102
Resolves #102

This creates a clean work-tracking flow.
23. GitHub Discussions — optional
Briefly introduce:
Issues
 → specific work/problems

Discussions
 → questions, ideas, broader conversations

Don't spend much time here.
24. Releases
Connect your Phase 6 tags to GitHub.
Locally:
git tag v1.0.0

Push:
git push origin v1.0.0

GitHub can then present a release around that version.
Visual:
Commit
   │
   ▼
Tag v1.0.0
   │
   ▼
GitHub Release
   │
   ├── Release notes
   ├── Changes
   └── Assets

This is a nice connection between Phase 6 and Phase 7.
25. Repository permissions — introduction
Don't go deeply into organizations yet.
Just introduce the basic idea:
Repository
    │
    ├── Who can see it?
    ├── Who can contribute?
    ├── Who can review?
    └── Who can administer?

You can explain that GitHub has permission levels and organization/team controls, which we'll cover in Phase 8.
26. Branch protection — introduction
This is also a preview.
Imagine:
main
 │
 🚧 Protected
 │
 ├── Direct push restricted
 ├── PR required
 ├── Review required
 └── Checks required

Developer:
❌ git push origin main

Instead:
feature
   ↓
Pull Request
   ↓
Review
   ↓
Checks
   ↓
main

Don't configure all the rules yet.
That belongs in the real team workflow phase.
27. The complete team workflow
Now bring everything together.
                         GitHub
                           │
                         main
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
       Alice             Bob             Shane
          │                │                │
    feature/login    feature/payment  feature/dashboard
          │                │                │
       commits          commits          commits
          │                │                │
          ▼                ▼                ▼
         Push             Push             Push
          │                │                │
          └────────────────┼────────────────┘
                           ▼
                    Pull Requests
                           │
                           ▼
                      Code Review
                           │
                           ▼
                    Automated Checks
                           │
                           ▼
                         Merge
                           │
                           ▼
                          main

This is the core diagram of Phase 7.
28. The "real developer day" demo
I strongly recommend making this the main practical demonstration.
Alice
git switch -c feature/login

Works:
login.js

Then:
git add .
git commit -m "Add login"
git push -u origin feature/login

Creates PR.
Bob
git switch -c feature/payment

Works:
payment.js

Then:
git add .
git commit -m "Add payment"
git push -u origin feature/payment

Creates PR.
Shane
git switch -c feature/dashboard

Works:
dashboard.js

Then:
git add .
git commit -m "Add dashboard"
git push -u origin feature/dashboard

Creates PR.
Now GitHub:
                    main
                      │
             ┌────────┼────────┐
             ▼        ▼        ▼
           Login    Payment  Dashboard
             │        │        │
             ▼        ▼        ▼
            PR       PR       PR
             │        │        │
             └────────┼────────┘
                      ▼
                  Code Review
                      │
                      ▼
                     main

This single demo teaches far more than 30 isolated commands.
29. The complete GitHub mental model
End Phase 7 with:
                    👨‍💻 Developer
                          │
                          ▼
                    Local Git Repo
                          │
                    git push
                          │
                          ▼
                     ☁️ GitHub
                          │
             ┌────────────┼────────────┐
             ▼            ▼            ▼
         Repository      Issues       Actions
             │
             ▼
          Branches
             │
             ▼
       Pull Request
             │
       ┌─────┴─────┐
       ▼           ▼
   Code Review   Checks
       │           │
       └─────┬─────┘
             ▼
           Merge
             │
             ▼
            main

🎬 Phase 7 Video Flow
00:00  🌐 Why GitHub?
          ↓
02:00  Local Git vs GitHub
          ↓
05:00  Create GitHub repository
          ↓
08:00  Connect local repo to GitHub
          ↓
10:00  origin
          ↓
12:00  git push
          ↓
14:00  git clone
          ↓
16:00  git fetch vs git pull
          ↓
19:00  Remote branches
          ↓
22:00  Local vs remote-tracking branches
          ↓
25:00  👥 Multiple developers
          ↓
28:00  Pull Request
          ↓
31:00  PR lifecycle
          ↓
34:00  Code Review
          ↓
37:00  PR checks
          ↓
39:00  Merge options
          ↓
42:00  GitHub Issues
          ↓
44:00  Issue → Branch → PR → Merge
          ↓
47:00  Releases
          ↓
49:00  Branch protection preview
          ↓
51:00  Complete team workflow
          ↓
54:00  Practice exercise
          ↓
57:00  Summary

📄 Phase 7 README
# Phase 7 — GitHub Collaboration

## 🎯 Goal

Understand how Git and GitHub work together
for real-world collaboration.

## ☁️ What Is a GitHub Repository?

## 💻 Local vs Remote Repository

## 🔗 Git Remotes

### origin

## ⬆️ git push

## ⬇️ git pull

## 📥 git fetch

## 🌿 Remote Branches

## 🔄 Local vs Remote-Tracking Branches

## 👥 Multiple Developers

## 🔀 Pull Requests

### Creating a PR
### PR Description
### Code Review
### Review Comments
### Requested Changes
### Approval
### Merge

## 🔍 PR Checks

## 🔀 Merge Options

### Merge Commit
### Squash and Merge
### Rebase and Merge

## 🐛 GitHub Issues

## 🔗 Issue → Branch → PR → Merge

## 🏷️ GitHub Releases

## 🔐 Branch Protection — Introduction

## 🧠 Real Team Workflow

## 🧪 Practice Project

## 🚨 Common Mistakes

## 📌 Key Takeaways

🧪 Phase 7 Practice Project
This time, make the exercise genuinely collaborative even if the viewer is alone.
Create:
GitHub Repository
└── ecommerce-demo

Simulate three developers:
👨‍💻 Alice
feature/login

👨‍💻 Bob
feature/payment

👨‍💻 Shane
feature/dashboard

Each branch should:
Create branch
     ↓
Make 2 commits
     ↓
Push branch
     ↓
Open PR
     ↓
Review PR
     ↓
Make requested change
     ↓
Approve
     ↓
Merge

Then check:
git fetch --all
git branch -a
git log --oneline --graph --all

The viewer should be able to explain:
Local branch
      ↓
Remote branch
      ↓
Pull Request
      ↓
Review
      ↓
Checks
      ↓
Merge
      ↓
main

🧠 Five things viewers must remember
1️⃣ Git
   → Version control.

2️⃣ GitHub
   → Collaboration platform built around Git.

3️⃣ Remote
   → Shared repository location.

4️⃣ Pull Request
   → Proposal + discussion + review + checks
     around a code change.

5️⃣ Team workflow
   → Branch → Commit → Push → PR → Review → Merge.

And now the course has reached an important transition:
PHASE 1
Why Git & GitHub
       ↓
PHASE 2
Git Fundamentals
       ↓
PHASE 3
History & Recovery
       ↓
PHASE 4
Branching
       ↓
PHASE 5
Merge / Rebase / Conflicts
       ↓
PHASE 6
Advanced Local Workflow
       ↓
PHASE 7
🌐 GitHub Collaboration
       │
       ▼
   "I can work with GitHub."
       │
       ▼
PHASE 8
🏢 Real Team Git Workflow
       │
       ├── Organization
       ├── Teams
       ├── Permissions
       ├── Branch Protection
       ├── Release Strategy
       ├── Feature / Release / Hotfix
       └── DEV → STAGING → PROD

Phase 8 is where we move from “GitHub user” to “developer working inside a real company's GitHub environment.” That's where the organization/team model, permissions, protected branches, Git Flow vs trunk-based development, release/hotfix workflows, and a realistic 3–5 developer simulation should come together.
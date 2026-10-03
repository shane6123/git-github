Phase 8 — Real Team Git Workflow 🏢👥
This is where the course moves from:
“I know Git and GitHub.”

to:
“I understand how a real engineering team manages code.”

Phase 7 showed:
Developer
   ↓
Branch
   ↓
Commit
   ↓
Push
   ↓
Pull Request
   ↓
Review
   ↓
Merge

Phase 8 expands this into an actual company environment:
                         🏢 COMPANY
                             │
                        GitHub Organization
                             │
             ┌───────────────┼───────────────┐
             ▼               ▼               ▼
          Frontend         Backend          DevOps
             │               │               │
          Developers      Developers      Engineers
             │               │               │
             └───────────────┼───────────────┘
                             ▼
                       Repositories
                             │
                       Branch Strategy
                             │
             ┌───────────────┼───────────────┐
             ▼               ▼               ▼
           Feature         Release          Hotfix
             │               │               │
             └───────────────┼───────────────┘
                             ▼
                       Pull Requests
                             │
                       Code Review
                             │
                       CI / Checks
                             │
                             ▼
                           main

🎯 Phase 8 Goal
By the end of this phase, viewers should understand:
GitHub Organization
        ↓
Teams
        ↓
Repository permissions
        ↓
Branch strategy
        ↓
Feature development
        ↓
Pull Request
        ↓
Code review
        ↓
Protected branches
        ↓
Release
        ↓
Hotfix
        ↓
Production

And most importantly:
How 5–20 developers can work on the same product without randomly pushing code into production.

1. Start with a real company scenario
Imagine our company:
                    🏢 ACME SOFTWARE
                           │
                           ▼
                  GitHub Organization
                           │
             ┌─────────────┼─────────────┐
             ▼             ▼             ▼
          Frontend       Backend        DevOps
             │             │             │
          React Team     Node Team      Platform

Repositories:
ACME Organization
│
├── ecommerce-frontend
├── ecommerce-api
├── payment-service
├── notification-service
└── infrastructure

This immediately introduces the difference between:
Personal GitHub account
        vs
Company GitHub Organization

2. GitHub Organization
Explain:
An organization provides a shared space for repositories, teams, permissions, and company-level collaboration.

Visual:
                 GitHub Organization
                        🏢
                         │
        ┌────────────────┼────────────────┐
        │                │                │
        ▼                ▼                ▼
     Backend          Frontend          DevOps
      Team              Team             Team
        │                │                │
        ▼                ▼                ▼
     APIs             React            Infra

This is much closer to how companies actually organize GitHub.
3. Teams
Don't give repository access individually to every person if the organization can manage access through teams.
Example:
Organization
     │
     ├── Backend Team
     │     ├── Alice
     │     ├── Bob
     │     └── Shane
     │
     ├── Frontend Team
     │     ├── Charlie
     │     └── David
     │
     └── DevOps Team
           ├── Eve
           └── Frank

Then:
Backend Team
      │
      ▼
backend-api repository

The key idea:
Teams make access management scalable.

4. Repository permissions
Introduce the permission model.
Conceptually:
                    Repository
                        │
       ┌────────────────┼────────────────┐
       ▼                ▼                ▼
     Read             Write             Admin
       │                │                │
    View code       Push/work        Manage settings

GitHub has more granular repository roles as well.
For your video, explain the principle first:
Permission
    ↓
What can this person/team do?

Don't turn this section into memorizing every permission level.
5. Why permissions matter
Imagine:
Production Repository
        │
        ▼
20 developers
        │
        ├── Everyone can push directly ❌
        │
        └── Anyone can modify settings ❌

That's risky.
Instead:
Production Repository
        │
        ▼
Protected main
        │
        ├── PR required
        ├── Review required
        └── Checks required

Now the workflow becomes:
Developer
   ↓
Feature branch
   ↓
PR
   ↓
Review
   ↓
Checks
   ↓
main

6. Branch protection
This is one of the most important sections of Phase 8.
Imagine:
                       main
                        🔐
                         │
                ┌────────┼────────┐
                │        │        │
             PR only   Review    Tests

You can configure policies such as:
- Require Pull Requests
- Require approvals
- Require status checks
- Restrict direct pushes
- Require conversation resolution
- Control who can push
- Require signed commits in appropriate workflows
The exact available options can change over time, so in the README you can link viewers to the current GitHub documentation rather than hard-coding every UI detail.
7. The golden rule
Give viewers this mental model:
              main
               🔐
               │
          ❌ Direct Push
               │
               ▼
        Pull Request
               │
       ┌───────┴───────┐
       ▼               ▼
   Code Review      Automated Checks
       │               │
       └───────┬───────┘
               ▼
             Merge
               │
               ▼
              main

Then say:
The main branch represents shared, trusted code—not your personal workspace.

This is a useful team principle without pretending every organization uses the exact same policy.
8. Branching strategies
Now introduce the two major approaches.
              Branch Strategy
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
      Git Flow           Trunk-Based

Don't declare one universally better.
Explain the difference.
9. Git Flow
Classic model:
                    main
                     │
                     │
                  release
                     │
                     ▼
                  develop
                     │
          ┌──────────┼──────────┐
          ▼          ▼          ▼
       feature     feature     feature
          │          │          │
          └──────────┼──────────┘
                     ▼
                  develop
                     │
                     ▼
                  release
                     │
                     ▼
                   main

Common branch types:
main
develop
feature/*
release/*
hotfix/*

Explain the intent of each.
Feature
feature/login
feature/payment

Release
release/1.4.0

Hotfix
hotfix/payment-failure

10. Trunk-based development
Now contrast it.
                    main
                     │
          ┌──────────┼──────────┐
          │          │          │
      short-lived short-lived short-lived
       branch      branch      branch
          │          │          │
          ▼          ▼          ▼
         PR         PR         PR
          │          │          │
          └──────────┼──────────┘
                     ▼
                    main

The central idea:
Developers integrate small changes into the main line frequently, often using short-lived branches.

Feature flags can also be used when functionality should be merged before it is exposed to users.
11. Git Flow vs Trunk-Based
Use this table:
Git Flow	Trunk-Based
More long-lived branches	Short-lived branches
Explicit develop/release model	Main/trunk is central
Can suit release-oriented workflows	Often emphasizes frequent integration
More branch management	Simpler branch structure
Release branches can be useful	Feature flags may handle incomplete features


Don't say:
"Trunk-based is better."

Instead:
"Different teams choose different strategies based on release process, team size, deployment model, and product requirements."

12. Real developer workflow
Now simulate your company.
                   main 🔐
                     │
                     ▼
                Jira / Issue
                     │
                     ▼
              Create branch
                     │
                     ▼
             feature/payment
                     │
             ┌───────┴───────┐
             ▼               ▼
          Commit 1        Commit 2
             │               │
             └───────┬───────┘
                     ▼
                   Push
                     │
                     ▼
                    PR
                     │
             ┌───────┴────────┐
             ▼                ▼
          Reviewer          CI
             │                │
             └───────┬────────┘
                     ▼
                  Approval
                     │
                     ▼
                   Merge
                     │
                     ▼
                   main

This should be the main practical demonstration.
13. Feature Branch
Example:
git switch main
git pull

git switch -c feature/payment

Work:
payment.js
payment.service.js
payment.test.js

Then:
git add .
git commit -m "Add payment processing"

Push:
git push -u origin feature/payment

Then:
GitHub
   ↓
Create PR

14. Pull Request review loop
Now make the process realistic.
PR Created
    │
    ▼
Reviewer comments
    │
    ▼
"Please add validation"
    │
    ▼
Developer updates code
    │
    ▼
git commit
    │
    ▼
git push
    │
    ▼
PR automatically updates
    │
    ▼
Reviewer approves
    │
    ▼
Merge

This is what viewers should understand:
A Pull Request is a living collaboration process, not a one-time form submission.

15. Code ownership
Introduce another professional concept.
Imagine:
Repository
    │
    ├── authentication/
    ├── payments/
    ├── users/
    └── infrastructure/

Different teams may own different areas.
authentication → Backend Team
payments       → Payment Team
infrastructure → DevOps

GitHub can support code-review ownership workflows through mechanisms such as CODEOWNERS.
Conceptually:
Changed:
payments/*
     │
     ▼
Payment Team review

This introduces a scalable code-review model.
16. CODEOWNERS
Example:
# CODEOWNERS

/payments/       @company/payment-team
/infrastructure/ @company/devops
/frontend/       @company/frontend

Then:
PR modifies payments/
          │
          ▼
Payment team requested/recommended
for review according to repository settings

Don't spend too long on syntax; the concept matters more here.
17. Release branches
Now imagine:
main
 │
 ├── feature/A
 ├── feature/B
 └── feature/C

Product says:
"Version 2.0 is ready for release."

Depending on the team's chosen strategy:
main
 │
 ▼
release/2.0.0

Then:
Testing
Bug fixes
Final validation
     │
     ▼
Production

And:
v2.0.0

tag marks the release.
18. Hotfix workflow
Now show the most important production scenario.
Production:
main
 │
 ▼
v2.0.0
 │
 💥 Payment broken

Don't create a random branch from whatever someone happens to have locally.
Conceptually:
main
 │
 ├── hotfix/payment-failure
 │         │
 │       fix
 │         │
 │         ▼
 │        PR
 │         │
 │         ▼
 └─────── main
            │
            ▼
         v2.0.1

This is where viewers see how Git supports production maintenance.
19. DEV → STAGING → PROD
Now introduce environments.
                    GitHub
                       │
                       ▼
                    main
                       │
                       ▼
                  CI Pipeline
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
         DEV         STAGING       PROD

Or, depending on the organization's strategy:
feature
   ↓
PR
   ↓
develop
   ↓
DEV
   ↓
staging
   ↓
main/release
   ↓
PROD

Important:
There isn't one universal branch-to-environment mapping. Teams design this according to their deployment strategy.

This sets up Phase 11 later when you teach actual CI/CD.
20. Why environments matter
Imagine:
Developer code
      │
      ▼
DEV
 │
 ├── Test
 ├── Integration
 └── Debug
      │
      ▼
STAGING
 │
 ├── QA
 └── Final validation
      │
      ▼
PRODUCTION

This creates a controlled path from development to users.
21. Branch naming convention
Now establish a professional convention.
feature/*
bugfix/*
hotfix/*
release/*
chore/*
refactor/*

Examples:
feature/google-login
feature/payment-gateway

bugfix/token-expiration
bugfix/user-validation

hotfix/payment-production

release/2.1.0

chore/update-node

The exact naming convention should be agreed by the team; the important point is consistency.
22. Commit conventions
Introduce this lightly.
Bad:
fix
update
changes
final
done

Better:
Add payment validation
Fix token expiration
Update authentication middleware
Add payment unit tests

You can optionally introduce Conventional Commits:
feat: add payment gateway
fix: resolve token expiration
docs: update API documentation
test: add payment tests
refactor: simplify auth middleware
chore: update dependencies

This becomes useful later for automation and release tooling.
23. Commit vs Pull Request
Another important distinction:
Commit
 ↓
Developer-level unit of change

while:
Pull Request
 ↓
Team-level collaboration/review unit

Visual:
Developer
   │
   ├── Commit
   ├── Commit
   └── Commit
         │
         ▼
       PR
         │
         ▼
     Team Review

24. Branch protection + CODEOWNERS + CI
Now combine the professional controls:
                    main 🔐
                      │
             ┌────────┼─────────┐
             │        │         │
          PR only  CODEOWNERS   CI
             │        │         │
             ▼        ▼         ▼
          Reviewer  Required   Tests
             │        Review    Build
             └────────┼─────────┘
                      ▼
                    Merge

This is the foundation of a mature GitHub workflow.
25. What happens when two developers touch the same code?
You already taught conflicts in Phase 5.
Now put it into the team context:
Alice
feature/login
     │
     ▼
     PR
     │
     └──────┐
            │
Bob         │
feature/auth│
     │      │
     ▼      │
     PR     │
     │      │
     └──┬───┘
        ▼
     Conflict
        │
        ▼
    Resolve
        │
        ▼
     Review
        │
        ▼
      Merge

This shows that conflicts aren't exceptional Git magic—they're a normal part of collaborative development.
26. Complete company workflow
This is the hero diagram for Phase 8:
                         🏢 COMPANY
                              │
                              ▼
                     GitHub Organization
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
           Frontend         Backend          DevOps
              │               │               │
              └───────────────┼───────────────┘
                              ▼
                         Repository
                              │
                         main 🔐
                              │
                 ┌────────────┼────────────┐
                 ▼            ▼            ▼
              Feature       Feature      Hotfix
                 │            │            │
                 ▼            ▼            ▼
               Push         Push         Push
                 │            │            │
                 └────────────┼────────────┘
                              ▼
                            PRs
                              │
                 ┌────────────┼────────────┐
                 ▼            ▼            ▼
              Review       CODEOWNERS      CI
                 │            │            │
                 └────────────┼────────────┘
                              ▼
                           Approval
                              │
                              ▼
                            Merge
                              │
                              ▼
                            Release
                              │
                              ▼
                     DEV → STAGING → PROD
                              │
                              ▼
                           v2.0.0

27. Phase 8 practical simulation
For your YouTube demo, I would create one fictional company.
Company
🏢 DevShop

Teams
Frontend
Backend
DevOps
QA

Developers
Alice → Frontend
Bob   → Backend
Shane → Backend
Eve   → DevOps

Repository
ecommerce-api

Workflow
Issue #101
"Add payment gateway"
       │
       ▼
Shane creates:
feature/payment-gateway
       │
       ▼
2–3 commits
       │
       ▼
Push
       │
       ▼
PR #120
       │
       ├── CI
       ├── CODEOWNERS
       └── Review
       │
       ▼
Requested changes
       │
       ▼
Shane updates
       │
       ▼
Approval
       │
       ▼
Merge
       │
       ▼
Deployment

This single story can demonstrate nearly the entire phase.
28. Phase 8 command set
The commands aren't the main focus anymore, but viewers should see the practical workflow.
# Update main
git switch main
git pull

# Create feature
git switch -c feature/payment

# Work
git add .
git commit -m "feat: add payment gateway"

# Push branch
git push -u origin feature/payment

# Update from remote
git fetch origin

# Inspect branches
git branch -a

# Update feature branch if required
git switch feature/payment
git rebase origin/main

Then the rest happens on GitHub:
Push
 ↓
Pull Request
 ↓
Review
 ↓
Checks
 ↓
Approval
 ↓
Merge

29. What NOT to overload Phase 8 with
Keep these for later:
❌ Personal Access Tokens
❌ SSH deep dive
❌ GitHub Secrets
❌ Environment secrets
❌ GitHub Actions YAML
❌ CI/CD implementation
❌ Deployment implementation
❌ OIDC/cloud credentials

Those deserve dedicated phases.
Phase 8 should answer:
How does an engineering organization structure and control collaboration?

🎬 Phase 8 Video Flow
00:00  🏢 From Developer → Engineering Team
          ↓
02:00  Company GitHub Organization
          ↓
05:00  Teams
          ↓
08:00  Repository permissions
          ↓
11:00  Why direct push to main is dangerous
          ↓
14:00  Branch protection
          ↓
17:00  Git Flow
          ↓
22:00  Trunk-Based Development
          ↓
27:00  Git Flow vs Trunk-Based
          ↓
30:00  Real feature workflow
          ↓
34:00  Pull Request review loop
          ↓
37:00  CODEOWNERS
          ↓
40:00  Release branches
          ↓
43:00  Hotfix workflow
          ↓
46:00  DEV → STAGING → PROD
          ↓
49:00  Commit conventions
          ↓
51:00  Complete company workflow
          ↓
55:00  Real-world simulation
          ↓
60:00  Summary

📄 Phase 8 README
# Phase 8 — Real Team Git Workflow

## 🎯 Goal

Understand how GitHub is used inside
a real engineering organization.

## 🏢 GitHub Organization

## 👥 Teams

## 🔐 Repository Permissions

## 🛡️ Branch Protection

## 🌿 Branching Strategies

### Git Flow

### Trunk-Based Development

## 🔀 Feature Development

## 🔎 Pull Request Review

## 👑 CODEOWNERS

## 🏷️ Release Branches

## 🚨 Hotfix Workflow

## 🌎 DEV / STAGING / PROD

## 📝 Commit Conventions

## 🔄 Complete Team Workflow

## 🧪 Practice Simulation

## 🚨 Common Mistakes

## 📌 Key Takeaways

🧪 Phase 8 Challenge
Give viewers a mini-company simulation.
🏢 DevShop

Teams:
├── Frontend
├── Backend
├── DevOps
└── QA

Repository:
└── ecommerce-api

Set:
main = protected

Then simulate:
Issue
 ↓
Feature Branch
 ↓
2 commits
 ↓
Push
 ↓
PR
 ↓
Reviewer requests change
 ↓
Developer updates PR
 ↓
CI/checks
 ↓
Approval
 ↓
Merge

Then create a production bug:
PROD
 │
 💥 Payment failure
 │
 ▼
hotfix/payment-failure
 │
 ▼
PR
 │
 ▼
Review
 │
 ▼
Merge
 │
 ▼
v2.0.1

Finally ask the viewer to draw their own workflow:
Issue
  ↓
?
  ↓
?
  ↓
?
  ↓
Production

If they can fill that out correctly, they understand Phase 8.
🧠 Five things viewers must remember
1️⃣ Organization
   → Company-level GitHub workspace.

2️⃣ Teams
   → Group people and manage access/workflow.

3️⃣ Protected main
   → Shared trusted branch with controlled changes.

4️⃣ Branch strategy
   → Defines how developers integrate work.

5️⃣ Production workflow
   → Issue → Branch → Commit → PR → Review
     → Checks → Merge → Release → Deploy

And now the roadmap becomes:
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
GitHub Collaboration
      ↓
PHASE 8
🏢 Real Team Workflow
      │
      ▼
"I understand how teams work."
      │
      ▼
PHASE 9
🔐 GitHub Authentication & Security
      │
      ├── HTTPS
      ├── PAT
      ├── SSH
      ├── SSH Keys
      ├── Permissions
      └── Secure Git workflow

Phase 9 should now move into security: how developers authenticate to GitHub, HTTPS vs SSH, Personal Access Tokens, SSH keys, credential management, least-privilege access, and—most importantly—what happens when someone accidentally commits a secret. That sets up Phase 10 naturally: GitHub Secrets & Environment Variables.
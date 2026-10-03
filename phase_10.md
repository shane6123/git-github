Phase 10 — GitHub Secrets & Environment Variables 🔑
The progression is now:
Phase 7
GitHub Collaboration
        ↓
Phase 8
Real Team Workflow
        ↓
Phase 9
Authentication & Security
        ↓
Phase 10
🔑 Secrets & Environment Variables
        ↓
Phase 11
⚙️ GitHub Actions / CI/CD

🎯 Phase 10 Goal
By the end, viewers should understand:
Application Code
      │
      ├── Public configuration
      │
      └── Sensitive configuration
                 │
                 ▼
        Environment Variables
                 │
       ┌─────────┼─────────┐
       ▼         ▼         ▼
      DEV     STAGING      PROD
       │         │         │
       ▼         ▼         ▼
    Secrets   Secrets    Secrets

They should understand what belongs in Git and what doesn't.
1. Start with the real problem
Imagine your Node.js application needs:
Database
Stripe
JWT
AWS
Email service

A beginner might write:
const dbPassword = "my-password";
const stripeSecret = "sk_live_xxx";
const jwtSecret = "super-secret";

Then:
Code
  ↓
git add .
  ↓
git commit
  ↓
git push
  ↓
GitHub 🚨

Now your secrets are inside the repository.
2. The correct mental model
Instead:
                    APPLICATION
                         │
                         ▼
                  Environment
                         │
              ┌──────────┼──────────┐
              ▼          ▼          ▼
             DEV      STAGING      PROD
              │          │          │
              ▼          ▼          ▼
          DB_URL      DB_URL       DB_URL
          API_KEY     API_KEY      API_KEY
          JWT_SECRET  JWT_SECRET   JWT_SECRET

Your application asks for:
process.env.DB_URL

not:
const DB_URL = "actual-secret";

3. Configuration vs Secrets
This distinction is extremely important.
Not every environment variable is necessarily a secret.
Configuration
PORT=3000
NODE_ENV=development
API_BASE_URL=https://api.example.com

Sensitive values
DATABASE_PASSWORD=********
JWT_SECRET=********
STRIPE_SECRET_KEY=********
AWS_SECRET_ACCESS_KEY=********

Mental model:
Environment Variables
       │
       ├── Configuration
       │
       └── Secrets

So don't teach:
"All environment variables are secrets."

Instead:
Secrets are sensitive values that require restricted handling; environment variables are one mechanism for providing configuration and secrets to applications.

4. .env file
For local development:
ecommerce-api/
│
├── src/
├── package.json
├── .gitignore
└── .env

Example:
NODE_ENV=development
PORT=5000

DATABASE_URL=mongodb://localhost:27017/ecommerce

JWT_SECRET=local-development-secret
STRIPE_SECRET_KEY=local-test-key

Application:
const port = process.env.PORT;
const database = process.env.DATABASE_URL;

5. Why .env shouldn't be committed
Your project:
.env
   │
   ├── DATABASE_URL
   ├── JWT_SECRET
   └── STRIPE_SECRET_KEY

If committed:
Developer
   ↓
Git
   ↓
GitHub
   ↓
Anyone with sufficient repository access

Instead:
.env
  ↓
.gitignore
  ↓
❌ Not committed

Add:
.env
.env.*
!.env.example

Be careful with the exact pattern because you want an example file to remain tracked.
6. .env.example
This is a very useful professional practice.
Instead of committing:
DATABASE_URL=actual-production-password
JWT_SECRET=actual-secret

commit:
DATABASE_URL=
JWT_SECRET=
STRIPE_SECRET_KEY=

Or:
DATABASE_URL=your-database-url
JWT_SECRET=your-jwt-secret
STRIPE_SECRET_KEY=your-stripe-secret

File:
.env.example

Mental model:
.env.example
      │
      ▼
"What configuration does this application need?"

while:
.env
      │
      ▼
"What are my actual local values?"

7. .env vs .env.example
.env	.env.example
Actual values	Placeholder values
Usually ignored	Usually committed
Local/private	Documentation
Contains secrets	Should contain no real secrets


Visual:
             Configuration
                  │
          ┌───────┴────────┐
          ▼                ▼
       .env            .env.example
          │                │
     Actual values     Placeholders
          │                │
        ❌ Git             ✅ Git

8. Never hardcode secrets
Bad:
const config = {
  dbPassword: "secret123",
  jwtSecret: "abc123"
};

Better:
const config = {
  dbPassword: process.env.DB_PASSWORD,
  jwtSecret: process.env.JWT_SECRET
};

Best depends on environment:
Local
 ↓
.env

CI/CD
 ↓
GitHub Secrets

Cloud
 ↓
Cloud Secret Manager / secure environment configuration

9. DEV / STAGING / PROD
Now introduce the most important environment concept.
Suppose we have:
DEV
STAGING
PROD

They should not necessarily use the same credentials.
DEV
DATABASE_URL → dev database

STAGING
DATABASE_URL → staging database

PROD
DATABASE_URL → production database

Visual:
                 APPLICATION
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
         DEV       STAGING       PROD
          │           │           │
       DB_DEV     DB_STAGING     DB_PROD

This prevents a developer's local environment from accidentally connecting to production.
10. Environment-specific configuration
Example:
DEV
NODE_ENV=development
DATABASE_URL=mongodb://dev-db/ecommerce
API_URL=https://dev-api.example.com

STAGING
NODE_ENV=staging
DATABASE_URL=mongodb://staging-db/ecommerce
API_URL=https://staging-api.example.com

PROD
NODE_ENV=production
DATABASE_URL=mongodb://prod-db/ecommerce
API_URL=https://api.example.com

Notice:
Same application
       │
       ├── DEV configuration
       ├── STAGING configuration
       └── PROD configuration

11. GitHub Secrets
Now introduce GitHub's secure storage for secrets used by GitHub workflows.
Mental model:
                    GitHub
                       │
              ┌────────┴────────┐
              ▼                 ▼
         Repository          Secrets
                               │
                ┌──────────────┼──────────────┐
                ▼              ▼              ▼
             DEV_SECRET    STAGING_SECRET   PROD_SECRET

The important idea:
The secret value isn't stored in your source code.

Later, GitHub Actions can consume these secrets during a workflow.
12. Repository secrets vs Environment secrets
This is an important professional distinction.
Conceptually:
GitHub Repository
       │
       ├── Repository secrets
       │
       └── Environments
              │
              ├── DEV
              ├── STAGING
              └── PROD
                    │
                    └── Environment secrets

Why does this matter?
Because production credentials should not automatically be available to every workflow or environment.
13. Environment protection
Now connect Phase 8.
Imagine:
              GitHub
                │
                ▼
            Repository
                │
          ┌─────┴─────┐
          ▼           ▼
         DEV         PROD
                      🔐

Production can have additional controls such as:
Approval
Restrictions
Environment-specific secrets
Deployment protection

This gives you:
DEV
 ↓
automatic

STAGING
 ↓
testing

PROD
 ↓
controlled deployment

The exact GitHub configuration should be demonstrated against the current GitHub UI/documentation because GitHub's environment and rules features evolve.
14. The biggest mistake: one secret everywhere
Bad:
DEV
   │
   └── PROD_DATABASE_PASSWORD

STAGING
   │
   └── PROD_DATABASE_PASSWORD

PROD
   │
   └── PROD_DATABASE_PASSWORD

Now if DEV is compromised:
DEV
 ↓
Production credential
 ↓
🚨

Better:
DEV
 └── DEV credentials

STAGING
 └── STAGING credentials

PROD
 └── PROD credentials

This is another application of:
Least privilege.

15. GitHub Secrets in a workflow
Don't go deeply into Actions yet, but show the future connection.
Imagine:
- name: Deploy
  env:
    DATABASE_URL: ${{ secrets.DATABASE_URL }}
  run: npm run deploy

Mental model:
GitHub Secret
      │
      ▼
GitHub Actions
      │
      ▼
Environment Variable
      │
      ▼
Application / deployment

Don't teach the entire YAML syntax here.
That belongs in Phase 11.
16. Secrets should not appear in logs
Another critical concept.
Bad:
Deploying with:
DATABASE_PASSWORD=abc123

🚨
Logs can be viewed by people or systems that shouldn't see credentials.
Instead:
Deploying application...
Database connection configured.

Mental model:
Secret
  ↓
Workflow
  ↓
Application
  ↓
❌ Don't print

17. Secret masking
GitHub Actions can mask supported secret values in logs, but teach the broader principle:
Never deliberately print secrets to logs, even if your CI system offers masking.

For example, don't do:
echo $DATABASE_PASSWORD

This is an excellent real-world mistake to demonstrate with a fake value.
18. Public vs private repository
Another misconception:
"If my repository is private, it's okay to commit secrets."

No.
Private repository
      │
      ▼
Still has humans
Still has integrations
Still has CI
Still has logs
Still has backups/history

Therefore:
Private repo ≠ Secret manager

Very important sentence for the video.
19. Secret rotation
Suppose:
STRIPE_SECRET_KEY
       │
       ▼
Compromised

Don't simply change the code.
Correct conceptual workflow:
Old secret
    │
    ▼
Revoke
    │
    ▼
Generate new secret
    │
    ▼
Update secure storage
    │
    ▼
Deploy
    │
    ▼
Verify

20. Secret rotation across environments
Now make it realistic.
DEV
 │
 └── DEV_STRIPE_KEY

STAGING
 │
 └── STAGING_STRIPE_KEY

PROD
 │
 └── PROD_STRIPE_KEY

If only PROD is compromised:
PROD secret
    ↓
Rotate PROD

You don't necessarily need to rotate unrelated credentials.
This is another benefit of environment separation.
21. Secret management hierarchy
Give viewers this mental model:
                  Sensitive Data
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
        Local         CI/CD       Cloud
          │            │            │
         .env       GitHub       Secret Manager
                    Secrets

Examples:
Local development
→ .env

GitHub Actions
→ GitHub Secrets

Cloud application
→ AWS Secrets Manager / Parameter Store
→ Azure Key Vault
→ Google Secret Manager

You don't need to teach cloud secret managers in depth here.
Just establish the architecture.
22. Configuration hierarchy
This is a powerful mental model:
                   Application
                       │
                       ▼
                Configuration
                       │
       ┌───────────────┼───────────────┐
       ▼               ▼               ▼
      Code          Environment      Secret Store
       │               │               │
 Defaults         Non-sensitive      Sensitive
                  configuration       values

Example:
PORT=5000

might be configuration.
JWT_SECRET=...

is sensitive.
23. Real Ecommerce example
Let's use the same project from every previous phase.
ecommerce-api/
│
├── src/
│
├── package.json
├── .gitignore
├── .env
└── .env.example

Application requires:
DATABASE_URL
JWT_SECRET
STRIPE_SECRET_KEY
AWS_REGION
AWS_BUCKET

Classify them:
Variable	Sensitive?
DATABASE_URL	Depends on contents
JWT_SECRET	Yes
STRIPE_SECRET_KEY	Yes
AWS_REGION	Usually no
AWS_BUCKET	Usually not a secret


This is a nice teaching point:
Don't blindly call every configuration value a secret.

24. Local development
Developer:
Developer laptop
       │
       ▼
     .env
       │
       ▼
Application

Example:
NODE_ENV=development
PORT=5000
DATABASE_URL=mongodb://localhost:27017/ecommerce
JWT_SECRET=local-secret
STRIPE_SECRET_KEY=test-secret

And:
.env
.env.*
!.env.example

25. GitHub
Repository contains:
src/
package.json
.gitignore
.env.example

Not:
.env

GitHub secrets contain values needed by workflows.
GitHub
 │
 ├── Repository
 │
 └── Secrets
      ├── DEV
      ├── STAGING
      └── PROD

26. Deployment architecture
Now show the bridge to Phase 11:
Developer
    │
    ▼
Git Push
    │
    ▼
GitHub
    │
    ▼
GitHub Actions
    │
    ├── Code
    ├── Tests
    ├── Build
    └── Secrets
            │
            ▼
       Deployment
            │
       ┌────┼────┐
       ▼    ▼    ▼
      DEV STG   PROD

This should be the final big diagram of Phase 10.
27. A very important security distinction
Teach these three separately:
.env
GitHub Secrets
Cloud Secret Manager

.env
Local developer machine

GitHub Secrets
GitHub workflows / deployment process

Cloud Secret Manager
Runtime infrastructure

So:
Developer
   ↓
.env

CI/CD
   ↓
GitHub Secrets

Production application
   ↓
Cloud Secret Manager

This prevents viewers from thinking:
"I'll put every secret into .env forever."

28. Common mistakes
Make this section strong.
❌ Secret in source code
const password = "123456";

❌ .env committed
git add .env

❌ Real secrets inside .env.example
STRIPE_SECRET_KEY=sk_live_actual_key

❌ Same credential across environments
DEV → PROD credentials

❌ Printing secrets
echo $SECRET

❌ Sharing secrets through Slack/email
❌ Assuming private repository means secret storage
❌ Assuming .gitignore removes previously committed secrets
29. Practical exercise
This should be the main Phase 10 lab.
Step 1
Create:
ecommerce-api/
├── src/
├── .env
├── .env.example
├── .gitignore
└── package.json

Step 2
.env.example:
NODE_ENV=
PORT=
DATABASE_URL=
JWT_SECRET=
STRIPE_SECRET_KEY=

Step 3
.env:
NODE_ENV=development
PORT=5000
DATABASE_URL=mongodb://localhost:27017/ecommerce
JWT_SECRET=my-local-secret
STRIPE_SECRET_KEY=test-key

Step 4
.gitignore:
node_modules/
.env
.env.*
!.env.example

Step 5
Verify:
git status

Expected:
.env

should not be shown as an untracked file.
But:
.env.example

should be tracked.
30. Second exercise — environment separation
Create:
DEV
STAGING
PROD

Then conceptually map:
DEV
 ├── database → dev-db
 ├── stripe → test
 └── jwt → dev secret

STAGING
 ├── database → staging-db
 ├── stripe → test/staging
 └── jwt → staging secret

PROD
 ├── database → production-db
 ├── stripe → live
 └── jwt → production secret

The values should be different.
31. Third exercise — simulate a leak
Use fake credentials only.
Commit:
demo-secret-123

Then:
Remove secret
   ↓
Commit again

Show:
git log

and explain:
A ─── B
      │
      └── secret existed

Then teach:
Real secret?
     ↓
Revoke / rotate FIRST
     ↓
Then clean up

32. Phase 10 Hero Diagram
This should probably be one of your primary README visuals:
                         👨‍💻 DEVELOPER
                              │
                              ▼
                           Code
                              │
                    ┌─────────┴─────────┐
                    ▼                   ▼
              Public Config          Secrets
                    │                   │
                    │                   ▼
                    │              Secure Storage
                    │                   │
                    │        ┌──────────┼──────────┐
                    │        ▼          ▼          ▼
                    │       DEV       STAGING     PROD
                    │        │          │          │
                    │        ▼          ▼          ▼
                    │     Secrets    Secrets    Secrets
                    │
                    ▼
                  GitHub
                    │
                    ▼
              GitHub Actions
                    │
                    ▼
               Deployment
                    │
          ┌─────────┼─────────┐
          ▼         ▼         ▼
         DEV      STAGING     PROD

33. Phase 10 Complete Mental Model
The whole security architecture:
┌─────────────────────────────────────────────────────────┐
│                    DEVELOPER MACHINE                    │
│                                                         │
│   Code ──────────────── Git                             │
│    │                                                    │
│    └── .env ── local configuration/secrets              │
└─────────────────────────┬───────────────────────────────┘
                          │
                          │ git push
                          ▼
┌─────────────────────────────────────────────────────────┐
│                       GITHUB                            │
│                                                         │
│  Repository                                             │
│     │                                                   │
│     ├── Source code                                     │
│     ├── .env.example                                    │
│     └── Git history                                     │
│                                                         │
│  Secrets                                                │
│     ├── DEV                                             │
│     ├── STAGING                                         │
│     └── PROD                                            │
└─────────────────────────┬───────────────────────────────┘
                          │
                          │ CI/CD
                          ▼
┌─────────────────────────────────────────────────────────┐
│                     ENVIRONMENTS                        │
│                                                         │
│       DEV              STAGING              PROD        │
│        │                  │                  │           │
│     secrets            secrets            secrets       │
│        │                  │                  │           │
│        ▼                  ▼                  ▼           │
│    Application        Application        Application    │
└─────────────────────────────────────────────────────────┘

🎬 Phase 10 Video Flow
00:00  🔑 Why secrets matter
   ↓
03:00  Configuration vs Secrets
   ↓
06:00  Environment Variables
   ↓
09:00  .env
   ↓
12:00  .env.example
   ↓
15:00  .gitignore
   ↓
18:00  DEV / STAGING / PROD
   ↓
22:00  GitHub Secrets
   ↓
27:00  Repository vs Environment Secrets
   ↓
31:00  Environment Protection
   ↓
34:00  Secrets in CI/CD
   ↓
37:00  🚨 Secret accidentally committed
   ↓
41:00  Secret rotation
   ↓
44:00  Secret scanning
   ↓
47:00  Local vs GitHub vs Cloud secrets
   ↓
51:00  Real Ecommerce example
   ↓
55:00  Practical exercise
   ↓
60:00  Complete architecture
   ↓
63:00  Summary

📄 Phase 10 README Structure
# Phase 10 — GitHub Secrets & Environment Variables

## 🎯 Goal

## 🔐 Configuration vs Secrets

## 🌎 Environment Variables

## 📄 .env

## 📋 .env.example

## 🚫 .gitignore

## 🏗️ DEV / STAGING / PROD

## 🔑 GitHub Secrets

### Repository Secrets

### Environment Secrets

## 🔐 Environment Protection

## ⚙️ Secrets in CI/CD

## 🚨 Accidentally Committed Secrets

### Why Deleting Isn't Enough

### Revoke

### Rotate

### Clean Up

## 🛡️ Secret Scanning

## ☁️ Cloud Secret Management

### Local

### GitHub

### Cloud

## 🔄 Secret Rotation

## 🧪 Practical Exercise

## 🚨 Common Mistakes

## 📋 Security Checklist

## 📌 Key Takeaways

🧠 The 8 things viewers MUST remember
1️⃣ Configuration ≠ Secret

2️⃣ Environment variables are a mechanism
   for providing configuration.

3️⃣ .env is useful for local development.

4️⃣ .env should normally NOT be committed.

5️⃣ .env.example documents required configuration.

6️⃣ DEV / STAGING / PROD should have
   appropriately separated configuration
   and credentials.

7️⃣ GitHub Secrets keep workflow credentials
   out of source code.

8️⃣ If a real secret leaks:
   REVOKE → ROTATE → CLEAN UP → INVESTIGATE

And now the entire course is approaching the big payoff:
PHASE 1
Why Git?
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
Real Team Workflow
   ↓
PHASE 9
Authentication & Security
   ↓
PHASE 10
🔑 Secrets & Environment Variables
   │
   ▼
"I can securely manage code and configuration."
   │
   ▼
PHASE 11
⚙️ GitHub Actions
   │
   ├── Workflow
   ├── Events
   ├── Jobs
   ├── Steps
   ├── Runners
   ├── Build
   ├── Test
   ├── Secrets
   ├── Artifacts
   ├── Environments
   └── Deployment
        │
        ▼
PHASE 12
🚀 Production-Grade GitHub Workflow

Phase 11 is where the course becomes genuinely DevOps-oriented: we'll take everything built so far and create a real GitHub Actions CI/CD pipeline for the same project—push → test → build → artifact → environment → deployment, including branches, PR checks, secrets, manual approvals, and DEV/STAGING/PROD.
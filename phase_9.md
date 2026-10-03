Phase 9 — GitHub Authentication & Security 🔐
Now we're moving from:
“How does a team collaborate?”

to:
“How does GitHub know who I am, what I'm allowed to do, and how do I keep credentials/secrets safe?”

This phase is important because beginners often think:
git push
   ↓
GitHub

But the real question is:
git push
   ↓
GitHub asks:
"Who are you?"
"How do I verify you?"
"What are you allowed to access?"

🎯 Phase 9 Goal
By the end, viewers should understand:
Developer
    │
    ▼
Git Client
    │
    ├── HTTPS
    │     ↓
    │   Token / Credential
    │
    └── SSH
          ↓
       SSH Key
          │
          ▼
       GitHub
          │
          ▼
    Authentication
          │
          ▼
     Authorization
          │
          ▼
      Repository

And the most important security principle:
Authentication answers “Who are you?” Authorization answers “What are you allowed to do?”

1. Start with the real problem
Imagine Shane runs:
git push origin main

GitHub cannot simply trust:
"Shane says he's Shane."

It needs proof.
So:
                         git push
                            │
                            ▼
                       GitHub
                            │
                  ┌─────────┴─────────┐
                  │                   │
             Authenticate        Authorize
                  │                   │
              "Who are you?"     "What can you do?"

This distinction should become unforgettable.
2. Authentication vs Authorization
Authentication
WHO ARE YOU?

Examples:
Password
Token
SSH key
Identity provider

Authorization
WHAT ARE YOU ALLOWED TO DO?

Examples:
Read repository
Write repository
Create PR
Manage repository
Manage organization

Visual:
                 Developer
                     │
                     ▼
             Authentication
                     │
               "Shane?"
                     │
                     ▼
               Authorization
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
       Read         Write        Admin

3. GitHub Account vs Git Identity
This is a very important beginner confusion.
There are two different concepts:
Git identity
     vs
GitHub identity

Git configuration:
git config --global user.name "Shane Alam"
git config --global user.email "you@example.com"

This identifies who authored the commit.
It does not automatically authenticate you to GitHub.
Mental model:
Commit
  │
  └── user.name / user.email
          ↓
       Author info


git push
  │
  └── authentication
          ↓
        GitHub

This distinction is extremely useful.
4. Git authentication methods
For your course, introduce the two major developer workflows:
GitHub Authentication
        │
        ├──────────────┐
        ▼              ▼
      HTTPS           SSH
        │              │
      Token         SSH Key

You can then explain that GitHub also supports organization-level identity and other authentication mechanisms, but HTTPS and SSH are the practical Git transport methods viewers will encounter.
5. HTTPS Authentication
The remote might look like:
https://github.com/company/ecommerce-api.git

Workflow:
Developer
    │
    ▼
git push
    │
    ▼
HTTPS
    │
    ▼
GitHub
    │
    ▼
Credential / Token
    │
    ▼
Authentication

Historically, GitHub supported password authentication for Git operations over HTTPS, but GitHub removed password-based Git authentication.
So don't teach:
Username + GitHub password

as the modern workflow.
Instead:
HTTPS
  ↓
Personal Access Token / credential manager

6. Personal Access Token — PAT
A PAT is essentially a credential that can be used to authenticate GitHub API/Git operations depending on its type and permissions.
Mental model:
GitHub account
      │
      ▼
Create token
      │
      ▼
Token = credential
      │
      ▼
Git operation
      │
      ▼
GitHub

But immediately teach:
A PAT is a secret. Treat it like a password.

Never:
❌ Put token in GitHub repository
❌ Put token in README
❌ Send token in Slack
❌ Commit token into source code
❌ Upload token to screenshots

7. Fine-grained tokens
This is where you introduce modern GitHub security.
Instead of:
Token
   ↓
Everything

Think:
Token
 │
 ├── Repository access
 ├── Expiration
 └── Specific permissions

Example mental model:
PAT
 │
 ├── Repo A
 │
 ├── Read-only
 │
 └── Expires in 30 days

This demonstrates the principle of:
Least privilege.

Give a credential only the access it actually needs.
8. Token lifecycle
Don't just teach creation.
Teach the complete lifecycle:
Create
  ↓
Use
  ↓
Rotate
  ↓
Revoke

More realistically:
Create token
     │
     ▼
Store securely
     │
     ▼
Use
     │
     ├── Compromised? ──→ Revoke immediately
     │
     ▼
Rotate / expire

Important message:
A credential should have a lifecycle, not live forever.

9. SSH Authentication
Now introduce the other major workflow.
Instead of:
HTTPS
 ↓
Token

you can use:
SSH
 ↓
SSH key pair

The fundamental idea:
Your computer
     │
     ├── Private key 🔐
     │
     └── Public key 🔓
                │
                ▼
             GitHub

10. Public key vs Private key
This needs a strong mental model.
              SSH Key Pair

        ┌─────────────────────┐
        │                     │
        ▼                     ▼
   Private Key           Public Key
      🔐                      🔓
   Your machine             GitHub

Private key
KEEP SECRET

Never upload it.
Public key
Can be shared with GitHub

GitHub stores the public key associated with your account.
11. SSH Authentication — Mental Model
                 Your Mac
                    │
             Private Key 🔐
                    │
                    ▼
                 SSH
                    │
                    ▼
                 GitHub
                    │
              Public Key 🔓
                    │
                    ▼
              Verification
                    │
                    ▼
                 Access

The private key doesn't get uploaded to GitHub.
That's an important point to emphasize.
12. Generate an SSH key
On macOS/Linux, a common modern command is:
ssh-keygen -t ed25519 -C "you@example.com"

Then:
~/.ssh/
   │
   ├── id_ed25519
   └── id_ed25519.pub

Explain:
id_ed25519
      ↓
PRIVATE KEY 🔐

id_ed25519.pub
      ↓
PUBLIC KEY

Never confuse these.
13. SSH Agent
You can introduce the SSH agent conceptually:
Private key
     ↓
ssh-agent
     ↓
SSH authentication
     ↓
GitHub

The purpose is to help manage/use private keys without repeatedly exposing or manually handling them.
For your Mac demo, you can later show:
ssh-add --apple-use-keychain ~/.ssh/id_ed25519

and:
ssh -T git@github.com

Keep the detailed macOS keychain behavior as a practical demo rather than turning it into a theory section.
14. Change Git remote from HTTPS → SSH
HTTPS:
https://github.com/company/ecommerce-api.git

SSH:
git@github.com:company/ecommerce-api.git

Check:
git remote -v

Change:
git remote set-url origin git@github.com:company/ecommerce-api.git

Verify:
git remote -v

This is a very useful real-world demonstration.
15. Test SSH
ssh -T git@github.com

Conceptually:
Your machine
     │
     ▼
SSH authentication
     │
     ▼
GitHub
     │
     ▼
"Authentication successful"

Then:
git push

No GitHub password needs to be entered for every push.
16. HTTPS vs SSH
Give viewers this comparison:
HTTPS	SSH
Uses HTTPS remote URL	Uses SSH remote URL
Commonly uses token/credential	Uses SSH key
Easy to understand initially	Requires key setup
Credential manager can help	SSH agent can help
Useful in many environments	Common developer workflow


Don't say:
"SSH is always better."

Instead:
Both are valid authentication approaches; teams choose based on environment, tooling, security requirements, and organizational policy.

17. The .git directory
Now show where Git stores local repository information.
project/
│
├── src/
├── package.json
├── README.md
└── .git/

.git contains Git's local repository metadata.
But make an important distinction:
.git/
    ≠
GitHub credentials

Don't teach beginners that .git is where their GitHub token lives.
18. Credential storage
Now introduce a practical issue.
Developer runs:
git push

The credential needs to be handled securely.
Depending on the operating system/environment, Git can work with credential helpers.
Mental model:
Git
 │
 ▼
Credential Helper
 │
 ▼
Secure credential storage
 │
 ▼
GitHub authentication

On macOS, credential storage can integrate with the system keychain.
The principle is:
Don't store credentials casually in plain text files or source code.

19. The biggest security mistake 🚨
Now create a realistic scenario.
Developer writes:
const stripeSecret = "sk_live_xxxxxxxxx";

Then:
git add .
git commit -m "Add payment integration"
git push

Now:
Developer
    │
    ▼
Git commit
    │
    ▼
GitHub
    │
    ▼
🚨 SECRET EXPOSED

This is where Phase 9 becomes memorable.
20. "But I deleted it!"
This is a critical Git concept.
Developer realizes:
"Oh no!"

Deletes the secret:
const stripeSecret = "";

Then commits:
git commit -m "Remove secret"

But:
A ─── B ─── C
          │
          └── secret existed here

The old commit may still contain the secret.
Therefore:
Deleting a secret from the latest file does not necessarily mean the secret is no longer present in Git history.

21. What to do if a secret is exposed
Teach the incident response order:
        SECRET EXPOSED
              │
              ▼
       1. Revoke / rotate
              │
              ▼
       2. Investigate usage
              │
              ▼
       3. Remove from code/history
              │
              ▼
       4. Replace with secret storage
              │
              ▼
       5. Review how it happened

The first priority is invalidating the exposed credential, not merely deleting the line from the current file.
This is an important security lesson.
22. GitHub Secret Scanning
Introduce GitHub's security capabilities at a high level.
Conceptually:
Developer
    │
    ▼
Push code
    │
    ▼
GitHub
    │
    ▼
Secret detection
    │
    ├── No secret
    │      ↓
    │    Continue
    │
    └── Potential secret
           ↓
         Alert / protection

GitHub provides secret scanning capabilities, including push protection in supported configurations.
Don't make this a substitute for secure development:
Detection is a safety net, not permission to commit secrets.

23. What should NOT be committed?
Create a very practical list:
❌ API keys
❌ Database passwords
❌ Cloud credentials
❌ Private SSH keys
❌ Production secrets
❌ JWT signing secrets
❌ Payment provider secret keys
❌ Third-party service credentials

Instead:
Code
  │
  ├── configuration
  └── references secret
             │
             ▼
        Secret Manager

This leads naturally into Phase 10.
24. .gitignore
Now teach .gitignore as the first line of defense.
Example:
node_modules/
.env
.env.local
*.log
.DS_Store

Python:
__pycache__/
.venv/
*.pyc
.env

But emphasize:
.gitignore prevents untracked files from being added normally. It does not magically remove a secret that has already been committed.

25. .env mental model
Instead of:
const DB_PASSWORD = "secret123";

use:
Application
     │
     ▼
process.env.DB_PASSWORD
     │
     ▼
Environment / Secret Store

Local development:
.env

Production:
Secret Manager / CI-CD secrets / platform environment

But don't teach GitHub Actions secrets deeply yet.
That's Phase 10.
26. GitHub Permissions + Authentication
Now connect Phase 8 and Phase 9.
Phase 8:
WHO CAN ACCESS THE REPOSITORY?
        │
        ▼
Teams / Roles / Permissions

Phase 9:
HOW DO WE PROVE WHO THE USER IS?
        │
        ▼
HTTPS / SSH / Identity

Together:
Developer
    │
    ▼
Authentication
"Who are you?"
    │
    ▼
Authorization
"What can you do?"
    │
    ▼
Repository

This is a very important connection.
27. SSH Key vs Deploy Key
You can briefly introduce this distinction because advanced viewers will encounter it.
SSH Key
   ↓
Usually associated with a user's GitHub identity

Deploy Key
   ↓
Associated with a specific repository

Mental model:
User SSH Key
      ↓
   GitHub User
      ↓
Multiple repositories according to permissions


Deploy Key
      ↓
Specific repository

Don't go deep here; just make viewers aware of the distinction.
28. Machine-to-machine authentication
Now introduce a production scenario.
Suppose:
GitHub
   │
   ▼
Deployment Server

Should you give a developer's personal token to the server?
❌ Usually a poor design

Instead:
Machine
   │
   ▼
Machine-specific credential
   │
   ▼
Required access only

This sets up later discussions around:
- deploy keys
- GitHub Apps
- CI/CD credentials
- cloud identity
- OIDC
Don't go deep yet.
29. Security Principle — Least Privilege
Make this one of the phase's major concepts.
Bad:
Token
 │
 └── Full access to everything

Better:
Token
 │
 ├── Specific repository
 ├── Required permissions
 └── Limited lifetime

The principle:
Give each user, token, application, or machine only the permissions it needs.

30. Security checklist
Give viewers something they can actually use.
GitHub Security Checklist
──────────────────────────

☐ Use SSH or secure HTTPS authentication
☐ Never commit secrets
☐ Use .gitignore
☐ Protect main
☐ Use least privilege
☐ Use short-lived/expiring credentials where appropriate
☐ Rotate compromised credentials
☐ Revoke leaked tokens immediately
☐ Use secret scanning where available
☐ Don't share private SSH keys
☐ Don't use personal credentials for production machines
☐ Review repository permissions

31. Phase 9 Practical Project
Use the same ecommerce-api project.
Step 1 — Configure Git identity
git config --global user.name "Shane Alam"
git config --global user.email "you@example.com"

Step 2 — Check
git config --global --list

Step 3 — Check remote
git remote -v

Step 4 — Configure SSH
Generate SSH key
      ↓
Add public key to GitHub
      ↓
Test SSH
      ↓
Change origin

Step 5 — Test
ssh -T git@github.com
git push

Step 6 — Create .gitignore
node_modules/
.env
.env.local
.DS_Store

Step 7 — Create .env
DATABASE_URL=secret-value
JWT_SECRET=secret-value

Step 8 — Verify
git status

The .env should not appear as a file ready to commit if .gitignore is configured correctly.
32. Intentional security mistake
For the video, you can make a fake test secret, never a real credential.
Example:
const API_KEY = "demo-secret-123";

Commit it.
Then explain:
Commit
  ↓
Git history
  ↓
Secret exists

Remove it:
const API_KEY = process.env.API_KEY;

But show:
Old commit
   │
   └── demo-secret-123

Then explain the incident-response concept:
If real:
      ↓
Revoke immediately
      ↓
Rotate
      ↓
Clean history if necessary
      ↓
Use proper secret management

This will make the lesson much more memorable.
33. Phase 9 Hero Diagram
This should be one of the major visuals in your README/video:
                         👨‍💻 DEVELOPER
                              │
                              ▼
                       Git Authentication
                              │
                 ┌────────────┴────────────┐
                 ▼                         ▼
               HTTPS                      SSH
                 │                         │
              Token                    SSH Key Pair
                 │                         │
                 └────────────┬────────────┘
                              ▼
                           GitHub
                              │
                              ▼
                      Authentication
                     "WHO ARE YOU?"
                              │
                              ▼
                       Authorization
                    "WHAT CAN YOU DO?"
                              │
                 ┌────────────┼────────────┐
                 ▼            ▼            ▼
               Read         Write         Admin
                              │
                              ▼
                         Repository
                              │
                         🔐 main
                              │
                              ▼
                        Pull Request
                              │
                              ▼
                         Code Review

And the security side:
              🚨 SECRET
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
   Source Code           Environment
        │                   │
        ▼                   ▼
      ❌ BAD              Secret Store
                            │
                            ▼
                           ✅

🎬 Phase 9 Video Flow
00:00  🔐 Why GitHub needs authentication
        ↓
02:00  Authentication vs Authorization
        ↓
05:00  Git identity vs GitHub identity
        ↓
08:00  HTTPS authentication
        ↓
12:00  Personal Access Tokens
        ↓
17:00  Fine-grained permissions
        ↓
20:00  Token lifecycle
        ↓
22:00  SSH authentication
        ↓
27:00  Public vs Private keys
        ↓
30:00  Generate SSH key
        ↓
34:00  Configure GitHub SSH
        ↓
37:00  HTTPS vs SSH
        ↓
40:00  Credential management
        ↓
43:00  🚨 Accidentally committing a secret
        ↓
47:00  Why deleting the secret isn't enough
        ↓
50:00  Secret scanning
        ↓
52:00  .gitignore
        ↓
55:00  Least privilege
        ↓
58:00  Complete security workflow
        ↓
62:00  Practical exercise

📄 Phase 9 README Structure
# Phase 9 — GitHub Authentication & Security

## 🎯 Goal

## 🔐 Authentication vs Authorization

## 👤 Git Identity vs GitHub Identity

## 🔑 GitHub Authentication Methods

### HTTPS

### Personal Access Tokens

### Fine-Grained Tokens

### SSH

### SSH Key Pair

## 🔐 Public Key vs Private Key

## 🖥️ SSH Setup

### Generate SSH Key

### Add Public Key

### Test SSH

### Configure Git Remote

## 🔄 HTTPS vs SSH

## 🔐 Credential Management

## 🚨 Secrets in Git

### What Is a Secret?

### What Should Never Be Committed?

### Accidental Secret Commit

### Why Deleting Isn't Enough

### Incident Response

## 🛡️ Secret Scanning

## 📄 .gitignore

## 🔐 Least Privilege

## 🤖 Machine Authentication

### Deploy Keys

### GitHub Apps — Introduction

## 🧪 Practical Exercise

## 🚨 Common Mistakes

## 📋 Security Checklist

## 📌 Key Takeaways

🧠 The 7 things viewers MUST remember
1️⃣ Authentication
   → WHO ARE YOU?

2️⃣ Authorization
   → WHAT CAN YOU DO?

3️⃣ Git identity
   → Who authored the commit?

4️⃣ GitHub authentication
   → Who is performing the GitHub operation?

5️⃣ SSH private key
   → NEVER SHARE IT.

6️⃣ Secret committed to Git
   → Deleting it later doesn't necessarily remove
     it from history.

7️⃣ Least privilege
   → Give only the access that is actually required.

And now the progression becomes very natural:
PHASE 8
🏢 Real Team Workflow
       │
       ▼
How teams control code
       │
       ▼
PHASE 9
🔐 Authentication & Security
       │
       ▼
How users/machines prove identity
       │
       ▼
🚨 "Where should secrets go?"
       │
       ▼
PHASE 10
🔑 GitHub Secrets & Environment Variables
       │
       ├── .env
       ├── GitHub Secrets
       ├── Environment variables
       ├── DEV secrets
       ├── STAGING secrets
       ├── PROD secrets
       ├── Secret rotation
       └── Secure configuration
       │
       ▼
PHASE 11
⚙️ GitHub Actions / CI/CD

Phase 10 is where we should make the security story practical: take the ecommerce-api, create separate DEV / STAGING / PROD configuration, show why .env is for local development but shouldn't be committed, then introduce GitHub Secrets and environment-specific secrets. After that, Phase 11 can use those secrets inside a real GitHub Actions pipeline.
🔐 GitHub Authentication

<p align="center">





</p>

Connect your local Git to GitHub securely --- using a Personal
Access Token or SSH key.

📋 Table of Contents

💡 Why Authentication is Needed

🔑 Authentication Methods
Overview

🪙 Method 1 --- Personal Access Token
(PAT)

Step 1 --- Generate a PAT on
GitHub

Step 2 --- Use the PAT for Git
Operations

Step 3 --- Verify PAT Works

🔐 Method 2 --- SSH Keys

Step 1 --- Generate an SSH Key
Pair

Step 2 --- Start the SSH Agent and Add Your
Key

Step 3 --- Copy Your Public Key

Step 4 --- Add the Public Key to
GitHub

Step 5 --- Use an SSH Remote
URL

Step 6 --- Test SSH Connection to
GitHub

⚖️ PAT vs SSH Comparison

✅ Verify Your Connection

🛡️ Security Best Practices

💡 Why Authentication is Needed

When you run git push, git pull, or git clone against a private or
write-enabled GitHub repository, GitHub needs to verify your identity
before allowing the operation.

GitHub supports different authentication methods for Git operations.

Common Methods

HTTPS + Personal Access Token (PAT) --- use a token instead of
your GitHub password.

SSH Keys --- use a public/private key pair for passwordless Git
authentication.

GitHub CLI (gh auth) --- use an interactive browser-based
authentication flow.

⚠️ GitHub does not accept your normal GitHub account password for Git
over HTTPS. Use a supported authentication method such as a PAT, SSH,
or GitHub CLI.

🔑 Authentication Methods Overview

Method              How It Works        Best For

HTTPS + PAT         Token is used as the    Quick setup and
password for HTTPS Git  environments where
operations              HTTPS is preferred

SSH Keys            Public key is stored on Daily Git usage and
GitHub and private key  passwordless
stays on your machine   authentication

🪙 Method 1 --- Personal Access Token (PAT)

A Personal Access Token (PAT) acts as a password replacement for
HTTPS Git operations and can be configured with appropriate permissions.

A token can also be revoked when it is no longer needed.

Step 1 --- Generate a PAT on GitHub

Go to GitHub.

Open Profile → Settings → Developer Settings → Personal Access
Tokens.

Choose the appropriate token type.

Click Generate new token.

Configure the token with only the permissions required for your
work.

Set an appropriate expiration date.

Generate the token.

Copy the token immediately and store it securely.

Example Token Configuration

Field     Example

Note          Git Push Token
Expiration    90 days
Permissions   Only the permissions required for your task

🔐 Security: Never commit a PAT to a Git repository, paste it into
a public issue, or share it with anyone.

Step 2 --- Use the PAT for Git Operations

Option A --- Enter When Prompted

Add your remote and push:

git remote add origin https://github.com/your-username/your-repo.git
git push -u origin main

Git may ask for credentials:

Username: your_github_username
Password: <paste your PAT here>

Use the PAT, not your GitHub account password.

Option B --- Store Credentials

Git can use a credential helper so that you do not have to enter
credentials repeatedly.

git config --global credential.helper store

⚠️ credential.helper store saves credentials in a local file in
plain text. Avoid using it on shared or untrusted systems.

For a temporary in-memory cache:

git config --global credential.helper cache

Option C --- Embed PAT in the Remote URL

For quick local testing only:

git remote add origin https://your-username:YOUR_PAT@github.com/your-username/repo.git

⚠️ Do not use this on shared systems. The token can appear in
shell history and Git configuration. Never commit or expose the token.

Step 3 --- Verify PAT Works

Push your branch:

git push -u origin main

A successful push will look similar to:

Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (5/5), done.
To https://github.com/your-username/your-repo.git
 * [new branch]      main -> main

🔐 Method 2 --- SSH Keys

SSH authentication uses a public/private key pair.

🔒 The private key stays on your machine.

🔑 The public key is added to GitHub.

GitHub uses the key pair to authenticate your Git operations.

Once configured, Git operations can work without repeatedly entering a
token.

Step 1 --- Generate an SSH Key Pair

Use Ed25519:

ssh-keygen -t ed25519 -C "your_email@example.com"

When prompted:

File location --- press Enter to accept the default.

Passphrase --- add one for additional security, or leave it
empty if appropriate.

This creates two files:

~/.ssh/id_ed25519        ← PRIVATE KEY — never share
~/.ssh/id_ed25519.pub    ← PUBLIC KEY — add this to GitHub

🚨 Never share your private key. Only the .pub public key should
be uploaded to GitHub.

Step 2 --- Start the SSH Agent and Add Your Key

eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

Step 3 --- Copy Your Public Key

Display the public key:

cat ~/.ssh/id_ed25519.pub

The output will look similar to:

ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... your_email@example.com

Copy the entire public key.

Step 4 --- Add the Public Key to GitHub

Go to GitHub.

Open Profile → Settings → SSH and GPG keys.

Click New SSH key.

Configure the key.

Field   Value

Title       My Laptop
Key type    Authentication Key
Key         Paste your public key

Click Add SSH key.

Step 5 --- Use an SSH Remote URL

When cloning a repository, use the SSH URL:

git clone git@github.com:your-username/your-repo.git

For an existing repository, change the remote:

git remote set-url origin git@github.com:your-username/your-repo.git

Check the configured remote:

git remote -v

SSH URLs normally follow this format:

git@github.com:username/repository.git

Step 6 --- Test SSH Connection to GitHub

Run:

ssh -T git@github.com

A successful authentication response will be similar to:

Hi your-username! You've successfully authenticated, but GitHub does not provide shell access.

After successful SSH configuration, normal Git operations can use the
SSH key:

git pull
git push

⚖️ PAT vs SSH Comparison

Feature             PAT (HTTPS)         SSH Key

Setup complexity        Simple                  Moderate

Credential prompt       May prompt depending on Usually no prompt after
credential management   setup

Expiration              Can have an expiration  Key remains valid until
date                    revoked/removed

Security                Good when properly      Excellent when the
scoped and protected    private key is secured

Corporate proxy         Usually very good       May be restricted in
compatibility                                   some environments

✅ Verify Your Connection

Check Your Configured Remote

git remote -v

Example:

origin  git@github.com:your-username/your-repo.git (fetch)
origin  git@github.com:your-username/your-repo.git (push)

Push to Verify Authentication

git push -u origin main

If authentication succeeds, your changes will be pushed to GitHub.

Check SSH Keys Loaded

ssh-add -l

This displays the identities currently loaded into the SSH agent.

🛡️ Security Best Practices

Never Commit Secrets

Do not commit:

❌ Personal Access Tokens

❌ Passwords

❌ API keys

❌ AWS access keys

❌ Private SSH keys

❌ Database credentials

❌ .env files containing secrets

Always

✅ Use .gitignore for sensitive local files.

✅ Give tokens only the permissions they need.

✅ Set token expiration where possible.

✅ Revoke unused or exposed tokens.

✅ Protect your private SSH key.

✅ Use a passphrase for important SSH keys.

✅ Check your changes before pushing.

✅ Rotate credentials immediately if they are exposed.

📌 Quick Reference

HTTPS + PAT

git remote add origin https://github.com/your-username/your-repo.git
git push -u origin main

SSH

ssh-keygen -t ed25519 -C "your_email@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
ssh -T git@github.com

Check Remote

git remote -v

Change HTTPS to SSH

git remote set-url origin git@github.com:your-username/your-repo.git

Recommended workflow for daily Git usage: Configure SSH once, keep
your private key secure, and use the SSH remote URL for your GitHub
repositories.

<div align="center">

![GitHub](https://img.shields.io/badge/GITHUB-181717?style=for-the-badge&logo=github&logoColor=white)
![SSH](https://img.shields.io/badge/SSH-SECURE-4EAA25?style=for-the-badge&logo=openssh&logoColor=white)
![PAT](https://img.shields.io/badge/PAT-TOKEN%20AUTH-orange?style=for-the-badge)
![Security](https://img.shields.io/badge/SECURITY-BEST%20PRACTICES-blue?style=for-the-badge)

# 🔐 GitHub Authentication

</div>

---

> **Connect your local Git to GitHub securely using HTTPS + Personal Access Token (PAT) or SSH.**

---

## 📋 Table of Contents

- [Why Authentication is Needed](#-why-authentication-is-needed)
- [Authentication Methods Overview](#-authentication-methods-overview)
- [Method 1 — Personal Access Token (PAT)](#-method-1--personal-access-token-pat)
  - [Step 1 — Generate a PAT on GitHub](#step-1--generate-a-pat-on-github)
  - [Step 2 — Use the PAT for Git Operations](#step-2--use-the-pat-for-git-operations)
  - [Step 3 — Verify PAT Authentication](#step-3--verify-pat-authentication)
- [Method 2 — SSH Authentication](#-method-2--ssh-authentication)
  - [Step 1 — Generate an SSH Key Pair](#step-1--generate-an-ssh-key-pair)
  - [Step 2 — Start the SSH Agent](#step-2--start-the-ssh-agent)
  - [Step 3 — Add the SSH Key](#step-3--add-the-ssh-key)
  - [Step 4 — Copy the Public Key](#step-4--copy-the-public-key)
  - [Step 5 — Add the Public Key to GitHub](#step-5--add-the-public-key-to-github)
  - [Step 6 — Test the SSH Connection](#step-6--test-the-ssh-connection)
  - [Step 7 — Use the SSH Remote URL](#step-7--use-the-ssh-remote-url)
- [Private Key vs Public Key](#-private-key-vs-public-key)
- [PAT vs SSH](#-pat-vs-ssh)
- [Verify Your Connection](#-verify-your-connection)
- [Important Security Rules](#-important-security-rules)
- [Quick Reference](#-quick-reference)
- [Key Takeaways](#-key-takeaways)

---

## 💡 Why Authentication is Needed

When you run Git commands such as:

```bash
git push
git pull
git clone
```

GitHub needs to verify your identity before allowing access to a private repository or allowing you to push changes.

GitHub does not use your normal GitHub account password for Git HTTPS authentication.

The commonly used authentication methods are:

- **HTTPS + Personal Access Token (PAT)**
- **SSH Keys**

> ⚠️ **Important:** Do not use your normal GitHub account password as the Git HTTPS password. Use a Personal Access Token or SSH authentication.

---

## 🔑 Authentication Methods Overview

| Method | How It Works | Best For |
|---|---|---|
| **HTTPS + PAT** | Personal Access Token is used for HTTPS authentication | Quick setup and HTTPS environments |
| **SSH Keys** | Public key is stored on GitHub and private key stays on your machine | Daily Git usage and passwordless authentication |
| **GitHub CLI** | Browser-based authentication from the terminal | Interactive GitHub CLI workflows |

---

# 🪙 Method 1 — Personal Access Token (PAT)

A **Personal Access Token (PAT)** can be used instead of your GitHub password when performing Git operations over HTTPS.

A PAT provides an authentication mechanism that can be configured with appropriate permissions.

### Important Principles

- Give the token only the permissions that are required.
- Keep the token secret.
- Never commit the token to Git.
- Never paste the token into a public repository.
- Revoke compromised tokens immediately.
- Set an appropriate expiration date.
- Use fine-grained permissions where appropriate.

---

## Step 1 — Generate a PAT on GitHub

To create a Personal Access Token:

1. Log in to GitHub.
2. Go to your **Profile**.
3. Open **Settings**.
4. Select **Developer Settings**.
5. Open **Personal Access Tokens**.
6. Select the appropriate token option.
7. Generate a new token.
8. Set an appropriate expiration date.
9. Select only the permissions that are required.
10. Generate the token.
11. Copy the token and store it securely.

### Example

```text
Token Name:
Git-Push-Token

Expiration:
90 Days

Permissions:
Only the permissions required for the repository
```

> 🔐 **Important:** Store your token securely. Never publish it or commit it to a repository.

---

## Step 2 — Use the PAT for Git Operations

First, check the remote repository:

```bash
git remote -v
```

Example:

```text
origin  https://github.com/USERNAME/REPOSITORY.git (fetch)
origin  https://github.com/USERNAME/REPOSITORY.git (push)
```

Now push your changes:

```bash
git push -u origin main
```

Git may ask for:

```text
Username:
Password:
```

Enter:

```text
Username: your-github-username
Password: your-PAT
```

> ⚠️ The **Password** field should contain your Personal Access Token, not your GitHub account password.

---

## Step 3 — Verify PAT Authentication

Run:

```bash
git push
```

If authentication is successful, Git will push your commits to GitHub.

Example:

```text
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (5/5), done.
To https://github.com/USERNAME/REPOSITORY.git
   main -> main
```

Your local Git repository is now authenticated with GitHub using HTTPS + PAT.

---

# 🔐 Method 2 — SSH Authentication

SSH is another common way to authenticate Git operations with GitHub.

SSH uses a **public/private key pair**.

```text
Your Computer
      |
      | Private Key
      |
      v
     SSH
      |
      v
    GitHub
      |
      | Public Key
      v
GitHub Account
```

The basic idea is:

```text
Private Key → Stays on your computer
Public Key  → Added to GitHub
```

> 🔒 **Never share your private SSH key.**

---

## Step 1 — Generate an SSH Key Pair

Generate an Ed25519 SSH key:

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

You will be asked where you want to save the key.

Press **Enter** to use the default location.

Typical files:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

The files are:

| File | Purpose |
|---|---|
| `id_ed25519` | Private key |
| `id_ed25519.pub` | Public key |

> 🚨 **Never share or upload `id_ed25519`.**

---

## Step 2 — Start the SSH Agent

Start the SSH agent:

```bash
eval "$(ssh-agent -s)"
```

Example:

```text
Agent pid 1234
```

The SSH agent manages your SSH keys for authentication.

---

## Step 3 — Add the SSH Key

Add your private key to the SSH agent:

```bash
ssh-add ~/.ssh/id_ed25519
```

Verify that the key is loaded:

```bash
ssh-add -l
```

---

## Step 4 — Copy the Public Key

Display your public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Example:

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... your-email@example.com
```

Copy the **entire public key**.

> ✅ The `.pub` file is the public key and can be added to GitHub.

---

## Step 5 — Add the Public Key to GitHub

Go to:

```text
GitHub
   ↓
Profile
   ↓
Settings
   ↓
SSH and GPG keys
   ↓
New SSH key
```

Enter:

```text
Title:
My Laptop

Key Type:
Authentication Key

Key:
Paste your public SSH key
```

Click:

```text
Add SSH key
```

Your public key is now associated with your GitHub account.

---

## Step 6 — Test the SSH Connection

Run:

```bash
ssh -T git@github.com
```

If authentication is successful, GitHub will return a message similar to:

```text
Hi USERNAME! You've successfully authenticated,
but GitHub does not provide shell access.
```

This means your SSH authentication is working.

---

## Step 7 — Use the SSH Remote URL

For a new repository:

```bash
git clone git@github.com:USERNAME/REPOSITORY.git
```

For an existing repository, change the remote URL:

```bash
git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
```

Check the remote:

```bash
git remote -v
```

Example:

```text
origin  git@github.com:USERNAME/REPOSITORY.git (fetch)
origin  git@github.com:USERNAME/REPOSITORY.git (push)
```

Now you can use:

```bash
git pull
git push
```

with SSH authentication.

---

# 🔒 Private Key vs Public Key

SSH authentication uses two keys.

| Key | Location | Share? |
|---|---|---|
| **Private Key** | Your computer | ❌ Never share |
| **Public Key** | GitHub | ✅ Can be added to GitHub |

### Private Key

```text
~/.ssh/id_ed25519
```

The private key must remain on your machine.

### Public Key

```text
~/.ssh/id_ed25519.pub
```

The public key is added to your GitHub account.

> 🚨 **Never upload your private key to GitHub.**

---

# ⚖️ PAT vs SSH

| Feature | PAT | SSH |
|---|---|---|
| Protocol | HTTPS | SSH |
| Setup | Easy | Moderate |
| Credential | Token | Key pair |
| Password prompt | May occur | Usually no |
| Daily Git usage | Good | Excellent |
| Token management | Required | Not required |
| Recommended for daily Git | Good | ⭐ Excellent |

### Simple Recommendation

For learning GitHub authentication:

```text
HTTPS + PAT
     ↓
Understand HTTPS authentication

SSH
     ↓
Configure secure daily Git access
```

For regular development work, SSH is a convenient option once it has been configured correctly.

---

# ✅ Verify Your Connection

## Check Git Remote

```bash
git remote -v
```

---

## Test SSH Authentication

```bash
ssh -T git@github.com
```

---

## Check SSH Keys Loaded

```bash
ssh-add -l
```

---

## Test Git Push

```bash
git push -u origin main
```

---

# 🛡️ Important Security Rules

Never commit or expose:

```text
❌ GitHub Passwords
❌ Personal Access Tokens
❌ AWS Access Keys
❌ Private SSH Keys
❌ API Keys
❌ Database Passwords
❌ .env files containing secrets
```

### Always Follow These Practices

- ✅ Use `.gitignore` for sensitive local files.
- ✅ Use least-privilege permissions.
- ✅ Give tokens only the permissions required.
- ✅ Set token expiration where possible.
- ✅ Protect your private SSH key.
- ✅ Use an SSH passphrase for important keys.
- ✅ Never commit secrets to Git.
- ✅ Review files before committing.
- ✅ Rotate credentials after accidental exposure.
- ✅ Revoke compromised credentials immediately.
- ✅ Use GitHub Secrets for CI/CD credentials.
- ✅ Scan repositories for accidentally exposed secrets.

> ⚠️ **Important:** If a credential is accidentally pushed to GitHub, simply deleting the file is not enough. The credential should be revoked or rotated immediately because it may still exist in Git history.

---

# 📌 Quick Reference

## HTTPS + PAT

Check the remote:

```bash
git remote -v
```

Push:

```bash
git push -u origin main
```

Authentication:

```text
Username → GitHub username
Password → Personal Access Token
```

---

## Generate SSH Key

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

---

## Start SSH Agent

```bash
eval "$(ssh-agent -s)"
```

---

## Add SSH Key

```bash
ssh-add ~/.ssh/id_ed25519
```

---

## Display Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

---

## Test GitHub SSH

```bash
ssh -T git@github.com
```

---

## Change Remote to SSH

```bash
git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
```

---

## Check Remote

```bash
git remote -v
```

---

# 🎯 Key Takeaways

```text
                    GitHub Authentication
                           |
              ┌────────────┴────────────┐
              |                         |
        HTTPS + PAT                   SSH
              |                         |
       Personal Access             Key Pair
           Token               ┌─────────┴─────────┐
              |                 |                   |
         HTTPS URL         Private Key         Public Key
                                  |                   |
                            Your Machine           GitHub
```

Remember:

- **PAT** → Used for HTTPS Git authentication.
- **SSH** → Uses a public/private key pair.
- **Private Key** → Never share it.
- **Public Key** → Add it to GitHub.
- **Least Privilege** → Give credentials only the required permissions.
- **Secrets** → Never commit them to Git.
- **Compromised Credential** → Revoke or rotate it immediately.
- **SSH** → Convenient for regular Git usage.
- **PAT** → Useful when working with HTTPS authentication.

---

## 🚀 GitHub Authentication Workflow

```text
Create GitHub Repository
          ↓
Choose Authentication
          ↓
   ┌──────┴──────┐
   ↓             ↓
 HTTPS          SSH
   ↓             ↓
 PAT          Key Pair
   ↓             ↓
Authenticate GitHub
          ↓
      git push
          ↓
     GitHub Repo
```

---

<div align="center">

---

### 🔐 Secure Authentication → Clean Git Workflow → Better DevOps Practices

**Mohammed Rinas**

Cloud / DevOps Engineer — Learning, Building & Practicing

---

</div>

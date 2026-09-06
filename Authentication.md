# 🔐 GitHub Authentication & Security

HTTPS/PAT, SSH authentication, GitHub Secrets and Git/GitHub security
rules.

------------------------------------------------------------------------

## 📚 Contents

-   [. GitHub Authentication](#-github-authentication)
-   [SSH Authentication](#-ssh-authentication)
-   [GitHub Secrets](#-github-secrets)
-   [Important Security Rules](#-important-security-rules)

------------------------------------------------------------------------

## GitHub Authentication

HTTPS

HTTPS repositories use authentication through GitHub-supported
credentials/tokens rather than your normal GitHub account password.

Personal Access Token --- PAT

A PAT can be used for HTTPS Git authentication.

Important principles:

Give the token only required permissions.

Keep it secret.

Never commit it.

Never paste it into public repositories.

Revoke compromised tokens immediately.

Prefer short-lived/fine-grained credentials where appropriate.

Typical remote:

git remote -v

Do not store a real token in scripts or source code.

------------------------------------------------------------------------

## SSH Authentication

SSH is a common way to authenticate Git operations without repeatedly
entering credentials.

Generate a key:

ssh-keygen -t ed25519 -C "your-email@example.com"

Start agent:

eval "\$(ssh-agent -s)"

Add key:

ssh-add \~/.ssh/id_ed25519

Copy the public key:

cat \~/.ssh/id_ed25519.pub

Add the public key to GitHub.

Test:

ssh -T git@github.com

Use SSH remote:

git remote set-url origin git@github.com:USERNAME/REPOSITORY.git

Private vs Public Key

Private key -\> Keep secret Public key -\> Upload to GitHub

Never share the private key.

------------------------------------------------------------------------

## GitHub Secrets

Never hard-code credentials inside workflow files.

Use:

Repository/Environment Secrets

Examples:

AWS credentials Docker registry credentials API tokens SSH keys

Access secrets in Actions using GitHub's secrets context.

Example:

env: API_TOKEN: \${{ secrets.API_TOKEN }}

Do not print secrets in logs.

------------------------------------------------------------------------

## 94. Important Security Rules

Never commit passwords.

Never commit AWS access keys.

Never commit private SSH keys.

Never commit .env files containing secrets.

Use GitHub Secrets for CI/CD credentials.

Use least privilege.

Rotate credentials immediately after accidental exposure.

Review .gitignore.

Scan repositories for secrets.

Do not assume deleting a secret from the latest commit removes it from
history.

------------------------------------------------------------------------


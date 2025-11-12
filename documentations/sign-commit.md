Git Commit Signing Documentation
Overview
Git commit signing allows you to cryptographically sign your commits to verify their authenticity. This ensures that commits actually come from the claimed author and haven't been tampered with.
Prerequisites
1. Install GPG
Linux: sudo apt-get install gnupg (Ubuntu/Debian) or sudo yum install gnupg (RHEL/CentOS)
macOS: brew install gnupg
Windows: Install Gpg4win

2. Generate GPG Key Pair
bash
# Generate new GPG key
gpg --full-generate-key

# Follow prompts:
# 1. Key type: RSA and RSA (default)
# 2. Key size: 4096 bits (recommended)
# 3. Expiration: Set as preferred (0 = never expire)
# 4. Enter your name and email (must match Git config)
# 5. Set a secure passphrase

3. List Your GPG Keys
bash
# List GPG keys with their IDs
gpg --list-secret-keys --keyid-format LONG

# Example output:
# sec   rsa4096/3AA5C34371567BD2 2023-01-01 [SC]
#       ABCDEF1234567890ABCDEF1234567890ABCDEF12
# uid                 [ultimate] Your Name <your.email@example.com>
Note the key ID after rsa4096/ (e.g., 3AA5C34371567BD2)

Configuration
1. Configure Git to Use Your GPG Key
bash
# Set your signing key in Git config
git config --global user.signingkey 3AA5C34371567BD2

# Enable commit signing by default
git config --global commit.gpgsign true

# Alternatively, enable signing for specific repository only
git config commit.gpgsign true

2. Add GPG Key to GitHub/GitLab
Export Public Key
bash
# Export public key in ASCII format
gpg --armor --export 3AA5C34371567BD2

# Copy the entire output including:
# -----BEGIN PGP PUBLIC KEY BLOCK-----
# ...
# -----END PGP PUBLIC KEY BLOCK-----
Add to GitHub
Go to Settings → SSH and GPG keys → New GPG key
Paste your public key
Click Add GPG key

Add to GitLab
Go to Preferences → GPG Keys
Paste your public key
Click Add key

Usage
Sign Individual Commits
bash
# Sign a specific commit
git commit -S -m "Your commit message"

# If you disabled global signing, force sign a commit
git commit -S -m "Your signed commit message"



Sign All Commits by Default
bash
# Enable signing for all commits globally
git config --global commit.gpgsign true

# For specific repository only
git config commit.gpgsign true
Sign Tags
bash
# Sign an annotated tag
git tag -s v1.0.0 -m "Version 1.0.0"

# Verify signed tag
git tag -v v1.0.0
Sign Previous Commits
bash
# Sign the last commit
git commit --amend --no-edit -S

# Rebase and sign multiple commits (interactive)
git rebase -i HEAD~3 -x "git commit --amend --no-edit -S"
Verification
Verify Local Commits
bash
# Verify specific commit
git verify-commit <commit-hash>

# Verify the last commit
git verify-commit HEAD

# View commit signature details
git show --show-signature <commit-hash>
Verify on Remote Repositories
GitHub: Look for "Verified" badge next to commits

GitLab: Look for "GPG" verified status

View via command line: git log --show-signature

Troubleshooting
Common Issues
1. GPG Agent Issues
bash
# Start GPG agent
gpgconf --launch gpg-agent

# Reload agent
gpg-connect-agent reloadagent /bye
2. Passphrase Caching
bash
# Add to ~/.gnupg/gpg-agent.conf
default-cache-ttl 3600
max-cache-ttl 7200

# Restart agent
gpgconf --kill gpg-agent
gpgconf --launch gpg-agent
3. Git Not Finding GPG
bash
# Specify GPG program path
git config --global gpg.program $(which gpg)

# On macOS with GPG Suite
git config --global gpg.program /usr/local/MacGPG2/bin/gpg2
4. Wrong Email in Git Config
bash
# Ensure email matches GPG key
git config --global user.email "your.email@example.com"
git config --global user.name "Your Name"
Debugging
bash
# Check Git configuration
git config --list | grep gpg

# Test GPG signing
echo "test" | gpg --clearsign

# Check GPG key list
gpg --list-keys --keyid-format LONG
Best Practices
1. Key Security
Use a strong passphrase

Store your private key securely

Consider using a hardware security key (YubiKey)

Set key expiration for additional security

2. Repository Setup
bash
# Create a script to setup signing for new repositories
#!/bin/bash
git config commit.gpgsign true
git config user.signingkey 3AA5C34371567BD2
echo "Commit signing enabled for repository"
3. Team Enforcement
Use protected branches that require signed commits:

GitHub: Settings → Branches → Branch protection rules → "Require signed commits"

GitLab: Settings → Repository → Protected Branches → "Require signed commits"

Advanced Configuration
Multiple Keys
bash
# Use different keys for different repositories
git config user.signingkey KEY_FOR_THIS_REPO

# Per-repository configuration in ~/.gitconfig
[includeIf "gitdir:~/work/"]
    path = ~/.gitconfig-work
SSH Key Signing (Git 2.34+)
bash
# Use SSH key for signing
git config gpg.format ssh
git config user.signingkey ~/.ssh/id_ed25519.pub
Platform-Specific Notes
Windows
Use Git Bash or WSL for better compatibility

Gpg4win provides GUI tools for key management

macOS
GPG Suite provides system integration

Keychain can store passphrases

Linux
Integrates well with system keyrings

Consider using a password manager for passphrase storage


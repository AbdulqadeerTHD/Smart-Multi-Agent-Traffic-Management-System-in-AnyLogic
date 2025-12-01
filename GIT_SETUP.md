# Git Repository Setup Instructions

This guide will help you complete the Git setup and push your project to GitHub.

## Current Status

Your local repository is set up with:
- `main` branch (production/stable)
- `dev` branch (development - current branch)
- All documentation files committed
- Remote repository configured

## Next Steps to Push to GitHub

### Option 1: Using HTTPS (Recommended for beginners)

1. **Push dev branch first:**
   ```bash
   git checkout dev
   git push -u origin dev
   ```
   - You'll be prompted for GitHub username and password
   - For password, use a Personal Access Token (not your GitHub password)
   - Create token at: https://github.com/settings/tokens
   - Select scope: `repo` (full control of private repositories)

2. **Push main branch:**
   ```bash
   git checkout main
   git push -u origin main
   ```

### Option 2: Using SSH (Recommended for advanced users)

1. **Set up SSH key** (if not already done):
   ```bash
   ssh-keygen -t ed25519 -C "your.email@example.com"
   # Follow prompts, then add to GitHub:
   # https://github.com/settings/keys
   ```

2. **Change remote to SSH:**
   ```bash
   git remote set-url origin git@github.com:AbdulqadeerTHD/Smart-Multi-Agent-Traffic-Management-System-in-AnyLogic.git
   ```

3. **Push branches:**
   ```bash
   git checkout dev
   git push -u origin dev
   git checkout main
   git push -u origin main
   ```

## Verify Setup

After pushing, verify on GitHub:

1. Go to: https://github.com/AbdulqadeerTHD/Smart-Multi-Agent-Traffic-Management-System-in-AnyLogic
2. Check that both `main` and `dev` branches exist
3. Verify all files are present

## Set Default Branch on GitHub

1. Go to repository Settings
2. Click "Branches" in left sidebar
3. Under "Default branch", select `main`
4. Click "Update"

## Configure Branch Protection (Optional but Recommended)

1. Go to repository Settings → Branches
2. Click "Add rule" for `main` branch
3. Enable:
   - Require pull request reviews
   - Require status checks to pass
   - Require branches to be up to date
4. Save changes

This ensures `main` can only be updated via pull requests from `dev`.

## Quick Reference Commands

```bash
# Switch to dev (always work here)
git checkout dev

# Pull latest changes
git pull origin dev

# Push your changes
git push origin dev

# Switch to main (only for merging)
git checkout main

# Merge dev into main (after testing)
git merge dev
git push origin main
```

## For Team Members

Share this repository URL and CONTRIBUTING.md with your team:

- Repository: https://github.com/AbdulqadeerTHD/Smart-Multi-Agent-Traffic-Management-System-in-AnyLogic
- Contributing Guide: See CONTRIBUTING.md

Team members should:
1. Clone the repository
2. Always work on `dev` branch
3. Never commit directly to `main`

## Troubleshooting

### Authentication Issues

If you get authentication errors:
- Use Personal Access Token instead of password
- Or set up SSH keys
- Or use GitHub CLI: `gh auth login`

### Branch Not Found

If GitHub shows only one branch:
- Make sure you pushed both branches
- Check: `git branch -a` to see all branches
- Push missing branch: `git push -u origin branch-name`

### Permission Denied

If you get permission errors:
- Make sure you have write access to the repository
- Check repository settings on GitHub
- Verify you're using the correct account

---

**Note**: After initial push, you can work normally on `dev` branch. Only merge to `main` when code is stable and tested.


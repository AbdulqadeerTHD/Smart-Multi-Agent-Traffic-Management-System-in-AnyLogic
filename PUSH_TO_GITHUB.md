# Push to GitHub - Quick Guide

Your repository is ready to push. Follow these steps:

## Step 1: Authenticate with GitHub

You'll need to authenticate. Choose one method:

### Option A: Personal Access Token (Recommended)

1. Create a token: https://github.com/settings/tokens
2. Click "Generate new token (classic)"
3. Select scope: `repo` (full control)
4. Copy the token

### Option B: SSH Key (If you have one set up)

```bash
git remote set-url origin git@github.com:AbdulqadeerTHD/Smart-Multi-Agent-Traffic-Management-System-in-AnyLogic.git
```

## Step 2: Push Dev Branch

```bash
git checkout dev
git push -u origin dev
```

When prompted:
- Username: `AbdulqadeerTHD`
- Password: Use your Personal Access Token (not GitHub password)

## Step 3: Push Main Branch

```bash
git checkout main
git push -u origin main
```

## Step 4: Verify on GitHub

Go to: https://github.com/AbdulqadeerTHD/Smart-Multi-Agent-Traffic-Management-System-in-AnyLogic

You should see:
- Both `main` and `dev` branches
- All documentation files
- README.md with project description

## Step 5: Set Default Branch

1. Go to repository Settings
2. Click "Branches" in left sidebar
3. Under "Default branch", select `main`
4. Click "Update"

## What's Different Between Branches?

- **main**: Clean README without dev branch note (production-ready)
- **dev**: README has note at top: "Development Branch (dev): This is the development branch where you can always make changes and contribute to the project"

Both branches have all the same documentation files.

## After Pushing

Your team members can now:
1. Clone the repository
2. Checkout `dev` branch
3. Start contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for team workflow.


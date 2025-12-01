# Contributing Guide

This document outlines the workflow and guidelines for contributing to the Smart Multi-Agent Traffic Management System project.

## Branch Strategy

This project uses a two-branch workflow:

- **main**: Production/stable branch - contains tested, working code
- **dev**: Development branch - all new work happens here

### Important Rules

1. **NEVER work directly on `main` branch**
2. **ALWAYS work on `dev` branch**
3. **Only merge to `main` when code is tested and stable**

## Getting Started

### Initial Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/AbdulqadeerTHD/Smart-Multi-Agent-Traffic-Management-System-in-AnyLogic.git
   cd Smart-Multi-Agent-Traffic-Management-System-in-AnyLogic
   ```

2. **Configure Git (if not already done):**
   ```bash
   git config user.name "Your Name"
   git config user.email "your.email@example.com"
   ```

3. **Switch to dev branch:**
   ```bash
   git checkout dev
   ```

### Daily Workflow

1. **Always start from dev branch:**
   ```bash
   git checkout dev
   git pull origin dev
   ```

2. **Create a feature branch for your work (optional but recommended):**
   ```bash
   git checkout -b feature/your-feature-name
   # or
   git checkout -b phase1-road-network
   # or
   git checkout -b fix/bug-description
   ```

3. **Make your changes:**
   - Work on your feature
   - Test your changes
   - Commit frequently with clear messages

4. **Commit your changes:**
   ```bash
   git add .
   git commit -m "Description of what you did"
   ```

5. **Push to dev branch:**
   ```bash
   # If you created a feature branch:
   git push origin feature/your-feature-name
   # Then create a pull request to merge into dev
   
   # If working directly on dev:
   git push origin dev
   ```

6. **After your work is complete and tested:**
   ```bash
   # Merge dev into main (only when stable)
   git checkout main
   git pull origin main
   git merge dev
   git push origin main
   ```

## Commit Message Guidelines

Write clear, descriptive commit messages:

**Good examples:**
- `Phase 1: Add road network with 3 intersections`
- `Fix: CarAgent statechart transition logic`
- `Add: Emergency vehicle priority handling`
- `Update: Documentation for Phase 3`

**Bad examples:**
- `fix`
- `update`
- `changes`
- `asdf`

## Branch Protection

- **main branch**: Protected - only merge from dev after testing
- **dev branch**: Open for all team members to push

## Workflow Diagram

```
main (stable)  ──────────────────────────────────────────────
                      ▲
                      │
                      │ (merge when stable)
                      │
dev (development) ────┴───────────────────────────────────────
         │
         ├── feature/phase1-road-network
         ├── feature/car-agent
         ├── feature/traffic-lights
         └── fix/bug-description
```

## Team Collaboration

### For Group Members

1. **Before starting work:**
   ```bash
   git checkout dev
   git pull origin dev
   ```

2. **During development:**
   - Work on your assigned phase/feature
   - Commit frequently
   - Push to dev regularly

3. **Before pushing:**
   - Test your changes
   - Make sure code compiles
   - Update documentation if needed

4. **Resolving conflicts:**
   ```bash
   git pull origin dev
   # Resolve conflicts if any
   git add .
   git commit -m "Resolve merge conflicts"
   git push origin dev
   ```

### Communication

- Coordinate with team before merging to main
- Discuss major changes in team meetings
- Update README/ARCHITECTURE.md for significant changes

## Phase Development Workflow

When working on a specific phase:

1. **Create phase branch:**
   ```bash
   git checkout dev
   git pull origin dev
   git checkout -b phase/phase-number-description
   ```

2. **Complete phase work:**
   - Follow phase instructions
   - Test thoroughly
   - Document changes

3. **Merge back to dev:**
   ```bash
   git checkout dev
   git merge phase/phase-number-description
   git push origin dev
   ```

4. **After testing, merge to main:**
   ```bash
   git checkout main
   git merge dev
   git push origin main
   ```

## File Organization

### What to Commit

- Documentation files (.md)
- Code snippets and examples
- Configuration files
- Project structure files

### What NOT to Commit (handled by .gitignore)

- AnyLogic .alp files (binary)
- Temporary files
- Build artifacts
- Large data files

## Troubleshooting

### If you accidentally commit to main:

```bash
# Move commits to dev
git checkout main
git log  # Note the commit hash
git checkout dev
git cherry-pick <commit-hash>
git checkout main
git reset --hard origin/main  # Reset main to remote state
```

### If dev branch is ahead:

```bash
git checkout dev
git pull origin dev
# Resolve any conflicts
git push origin dev
```

### If you need to update from main:

```bash
git checkout dev
git merge main
# Resolve conflicts if any
git push origin dev
```

## Best Practices

1. **Pull before push**: Always pull latest changes before pushing
2. **Small commits**: Commit frequently with logical changes
3. **Clear messages**: Write descriptive commit messages
4. **Test before merge**: Test thoroughly before merging to main
5. **Document changes**: Update documentation for significant changes
6. **Communicate**: Inform team of major changes

## Questions?

If you have questions about the workflow:
- Check this document first
- Ask team members
- Refer to Git documentation: https://git-scm.com/doc

---

**Remember**: Always work on `dev`, never directly on `main`!


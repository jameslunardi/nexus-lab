# Git Workflow

## Overview

This document describes the Git workflow used for Project Nexus. All three repositories (nexus, nexus-lab, and nexus-adsync) follow the same workflow conventions.

---

## Repository Structure

### Project Nexus Repositories

**Main Repository:**
- **nexus** - Overall project documentation and vision

**Implementation Repositories:**
- **nexus-lab** - Infrastructure as Code (Terraform, libvirt)
- **nexus-adsync** - PowerShell AD synchronisation tool

**Remote URLs:**
- `git@github.com:jameslunardi/nexus.git`
- `git@github.com:jameslunardi/nexus-lab.git`
- `git@github.com:jameslunardi/nexus-adsync.git`

---

## Git Configuration

### User Identity

```bash
git config --global user.name "James Lunardi"
git config --global user.email "217091044+jameslunardi@users.noreply.github.com"
```

The email address is GitHub's no-reply email to maintain privacy whilst allowing contributions.

### SSH Authentication

SSH keys are used for authentication with GitHub:

**Key Location:** `~/.ssh/id_ed25519`  
**Key Type:** ED25519  
**Public Key:** Added to GitHub account settings

**Testing SSH Connection:**
```bash
ssh -T git@github.com
```

Expected response: `Hi jameslunardi! You've successfully authenticated...`

---

## Branch Strategy

### Main Branch

All repositories use `main` as the default branch:
- Production-quality code only
- All commits should be working code
- Regular commits preferred over large changesets

### Feature Branches (Future)

For larger features or experimental work:
```bash
git checkout -b feature/feature-name
# Make changes
git add .
git commit -m "Description"
git checkout main
git merge feature/feature-name
git branch -d feature/feature-name
```

Currently working directly on `main` during initial development.

---

## Commit Conventions

### Commit Message Format

```
Brief summary (50 characters or less)

More detailed explanation if needed. Wrap at 72 characters.
Explain what and why, not how.

- Bullet points are acceptable
- Use present tense ("Add feature" not "Added feature")
- Reference issues/tickets if applicable
```

### Good Commit Messages

**Good Examples:**
```
Initial infrastructure setup: Project structure and documentation

Update project documentation for simplified architecture

Add network module with DNS and DHCP configuration
```

**Poor Examples:**
```
Update stuff
Fixed things
WIP
```

### Commit Frequency

- Commit logical units of work
- Don't commit broken code to main
- Commit before switching contexts
- Regular commits are better than infrequent large commits

---

## Standard Workflow

### Daily Workflow

```bash
# 1. Navigate to repository
cd /mnt/Dev/nexus-lab

# 2. Check status
git status

# 3. Stage changes
git add .
# Or stage specific files:
git add path/to/file

# 4. Review what's staged
git status

# 5. Commit changes
git commit -m "Descriptive commit message"

# 6. Push to GitHub
git push origin main
```

### Checking History

```bash
# View commit history
git log

# Compact view
git log --oneline

# View specific file history
git log -- path/to/file

# View changes in last commit
git show
```

### Undoing Changes

```bash
# Unstage a file (keep changes)
git restore --staged <file>

# Discard changes to a file (dangerous!)
git restore <file>

# Amend last commit (before pushing)
git commit --amend
```

---

## Repository-Specific Workflows

### nexus-lab (Infrastructure)

**Before Committing:**
1. Run `terraform fmt` to format code
2. Run `terraform validate` to check syntax
3. Test deployment if possible
4. Update documentation if needed

**Typical Commit Cycle:**
```bash
cd /mnt/Dev/nexus-lab
# Make infrastructure changes
terraform fmt
terraform validate
git add .
git commit -m "Add domain controller module"
git push origin main
```

### nexus-adsync (PowerShell)

**Before Committing:**
1. Test PowerShell code
2. Run Pester tests if available
3. Check for errors with `PSScriptAnalyzer` (when available)
4. Update documentation

**Typical Commit Cycle:**
```bash
cd /mnt/Dev/nexus-adsync
# Make PowerShell changes
# Run tests
git add .
git commit -m "Add user sync function with error handling"
git push origin main
```

### nexus (Documentation)

**Before Committing:**
1. Review for accuracy
2. Check spelling and grammar
3. Ensure links work
4. Verify formatting

**Typical Commit Cycle:**
```bash
cd /mnt/Dev/nexus
# Update documentation
git add .
git commit -m "Update vision document with current status"
git push origin main
```

---

## File Management

### .gitignore

Each repository has a `.gitignore` file to exclude:

**nexus-lab:**
- `.terraform/` directory
- `*.tfstate` files
- `*.iso` files
- `*.qcow2` VM disks
- Sensitive files (keys, secrets)

**nexus-adsync:**
- Sensitive configuration files
- Credential files
- Test outputs

### What Should Be Committed

**Always Commit:**
- Source code (Terraform, PowerShell)
- Documentation (Markdown files)
- Configuration files (non-sensitive)
- Test files
- README files

**Never Commit:**
- Passwords or API keys
- VM disk images
- ISO files
- Personal tokens
- Terraform state files (for this project)
- Large binary files

---

## Tagging Strategy

### Version Tags

Major milestones will be tagged for easy reference:

```bash
# Create a tag
git tag -a v1.0-phase1 -m "Phase 1: Foundation Infrastructure Complete"

# Push tag to GitHub
git push origin v1.0-phase1

# List all tags
git tag
```

### Planned Tags

- `v1.0-phase1` - Phase 1 completion (Foundation Infrastructure)
- `v1.0-phase2` - Phase 2 completion (ADSync Infrastructure)
- `v1.0-phase3` - Phase 3 completion (ADSync Tool)
- `v1.0` - Full project completion

---

## Collaboration with GitHub

### Syncing with Remote

```bash
# Fetch changes from GitHub (doesn't merge)
git fetch origin

# Pull changes from GitHub (fetch + merge)
git pull origin main

# Push changes to GitHub
git push origin main

# Push all tags
git push --tags
```

### Handling Conflicts

If there are conflicts (unlikely for solo project):

```bash
git pull origin main
# Fix conflicts in files
git add .
git commit -m "Resolve merge conflicts"
git push origin main
```

---

## Common Tasks

### Starting Work on a New Feature

```bash
cd /mnt/Dev/nexus-lab
git status  # Ensure working directory is clean
# Make changes
git add .
git commit -m "Description of changes"
git push origin main
```

### Reviewing Changes Before Commit

```bash
# See what's changed
git diff

# See what's staged
git diff --staged

# Check status
git status
```

### Fixing a Commit Message

```bash
# If you haven't pushed yet
git commit --amend -m "New commit message"

# If you have pushed, avoid amending (creates conflicts)
```

---

## Repository Maintenance

### Regular Maintenance Tasks

**Weekly:**
- Review commit history for clarity
- Ensure all working code is committed
- Check that documentation is up to date
- Verify .gitignore is working correctly

**At Stage Completions:**
- Review all commits for that stage
- Consider creating a tag
- Update main project documentation
- Ensure acceptance criteria documented

---

## Remote Repository Management

### Checking Remote Configuration

```bash
git remote -v
```

Expected output:
```
origin  git@github.com:jameslunardi/nexus-lab.git (fetch)
origin  git@github.com:jameslunardi/nexus-lab.git (push)
```

### Updating Remote URL

If remote URL needs updating:
```bash
git remote set-url origin git@github.com:jameslunardi/nexus-lab.git
```

---

## Troubleshooting

### Common Issues

**Problem:** Push rejected due to email privacy  
**Solution:** Use GitHub no-reply email: `217091044+jameslunardi@users.noreply.github.com`

**Problem:** SSH authentication fails  
**Solution:** 
```bash
ssh -T git@github.com  # Test connection
ssh-add ~/.ssh/id_ed25519  # Add key if needed
```

**Problem:** Remote uses HTTPS instead of SSH  
**Solution:**
```bash
git remote set-url origin git@github.com:jameslunardi/nexus-lab.git
```

**Problem:** Merge conflicts  
**Solution:**
1. Open conflicted files
2. Resolve conflicts manually
3. `git add <resolved-files>`
4. `git commit`

---

## Best Practices

### General Guidelines

1. **Commit Often**: Regular small commits are better than large infrequent ones
2. **Write Clear Messages**: Future you will thank present you
3. **Test Before Committing**: Don't commit broken code to main
4. **Keep Commits Focused**: One logical change per commit
5. **Update Documentation**: Keep docs in sync with code

### Quality Checklist

Before committing:
- [ ] Code works as expected
- [ ] Tests pass (when applicable)
- [ ] Code is formatted correctly
- [ ] Documentation is updated
- [ ] No sensitive information included
- [ ] Commit message is clear

---

## Quick Reference

### Essential Commands

```bash
# Status and information
git status                    # Check working directory status
git log --oneline            # View commit history
git diff                     # See unstaged changes

# Staging and committing
git add .                    # Stage all changes
git add <file>              # Stage specific file
git commit -m "message"     # Commit with message
git commit --amend          # Modify last commit

# Remote operations
git push origin main        # Push to GitHub
git pull origin main        # Pull from GitHub
git fetch origin           # Fetch without merging

# Tags
git tag -a v1.0 -m "msg"   # Create annotated tag
git push origin v1.0       # Push specific tag
git push --tags            # Push all tags

# Branches (future use)
git branch                 # List branches
git checkout -b new       # Create and switch to branch
git merge branch-name     # Merge branch
```

---

## Additional Resources

### Git Documentation
- Official Git Documentation: https://git-scm.com/doc
- GitHub Documentation: https://docs.github.com

### Learning Resources
- Pro Git Book: https://git-scm.com/book/en/v2
- GitHub Git Guides: https://github.com/git-guides

---

**Document Version:** 1.0  
**Last Updated:** 17 October 2025  
**Next Review:** On completion of Phase 1

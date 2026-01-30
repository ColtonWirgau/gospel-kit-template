# Git Workflow

How we use Git for version control and collaboration.

## Branch Strategy

### Main Branches
- `main` - Production-ready code, always deployable
- `develop` - Integration branch for features (if used)

### Feature Branches
```
feature/<short-description>
feature/add-group-registration
feature/update-member-search
```

### Bug Fix Branches
```
fix/<short-description>
fix/registration-validation
fix/login-redirect
```

### Other Branch Types
```
chore/<description>    # Maintenance, dependencies
docs/<description>     # Documentation only
refactor/<description> # Code refactoring
```

## Commit Messages

### Format
```
<type>: <short description>

<optional body>

<optional footer>
```

### Types
| Type | Description |
|------|-------------|
| feat | New feature |
| fix | Bug fix |
| docs | Documentation only |
| style | Formatting, no code change |
| refactor | Code change that neither fixes a bug nor adds a feature |
| test | Adding or updating tests |
| chore | Maintenance, dependencies, build |

### Examples
```
feat: add group registration form

fix: correct validation on email field

docs: update API documentation for groups endpoint

refactor: extract shared form logic into hook

chore: update dependencies to latest versions
```

### Guidelines
- Use present tense: "add feature" not "added feature"
- Keep first line under 72 characters
- Reference issues when relevant: `fix: resolve login error (#123)`
- Be specific: "fix button alignment" not "fix bug"

## Pull Request Process

### Creating a PR

1. **Branch from main** (or develop if used)
   ```bash
   git checkout main
   git pull origin main
   git checkout -b feature/your-feature
   ```

2. **Make your changes** following conventions

3. **Commit with meaningful messages**

4. **Push and create PR**
   ```bash
   git push -u origin feature/your-feature
   ```

### PR Description Template
```markdown
## Summary
Brief description of what this PR does.

## Changes
- List of specific changes
- Another change

## Testing
How to test these changes.

## Related Issues
Closes #123
```

### Before Requesting Review
- [ ] Self-reviewed the diff
- [ ] Tests pass locally
- [ ] No console.log or debug code
- [ ] Follows coding conventions
- [ ] PR description is complete

### Review Process
1. Request review from appropriate team member
2. Address feedback promptly
3. Re-request review after changes
4. Squash and merge when approved

## Common Commands

### Daily Workflow
```bash
# Start new work
git checkout main
git pull origin main
git checkout -b feature/my-feature

# Save progress
git add <files>
git commit -m "feat: description"

# Stay up to date
git fetch origin
git rebase origin/main

# Push changes
git push origin feature/my-feature
```

### Useful Commands
```bash
# See what's changed
git status
git diff

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Discard local changes to a file
git checkout -- <file>

# Stash changes temporarily
git stash
git stash pop

# View commit history
git log --oneline -10
```

## Conflict Resolution

1. Pull latest main: `git fetch origin`
2. Rebase onto main: `git rebase origin/main`
3. Resolve conflicts in each file
4. Stage resolved files: `git add <file>`
5. Continue rebase: `git rebase --continue`
6. Force push (only your branch): `git push --force-with-lease`

## Don'ts

- Don't commit directly to main
- Don't force push to shared branches
- Don't commit secrets or credentials
- Don't commit large binary files
- Don't leave merge commits from pulling (use rebase)

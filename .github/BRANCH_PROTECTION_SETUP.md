# Branch Protection Setup Instructions

To prevent everyone except codeowners from writing to the main branch, you need to configure the following settings in your GitHub repository:

## Repository Settings Configuration

1. Go to your repository on GitHub
2. Navigate to **Settings** → **Branches**
3. Click **Add rule** or **Add branch protection rule**

### Branch Protection Rule Configuration

**Branch name pattern:** `main`

Enable the following options:

#### Protect matching branches:
- ☑️ **Restrict pushes that create files larger than 100 MB**
- ☑️ **Require a pull request before merging**
  - ☑️ **Require approvals** (set to 1 or more)
  - ☑️ **Dismiss stale PR approvals when new commits are pushed**
  - ☑️ **Require review from CODEOWNERS**
  - ☑️ **Restrict reviews to users with push access**
- ☑️ **Require status checks to pass before merging**
  - ☑️ **Require branches to be up to date before merging**
  - Add status check: `enforce-codeowners` (from the GitHub Actions workflow)
- ☑️ **Require conversation resolution before merging**
- ☑️ **Require signed commits**
- ☑️ **Require linear history**
- ☑️ **Require deployments to succeed before merging** (if applicable)
- ☑️ **Lock branch** (optional - prevents all pushes to the branch)
- ☑️ **Do not allow bypassing the above settings**
- ☑️ **Restrict pushes that create files larger than specified limit**

#### Who can push to matching branches:
- Select **Restrict pushes that create files larger than 100 MB**
- Under **Restrict who can push to matching branches**:
  - ☑️ **Restrict to specified actors only**
  - Add: `@sylvester-francis` (the codeowner)

## Additional Security Settings

### Repository Access Settings:
1. Go to **Settings** → **Manage access**
2. Set base permissions to **Read** for organization members
3. Add `@sylvester-francis` as **Admin** or **Maintain** permission

### Webhook Settings (Optional):
1. Go to **Settings** → **Webhooks**
2. Add webhook for additional monitoring if needed

## Verification

After setting up these rules:
1. Only pull requests can merge into main
2. Pull requests require approval from CODEOWNERS
3. Status checks must pass (including the branch-protection workflow)
4. Direct pushes to main will be blocked
5. Only specified actors (codeowners) can override protections if needed

## Notes

- The GitHub Actions workflow in `.github/workflows/branch-protection.yml` provides additional enforcement
- CODEOWNERS file in `.github/CODEOWNERS` defines who must review changes
- These settings ensure that `@sylvester-francis` has ultimate control over the main branch
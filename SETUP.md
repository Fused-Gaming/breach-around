# Repository Setup Guide

This guide is for **repository maintainers** to set up the automated sync and release process between the private and public repositories.

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Step 1: GitHub Secrets Setup](#step-1-github-secrets-setup)
- [Step 2: PyPI Configuration](#step-2-pypi-configuration)
- [Step 3: Private Repository Setup](#step-3-private-repository-setup)
- [Step 4: Testing the Setup](#step-4-testing-the-setup)
- [Step 5: Branch Protection](#step-5-branch-protection)
- [Troubleshooting](#troubleshooting)
- [Maintenance](#maintenance)

---

## 🎯 Overview

This repository is set up as a **public release vehicle** for the private `breach-checker` repository. The workflow automatically:

1. Syncs code from private repo when releases are published
2. Creates tags in the public repo
3. Builds Python packages
4. Publishes to PyPI
5. Creates GitHub releases with assets

---

## 🏗️ Architecture

```
┌─────────────────────────────┐
│  Private Repo               │
│  (breach-checker)           │
│                             │
│  1. Developer creates       │
│     tagged release          │
│                             │
│  2. Workflow triggers       │
│     repository_dispatch     │
└──────────────┬──────────────┘
               │
               │ API Call
               ▼
┌─────────────────────────────┐
│  Public Repo                │
│  (breach-around)            │
│                             │
│  3. Sync workflow runs      │
│  4. Copies sanitized code   │
│  5. Creates tag             │
│                             │
│  6. Release workflow runs   │
│  7. Builds packages         │
│  8. Publishes to PyPI       │
└─────────────────────────────┘
```

---

## ✅ Prerequisites

Before starting, ensure you have:

- [ ] Admin access to both repositories
- [ ] GitHub account with 2FA enabled
- [ ] PyPI account with 2FA enabled (for PyPI publishing)
- [ ] Understanding of GitHub Actions and workflows

---

## 🔐 Step 1: GitHub Secrets Setup

### 1.1 Create Personal Access Token (PAT)

This token allows the private repo to trigger workflows in the public repo.

1. Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Click "Generate new token (classic)"
3. Configure the token:
   - **Name**: `Public Repo Dispatch Token`
   - **Expiration**: Set to 1 year (or No expiration if your org allows)
   - **Scopes**: Select:
     - ✅ `repo` (Full control of private repositories)
     - ✅ `workflow` (Update GitHub Action workflows)

4. Click "Generate token"
5. **⚠️ CRITICAL**: Copy the token immediately (you won't see it again!)

### 1.2 Add Secrets to Private Repository

In `Fused-Gaming/breach-checker`:

1. Go to Settings → Secrets and variables → Actions
2. Click "New repository secret"
3. Add the following secrets:

| Name | Value | Description |
|------|-------|-------------|
| `PUBLIC_REPO_DISPATCH_TOKEN` | `ghp_xxxxx...` | PAT created in step 1.1 |

### 1.3 Add Secrets to Public Repository

In `Fused-Gaming/breach-around`:

1. Go to Settings → Secrets and variables → Actions
2. Click "New repository secret"
3. Add the following secrets:

| Name | Value | Description |
|------|-------|-------------|
| `PRIVATE_REPO_ACCESS_TOKEN` | `ghp_xxxxx...` | PAT with access to private repo |

**Note**: You may use the same PAT for both repos, or create separate tokens for better security isolation.

---

## 📦 Step 2: PyPI Configuration

### Option A: PyPI OIDC (Recommended - No Secrets!)

PyPI now supports OIDC authentication, which eliminates the need for API tokens.

1. Go to your PyPI project page: `https://pypi.org/manage/project/breach-around/`
2. Navigate to "Publishing" tab
3. Click "Add a new publisher"
4. Configure:
   - **Repository name**: `breach-around`
   - **Repository owner**: `Fused-Gaming`
   - **Workflow name**: `release.yml`
   - **Environment**: Leave blank (or create one)

5. Save the publisher

**No secrets needed!** The workflow uses GitHub's OIDC token to authenticate.

### Option B: PyPI API Token (Alternative)

If OIDC is not available:

1. Log into PyPI account
2. Go to Account settings → API tokens
3. Click "Add API token"
4. Configure:
   - **Name**: `breach-around-github-actions`
   - **Scope**: Select specific project → `breach-around`

5. Copy the token (`pypi-xxx...`)

6. Add to public repository secrets:
   - **Name**: `PYPI_API_TOKEN`
   - **Value**: `pypi-xxx...`

7. Update `.github/workflows/release.yml`:
   ```yaml
   - name: Publish to PyPI
     uses: pypa/gh-action-pypi-publish@release/v1
     with:
       password: ${{ secrets.PYPI_API_TOKEN }}  # Add this line
   ```

---

## 🔧 Step 3: Private Repository Setup

### 3.1 Copy Workflow to Private Repo

1. Copy the file `.github/PRIVATE_REPO_WORKFLOW.yml` from this public repo
2. In `Fused-Gaming/breach-checker`, create the file:
   ```
   .github/workflows/trigger-public-sync.yml
   ```
3. Paste the contents from `PRIVATE_REPO_WORKFLOW.yml`

### 3.2 Ensure Package Configuration Exists

The private repo should have either:

- `pyproject.toml` (recommended)
- `setup.py` (legacy)

**Example `pyproject.toml`:**

```toml
[build-system]
requires = ["setuptools>=45", "wheel", "setuptools_scm[toml]>=6.2"]
build-backend = "setuptools.build_meta"

[project]
name = "breach-around"
version = "0.1.0"
description = "Breach database checker and OSINT toolkit"
readme = "README.md"
authors = [
    {name = "Fused Gaming", email = "info@fusedgaming.com"}
]
license = {text = "MIT"}
classifiers = [
    "Development Status :: 4 - Beta",
    "Intended Audience :: Developers",
    "License :: OSI Approved :: MIT License",
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.8",
    "Programming Language :: Python :: 3.9",
    "Programming Language :: Python :: 3.10",
    "Programming Language :: Python :: 3.11",
]
keywords = ["security", "breach", "osint", "pentesting"]
requires-python = ">=3.8"
dependencies = [
    "requests>=2.28.0",
    "python-dotenv>=0.19.0",
    "selenium>=4.0.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.0.0",
    "pytest-cov>=3.0.0",
    "black>=22.0.0",
    "flake8>=4.0.0",
    "mypy>=0.950",
]

[project.urls]
Homepage = "https://github.com/Fused-Gaming/breach-around"
Documentation = "https://github.com/Fused-Gaming/breach-around/blob/main/README.md"
Repository = "https://github.com/Fused-Gaming/breach-around"
"Bug Tracker" = "https://github.com/Fused-Gaming/breach-around/issues"

[project.scripts]
breach-checker = "breach_around.cli:main"

[tool.setuptools]
packages = ["breach_around"]

[tool.setuptools.package-data]
breach_around = ["py.typed"]
```

---

## 🧪 Step 4: Testing the Setup

### 4.1 Test Manual Sync

1. Go to public repo: `Fused-Gaming/breach-around`
2. Navigate to Actions → Sync from Private Repository
3. Click "Run workflow"
4. Enter a test tag (e.g., `v0.1.0-test`)
5. Click "Run workflow"
6. Monitor the workflow execution

**Expected Results:**
- ✅ Workflow completes successfully
- ✅ Code is synced to public repo
- ✅ Tag is created
- ✅ Release workflow is triggered
- ✅ Package is built (if pyproject.toml exists)

### 4.2 Test Automatic Sync

1. In private repo: `Fused-Gaming/breach-checker`
2. Create a test release:
   ```bash
   git tag v0.1.0-test
   git push origin v0.1.0-test
   ```
3. Go to Releases → Create a new release
4. Select the tag `v0.1.0-test`
5. Publish the release
6. Check Actions in **both** repositories

**Expected Results:**
- ✅ Private repo workflow runs and dispatches event
- ✅ Public repo sync workflow runs automatically
- ✅ Release is created in public repo
- ✅ Package is published to PyPI (if configured)

### 4.3 Verify PyPI Upload (if applicable)

1. Go to `https://pypi.org/project/breach-around/`
2. Verify the test version appears
3. Test installation:
   ```bash
   pip install breach-around==0.1.0.test
   ```

**Cleanup Test Release:**
```bash
# Remove test tag from both repos
git tag -d v0.1.0-test
git push origin :refs/tags/v0.1.0-test

# Delete test release from PyPI (if needed)
# This requires using PyPI web interface
```

---

## 🛡️ Step 5: Branch Protection

### 5.1 Protect Main Branch

In public repo: `Fused-Gaming/breach-around`

1. Go to Settings → Branches
2. Click "Add rule" for `main` branch
3. Configure:
   - ✅ Require pull request reviews before merging
   - ✅ Require status checks to pass before merging
   - ✅ Require branches to be up to date before merging
   - ✅ Do not allow bypassing the above settings
   - ✅ Restrict who can push to matching branches (maintainers only)

### 5.2 Tag Protection

1. Go to Settings → Tags
2. Click "Add rule"
3. Pattern: `v*.*.*`
4. Configure:
   - ✅ Restrict tag creation to maintainers only

---

## 🔍 Troubleshooting

### Issue: Sync Workflow Fails with 403 Error

**Cause**: PAT doesn't have correct permissions

**Solution**:
1. Verify PAT has `repo` and `workflow` scopes
2. Ensure PAT hasn't expired
3. Regenerate PAT if necessary
4. Update secrets in repositories

### Issue: PyPI Upload Fails

**Cause**: OIDC not configured or package name conflict

**Solution**:
1. Verify PyPI OIDC publisher settings
2. Check if package name is already taken
3. Verify `pyproject.toml` has correct package name
4. Check workflow logs for specific error

### Issue: Code Not Syncing Completely

**Cause**: rsync exclusions too aggressive

**Solution**:
1. Review exclusion patterns in `sync-from-private.yml`
2. Add any needed directories to the sync
3. Ensure `.gitignore` doesn't block needed files

### Issue: Workflow Doesn't Trigger Automatically

**Cause**: Webhook not firing or workflow disabled

**Solution**:
1. Verify workflow is enabled in Actions tab
2. Check that release was "published" not "draft"
3. Verify PAT has correct permissions
4. Check webhook deliveries in Settings → Webhooks

---

## 🔧 Maintenance

### Rotating Tokens

Tokens should be rotated regularly (recommended: every 90 days)

1. Generate new PAT following Step 1.1
2. Update secrets in both repositories
3. Verify workflows still function
4. Delete old PAT from GitHub settings

### Updating Workflows

When modifying workflows:

1. **Test in a fork first**
2. Make changes to public repo workflow
3. If needed, update private repo workflow template
4. Document changes in commit message
5. Monitor first automated run after changes

### Monitoring

Regularly check:

- [ ] Actions workflow runs for failures
- [ ] PyPI releases are published correctly
- [ ] GitHub releases are created properly
- [ ] Token expiration dates
- [ ] Security advisories for dependencies

---

## 📊 Workflow Status

Track the health of your automation:

### Key Metrics to Monitor

- **Sync Success Rate**: % of successful syncs from private to public
- **Release Success Rate**: % of successful PyPI publishes
- **Time to Release**: Time from private release to PyPI availability
- **Failure Alerts**: Set up notifications for workflow failures

### Recommended Actions

1. **Enable Email Notifications**:
   - GitHub Settings → Notifications
   - Enable "Actions" notifications

2. **Create Dashboard**:
   - Monitor workflow runs
   - Track release cadence
   - Identify bottlenecks

---

## 🎉 Success Checklist

Your setup is complete when:

- [x] Secrets are configured in both repositories
- [x] PyPI publishing is configured (OIDC or token)
- [x] Private repo has trigger workflow installed
- [x] Manual sync test completes successfully
- [x] Automatic sync test completes successfully
- [x] Branch protection is enabled
- [x] Team members are trained on the process
- [x] Monitoring is in place

---

## 📞 Support

If you encounter issues not covered in this guide:

1. Check workflow logs in GitHub Actions
2. Review [GitHub Actions documentation](https://docs.github.com/en/actions)
3. Review [PyPI publishing guide](https://packaging.python.org/en/latest/guides/publishing-package-distribution-releases-using-github-actions-ci-cd-workflows/)
4. Contact repository maintainers

---

**Last Updated**: 2026-01-09
**Document Version**: 1.0.0

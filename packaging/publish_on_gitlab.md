# README

## How to publish project as a package to gitlab registry

This tutorial is for those who want to create a package from their project and publish it in local gitlab registry, so they can later `pip install <package name>` it.

This guide does not include automation and CI/CD. If looking for the latter CI/CD token will be required.

*Gitlab registry* is a place where gitlab stores packages, manages access to the packages and works with different package managers.

# Publishing Python Packages to GitLab Registry

## Overview

This guide shows you how to create a Python package from your project and publish it to the GitLab Package Registry, making it installable via `pip install <package-name>`.

> **Note:** This guide covers manual publishing. For automated CI/CD pipelines, see the CI/CD section at the end.

**What is GitLab Package Registry?**  
A centralized location where GitLab stores packages, manages access control, and integrates with package managers like pip.

---

## Prerequisites

Before you begin, ensure you have:

1. **Project ownership** on GitLab (or at least Maintainer role)
2. **Proper project structure:**
   ```
   root/
   ├── src/
   │   └── <package_name>/
   │       └── __init__.py
   ├── tests/
   ├── pyproject.toml
   └── README.md
   ```

3. **Correct `__init__.py` files** in each Python package/subpackage
   - Missing or incorrect `__init__.py` files will cause import errors later

4. **Deploy Token** (for publishing):
   - Go to: **Project → Settings → Repository → Deploy Tokens**
   - Name: `package-publisher`
   - Scopes: ✅ `read_package_registry` and ✅ `write_package_registry`
   - Save the username and token (you'll only see them once!)

---

## Step 1: Create `pyproject.toml`

Create a `pyproject.toml` file in your project root with the following structure:

```toml
[build-system]
requires = ["setuptools>=68", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "your-package-name"  # Name users will use: pip install your-package-name
version = "0.1.0"            # ⚠️ Must increment for each new release
description = "Internal library Bitrobotics"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
  "numpy>=2.0,<2.3.0",
  "opcua==0.98.13",
  "opencv-python==4.12.0.88",
  "psutil==7.0.0",
  "pydantic==2.11.7",
]

[tool.setuptools]
package-dir = {"" = "src"}  # Tells setuptools to look inside src/

[tool.setuptools.packages.find]
where = ["src"]
```

### Important Configuration Notes:

- **`name`**: The package name users will use for `pip install`. Will be normalized (e.g., `My_Package` → `my-package`)
- **`version`**: ⚠️ **CRITICAL** - Must be incremented before each publish (e.g., `0.1.0` → `0.1.1`), otherwise publishing will fail
- **`dependencies`**: Use `pipreqs` to generate accurate dependencies (not the bloated list from your IDE)

### Finding Real Dependencies:

```shell
pip install pipreqs
pipreqs ./src/your_package_name --force
```

This generates only the dependencies your code actually imports, avoiding unnecessary packages that *PyCharm* may have installed.

---

## Step 2: Build Your Package

Run the build command:

```shell
python -m build
```

This creates two directories:

- `dist/` - Contains `.whl` and `.tar.gz` distribution files
- `src/your_package_name.egg-info/` - Package metadata

> ⚠️ **Important:** Clean these directories before each new build to avoid publishing stale versions:
>
> ```shell
> rm -rf dist/ src/*.egg-info/
> python -m build
> ```

---

## Step 3: Configure Twine for GitLab

Create or edit `~/.pypirc` to configure GitLab authentication:

```ini
[distutils]
index-servers =
    gitlab

[gitlab]
repository = https://gitlab.bitrobotics.com/api/v4/projects/YOUR_PROJECT_ID/packages/pypi
username = YOUR_DEPLOY_TOKEN_USERNAME
password = YOUR_DEPLOY_TOKEN
```

Replace:

- `YOUR_PROJECT_ID` - Find in **Project → Settings → General**
- `YOUR_DEPLOY_TOKEN_USERNAME` - From deploy token creation (e.g., `gitlab+deploy-token-123`)
- `YOUR_DEPLOY_TOKEN` - Token value from deploy token creation

---

## Step 4: Publish to GitLab

Install twine if needed:

```shell
pip install twine
```

Publish your package:

```shell
python -m twine upload -r gitlab dist/*
```

---

## Step 5: Verify Publication

Check if your package was published successfully:

1. Go to your project on *GitLab*
2. Navigate to **Deploy → Package Registry**
3. You should see your package listed with the version number

---

## Installing Your Published Package

### Configure pip

Create or edit `~/.config/pip/pip.conf` (Linux/Mac) or `%APPDATA%\pip\pip.ini` (Windows):

```ini
[global]
extra-index-url = http://YOUR_TOKEN@gitlab.bitrobotics.com/api/v4/projects/YOUR_PROJECT_ID/packages/pypi/simple
trusted-host = gitlab.bitrobotics.com
```

### Install

```shell
pip install your-package-name
```

---

## Troubleshooting

### "File name has already been taken"

You need to increment the version in `pyproject.toml`:

```toml
version = "0.1.1"  # Was 0.1.0
```

### Import errors after installation

Check that all packages have `__init__.py` files and imports are correctly specified.

### 404 Not Found

- Verify your project ID is correct
- Check deploy token has `write_package_registry` scope
- Ensure the package registry is enabled for your project

---

## CI/CD Automation

For automated publishing via GitLab CI/CD, create `.gitlab-ci.yml`:

```yaml
stages:
  - test
  - build

test:
  stage: test
  script:
    - pip install poetry pytest
    - poetry install
    - poetry run pytest

build:
  stage: build
  needs: [test]
  script:
    - pip install build twine
    - python -m build
    - python -m twine upload -r gitlab dist/* -u gitlab-ci-token -p ${CI_JOB_TOKEN}
  only:
    - main
    - master
```

This automatically builds and publishes your package when you push to the main branch.

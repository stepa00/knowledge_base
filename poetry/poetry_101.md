# Poetry 101

## Installation

Poetry should be installed **separately** from your project dependencies using _pipx_. This is critical because:

- **Isolation**: Poetry runs in its own environment, preventing conflicts with your project's dependencies
- **System-wide availability**: Poetry becomes available globally across all projects
- **Clean upgrades**: You can update Poetry without affecting any project environments

> Installing Poetry inside a project's virtual environment can cause dependency conflicts and break Poetry itself.

### Installing pipx

**Linux:**

```bash
# Debian/Ubuntu
sudo apt update
sudo apt install pipx
pipx ensurepath
```

**Windows:**

```powershell
# Using Python pip
python -m pip install --user pipx
python -m pipx ensurepath
```

After installation, restart your terminal.

### Installing Poetry

Once pipx is set up:

```bash
pipx install poetry
```

Verify installation:

```bash
poetry --version
```

### Updating Poetry

```bash
pipx upgrade poetry
```

## Project Setup

### Create a New Project

```bash
poetry new <proj-name>
```

Poetry will generate a basic project structure. There are two main layout options:

**Option 1: Library Structure** - Your package lives inside a `src` folder and is named according to PEP 423:

```bash
poetry-demo
├── pyproject.toml
├── README.md
├── src
│   └── poetry_demo
│       └── __init__.py
└── tests
    └── __init__.py
```

**Option 2: Flat Structure** - All files are stored in the root project folder. This is acceptable for application projects.

> If you are NOT planning to package and publish your project, or if your project is an application, add the following line to `pyproject.toml`:

```toml
[tool.poetry]
package-mode = false
```

### Initialize Poetry in an Existing Project

If your project already exists and you want to add Poetry to it, use:

```bash
cd <your-project>
poetry init
```

Ideally, restructure your folders to match the layouts shown above, but this is not mandatory.

```bash
cd <your-project>
poetry init
```

### pyproject.toml Configuration

This file manages your project configuration, dependencies, and package building. Without it, Poetry (and most other build tools) will not function.

Basic `pyproject.toml` structure:

```toml
[project]
name = "poetry-demo"
version = "0.1.0"
description = "Test project, write short desc. of your project."
authors = [
    {name = "Stepa00", email = "stttep@gmail.com"}
]
readme = "README.md"
license = { text = "Proprietary" }
requires-python = ">=3.12"

[project.urls]
documentation = "http://wiki.bitrobotics.com/some_doc_page"

dependencies = [
    "numpy"
]

[tool.poetry.group.dev.dependencies]
pytest = ""

[tool.poetry]
package-mode = true
packages = [ {include = "poetry_demo", from = "src" } ]

[build-system]
requires = ["poetry-core>=2.0.0,<3.0.0"]
build-backend = "poetry.core.masonry.api"
```

**Important Fields:**

- `name` - Must match the package name to be published
- `version` - Must follow PEP 440 (e.g., 1.0.0, 1.0.0a1, 1.0.0.post1, 1.0.0.dev1)
- `readme` - Required; project will not build without `README.md`
- `dependencies` - Specify runtime dependencies here; see [Dependencies Management](#dependencies-management) for details
- `package-mode` - Enabled by default; must be set to `false` for applications
- `packages` - Explicitly specifies your package location; required when using the `src` layout

## Virtual Environment

By default, Poetry creates virtual environments outside your project in `~/.cache/pypoetry/virtualenvs`. To make Poetry create `.venv` inside your project:

```bash
poetry config virtualenv.in-project true
```

To create a virtual environment and install all dependencies:

```bash
poetry install
```

To find your `.venv` location:

```bash
poetry env info
```

> To check your Poetry configuration: `poetry config --list`

### poetry.lock

`poetry.lock` records the exact versions of every dependency (and sub-dependency) that Poetry installed. It ensures all environments (you, teammates, CI) use identical dependency versions, guaranteeing reproducible builds.

**Important:**

- **DO NOT** commit `poetry.lock` for library packages
- **DO** commit `poetry.lock` for applications to ensure reproducibility

Update the lock file:

```bash
poetry update <optional-dep>
```

Regenerate lock after manual changes:

```bash
poetry lock
```

## Dependencies Management

Add a package to `pyproject.toml` and install it (like `pip install`, but better):

```bash
poetry add <package>
```

Synchronize `poetry.lock` with your `.venv`, installing only the dependencies specified in the lock file (**very useful**):

```bash
poetry sync
```

### Dependency Groups

Organize dependencies into groups to separate development tools from runtime dependencies. To specify a group in `pyproject.toml`:

```toml
[tool.poetry.group.group_name.dependencies]
pytest = ""
```

### Git Dependencies

To use packages directly from Git repositories:

```toml
[project]
# ...
dependencies = [
    "requests @ git+https://github.com/requests/requests.git@next",
    "flask @ git+https://github.com/pallets/flask.git@38eb5d3b",
    "numpy @ git+https://github.com/numpy/numpy.git@v0.13.2",
]
```

Poetry still uses the legacy format for group dependencies with Git URLs:

```toml
[tool.poetry.group.group_name.dependencies]
btr-py-lib = { git = "https://gitlab.bitrobotics.com/libs/btr_py_lib.git", branch = "main"}
```

### Path dependencies

This can be beneficial for those project that include submodules. Go with the following
structure of the project:

```

### Path Dependencies

This is useful for projects that include submodules. Use the following project structure:

```bash
.
├── poetry.lock
├── pyproject.toml
├── README.md
├── src
│   └── test_project
│       └── __init__.py
├── submodules
│   └── package
└── tests
    └── __init__.py
```

Keep all `submodules` in one folder, then reference them in your `src` code:

, where `submodules` are kept in one folder. Than you can use packages in your `src`
like this:

```toml
[project]
# ..
dependencies = [
        "my-package @ file:///absolute/path/to/my-package"
]
```

**Important:** If you want to develop a submodule in parallel, prevent it from being copied into `.venv` by using the `develop` flag:

```toml
[project]
# ...
dependencies = [
    { path = "./submodules/package", develop = true}
]
```

When releasing your project, change this to a stable release version.

### System-Specific Dependencies

Specify dependencies that should only be installed on certain operating systems:

```toml
[project]
# ...
dependencies = [
  'colorama; platform_system == "Windows"',
  'uvloop; platform_system == "Linux"',
]
# ... or ...
[tool.poetry.dependencies]
colorama = { version = "^0.4", markers = "platform_system == 'Windows'" }
uvloop = { version = "^0.20", markers = "platform_system == 'Linux'" }
```

### CUDA Dependencies

For more information on handling CUDA and exclusive extras, see the [Poetry documentation on dependency specification](https://python-poetry.org/docs/dependency-specification/#exclusive-extras).


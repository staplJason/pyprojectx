![pyprojectx](https://pyprojectx.github.io/assets/px.png)

# px: All-inclusive Python Projects (staplJason Fork)

Execute scripts from pyproject.toml, installing tools on-the-fly

> **Note**: This is a personal fork of [pyprojectx/pyprojectx](https://github.com/pyprojectx/pyprojectx). 
> It may include experimental features or customizations. For the official version, visit the upstream repository.

## Installation

Install directly from this GitHub repository:

```bash
# Install latest from main branch with pip
pip install git+https://github.com/staplJason/px.git

# Install latest from main branch with uv (into current environment)
uv pip install git+https://github.com/staplJason/px.git

# Install as a global tool with uv (recommended)
uv tool install git+https://github.com/staplJason/px.git

# Or install a specific release with pip
pip install https://github.com/staplJason/px/releases/download/v3.2.0/px-3.2.0-py3-none-any.whl

# Or install a specific release with uv (into current environment)
uv pip install https://github.com/staplJason/px/releases/download/v3.2.0/px-3.2.0-py3-none-any.whl

# Or install a specific release as a global tool with uv (recommended)
uv tool install https://github.com/staplJason/px/releases/download/v3.2.0/px-3.2.0-py3-none-any.whl
```

## Documentation

For documentation, see the [official pyprojectx documentation](https://pyprojectx.github.io).

## Usage

After installing with `uv tool install`, you can use the global `px` command:

```bash
px --help
px build
px test
```

Or use the traditional project wrapper approach by downloading the wrapper scripts to your project.

## Introduction
Pyprojectx makes it easy to create all-inclusive Python projects; no need to install any tools upfront,
not even Pyprojectx itself!

Tools that are specified within your pyproject.toml file will be installed on demand when invoked from Pyprojectx:
```shell
> ./pw ruff check src
Installed 1 package in 149ms
 + ruff==0.12.7

All checks passed!
```

## Feature highlights
* Reproducible builds by treating tools and utilities as (locked) dev-dependencies
* No global installs, everything is stored inside your project directory (like npm's _node_modules_)
* Bootstrap your entire build process with a small wrapper script (like Gradle's _gradlew_ wrapper)
* Configure shortcuts for routine tasks
* Simple configuration in _pyproject.toml_

Projects can be build/tested/used immediately without explicit installation nor initialization:
```bash
git clone https://github.com/staplJason/px.git
cd px
./pw build
```
![Clone and Build](https://raw.githubusercontent.com/pyprojectx/pyprojectx/main/docs/docs/assets/build.png)

## Wrapper Scripts Installation
One of the key features is that there is no need to install anything explicitly (except a Python 3.9+ interpreter).

`cd` into your project directory and download the
[wrapper scripts](https://github.com/staplJason/px/releases/latest/download/wrappers.zip):

**Linux/Mac**
```bash
curl -LO https://github.com/staplJason/px/releases/latest/download/wrappers.zip && unzip wrappers.zip && rm -f wrappers.zip
```

**Windows**
```powershell
Invoke-WebRequest https://github.com/staplJason/px/releases/latest/download/wrappers.zip -OutFile wrappers.zip; Expand-Archive -Path wrappers.zip -DestinationPath .; Remove-Item -Path wrappers.zip
```

## Getting started
Initialize a new or existing project by adding tools (on Windows, replace `./pw` with `pw`):
```bash
./pw --add uv,ruff,pre-commit,px-utils
./pw --install-context main
# invoke a tool via the wrapper script
./pw uv --version
./pw ruff check src
# or activate the tool context
source .pyprojectx/main/activate
uv --version
ruff check src
```

For reproducible builds and developer experience, it is recommended to lock the versions of the tools
and add the generated _pw.lock_ file to your repository:
```bash
./pw --lock
```

## Create command shortcuts
The _tool.pyprojectx.aliases_ section in _pyproject.toml_ can contain commandline aliases:
```toml
[tool.pyprojectx.aliases]
# convenience shortcuts
run = "uv run"
test = "uv run pytest"
lint = ["ruff check"]
check = ["@lint", "@test"]
```

## Usage
Instead of calling the CLI of a tool directly, prefix it with `./pw` (`pw` on Windows).

Examples:
```shell
./pw uv add --dev pytest
cd src
../pw lint
```

Aliases can be invoked as is or with extra arguments:
```shell
./pw uv run my-script --foo bar
# same as above, but using the run alias
./pw run my-script --foo bar
```

## Why yet another tool?
* As Python noob I had hard times setting up a project and building existing projects
* There is always someone in the team having issues with his setup, either with a specific tool, with Homebrew, pipx, ...
* Using (uv, PDM or Poetry) dev-dependencies to install tools, impacts your production dependencies and can even lead to dependency conflicts
* Different projects often require different versions of the same tool

## Example projects
* This project (using uv)
* [px-demo](https://github.com/pyprojectx/px-demo) (using uv, PDM or Poetry)

## Development
* Build/test:
```shell
git clone https://github.com/pyprojectx/pyprojectx.git
cd pyprojectx
./pw build
```

* Use your local pyprojectx copy in another project: set the path to pyprojectx in the _PYPROJECTX_PACKAGE_ environment variable
  and create a symlink to the wrapper script.
```shell
# Linux, Mac
export PYPROJECTX_PACKAGE=path/to/pyprojectx
ln -s $PYPROJECTX_PACKAGE/src/pyprojectx/wrapper/pw.py pw
# windows
set PYPROJECTX_PACKAGE=path/to/pyprojectx
mklink pw %PYPROJECTX_PACKAGE%\src\pyprojectx\wrapper\pw.py
# or copy the wrapper script if you can't create a symlink on windows
copy %PYPROJECTX_PACKAGE%\src\pyprojectx\wrapper\pw.py pw
```

## Fork Management

This fork includes helper scripts for managing releases and upstream synchronization:

### Creating Releases

Use the `fork-ver` script to create version-tagged releases:

```bash
./fork-ver
```

This script will:
- Show the current state (last tag, commits since last tag, etc.)
- Suggest the next version number
- Create and push a version tag
- Trigger GitHub Actions to build and publish the release

### Syncing with Upstream

Use the `fork-sync` script to pull changes from the upstream repository:

```bash
./fork-sync
```

This script will:
- Fetch changes from the upstream pyprojectx repository
- Show what changes would be merged
- Allow you to review and merge upstream changes

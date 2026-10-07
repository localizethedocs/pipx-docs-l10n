# CLI reference

```none

Install and execute apps from Python packages.

Binaries can either be installed globally into isolated Virtual Environments
or run directly in a temporary Virtual Environment.

Virtual Environment location is /opt/pipx/venvs.
Symlinks to apps are placed in /opt/pipx_bin.
Symlinks to manual pages are placed in /home/runner/.local/share/man.

optional environment variables:
  PIPX_HOME              Overrides default pipx location. Virtual Environments
                        will be installed to $PIPX_HOME/venvs.
  PIPX_GLOBAL_HOME       Used instead of PIPX_HOME when the `--global` option
                        is given.
  PIPX_BIN_DIR           Overrides location of app installations. Apps are
                        symlinked or copied here.
  PIPX_GLOBAL_BIN_DIR    Used instead of PIPX_BIN_DIR when the `--global`
                        option is given.
  PIPX_MAN_DIR           Overrides location of manual pages installations.
                        Manual pages are symlinked or copied here.
  PIPX_GLOBAL_MAN_DIR    Used instead of PIPX_MAN_DIR when the `--global`
                        option is given.
  PIPX_COMPLETION_DIR    Overrides location of shell completion installations.
                        Completion scripts are symlinked or copied here.
  PIPX_GLOBAL_COMPLETION_DIR
                        Used instead of PIPX_COMPLETION_DIR when the
                        `--global` option is given.
  PIPX_DEFAULT_PYTHON    Overrides default python used for commands.
  PIPX_DEFAULT_BACKEND   Overrides which backend (`pip` or `uv`) is used for
                        new venvs.
  PIPX_COOLDOWN          Default `--cooldown` in days for the commands that
                        accept it.
  PIPX_DISABLE_SHARED_LIBS_AUTO_UPGRADE
                        Skips automatic shared library upgrades.
  PIPX_USE_EMOJI         Overrides emoji behavior. Default value varies based
                        on platform.
```

```default
pipx [-h] [--version]
     {install,install-all,uninject,inject,expose,unexpose,pin,unpin,upgrade,upgrade-all,upgrade-shared,uninstall,uninstall-all,reset,reinstall,reinstall-all,health,repair,list,interpreter,cache,manifest,run,exec,runpip,ensurepath,environment,completions,help} ...
```

## pipx options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--version`**]() - Print version and exit

## pipx install

```none
The install command is the preferred way to globally install apps
from Python packages and PEP 723 scripts. It creates an isolated virtual
environment for the source, then ensures its apps are
accessible on your $PATH. The package's manual pages installed in
share/man/man[1-9] can be viewed with man on an operating system where
it is available and the path in the environment variable `PIPX_MAN_DIR`
(default: /home/runner/.local/share/man) is in the man search path
($MANPATH).

The result: apps you can run from anywhere, located in packages
you can cleanly upgrade or uninstall. Guaranteed to not have
dependency version conflicts or interfere with your OS's python
packages. 'sudo' is not required to do this.

pipx install PACKAGE_SPEC ...
pipx install --python PYTHON PACKAGE_SPEC
pipx install VCS_URL
pipx install ./LOCAL_PATH
pipx install ./SCRIPT.py
pipx install ZIP_FILE
pipx install TAR_GZ_FILE

Package specs are passed to the selected backend. PEP 723 scripts are
installed with their declared dependencies.

Virtual Environments will be installed to `$PIPX_HOME/venvs`.
The default pipx home location is /home/runner/.local/share/pipx and can
be overridden by setting the environment variable `PIPX_HOME`.
If the `--global` option is used, the default pipx home location
instead is /opt/pipx and can be overridden
by setting the environment variable `PIPX_GLOBAL_HOME`.

The default app location is /home/runner/.local/bin and can be
overridden by setting the environment variable `PIPX_BIN_DIR`.
If the `--global` option is used, the default app location instead
is /usr/local/bin and can be overridden by
setting the environment variable `PIPX_GLOBAL_BIN_DIR`.

The default manual pages location is /home/runner/.local/share/man and
can be overridden by setting the environment variable `PIPX_MAN_DIR`.
If the `--global` option is used, the default manual pages location
instead is /usr/local/share/man and can be overridden
by setting the environment variable `PIPX_GLOBAL_MAN_DIR`.

The default python executable used to install a package is
/home/runner/work/pipx-docs-l10n/pipx-docs-l10n/.conda/bin/python3.14 and can be overridden
by setting the environment variable `PIPX_DEFAULT_PYTHON`.
```

```default
pipx install [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] [--include-deps |
             --include-resources-from PACKAGE] [--force] [--upgrade]
             [--upgrade-strategy {only-if-needed,eager}] [--suffix SUFFIX] [--python PYTHON]
             [--fetch-python {always,missing,never} | --fetch-missing-python]
             [--preinstall PREINSTALL] [--app APP] [--lock LOCK] [--system-site-packages]
             [--index-url INDEX_URL] [--editable] [--pip-args PIP_ARGS] [--cooldown DAYS]
             [--backend {pip,uv}] [--output {human,json}]
             package_spec [package_spec ...]
```

### pipx install positional arguments

* [**`package_spec`**]() - package name(s), PEP 723 script(s), or installation spec(s)

### pipx install options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--include-deps`**]() - Include apps and manual pages from all dependencies
* [**`--include-resources-from`**]() `PACKAGE` - Include apps and manual pages from this dependency. Repeat for multiple dependencies (default: `[]`)
* [**`--force`**](), [**`-f`**]() - Modify existing virtual environment and files in PIPX_BIN_DIR and PIPX_MAN_DIR
* [**`--upgrade`**](), [**`-U`**]() - Upgrade or downgrade an existing package when its version does not satisfy the supplied spec
* [**`--upgrade-strategy`**]() `{only-if-needed,eager}` - How dependency upgrades are handled when --upgrade changes an existing package
* [**`--suffix`**]() `SUFFIX` - Optional suffix for virtual environment and executable names. (default: ``)
* [**`--python`**]() `PYTHON` - Python to install with. Possible values can be the executable name (python3.11), the version of an available system Python or to pass to py launcher (3.11), or the full path to the executable. Requires Python 3.10 or above.
* [**`--fetch-python`**]() `{always,missing,never}` - When to fetch a standalone Python build from python-build-standalone. `'always'` always downloads (ignores any system Python); `'missing'` downloads only when the requested version is not found locally; `'never'` never downloads. Defaults to PIPX_FETCH_PYTHON, or `'never'`.
* [**`--fetch-missing-python`**]() - Deprecated: alias for --fetch-python=missing.
* [**`--preinstall`**]() `PREINSTALL` - Optional package to be installed into the Virtual Environment before installing the main package. Use this flag multiple times if you want to preinstall multiple packages.
* [**`--app`**]() `APP` - Require this application entry point after installation. Repeat to require multiple entry points.
* [**`--lock`**]() `LOCK` - Install the environment from an explicit pylock.toml file.
* [**`--system-site-packages`**]() - Give the virtual environment access to the system site-packages dir.
* [**`--index-url`**]() `INDEX_URL`, [**`-i`**]() `INDEX_URL` - Base URL of Python Package Index
* [**`--editable`**](), [**`-e`**]() - Install a project in editable mode
* [**`--pip-args`**]() `PIP_ARGS` - Arbitrary pip arguments to pass directly to pip install/upgrade commands. Under the uv backend a subset is translated (--index-url/-i, --extra-index-url, --find-links/-f, --trusted-host, --no-binary, --only-binary, --pre, --upgrade/-U, --no-cache-dir); other flags raise an error so behaviour stays explicit.
* [**`--cooldown`**]() `DAYS` - Ignore index artifacts uploaded fewer than DAYS days ago. Defaults to PIPX_COOLDOWN; pass 0 to opt out.
* [**`--backend`**]() `{pip,uv}` - Backend used for creating venvs and managing packages. Defaults to PIPX_DEFAULT_BACKEND when set, else `'uv'` when uv is available (via the pipx[uv] extra or on PATH), else `'pip'`. Existing venvs always honor their recorded backend unless this flag is given.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx install-all

Installs all the packages according to spec metadata file.

```default
pipx install-all [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] [--force]
                 [--python PYTHON] [--fetch-python {always,missing,never} |
                 --fetch-missing-python] [--system-site-packages] [--index-url INDEX_URL]
                 [--editable] [--pip-args PIP_ARGS] [--cooldown DAYS] [--backend {pip,uv}]
                 spec_metadata_file
```

### pipx install-all positional arguments

* [**`spec_metadata_file`**]() - Spec metadata file generated from pipx list --output json

### pipx install-all options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--force`**](), [**`-f`**]() - Modify existing virtual environment and files in PIPX_BIN_DIR and PIPX_MAN_DIR
* [**`--python`**]() `PYTHON` - Python to install with. Possible values can be the executable name (python3.11), the version of an available system Python or to pass to py launcher (3.11), or the full path to the executable. Requires Python 3.10 or above.
* [**`--fetch-python`**]() `{always,missing,never}` - When to fetch a standalone Python build from python-build-standalone. `'always'` always downloads (ignores any system Python); `'missing'` downloads only when the requested version is not found locally; `'never'` never downloads. Defaults to PIPX_FETCH_PYTHON, or `'never'`.
* [**`--fetch-missing-python`**]() - Deprecated: alias for --fetch-python=missing.
* [**`--system-site-packages`**]() - Give the virtual environment access to the system site-packages dir.
* [**`--index-url`**]() `INDEX_URL`, [**`-i`**]() `INDEX_URL` - Base URL of Python Package Index
* [**`--editable`**](), [**`-e`**]() - Install a project in editable mode
* [**`--pip-args`**]() `PIP_ARGS` - Arbitrary pip arguments to pass directly to pip install/upgrade commands. Under the uv backend a subset is translated (--index-url/-i, --extra-index-url, --find-links/-f, --trusted-host, --no-binary, --only-binary, --pre, --upgrade/-U, --no-cache-dir); other flags raise an error so behaviour stays explicit.
* [**`--cooldown`**]() `DAYS` - Ignore index artifacts uploaded fewer than DAYS days ago. Defaults to PIPX_COOLDOWN; pass 0 to opt out.
* [**`--backend`**]() `{pip,uv}` - Backend used for creating venvs and managing packages. Defaults to PIPX_DEFAULT_BACKEND when set, else `'uv'` when uv is available (via the pipx[uv] extra or on PATH), else `'pip'`. Existing venvs always honor their recorded backend unless this flag is given.

## pipx uninject

Uninstalls injected packages from an existing pipx-managed virtual environment.

```default
pipx uninject [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] [--leave-deps]
              [--output {human,json}]
              ENVIRONMENT dependencies [dependencies ...]
```

### pipx uninject positional arguments

* [**`ENVIRONMENT`**]() - Name of the existing pipx-managed environment to uninject from
* [**`dependencies`**]() - the package names to uninject from the environment

### pipx uninject options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--leave-deps`**]() - Only uninstall the main injected package but leave its dependencies installed.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx inject

Installs packages to an existing pipx-managed virtual environment.

```default
pipx inject [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] [-r file]
            [--include-apps] [--include-deps | --include-resources-from PACKAGE]
            [--system-site-packages] [--index-url INDEX_URL] [--editable]
            [--pip-args PIP_ARGS] [--cooldown DAYS] [--force] [--with-suffix]
            [--backend {pip,uv}] [--output {human,json}]
            ENVIRONMENT [dependencies ...]
```

### pipx inject positional arguments

* [**`ENVIRONMENT`**]() - Name of the existing pipx-managed environment to inject into
* [**`dependencies`**]() - the packages to inject into the environment–either package name or pip package spec

### pipx inject options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`-r`**]() `file`, [**`--requirement`**]() `file` - file containing the packages to inject into the Virtual Environment–one package name or pip package spec per line. May be specified multiple times. (default: `[]`)
* [**`--include-apps`**]() - Add apps from the injected packages onto your PATH and expose their manual pages
* [**`--include-deps`**]() - Include apps and manual pages from all dependencies
* [**`--include-resources-from`**]() `PACKAGE` - Include apps and manual pages from this dependency. Repeat for multiple dependencies (default: `[]`)
* [**`--system-site-packages`**]() - Give the virtual environment access to the system site-packages dir.
* [**`--index-url`**]() `INDEX_URL`, [**`-i`**]() `INDEX_URL` - Base URL of Python Package Index
* [**`--editable`**](), [**`-e`**]() - Install a project in editable mode
* [**`--pip-args`**]() `PIP_ARGS` - Arbitrary pip arguments to pass directly to pip install/upgrade commands. Under the uv backend a subset is translated (--index-url/-i, --extra-index-url, --find-links/-f, --trusted-host, --no-binary, --only-binary, --pre, --upgrade/-U, --no-cache-dir); other flags raise an error so behaviour stays explicit.
* [**`--cooldown`**]() `DAYS` - Ignore index artifacts uploaded fewer than DAYS days ago. Defaults to PIPX_COOLDOWN; pass 0 to opt out.
* [**`--force`**](), [**`-f`**]() - Modify existing virtual environment and files in PIPX_BIN_DIR and PIPX_MAN_DIR
* [**`--with-suffix`**]() - Add the suffix (if given) of the Virtual Environment to the packages to inject
* [**`--backend`**]() `{pip,uv}` - Backend used for creating venvs and managing packages. Defaults to PIPX_DEFAULT_BACKEND when set, else `'uv'` when uv is available (via the pipx[uv] extra or on PATH), else `'pip'`. Existing venvs always honor their recorded backend unless this flag is given.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx expose

Relink each recorded app and manual page without rebuilding its environment.

```default
pipx expose [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
            [--output {human,json}]
            ENVIRONMENT
```

### pipx expose positional arguments

* [**`ENVIRONMENT`**]() - Environment to expose

### pipx expose options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx unexpose

Remove global links or copies while retaining the managed environment.

```default
pipx unexpose [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
              [--output {human,json}]
              ENVIRONMENT
```

### pipx unexpose positional arguments

* [**`ENVIRONMENT`**]() - Environment to hide

### pipx unexpose options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx pin

Pin the specified package to prevent it from being upgraded

```default
pipx pin [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] [--injected-only]
         [--skip SKIP [SKIP ...]] [--output {human,json}]
         ENVIRONMENT
```

### pipx pin positional arguments

* [**`ENVIRONMENT`**]() - Environment to pin

### pipx pin options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--injected-only`**]() - Pin injected packages in venv only, so that they will not be upgraded during upgrade operations. Note that this should not be passed if you wish to pin both main package and injected packages.
* [**`--skip`**]() `SKIP [SKIP ...]` - Skip these packages. Implies --injected-only. (default: `[]`)
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx unpin

Unpin the specified package and all injected packages in its venv to allow them to be upgraded

```default
pipx unpin [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] [--output {human,json}]
           ENVIRONMENT
```

### pipx unpin positional arguments

* [**`ENVIRONMENT`**]() - Environment to unpin

### pipx unpin options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx upgrade

Upgrade package(s) in pipx-managed Virtual Environment(s) by running ‘pip install --upgrade PACKAGE’

```default
pipx upgrade [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] [--include-injected]
             [--force] [--system-site-packages] [--index-url INDEX_URL] [--editable]
             [--pip-args PIP_ARGS] [--cooldown DAYS] [--install] [--python PYTHON]
             [--fetch-python {always,missing,never} | --fetch-missing-python]
             [--backend {pip,uv}] [--output {human,json}]
             packages [packages ...]
```

### pipx upgrade positional arguments

* [**`packages`**]() - package names(s) to upgrade

### pipx upgrade options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--include-injected`**]() - Also upgrade packages injected into the main app’s environment
* [**`--force`**](), [**`-f`**]() - Modify existing virtual environment and files in PIPX_BIN_DIR and PIPX_MAN_DIR
* [**`--system-site-packages`**]() - Give the virtual environment access to the system site-packages dir.
* [**`--index-url`**]() `INDEX_URL`, [**`-i`**]() `INDEX_URL` - Base URL of Python Package Index
* [**`--editable`**](), [**`-e`**]() - Install a project in editable mode
* [**`--pip-args`**]() `PIP_ARGS` - Arbitrary pip arguments to pass directly to pip install/upgrade commands. Under the uv backend a subset is translated (--index-url/-i, --extra-index-url, --find-links/-f, --trusted-host, --no-binary, --only-binary, --pre, --upgrade/-U, --no-cache-dir); other flags raise an error so behaviour stays explicit.
* [**`--cooldown`**]() `DAYS` - Ignore index artifacts uploaded fewer than DAYS days ago. Defaults to PIPX_COOLDOWN; pass 0 to opt out.
* [**`--install`**]() - Install package spec if missing
* [**`--python`**]() `PYTHON` - Python to install with. Possible values can be the executable name (python3.11), the version of an available system Python or to pass to py launcher (3.11), or the full path to the executable. Requires Python 3.10 or above.
* [**`--fetch-python`**]() `{always,missing,never}` - When to fetch a standalone Python build from python-build-standalone. `'always'` always downloads (ignores any system Python); `'missing'` downloads only when the requested version is not found locally; `'never'` never downloads. Defaults to PIPX_FETCH_PYTHON, or `'never'`.
* [**`--fetch-missing-python`**]() - Deprecated: alias for --fetch-python=missing.
* [**`--backend`**]() `{pip,uv}` - Backend used for creating venvs and managing packages. Defaults to PIPX_DEFAULT_BACKEND when set, else `'uv'` when uv is available (via the pipx[uv] extra or on PATH), else `'pip'`. Existing venvs always honor their recorded backend unless this flag is given.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx upgrade-all

Upgrades all packages within their virtual environments by running ‘pip install --upgrade PACKAGE’

```default
pipx upgrade-all [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
                 [--include-injected] [--skip SKIP [SKIP ...]] [--force]
                 [--system-site-packages] [--index-url INDEX_URL] [--editable]
                 [--pip-args PIP_ARGS] [--cooldown DAYS] [--backend {pip,uv}]
                 [--output {human,json}]
```

### pipx upgrade-all options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--include-injected`**]() - Also upgrade packages injected into the main app’s environment
* [**`--skip`**]() `SKIP [SKIP ...]` - skip these packages (default: `[]`)
* [**`--force`**](), [**`-f`**]() - Modify existing virtual environment and files in PIPX_BIN_DIR and PIPX_MAN_DIR
* [**`--system-site-packages`**]() - Give the virtual environment access to the system site-packages dir.
* [**`--index-url`**]() `INDEX_URL`, [**`-i`**]() `INDEX_URL` - Base URL of Python Package Index
* [**`--editable`**](), [**`-e`**]() - Install a project in editable mode
* [**`--pip-args`**]() `PIP_ARGS` - Arbitrary pip arguments to pass directly to pip install/upgrade commands. Under the uv backend a subset is translated (--index-url/-i, --extra-index-url, --find-links/-f, --trusted-host, --no-binary, --only-binary, --pre, --upgrade/-U, --no-cache-dir); other flags raise an error so behaviour stays explicit.
* [**`--cooldown`**]() `DAYS` - Ignore index artifacts uploaded fewer than DAYS days ago. Defaults to PIPX_COOLDOWN; pass 0 to opt out.
* [**`--backend`**]() `{pip,uv}` - Backend used for creating venvs and managing packages. Defaults to PIPX_DEFAULT_BACKEND when set, else `'uv'` when uv is available (via the pipx[uv] extra or on PATH), else `'pip'`. Existing venvs always honor their recorded backend unless this flag is given.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx upgrade-shared

Upgrade shared libraries.

```default
pipx upgrade-shared [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
                    [--pip-args PIP_ARGS] [--output {human,json}]
```

### pipx upgrade-shared options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--pip-args`**]() `PIP_ARGS` - Arbitrary pip arguments to pass directly to pip install/upgrade commands
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx uninstall

Uninstalls a pipx-managed Virtual Environment by deleting it and any files that point to its apps.

```default
pipx uninstall [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
               [--output {human,json}]
               ENVIRONMENT
```

### pipx uninstall positional arguments

* [**`ENVIRONMENT`**]()

### pipx uninstall options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx uninstall-all

Uninstall all pipx-managed packages

```default
pipx uninstall-all [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
                   [--output {human,json}]
```

### pipx uninstall-all options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx reset

Uninstall every pipx-managed package and remove the shared libraries, the caches, the standalone interpreters, the logs and the trash, which returns pipx to the state it had when it was installed.

```default
pipx reset [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] [--yes] [--dry-run]
           [--output {human,json}]
```

### pipx reset options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--yes`**](), [**`-y`**]() - Reset without asking for confirmation
* [**`--dry-run`**]() - Report what a reset would remove and stop
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx reinstall

```none
Reinstalls a package.

Package is uninstalled, then installed with pipx install PACKAGE
with the same options used in the original install of PACKAGE.
```

```default
pipx reinstall [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] [--python PYTHON]
               [--fetch-python {always,missing,never} | --fetch-missing-python]
               [--backend {pip,uv}] [--output {human,json}]
               ENVIRONMENT
```

### pipx reinstall positional arguments

* [**`ENVIRONMENT`**]()

### pipx reinstall options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--python`**]() `PYTHON` - Python to install with. Possible values can be the executable name (python3.11), the version of an available system Python or to pass to py launcher (3.11), or the full path to the executable. Requires Python 3.10 or above.
* [**`--fetch-python`**]() `{always,missing,never}` - When to fetch a standalone Python build from python-build-standalone. `'always'` always downloads (ignores any system Python); `'missing'` downloads only when the requested version is not found locally; `'never'` never downloads. Defaults to PIPX_FETCH_PYTHON, or `'never'`.
* [**`--fetch-missing-python`**]() - Deprecated: alias for --fetch-python=missing.
* [**`--backend`**]() `{pip,uv}` - Backend used for creating venvs and managing packages. Defaults to PIPX_DEFAULT_BACKEND when set, else `'uv'` when uv is available (via the pipx[uv] extra or on PATH), else `'pip'`. Existing venvs always honor their recorded backend unless this flag is given.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx reinstall-all

```none
Reinstalls all packages.

Packages are uninstalled, then installed with pipx install PACKAGE
with the same options used in the original install of PACKAGE.
This is useful if you upgraded to a new version of Python and want
all your packages to use the latest as well.
```

```default
pipx reinstall-all [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
                   [--python PYTHON] [--fetch-python {always,missing,never} |
                   --fetch-missing-python] [--skip SKIP [SKIP ...]] [--backend {pip,uv}]
                   [--output {human,json}]
```

### pipx reinstall-all options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--python`**]() `PYTHON` - Python to install with. Possible values can be the executable name (python3.11), the version of an available system Python or to pass to py launcher (3.11), or the full path to the executable. Requires Python 3.10 or above.
* [**`--fetch-python`**]() `{always,missing,never}` - When to fetch a standalone Python build from python-build-standalone. `'always'` always downloads (ignores any system Python); `'missing'` downloads only when the requested version is not found locally; `'never'` never downloads. Defaults to PIPX_FETCH_PYTHON, or `'never'`.
* [**`--fetch-missing-python`**]() - Deprecated: alias for --fetch-python=missing.
* [**`--skip`**]() `SKIP [SKIP ...]` - skip these packages (default: `[]`)
* [**`--backend`**]() `{pip,uv}` - Backend used for creating venvs and managing packages. Defaults to PIPX_DEFAULT_BACKEND when set, else `'uv'` when uv is available (via the pipx[uv] extra or on PATH), else `'pip'`. Existing venvs always honor their recorded backend unless this flag is given.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx health

Check whether installed package environments can run their Python interpreter.

```default
pipx health [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
            [--output {human,json}]
            [packages ...]
```

### pipx health positional arguments

* [**`packages`**]() - Installed packages to check

### pipx health options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx repair

Reinstall packages whose environments cannot run their Python interpreter.

```default
pipx repair [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] [--python PYTHON]
            [--fetch-python {always,missing,never} | --fetch-missing-python]
            [--backend {pip,uv}] [--output {human,json}]
            [packages ...]
```

### pipx repair positional arguments

* [**`packages`**]() - Installed packages to repair

### pipx repair options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--python`**]() `PYTHON` - Python to install with. Possible values can be the executable name (python3.11), the version of an available system Python or to pass to py launcher (3.11), or the full path to the executable. Requires Python 3.10 or above.
* [**`--fetch-python`**]() `{always,missing,never}` - When to fetch a standalone Python build from python-build-standalone. `'always'` always downloads (ignores any system Python); `'missing'` downloads only when the requested version is not found locally; `'never'` never downloads. Defaults to PIPX_FETCH_PYTHON, or `'never'`.
* [**`--fetch-missing-python`**]() - Deprecated: alias for --fetch-python=missing.
* [**`--backend`**]() `{pip,uv}` - Backend used for creating venvs and managing packages. Defaults to PIPX_DEFAULT_BACKEND when set, else `'uv'` when uv is available (via the pipx[uv] extra or on PATH), else `'pip'`. Existing venvs always honor their recorded backend unless this flag is given.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx list

List packages and apps installed with pipx

```default
pipx list [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] [--include-injected]
          [--outdated] [--short | --pinned] [--output {human,json} | --json]
          [packages ...]
```

### pipx list positional arguments

* [**`packages`**]() - Installed packages to list

### pipx list options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--include-injected`**]() - Show packages injected into the main app’s environment
* [**`--outdated`**]() - List packages with an available upgrade.
* [**`--short`**]() - List packages only.
* [**`--pinned`**]() - List pinned packages only. Pass --include-injected at the same time to list injected packages that were pinned.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)
* [**`--json`**]() - Print the installed-package snapshot as JSON, the format install-all consumes.

## pipx interpreter

Interact with interpreters managed by pipx

```default
pipx interpreter [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
                 {list,prune,upgrade} ...
```

### pipx interpreter options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.

## pipx interpreter list

List available interpreters

```default
pipx interpreter list [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
                      [--output {human,json}]
```

### pipx interpreter list options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx interpreter prune

Prune unused interpreters

```default
pipx interpreter prune [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
                       [--output {human,json}]
```

### pipx interpreter prune options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx interpreter upgrade

Upgrade installed interpreters to the latest available micro/patch version

```default
pipx interpreter upgrade [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
                         [--output {human,json}]
```

### pipx interpreter upgrade options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx cache

Manage cached run environments

```default
pipx cache [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] {dir,purge} ...
```

### pipx cache options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.

## pipx cache dir

Show the cache directory

```default
pipx cache dir [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
               [--output {human,json}]
```

### pipx cache dir options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx cache purge

Remove cached run environments

```default
pipx cache purge [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
                 [--output {human,json}]
```

### pipx cache purge options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx manifest

Manage tools declared in an explicit manifest

```default
pipx manifest [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] {lock,sync} ...
```

### pipx manifest options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.

## pipx manifest lock

Resolve each locked tool in an explicit manifest into a separate PEP 751 lock file.

```default
pipx manifest lock [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
                   [--output {human,json}]
                   manifest
```

### pipx manifest lock positional arguments

* [**`manifest`**]() - Path to the tool manifest.

### pipx manifest lock options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx manifest sync

Install, upgrade, or downgrade the tools declared in an explicit manifest.

```default
pipx manifest sync [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] [--prune]
                   [--backend {pip,uv}] [--output {human,json}]
                   manifest
```

### pipx manifest sync positional arguments

* [**`manifest`**]() - Path to the tool manifest.

### pipx manifest sync options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--prune`**]() - Uninstall environments absent from the manifest.
* [**`--backend`**]() `{pip,uv}` - Backend used for creating venvs and managing packages. Defaults to PIPX_DEFAULT_BACKEND when set, else `'uv'` when uv is available (via the pipx[uv] extra or on PATH), else `'pip'`. Existing venvs always honor their recorded backend unless this flag is given.
* [**`--output`**]() `{human,json}` - Select the output format. (default: `OutputFormat.HUMAN`)

## pipx run

```none
Download the latest version of a package to a temporary virtual environment,
then run an app from it. The environment will be cached
and reused for up to 14 days. This
means subsequent calls to 'run' for the same package will be faster
since they can reuse the cached Virtual Environment.

In support of PEP 582 'run' will use apps found in a local __pypackages__
directory, if present. Please note that this behavior is experimental,
and acts as a companion tool to pythonloc. It may be modified or
removed in the future. See https://github.com/cs01/pythonloc.
```

```default
pipx run [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
         [--no-cache | --refresh] [--no-path-check]
         [--python-args ARGS] [--path] [--pypackages] [--with WITH_]
         [--spec SPEC] [--python PYTHON]
         [--fetch-python {always,missing,never} |
         --fetch-missing-python] [--system-site-packages]
         [--index-url INDEX_URL] [--editable] [--pip-args PIP_ARGS]
         [--cooldown DAYS] [--backend {pip,uv}]
         app ...
```

### pipx run positional arguments

* [**`app ...`**]() - app/package name and any arguments to be passed to it (default: `[]`)

### pipx run options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--no-cache`**]() - Do not reuse cached virtual environment if it exists
* [**`--refresh`**]() - Rebuild and cache the virtual environment
* [**`--no-path-check`**]() - Do not check whether the app is already on PATH
* [**`--python-args`**]() `ARGS` - Arguments to pass to the Python interpreter that runs the application (default: `[]`)
* [**`--path`**]() - Interpret app name as a local path
* [**`--pypackages`**]() - Require app to be run from local \_\_pypackages_\_ directory
* [**`--with`**]() `WITH_` - Extra dependencies to add to the temporary environment (default: `[]`)
* [**`--spec`**]() `SPEC` - The package name or specific installation source passed to pip.
  Runs pip install -U SPEC.
  For example --spec mypackage==2.0.0 or --spec  git+https://github.com/user/repo.git@branch
* [**`--python`**]() `PYTHON` - Python to install with. Possible values can be the executable name (python3.11), the version of an available system Python or to pass to py launcher (3.11), or the full path to the executable. Requires Python 3.10 or above.
* [**`--fetch-python`**]() `{always,missing,never}` - When to fetch a standalone Python build from python-build-standalone. `'always'` always downloads (ignores any system Python); `'missing'` downloads only when the requested version is not found locally; `'never'` never downloads. Defaults to PIPX_FETCH_PYTHON, or `'never'`.
* [**`--fetch-missing-python`**]() - Deprecated: alias for --fetch-python=missing.
* [**`--system-site-packages`**]() - Give the virtual environment access to the system site-packages dir.
* [**`--index-url`**]() `INDEX_URL`, [**`-i`**]() `INDEX_URL` - Base URL of Python Package Index
* [**`--editable`**](), [**`-e`**]() - Install a project in editable mode
* [**`--pip-args`**]() `PIP_ARGS` - Arbitrary pip arguments to pass directly to pip install/upgrade commands. Under the uv backend a subset is translated (--index-url/-i, --extra-index-url, --find-links/-f, --trusted-host, --no-binary, --only-binary, --pre, --upgrade/-U, --no-cache-dir); other flags raise an error so behaviour stays explicit.
* [**`--cooldown`**]() `DAYS` - Ignore index artifacts uploaded fewer than DAYS days ago. Defaults to PIPX_COOLDOWN; pass 0 to opt out.
* [**`--backend`**]() `{pip,uv}` - Backend used for creating venvs and managing packages. Defaults to PIPX_DEFAULT_BACKEND when set, else `'uv'` when uv is available (via the pipx[uv] extra or on PATH), else `'pip'`. Existing venvs always honor their recorded backend unless this flag is given.

## pipx exec

Run an application from an existing pipx environment

```default
pipx exec [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] ENVIRONMENT APP ...
```

### pipx exec positional arguments

* [**`ENVIRONMENT`**]() - Name of the existing pipx environment
* [**`APP`**]() - Application to run
* [**`app_args`**]() - Arguments to pass to the application (default: `[]`)

### pipx exec options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.

## pipx runpip

Run pip in an existing pipx-managed Virtual Environment

```default
pipx runpip [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] package ...
```

### pipx runpip positional arguments

* [**`package`**]() - Name of the existing pipx-managed Virtual Environment to run pip in
* [**`pipargs`**]() - Arguments to forward to pip command (default: `[]`)

### pipx runpip options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.

## pipx ensurepath

Ensure directory where pipx stores apps is in your PATH environment variable. Also if pipx was installed via \`pip install --user\`, ensure pipx itself is in your PATH. This command may modify your shell configuration, such as ‘~/.bashrc’, or the system PATH configuration when used with \`--global\`.

```default
pipx ensurepath [-h] [--quiet] [--verbose] [--skip-maintenance] [--global] [--prepend]
                [--force] [--all-shells] [--dry-run]
```

### pipx ensurepath options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--prepend`**]() - Prepend directories to your PATH instead of appending. This is useful if you want to prioritize pipx apps over system apps.
* [**`--force`**](), [**`-f`**]() - Add text to your shell’s config file even if it looks like your PATH already contains paths to pipx and pipx-install apps.
* [**`--all-shells`**]() - Add directories to PATH in all shells instead of just the current one.
* [**`--dry-run`**]() - Report what would be added to PATH without modifying PATH or any shell configuration file.

## pipx environment

```none
Prints the names and current values of environment variables used by pipx,
followed by internal pipx variables which are derived from the environment
variables and platform specific default values.

Available variables:
PIPX_HOME, PIPX_GLOBAL_HOME, PIPX_BIN_DIR, PIPX_GLOBAL_BIN_DIR,
PIPX_MAN_DIR, PIPX_COMPLETION_DIR, PIPX_GLOBAL_MAN_DIR,
PIPX_GLOBAL_COMPLETION_DIR, PIPX_SHARED_LIBS, PIPX_DEFAULT_PYTHON,
PIPX_DEFAULT_BACKEND, PIPX_COOLDOWN, PIPX_FETCH_MISSING_PYTHON,
PIPX_FETCH_PYTHON, PIPX_DISABLE_SHARED_LIBS_AUTO_UPGRADE,
PIPX_USE_EMOJI, PIPX_MAX_LOGS
```

```default
pipx environment [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
                 [--value VARIABLE]
```

### pipx environment options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.
* [**`--value`**]() `VARIABLE`, [**`-V`**]() `VARIABLE` - Print the value of the variable.

## pipx completions

Print instructions on enabling shell completions for pipx

```default
pipx completions [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
```

### pipx completions options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.

## pipx help

Show help for pipx or a command

```default
pipx help [-h] [--quiet] [--verbose] [--skip-maintenance] [--global]
```

### pipx help options

* [**`-h`**](), [**`--help`**]() - show this help message and exit
* [**`--quiet`**](), [**`-q`**]() - Give less output. May be used multiple times corresponding to the ERROR and CRITICAL logging levels. The count maxes out at 2. (default: `0`)
* [**`--verbose`**](), [**`-v`**]() - Give more output. May be used multiple times corresponding to the INFO, DEBUG and NOTSET logging levels. The count maxes out at 3. (default: `0`)
* [**`--skip-maintenance`**]() - Do not upgrade shared libraries; use bundled pip when creating them.
* [**`--global`**]() - Perform action globally for all users.

# Configure paths

Choose where pipx stores environments, apps, manual pages, and shell completions, and how it installs for all users.

## Directory layout

pipx splits its files across four locations, each with an environment-variable override and a `--global` counterpart:

| Variable              | Holds                                     | Default               |
|-----------------------|-------------------------------------------|-----------------------|
| `PIPX_HOME`           | Virtual environments and shared libraries | `~/.local/share/pipx` |
| `PIPX_BIN_DIR`        | App links                                 | `~/.local/bin`        |
| `PIPX_MAN_DIR`        | Manual page links                         | `~/.local/share/man`  |
| `PIPX_COMPLETION_DIR` | Shell completion links                    | `~/.local/share`      |

The default `PIPX_HOME` is typically `~/.local/share/pipx` on Linux, `~/Library/Application Support/pipx` on
macOS, and `%USERPROFILE%\AppData\Local\pipx\pipx` on Windows. Set any variable to override its location.

<a id="platformdirs-migration"></a>

## platformdirs migration

After version 1.16.0, pipx moved its default paths from `~/.local/pipx` to per-platform user directories from the
[platformdirs](https://pypi.org/project/platformdirs/) library:

| Old path               | New path                                   |
|------------------------|--------------------------------------------|
| `~/.local/pipx/.trash` | `platformdirs.user_data_dir()/pipx/trash`  |
| `~/.local/pipx/shared` | `platformdirs.user_data_dir()/pipx/shared` |
| `~/.local/pipx/venvs`  | `platformdirs.user_data_dir()/pipx/venvs`  |
| `~/.local/pipx/.cache` | `platformdirs.user_cache_dir()/pipx`       |
| `~/.local/pipx/logs`   | `platformdirs.user_log_dir("pipx")`        |

For compatibility, if `~/.local/pipx` exists (on Windows also `~\pipx`), pipx keeps using it. See the [platformdirs
platform table](https://platformdirs.readthedocs.io/en/latest/api.html#platforms) for the resolved directories. On
Linux and macOS, `platformdirs` reads `XDG_DATA_HOME` and `XDG_CACHE_HOME`, and on Linux also `XDG_STATE_HOME`
for the log directory, so exporting any of them moves the matching pipx directory; leave them unset for the platform
default.

These paths were introduced in 1.2.0 and temporarily reverted for macOS and Windows between 1.5.0 and 1.16.0. To move an
existing installation to the new default, see [Move installation](move-installation.html.md).

## Install for all users

Use `--global` **after** the subcommand to operate on the system-wide installation:

```console
$ sudo pipx install --global pycowsay
$ pipx list --global
```

Commands that write to the global locations need root. `pipx list --global` and `pipx environment --global` only
read them, so any user can run those.

Putting `--global` before the subcommand, as in `sudo pipx --global install pycowsay`, raises
`unrecognized arguments: --global`.

Default global paths are `/usr/local/bin` for binaries, `/usr/local/share/man` for man pages, `/usr/local/share`
for shell completions, and `/opt/pipx` for environments. Override them with `PIPX_GLOBAL_BIN_DIR`,
`PIPX_GLOBAL_MAN_DIR`, `PIPX_GLOBAL_COMPLETION_DIR`, and `PIPX_GLOBAL_HOME`. Add the
global binary directory to the system `PATH` with `sudo pipx ensurepath --global`, which writes
`/etc/profile.d/pipx.sh` on Linux and `/etc/paths.d/pipx` on macOS.

When pipx itself is installed with `--global` and `PIPX_GLOBAL_HOME` is overridden, set the override for every user
(for example in `/etc/environment`), so that pipx recognizes its own environment as the global one; see
[Environment variables](../reference/environment-variables.html.md).

You can also set the variables explicitly instead of using `--global`:

```console
$ sudo PIPX_HOME=/opt/pipx PIPX_BIN_DIR=/usr/local/bin PIPX_MAN_DIR=/usr/local/share/man pipx install <PACKAGE>
```

#### NOTE
`--global` is not supported on Windows.

## Prepend or preview the PATH edit

Pass `--prepend` to `pipx ensurepath` to prepend the pipx bin directory to `PATH` instead of appending it, so
pipx-installed binaries win over system binaries of the same name.

Pass `--dry-run` to report which directories `ensurepath` would add, without touching `PATH` or any shell
configuration file:

```console
$ pipx ensurepath --dry-run
```

## Point pip at an index

Under the pip backend, pipx uses pip for all installs, including its own shared libraries. Set `PIP_*` environment
variables to route pip at a private index; pipx forwards them to every pip call. The uv backend ignores `PIP_*`; use
uv’s own variables such as `UV_INDEX_URL`. For per-command control use `--pip-args`. See [Use a private index](use-private-index.html.md)
for the full recipe.

Set `PIPX_DISABLE_SHARED_LIBS_AUTO_UPGRADE=1` to skip automatic shared-library upgrades during commands such as
`pipx install` and `pipx upgrade`; `pipx upgrade-shared` still upgrades them. Use `--skip-maintenance` to apply
the same policy to one command:

```console
$ pipx install --skip-maintenance my-package
```

## Verify it

```console
$ pipx environment --value PIPX_HOME
$ pipx environment --value PIPX_BIN_DIR
```

Each prints the path pipx will use, reflecting any override you set.

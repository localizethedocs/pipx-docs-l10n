# How pipx works

#### NOTE
This page describes the **pip backend** model, where a shared `pip` environment backs every managed venv through a
`.pth` file. When the **uv backend** is active (the default whenever uv is available, via the `pipx[uv]` extra
or on `PATH`) pipx skips the shared environment and uses `uv venv` and `uv pip` instead; uv-created venvs ship
without pip. See [Use the uv backend](../how-to/use-uv-backend.html.md).

## Installing an application

`pipx install PACKAGE` creates an isolated virtual environment under `PIPX_HOME` (for example
`~/.local/share/pipx/venvs/black`) and installs the package into it. Under the pip backend pipx creates the venv
without pip, then creates or reuses a shared environment that holds an up-to-date `pip` and links it into the venv
through a [.pth file](https://docs.python.org/3/library/site.html), so every venv borrows one pip rather than
installing its own. pipx refreshes that shared pip once it is more than 30 days old; `pipx upgrade-shared` refreshes
it immediately. When pip’s files are missing from the shared environment, pipx rebuilds it before running pip; when a
pip command fails because pip no longer imports, pipx rebuilds it and retries the command once. Under the uv backend
there is no shared environment: `uv venv` builds the venv (without pip) and `uv pip` installs into it.

Once the package is installed, pipx exposes the application’s resources so you can reach them from anywhere. It handles
three kinds:

- console and GUI scripts, into `PIPX_BIN_DIR` (default `~/.local/bin`), for example
  `~/.local/bin/black` -> `~/.local/share/pipx/venvs/black/bin/black`;
- manual pages, into `PIPX_MAN_DIR` (default `~/.local/share/man/man[1-9]`);
- shell completion scripts, into `PIPX_COMPLETION_DIR` (default `~/.local/share`).

pipx symlinks each resource into place. On Windows it copies the file instead when the target directory does not
support symlinks. As long as `PIPX_BIN_DIR` is on your `PATH` the commands are available globally, and on systems
with `man` support the pages are too.

pipx records what it installed and exposed in a per-venv `pipx_metadata.json` file. Later commands (`upgrade`,
`reinstall`, `expose`) read it to reproduce the same state. When an upgrade or reinstall changes the resources a
package ships, pipx exposes the new ones and deletes the links or copies of the ones the package dropped.

Adding `--global` to any command targets a system-wide location shared by all users instead of your home directory;
the concrete paths and overrides live in [Configure paths](../how-to/configure-paths.html.md). It is not available on Windows.

For a `.py` script carrying PEP 723 metadata, pipx builds a temporary wheel from the script and its declared
dependencies, installs that wheel like any other package, then records the original path or URL so `upgrade` and
`reinstall` can rebuild it.

## Running an application

`pipx run APP` executes an application without installing it permanently. Under the pip backend pipx either reuses a
cached temporary venv or builds a fresh one, then invokes the app. The cache key is a hash of the requirements (the spec
plus any `--with` packages), the `--python` value, pip and venv arguments, the backend, and the cooldown; cached
environments expire after 14 days, after which the next run rebuilds against the latest release. The temporary venv
borrows the shared pip.

Under the uv backend pipx usually hands the whole run to `uv tool run`, which uses uv’s own cache. `pipx run --with
PKG` adds extra dependencies to the temporary environment either way.

## Exposing apps on PATH

When you type an exposed command, the shell finds the symlink (or copy) in `PIPX_BIN_DIR` and follows it to the
launcher inside the app’s venv. That launcher runs the venv’s own Python against the installed package, so the app
always uses its isolated dependencies and never your system site-packages.

## Directory layout

pipx keeps its venvs and shared libraries under one home directory; the run cache and logs use the platform cache and
log directories unless `PIPX_HOME` is set. The tree below is the conceptual shape; the concrete locations, defaults,
and every override live in [Configure paths](../how-to/configure-paths.html.md).

You can do all of this yourself; pipx automates it. Pass `--verbose` to see each command it runs and to stream
installer output.

## Resolving the Python interpreter

pipx prefers a real system Python and only downloads a standalone build as a fallback. When you pass `--python`, it
tries the value as a literal path, then as a command on `PATH`, then as a `pythonX.Y` command, then through the
`py` launcher when one is on `PATH`. Only if every step fails, and `--fetch-python=missing` is set, does pipx
fetch a build from [python-build-standalone](https://github.com/astral-sh/python-build-standalone).
`--fetch-python=always` skips these steps and downloads directly. The default, `--fetch-python=never`, keeps pipx
offline and errors out instead.

For the authoritative step-by-step order, the `--fetch-python` values, and when a downloaded build beats a patched
system Python, see [Standalone Python](../how-to/standalone-python.html.md).

# Making packages compatible

## Developing for pipx

If you are a developer and want users to be able to run

```console
$ pipx install MY_PACKAGE
```

make sure you include `scripts` in your main table <sup>[1](#main-table)</sup> in `pyproject.toml` or its legacy equivalents for
`setup.cfg` and `setup.py`. pipx also exposes `gui-scripts` entry points, which are useful for GUI applications on
Windows (they launch without opening a console window).

### pyproject.toml

```toml
[project.scripts]
foo = "my_package.some_module:main_func"
bar = "other_module:some_func"

[project.gui-scripts]
baz = "my_package_gui:start_func"
```

### setup.cfg

```ini
[options.entry_points]
console_scripts =
    foo = my_package.some_module:main_func
    bar = other_module:some_func
gui_scripts =
    baz = my_package_gui:start_func
```

### setup.py

```python
setup(
    # other arguments here...
    entry_points={
        "console_scripts": [
            "foo = my_package.some_module:main_func",
            "bar = other_module:some_func",
        ],
        "gui_scripts": [
            "baz = my_package_gui:start_func",
        ],
    },
)
```

Here `foo` and `bar` (and `baz` on Windows) become applications that pipx exposes after installing the package,
invoking their corresponding entry point functions.

### The `pipx.run` entry point group

When a user runs `pipx run PACKAGE`, pipx looks for a console script matching the package name. If the package name
and script name differ, the user has to write `pipx run --spec PACKAGE SCRIPT`, which is less convenient.

Package authors can declare a `pipx.run` entry point group to tell pipx which function to invoke for `pipx run`.
This entry point takes priority over console scripts when present.

### pyproject.toml

```toml
[project.entry-points."pipx.run"]
my-package = "my_package.cli:main"
```

### setup.cfg

```ini
[options.entry_points]
pipx.run =
    my-package = my_package.cli:main
```

With this declaration, `pipx run my-package` invokes `my_package.cli:main` even if no console script named
`my-package` exists. The [build](https://github.com/pypa/build) package uses this pattern so that `pipx run build`
works even though build’s console script is named `pyproject-build`.

### Detect a pipx installation

pipx records the environment name in `pipx_metadata.json` for packages installed with `pipx install`. Package code
can read this name when it needs to give pipx-specific recovery instructions:

```python
import json
import sys
from pathlib import Path


def pipx_environment() -> str | None:
    try:
        metadata = json.loads((Path(sys.prefix) / "pipx_metadata.json").read_text(encoding="utf-8"))
    except (json.JSONDecodeError, OSError):
        return None
    environment = metadata.get("environment") if isinstance(metadata, dict) else None
    return environment if isinstance(environment, str) else None
```

Use the returned name as the target for commands such as `pipx inject ENVIRONMENT 'mytool[feature]'`. pipx records the
extra on the main package, so reinstalling the environment retains it.

`pipx run` may create internal metadata without setting `environment`, so check the field value. Treat other keys as
internal pipx state. If the function returns `None`, show the package’s usual installer-neutral advice.

### Manual pages

To provide documentation via `man` pages on UNIX-like systems, ship them as data files. pipx exposes them the same way
it exposes apps.

### setuptools

```toml
# pyproject.toml
[tool.setuptools.data-files]
"share/man/man1" = [
  "manpage.1",
]
```

```ini
# setup.cfg
[options.data_files]
share/man/man1 =
    manpage.1
```

```python
# setup.py
setup(
    # other arguments here...
    data_files=[("share/man/man1", ["manpage.1"])],
)
```

#### WARNING
The `data-files` keyword is “discouraged” in the [setuptools documentation](https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html#setuptools-specific-configuration),
but there is no alternative when `man` pages are a requirement.

### pdm-backend

```toml
# pyproject.toml
[tool.pdm.build]
source-includes = ["share"]

[tool.pdm.build.wheel-data]
data = [
  { path = "share/man/man1/*", relative-to = "." },
]
```

The manual page `manpage.1` can then be read with `man manpage` after installing the package. For a real-world
example, see [pycowsay’s setup.py](https://github.com/cs01/pycowsay/blob/master/setup.py).

### Shell completions

A package can ship the completion script for its own command the same way it ships a man page. pipx picks up the three
directories a wheel installs completion scripts into and links each one under `PIPX_COMPLETION_DIR` (default
`~/.local/share`), so the completions arrive with the package and leave with it.

| Shipped by the package               | Exposed to                                    | Loaded by                           |
|--------------------------------------|-----------------------------------------------|-------------------------------------|
| `share/bash-completion/completions/` | `~/.local/share/bash-completion/completions/` | bash                                |
| `share/fish/vendor_completions.d/`   | `~/.local/share/fish/vendor_completions.d/`   | fish                                |
| `share/zsh/site-functions/`          | `~/.local/share/zsh/site-functions/`          | zsh (with the directory on `fpath`) |

Declare them through the data files of your build backend, exactly as with man pages:

```python
setup(
    name="your-tool",
    data_files=[
        ("share/bash-completion/completions", ["completions/your-tool"]),
        ("share/zsh/site-functions", ["completions/_your-tool"]),
        ("share/fish/vendor_completions.d", ["completions/your-tool.fish"]),
    ],
)
```

See [Shell completions](../how-to/shell-completions.html.md) for how each shell loads these directories.

You can read more about entry points in the [setuptools quickstart](https://setuptools.pypa.io/en/latest/userguide/quickstart.html#entry-points-and-automatic-script-creation).

* <a id='main-table'>**[1]**</a> This is often the `[project]` table, but it might be named differently. Read more in the [PyPUG](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#writing-your-pyproject-toml).

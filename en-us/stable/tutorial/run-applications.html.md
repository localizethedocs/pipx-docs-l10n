# Running applications

`pipx run` downloads and runs a Python application in a one-time, temporary environment, then leaves your system
untouched. Reach for it to scaffold a new project, check an app’s help text, or try a tool without committing to an
install.

## Basic usage

```console
$ pipx run pycowsay moo
```

pipx installs the package in an isolated, temporary directory and invokes the app:

```console
$ pipx run pycowsay moo
  ---
< moo >
  ---
   \   ^__^
    \  (oo)\_______
       (__)\       )\/\
           ||----w |
           ||     ||
```

Arguments after the app name pass straight through to it:

```console
$ pipx run pycowsay these arguments all go to pycowsay
```

## Ambiguous arguments

On Python 3.11, pipx can swallow an argument meant for the app when it looks like one of pipx’s own options:

```console
$ pipx run pycowsay --py
pipx run: error: ambiguous option: --py could match --python-args, --pypackages, --python
```

Put `--` before the app name to forward everything after it verbatim:

```console
$ pipx run -- pycowsay --py
```

## When the app name differs

Some packages expose an app under a different name, or expose several. Use `--spec` to name the package and the app
separately:

```console
$ pipx run --spec PACKAGE APP
```

Under the pip backend, pipx lists the executables of a package when you guess wrong. [esptool](https://github.com/espressif/esptool) 4.6.2 ships no `esptool` script:

```console
$ pipx run --spec esptool==4.6.2 esptool
'esptool' executable script not found in package 'esptool'.
Available executable scripts:
    esp_rfc2217_server.py - usage: 'pipx run --spec esptool==4.6.2 esp_rfc2217_server.py [arguments?]'
    espefuse.py - usage: 'pipx run --spec esptool==4.6.2 espefuse.py [arguments?]'
    espsecure.py - usage: 'pipx run --spec esptool==4.6.2 espsecure.py [arguments?]'
    esptool.py - usage: 'pipx run --spec esptool==4.6.2 esptool.py [arguments?]'
```

Run the one you want with `--spec`:

```console
$ pipx run --spec esptool==4.6.2 esptool.py
```

Package authors can remove this requirement by declaring a [pipx.run entry point](../explanation/making-packages-compatible.html.md) in their metadata.

<a id="tutorial-run-cache"></a>

## Run cache

pipx caches each `pipx run` environment for 14 days so repeated runs start instantly. After the cache expires, the
next run fetches the latest version. Under the uv backend pipx hands the run to `uv tool run`, so uv’s own cache
applies instead.

Force a rebuild before the cache expires with `--refresh`, which replaces that app’s cached environment and keeps the
replacement:

```console
$ pipx run --refresh APP
```

`--no-cache` also rebuilds, but under the pip backend marks the new environment for cleanup by a later run. The two
options are mutually exclusive.

Show the cache directory or remove pipx’s cached run environments:

```console
$ pipx cache dir
$ pipx cache purge
```

For how pipx decides what to cache and when, see [How pipx works](../explanation/how-pipx-works.html.md).

## Learn more

- [Use the uv backend](../how-to/use-uv-backend.html.md): run with uv, which keeps its own ephemeral cache.
- [CLI reference](../reference/cli.html.md): every `pipx run` flag, including `--python-args` and `--with`.

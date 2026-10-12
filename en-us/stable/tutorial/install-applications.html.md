# Installing applications

[Getting started](getting-started.html.md) walked through the `install` → `list` → `run` → `uninstall` cycle with the toy
`pycowsay` package. This tutorial repeats it with a real tool so you can see how a package’s name and its apps differ.

## Install a tool

Install [HTTPie](https://httpie.io/), a command-line HTTP client:

```console
$ pipx install httpie
  installed package httpie 3.2.4, Python 3.12.3
  These apps are now globally available
    - http
    - https
done! ✨ 🌟 ✨
```

pipx creates a virtual environment, installs the `httpie` package into it, and exposes the apps it declares on your
`PATH`. Note that the package is `httpie` but the commands are `http` and `https`: one package, two apps.

#### TIP
To install a tool for every user on the system, pass `--global`. See [Configure paths](../how-to/configure-paths.html.md).

## See what you got

```console
$ pipx list
venvs are in /home/user/.local/share/pipx/venvs
apps are exposed on your $PATH at /home/user/.local/bin
   package httpie 3.2.4, Python 3.12.3
    - http
    - https
```

## Run it and verify

Call one of the exposed apps to confirm the install:

```console
$ http --version
3.2.4
```

If the version prints, the app is on your `PATH` and ready to use. (Missing? Run `pipx ensurepath` and open a new
terminal, then see [Troubleshoot](../how-to/troubleshoot.html.md).)

## Hold back fresh releases

A brand-new release is the one most likely to be broken or compromised. Install the next tool a week behind the index:

```console
$ pipx install --cooldown 7 black
  installed package black 24.8.0, Python 3.12.3
  These apps are now globally available
    - black
    - blackd
done! ✨ 🌟 ✨
```

pipx remembers the number, so later upgrades of `black` keep the same seven-day lag without you repeating the flag.
Set `PIPX_COOLDOWN=7` in your shell profile to get the same lag for every tool.

## Learn more

`pipx install` has more to offer once you outgrow the basics:

- [Expose apps](../how-to/expose-apps.html.md): hide or reveal commands with `expose` / `unexpose`, and pull apps out
  of dependencies with `--include-deps`.
- [Tool manifests](../how-to/tool-manifest.html.md): manage a set of tools with a `pipx.toml` manifest and PEP 751 lock
  files.
- [Run scripts](../how-to/run-scripts.html.md): install a local PEP 723 script as a managed app.
- [Use the uv backend](../how-to/use-uv-backend.html.md): swap pip for uv to resolve and install faster.
- [Standalone Python](../how-to/standalone-python.html.md): install against a specific Python version, downloading one if
  needed.
- [Dependency cooldown](../how-to/dependency-cooldown.html.md): set the release-age lag once with `PIPX_COOLDOWN`.

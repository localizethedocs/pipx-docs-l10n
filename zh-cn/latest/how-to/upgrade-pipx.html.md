# Upgrade pipx

Move pipx itself to the latest release. Pick the method that matches how you installed it.

## Upgrade per OS

macOS:

```console
$ brew update && brew upgrade pipx
```

Ubuntu Linux:

```console
$ sudo apt update && sudo apt upgrade pipx
```

Fedora Linux:

```console
$ sudo dnf upgrade pipx
```

Windows (Scoop):

```console
$ scoop update pipx
```

## Self-managed installs

If pipx manages its own installation (see [Install from source control](install-pipx.html.md#install-from-source-control) and the self-managed bootstrap in
[Install pipx](install-pipx.html.md)), upgrade it like any other pipx app:

```console
$ pipx upgrade pipx
```

## Upgrade via pip

When you installed pipx with `pip install --user` and cannot upgrade it as a pipx app:

```console
$ python3 -m pip install --user --upgrade pipx
```

#### WARNING
On systems that adopt [PEP 668](https://peps.python.org/pep-0668/) (Ubuntu 23.04+, Debian 12+, Fedora 38+), this
fails with `externally-managed-environment`. Use the distribution package or the self-managed path above instead.
See [Install pipx](install-pipx.html.md) for the PEP 668 install options.

## Verify it

```console
$ pipx --version
```

#### NOTE
Upgrading from a pre-0.15.0.0 pipx to 0.15.0.0 or later requires reinstalling your packages so they gain the
persistent metadata files that release introduced. These files store the pip spec, injected packages, and custom pip
arguments in each venv. With no `--spec` installs and no injected packages, run `pipx reinstall-all`. Otherwise
reinstall manually: `pipx uninstall-all` followed by `pipx install` and, where needed, `pipx inject`.

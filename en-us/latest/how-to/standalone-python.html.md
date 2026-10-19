# Standalone Python

Install an app under a Python interpreter downloaded from [python-build-standalone](https://github.com/astral-sh/python-build-standalone) instead of the system one. Reach for this when the system
Python lacks the version you need, or when a distro patched its Python in ways that break the app.

## How pipx decides

`--fetch-python` (or `PIPX_FETCH_PYTHON`) sets the policy. With `missing`, pipx downloads only when no local
interpreter satisfies the app’s `requires-python`:

Naming `--python` yourself keeps the last word: pipx uses the interpreter you named and reports it, rather than
overriding you when the package rejects it.

## Set the policy

| Value     | Behavior                                                                    |
|-----------|-----------------------------------------------------------------------------|
| `never`   | Default. Never download; use interpreters from `PATH` or the `py` launcher. |
| `missing` | Look locally first; download when the requested version is not found.       |
| `always`  | Skip the local search and use a standalone build for the requested version. |
```console
$ pipx install --python 3.13 --fetch-python=missing my-package
$ pipx install --python 3.13 --fetch-python=always my-package
```

Set it for the whole shell session with the environment variable:

```console
$ export PIPX_FETCH_PYTHON=missing
$ pipx install --python 3.13 my-package
```

Reach for `always` in CI runs that should not depend on the runner’s Python, on distros that strip modules like
`tkinter` or `lzma`, or on air-gapped hosts with a populated standalone cache.

## Manage cached interpreters

pipx unpacks each interpreter into its standalone cache. Manage them with:

```console
$ pipx interpreter list
$ pipx interpreter prune
$ pipx interpreter upgrade
```

#### NOTE
`--fetch-missing-python` and `PIPX_FETCH_MISSING_PYTHON` still work but are deprecated aliases for
`--fetch-python=missing` / `PIPX_FETCH_PYTHON=missing`. pipx errors if you set both
`PIPX_FETCH_MISSING_PYTHON` and `PIPX_FETCH_PYTHON`.

## Verify it

```console
$ pipx list
```

The environment lists the interpreter version pipx settled on. For how pipx resolves interpreters, see
[How pipx works](../explanation/how-pipx-works.html.md).

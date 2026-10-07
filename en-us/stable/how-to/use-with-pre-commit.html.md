# Use with pre-commit

Run a pipx app as a [pre-commit](https://pre-commit.com/) hook. This is useful for apps that work with `pipx run`
but have no native pre-commit support, when you want the prebuilt wheel from PyPI rather than a source build, or when you
need pipx’s `--spec` and `--index-url` flags.

## Add the hook

Add this to your `.pre-commit-config.yaml`, using the code linter [yapf](https://github.com/google/yapf/) as the
example. Set `rev` to the latest pipx release tag from the [releases page](https://github.com/pypa/pipx/releases) (`1.15.0` shown here):

```yaml
- repo: https://github.com/pypa/pipx
  rev: "1.15.0"
  hooks:
    - id: pipx
      alias: yapf
      name: yapf
      args: [yapf, -i]
      types: [python]
```

The `args` list is passed to `pipx run`: the first item is the app to run, the rest are its arguments.

## Verify it

```console
$ pre-commit run yapf --all-files
```

The hook resolves the app through `pipx run` and reports its result.

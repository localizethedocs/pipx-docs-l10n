# Dependency cooldown

Skip index artifacts uploaded in the last few days, so a compromised or broken release has time to be caught and yanked
before your machine picks it up. Reach for this when you install or upgrade tools from a public index and can afford to
lag the newest version.

## Set it per command

`--cooldown DAYS` works on `install`, `install-all`, `inject`, `upgrade`, `upgrade-all` and `run`:

```console
$ pipx install --cooldown 7 httpie
```

pipx translates one policy for both backends: pip receives `--uploaded-prior-to P7D` and uv receives
`--exclude-newer P7D`. Relative cooldowns need uv 0.9.17 or newer.

## Set it for a machine

Export `PIPX_COOLDOWN` so every one of those commands picks it up:

```console
$ export PIPX_COOLDOWN=7
$ pipx install httpie
$ pipx upgrade-all
```

Put the export in your shell profile to make it stick across sessions. pipx has no configuration file; see
[Environment variables](../reference/environment-variables.html.md) for the full list.

## Which value pipx uses

|   Rank | Source                                        | Notes                                                                                                      |
|--------|-----------------------------------------------|------------------------------------------------------------------------------------------------------------|
|      1 | `--cooldown DAYS`                             | Wins over everything. `--cooldown 0` opts one command out of a machine-wide setting.                       |
|      2 | `PIPX_COOLDOWN`                               | Applies to the commands listed above. Outranks whatever an earlier install recorded.                       |
|      3 | `cooldown_days` in the environment’s metadata | What the environment was last installed with. See [Metadata](../reference/metadata.html.md). |

A locked install ignores all three: `pylock.toml` already pins every version. Passing `--cooldown` together with
`--lock` errors, while `PIPX_COOLDOWN` yields and the install proceeds.

## Verify it

Read back the variable and the value the environment recorded:

```console
$ pipx environment --value PIPX_COOLDOWN
7
$ pipx list --output json | jq '.venvs.httpie.metadata.main_package.cooldown_days'
7
```

An invalid value stops the command before it starts:

```console
$ PIPX_COOLDOWN=lots pipx install httpie
PIPX_COOLDOWN must be unset or a non-negative integer, got 'lots'.
```

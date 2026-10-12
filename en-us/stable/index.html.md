# pipx

`pipx` installs and runs end-user Python applications in isolated environments. It fills the same role as macOS’s
`brew`, JavaScript’s [npx](https://docs.npmjs.com/cli/commands/npx), and Linux’s `apt`. Under the hood it uses
pip, but unlike pip it creates a separate virtual environment for each application, so their dependencies never collide
and an uninstall leaves nothing behind.

## Start here

Install your first application and run one in a throwaway environment, one step at a time.

Task recipes for installing pipx, injecting packages, pinning versions, configuring paths, and more.

The full CLI, environment variables, exit codes, the JSON envelope, and worked examples.

How pipx works, what it manages on disk, and how it compares to other tools.

## pip vs pipx

pip installs both libraries and applications into whatever environment is active, with no isolation. pipx installs
only applications, each in its own virtual environment, and exposes their commands on your `PATH`. You get clean
uninstalls, zero dependency conflicts between tools, and no `sudo pip install` — pipx runs with regular user
permissions.

## Where apps come from

pipx pulls packages from [PyPI](https://pypi.org/) by default, but accepts any source pip supports: local
directories, wheels, and git URLs. Any package that declares [console script entry points](https://packaging.python.org/en/latest/specifications/entry-points/) works with pipx.
[Poetry](https://python-poetry.org/docs/pyproject/#scripts) and [Hatch](https://hatch.pypa.io/latest/config/metadata/#cli) users can add entry points the same way.

## Highlights

- Install CLI apps into isolated environments with `pipx install`, so there are no dependency conflicts and uninstalls
  are clean.
- List, upgrade, and uninstall managed apps in one command.
- Run the latest version of any app in a temporary environment with `pipx run`, without installing it first.

## Testimonials

> “Thanks for improving the workflow that pipsi has covered in the past. Nicely done!”
> “My setup pieces together pyenv, poetry, and pipx. […] For the things I need, it’s perfect.”
> “I’m a big fan of pipx. I think pipx is super cool.”

## Credits

pipx was inspired by [pipsi](https://github.com/mitsuhiko/pipsi) and [npx](https://github.com/npm/npx). It was
created by [Chad Smith](https://github.com/cs01/) and has had lots of help from [contributors](https://github.com/pypa/pipx/graphs/contributors). The logo was created by [@IrishMorales](https://github.com/IrishMorales).

pipx is maintained by a team of volunteers (in alphabetical order):

- [Bernát Gábor](https://github.com/gaborbernat)
- [Henry Schreiner](https://github.com/henryiii)
- [Jason Lam](https://github.com/dukecat0)
- [Tzu-ping Chung](https://github.com/uranusjr)
- [Xuan Hu](https://github.com/huxuan)

Emeritus maintainers, who shaped earlier releases:

- [Chad Smith](https://github.com/cs01)
- [Chrysle](https://github.com/chrysle)
- [Matthew Clapp](https://github.com/itsayellow)
- [Robert Offner](https://github.com/gitznik)

The documentation follows the [Diátaxis](https://diataxis.fr) framework.

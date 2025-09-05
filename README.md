# Renovate configuration for glotzerlab

To migrate a package from dependabot to Renovate:

- [ ] Enable auto-merge in settings.
- [ ] Allow squash commits in settings.
- Configure the branch protection rule:
  - [ ] Uncheck "Restrict updates".
  - [ ] Require 1 approval.
  - [ ] Ensure that specific status checks are required.

Then request installation of the *Renovate* and *renovate-approve* apps.

On the PR that it creates:
- [ ] Remove `dependabot.ya?ml`.
- If using any `uv pip compile` files:
  - [ ] Modify the headers to use the syntax `--python-version={version}` (if present).
  - [ ] Modify the header to include `--output-file={output-file-name}.txt`
  - [ ] Remove any use of `--python-platform`.
  - [ ] (recommended) Remove any pinning in the `.in` files. Updates
    to one dependency at a time often fail to resolve (e.g. the latest *nbsphinx*
    does not work with the latest *sphinx*). We could either pin in `.in` files and
    accept many permanently open failing PRs, or unpin in `.in` files and get the
    latest full compatible set of packages weekly. The disadvantage to unpinning
    is that you don't get notification that some dependencies failed to update.
  - [ ] Check the renovate logs for the repository and ensure that pip-compile
    finds the `requirements*.txt` files and does not report any warnings or errors.
    These files should also be listed in the PR description.
  - [ ] Remove the `update-uv-lockfiles` action (if used).
- If using any conda lockfiles (see https://github.com/glotzerlab/fresnel/pull/294 for an example):
  - [ ] Convert the conda `environment.yaml` to `pixi.toml`.
  - [ ] Remove `environment.yaml` and all generated conda lockfiles.
  - [ ] Switch from `setup-micromamba` to `setup-pixi` in the GitHub Actions workflows.
  - [ ] Remove the `update-conda-lockfiles` action (if used).

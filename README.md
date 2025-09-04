# Renovate configuration for glotzerlab

To migrate a package from dependabot to Renovate:

- [ ] Create `.github/CODEOWNERS` and set the contents to:
   ```
   * @{primary maintainer} @glotzerlab/maintainers
- [ ] Enable auto-merge in settings.
- Configure the branch protection rule:
  - [ ] Uncheck "Restrict updates".
  - [ ] Check "Require review from code owners".
  - [ ] Require 1 approval.
  - [ ] Ensure that specific status checks are required.
  - [ ] Add the Renovate App to the bypass list.
  - [ ] Add `glotzerlab/maintainers` to the contributors with *maintain* privileges.
    Navigate to `.github/CODEOWNERS` in the GitHub web interface and confirm
    there is a message at the top stating:
    > This CODEOWNERS file is valid.

Then request installation of the Renovate App.

On the PR that it creates:
- [ ] Remove `dependabot.ya?ml`.
- If using any `uv pip compile` files:
  - [ ] Modify the headers to use the syntax `--python-version={version}` (if present).
  - [ ] Remove any use of `--python-platform`.
  - [ ] Check the renovate logs for the repository and ensure that pip-compile
    finds the `requirements*.txt` files and does not report any warnings or errors.
    These files should also be listed in the PR description.
  - [ ] Remove the `update-uv-lockfiles` action if used.
- If using any conda lockfiles (see https://github.com/glotzerlab/fresnel/pull/294 for an example):
  - [ ] Convert the conda `environment.yaml` to `pixi.toml`.
  - [ ] Remove `environment.yaml` and all generated lockfiles.
  - [ ] Switch from `setup-micromamba` to `setup-pixi` in the GitHub Actions workflows.
  - [ ] Remove the `update-conda-lockfiles` action if used.

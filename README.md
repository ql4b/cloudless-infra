# cloudless-infra

> Foundation module for cloudless infrastructure. Standardizes the boring stuff so you focus on the unique parts.

Provides consistent labeling, naming conventions, and environment context for all cloudless modules.

## Quick Start

```bash
# Bootstrap into existing project
curl -sL https://raw.githubusercontent.com/ql4b/cloudless-infra/main/bootstrap | bash
```

or

```bash
# specify project name and directory
curl -sL https://raw.githubusercontent.com/ql4b/cloudless-infra/main/bootstrap | bash -s my-project infra
```

Then load the environment and `tf` wrapper, and apply:

```bash
# edit .env with your settings first (NAME, NAMESPACE, AWS_PROFILE, AWS_REGION)
source ./activate

tf init
tf apply --auto-approve

tf output -json env | jq
```

## Manual Setup

```bash
# setup environment variables
cp .env.example .env

# edit variables in .env
```

## activate

`activate` sources `.env` and puts the repo root on `$PATH` (so the `tf`
wrapper is callable) for the current shell:

```bash
source ./activate
which tf
```

## tf wrapper

`tf` is a thin wrapper that sources `.env` and runs the pinned terraform binary
with `-chdir=infra`. If you prefer not to use `activate`, add the repo root to
`$PATH` yourself:

```bash
# add . to the $PATH
export PATH="$(pwd):$PATH"

# check tf is in the $PATH
which tf

tf version
```

## tf init

```bash
tf init
```

## outputs

```bash
tf apply --auto-approve
tf output -json env | jq
```

## Releases

Releases are automated with [semantic-release](https://semantic-release.gitbook.io/)
driven by [Conventional Commits](https://www.conventionalcommits.org/). The
`.github/workflows/release.yml` workflow validates (`terraform fmt`, `init`,
`validate` against `infra/`) and, on push to `main`, cuts a version tag,
GitHub release, and `CHANGELOG.md` entry.

Commit messages determine the version bump:

- `fix:` / `perf:` → patch release
- `feat:` → minor release
- `feat!:` or a `BREAKING CHANGE:` footer → major release
- `docs:` / `chore:` → recorded in the changelog, no release on their own

This is a project scaffold (not a published module), so only functional
changes cut a release.

# AGENTS.md — layer-gnupg

Standalone candy repo for the `gnupg` layer — GnuPG encryption and signing tools
for GPG agent forwarding: `gpg`, `gpg-agent`, `gpgconf`, and `gpg-connect-agent`
from the distro package. The candy lives in `charly.yml` at the repo root and
projects the `gnupg` skill entity (`family: infrastructure`).

Canonical files:

- `charly.yml` — the `gnupg:` candy entity and the `gnupg-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-infrastructure:gnupg` — the owning skill: the per-distro package
  names, the agent-forwarding runtime behavior, and the related commands. Load
  before editing or troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package
  sections). Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence: the fixed-path
  file checks for `gpg`/`gpg-agent`/`gpgconf`, the `gpg --version` stdout check,
  and the `package=gnupg` check with its `package_map` (fedora → `gnupg2`).
- Package-only candy; the per-distro arms (`arch`/`debian`/`fedora`/`ubuntu`) are
  the install content.

## Modify this repo

- Edit the `gnupg:` candy entity in `charly.yml`; keep the matching
  `gnupg-skill:` entity in step with it.
- Keep the `package_map` in the `package=gnupg` check aligned with the distro
  package arms — a package rename on a distro is a change in both places.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.

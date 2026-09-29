# gnupg

GnuPG encryption and signing tools — `gpg`, `gpg-agent`, `gpgconf` — for GPG
agent forwarding inside containers.

`gnupg` installs the GnuPG suite from the distro package (`gnupg` on
arch/debian/ubuntu, `gnupg2` on fedora). The binaries land in `/usr/bin` and
`gpg` reports its GnuPG version string, so the install and the agent-forwarding
tooling are directly verifiable inside the built image. It is usually composed
through the `agent-forwarding` metalayer rather than named directly.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `gnupg` |
| Distro | all — `gnupg` (arch/debian/ubuntu), `gnupg2` (fedora) |
| Binaries | `gpg`, `gpg-agent`, `gpgconf`, `gpg-connect-agent` in `/usr/bin` |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-image:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-gnupg:v2026.239.1615'
```

Then, inside the built image:

```bash
gpg --version            # gpg (GnuPG) X.Y.Z
gpgconf --list-dirs      # agent socket path resolution
```

## Runtime behavior

When combined with SSH/GPG agent forwarding (`charly shell`, `charly start`
direct mode), the container's GPG uses the host's agent for private-key
operations (signing, decryption) via a forwarded socket. The container keeps its
own keyring — public keys must be imported separately with `gpg --import`. No
host keyring is mounted; only the agent socket is forwarded.

## Layout

- `charly.yml` — the `gnupg:` candy entity: the per-distro package arms and the
  `check:` steps.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Family skill: `/charly-infrastructure:gnupg`
- `/charly-distros:agent-forwarding` — metalayer composing gnupg + direnv + ssh-client
- `/charly-build:secrets` — `charly secrets gpg` key management and agent setup
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella

# skill-a2a

The `a2a` agent skill as a root-level package, so Curator profiles can declare it as an ordinary Git skill.

Source of truth: `relux-works/a2a-swarm`, path `internal/skill/files/a2a`. This repository publishes a verbatim mirror of `SKILL.md` and `references/`:
change the skill in a2a-swarm first, then update these files to the same content and record the source commit below.
The command package manifest `agent-skill.json` is maintained here separately from the source skill manifest.

| Mirror of | Commit |
| --- | --- |
| relux-works/a2a-swarm `internal/skill/files/a2a` prose | a17b483e9850ccaae6a44e6a2d1389665c3f5f29 |

## Command package v0.2.3

The schema-7 manifest exports `a2a` through Curator's `go-repository-v1` driver.
It pins the external `a2a-swarm` repository; the embedded skill prose remains
the mirror identified above. The mirror source includes a text-only installation-route
fix landed after the binary build, so its source commit differs from the unchanged
binary pin. The binary source is main `72156cf666a1e7c15b26a9b3011fadef1d2bb2c5`, including the landed
managed-command guidance. The build refresh preserves the original pin as an
ancestor and regenerates the vendor closure from that exact main tree.

The candidate build pin is `69f361b6bb4684a32b5d90949cdd9606cae4bdf6`,
on the append-only [curator-build branch](https://github.com/relux-works/a2a-swarm/tree/curator-build).
That commit contains the root build descriptor and regenerated vendor closure;
vendor is deliberately absent from a2a-swarm main. Consumers select the full
commit, not the branch name. See the [branch maintenance rule](https://github.com/relux-works/a2a-swarm/blob/72156cf666a1e7c15b26a9b3011fadef1d2bb2c5/docs/curator-build.md).
No build tag is declared. A platform owner must create signed release tag
`v0.2.3` on the accepted skill commit; this candidate creates no release tag.

### Compatibility and source access

Schema 7 requires Curator **v0.14.0-rc.1 or newer**; older versions reject
this manifest. Repository-build support also depends on the target platform
and Curator revision. The macOS witness uses a locally built Curator main at
`8eefd7022dc4a3ad74483a666b2ad439c70c02a1`, not an installed release.
Linux needs a new Curator release tag and fresh evidence from a Linux seat;
this candidate supplies no Linux qualification.

The locally witnessed Curator revision supports Go families **1.25, 1.26 and
1.27**; this source requires at least **1.25.5**. Select a real executable:
`CURATOR_GO="$(go env GOROOT)/bin/go"`. A symlink such as a package manager's
`bin/go` can be refused with `toolchain_executable_mismatch`; changing PATH
alone does not select Curator's compiler. Git and writable user/project build
state are required; no root is needed. On a fresh machine, run
`curator bootstrap --skills-root "$SKILLS_ROOT" --non-interactive` before
project installation. Version v0.2.3 adds a mandatory compiled command to the previous
prose-only package: unavailable source or an unusable toolchain fails the
**whole project install**, not just the command.

The public build repository uses HTTPS so an isolated install needs no SSH
identity or a2a account. Fetching the skill source and fetching its build
repository are separate operations. Successful installation does not provision
an account, connect to Matrix, or qualify runtime activation.

### Tools

| Tool | Purpose | Command | Output |
| --- | --- | --- | --- |
| Curator | Install the skill and compile its command without root | `curator install .` in a project declaring this skill | Project `.agents/bin/a2a` and manager-private build state |
| a2a | Show the installed command surface without authentication | `a2a --help` with project `.agents/bin` on PATH | Standard output |

The managed build disables CGO and does not enable native Sherpa voice.
Legacy/default credentials use the file backend (0600 files) when
`A2A_SECRET_STORE` is unset or `file`; no libsecret or DBus is required.
**Keychain mode and named connections are currently unusable under the
managed build on macOS too**: the required sibling `a2a-keychain-status`
helper is not delivered beside Curator's artifact. Linux also rejects the
Keychain backend. Installing the CLI does not fix these runtime limits.
They belong to the source-owner runtime task; account provisioning and live
activation remain separate owner steps.

Install and `--help` evidence does not qualify keeper use, hooks across a pin
refresh, named connections, or idle wake. Hook paths may resolve to a
content-keyed artifact that changes on refresh; a separate owner-approved
runtime witness is required before calling this package keeper-ready.

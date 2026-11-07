# skill-a2a

The `a2a` agent skill as a root-level package, so Curator profiles can declare it as an ordinary Git skill.

Source of truth: `relux-works/a2a-swarm`, path `internal/skill/files/a2a`. This repository is a published mirror:
change the skill in a2a-swarm first, then update this mirror to the same content and record the source commit below.

| Mirror of | Commit |
| --- | --- |
| relux-works/a2a-swarm `internal/skill/files/a2a` | 3f66817418942bc2157a4437e0384b27cd3663e8 |

## Command package v0.2.3

The schema-7 manifest exports `a2a` through Curator's `go-repository-v1` driver.
It pins the external `a2a-swarm` repository; the embedded skill prose remains
the mirror identified above. The package manifest is maintained here. The mirrored installation guidance
comes from the source candidate identified above, newer than the binary's
main baseline `711ed0d6f88ddb498b711521039f0e9980788aa0`. The binary pin remains
unchanged because those source instructions have not landed. Its embedded
`a2a skill install` text is older; retain the Curator-managed mirror instead.
The source owner controls the later main-to-build refresh and consumer repin.

The candidate build pin is `9eb007917d7fc18e7e3f1f2c820fad6119c6b573`,
on the append-only [curator-build branch](https://github.com/relux-works/a2a-swarm/tree/curator-build).
That commit contains the root build descriptor and regenerated vendor closure;
vendor is deliberately absent from a2a-swarm main. Consumers select the full
commit, not the branch name. See the [branch maintenance rule](https://github.com/relux-works/a2a-swarm/blob/3f66817418942bc2157a4437e0384b27cd3663e8/docs/curator-build.md).
No build tag is declared. A platform owner must create signed release tag
`v0.2.3` on the accepted skill commit; this candidate creates no release tag.

### Compatibility and source access

Schema 7 requires Curator **v0.14.0-rc.1 or newer**; older versions reject
this manifest. Repository-build support also depends on the target platform
and Curator revision. The macOS witness uses a locally built Curator main at
`470331906654c65169b88bb9579c0c929ed272df`, not an installed release.
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

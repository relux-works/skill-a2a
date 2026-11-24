# skill-a2a

The `a2a` agent skill and managed command package for Curator profiles.

## Source

The canonical skill lives in `relux-works/a2a-swarm`, at
`internal/skill/files/a2a`. This package mirrors `SKILL.md` and `references/`
byte for byte from that source.

| Mirror of | Source commit |
| --- | --- |
| `relux-works/a2a-swarm` skill files | `2c21912a777ad4822d65f601a056322afc851702` |

## Package v0.3.0

The schema-7 manifest exports the `a2a` command through Curator's
`go-repository-v1` driver. Its build declaration pins the source repository to
commit `69f361b6bb4684a32b5d90949cdd9606cae4bdf6`, which contains the command
build descriptor and vendor closure. The build pin is separate from the skill
source commit above.

Schema 7 requires Curator v0.14.0-rc.1 or newer. The command build requires Go
1.25.5 or newer and Git. On a fresh development setup, bootstrap the selected
skills root before installing:

```sh
curator bootstrap --skills-root "$SKILLS_ROOT" --non-interactive
curator install .
a2a --help
```

The managed build disables CGO and native Sherpa synthesis. File credentials
are available. Keychain-backed credentials are unavailable in this package
build because the companion `a2a-keychain-status` helper is not included; Linux
also rejects the Keychain backend. Installing the package does not provision a
Matrix account, choose a profile, or activate a runtime.

## Matrix profile

The homeserver URL and Matrix `server_name` come from the selected operator
profile. They are separate values, and this package supplies no universal
server. For example, a profile may use URL `https://matrix.example.org` and
`server_name` `example.org`. See the source skill for profile selection and
connection limits.

## License

Distributed under Apache License 2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE)
for copyright and attribution notices.

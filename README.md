# I'm a guy who mashes buttons for a living

Some of those buttons have already been mashed on your behalf. I'm passionate about automating everything in reach, and lately that includes getting the AI to mash some of these buttons on my behalf too.

Here's an eclectic collection of oddities and real projects I've gone through, in no specific order. If any of it helps you, treat it like open source: copy, paste, fork, ignore. Use any, all, or none of it.

The full-time job keeps me anchored to whatever's in front of me there. Anything in this collection is hobby time, so some of it is more finished than others.

Some of what's here:

- **[merg](https://github.com/freedomfury/merg)** — Pythonic deep merging of dicts and lists for configuration data, with strict JSON/YAML-shaped type validation. Inspired by Ruby's `deep_merge` gem, the engine behind Puppet's Hiera. Includes knockout prefix semantics. Published to PyPI: `pip install merg`.

- **[shopts](https://github.com/freedomfury/shopts)** — A modern, schema-driven alternative to `getopts` for Bash, written in Go. Declare options once (flags, types, defaults, validation rules, named built-in validators for things like email/semver/IPv4/CIDR) and `shopts` handles parsing, type checking, help text, and shell-safe output. Hundreds of tests across Go and Bash, benchmarks against a hand-rolled Bash reference parser, build-once-promote release workflow. Distinct exit codes for schema errors vs. bad args. No `eval`, no subshells, no dependencies.

- **[docker-utility-images](https://github.com/freedomfury/docker-utility-images-)** — A GitHub Actions monorepo for building, scanning, and publishing utility Docker containers, structured as a chain of custody for container artifacts. Each artifact passes through Hadolint (Dockerfile linting), Shellcheck (lib hygiene), Skopeo registry inspection (hash-based change detection so unchanged sources don't rebuild), Trivy (CVE scanning), and CycloneDX SBOM generation before it reaches the registry. Every release is date-stamped and ships with all scan reports and SBOMs attached as GitHub Release artifacts — the audit trail any compliance-adjacent platform team eventually needs. Designed around the two change vectors that matter for image freshness: source changes (rebuild only when content actually changed, via the hash label check) and upstream changes (a scheduled job that rebuilds unconditionally on a fixed cadence to pick up base-image security patches you don't control). Adding a new container is "create a folder with a Dockerfile, PR it, merge it" — no matrix list to maintain.

- **[bash-bootstrap](https://github.com/freedomfury/bash-bootstrap)** — A research POC asking whether strict conventions can make AI agents produce deterministic code in a language that's hostile to it. I picked Bash on purpose as the worst case, wrote a [1,400-line conventions document](https://github.com/freedomfury/bash-bootstrap/blob/main/docs/bash-conv.md) with an empirical "Common AI Mistakes" table, a hermetic vendored toolchain, a small standard library, four agent skills (sandbox, compose, secrets, bootstrap-task), and a Packer + QEMU + virtiofsd lab as a Molecule-replacement sketch. The answer was yes, with enough rules. See the project [README](https://github.com/freedomfury/bash-bootstrap) for the full architecture.

- **[facts-aws-compute](https://github.com/freedomfury/facts-aws-compute)** — Small Go binary that queries AWS instance metadata and tags. JSON in, JSON out. Built as a lightweight alternative to dragging in the full AWS CLI when you only need a handful of commands at boot.

- **[bash-static](https://github.com/freedomfury/bash-static)** — Makefile that fetches the latest upstream Bash and produces a statically-linked binary. Fully automated via GitHub Actions. Originally part of bash-bootstrap when I was going to compile Bash from scratch — split out once I realized I could just pull a static binary from a container.

- **[apache_traffic_server](https://github.com/freedomfury/apache_traffic_server)** — Ansible Collection for Apache Traffic Server. Has a build phase (because Red Hat doesn't ship the current version) and a separate deploy-and-configure phase. Includes Molecule tests. (2021)

- **[ansible_lxd](https://github.com/freedomfury/ansible_lxd)** — Multi-tier environment setup using QEMU cloud images on LXD. A way to model how enterprise multi-tier environments come together on a laptop. (2019)

- **[invoke-layout](https://github.com/freedomfury/invoke-layout)** — A reference layout for pyinvoke-based task runners. The idea: pull pipeline logic out of CI tooling and into actual code, so the pipeline isn't trapped in a Jenkinsfile.

Also got an open contribution to [harness/harness-docker-runner](https://github.com/harness/harness-docker-runner/pull/48) adding device passthrough support. Not a Go shop in my day job, so I leaned on AI tooling to bridge the language gap and ship a fix for something that was bugging me.

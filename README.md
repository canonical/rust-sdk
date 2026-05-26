# Rust SDK for Workshop

A development environment for Rust projects. It ships rustup with
symlinks for `rustc`, `cargo`, `clippy`, `rustfmt`, `rust-analyzer`
and other standard Rust tools, persists toolchains and Cargo configuration
on the host across workshop updates.

---

## Reference workshop

A minimal workshop:

```yaml
# workshop.yaml
name: rust-app
base: ubuntu@24.04
sdks:
  - name: rust
    channel: latest/stable

actions:
  build: cargo build "$@"
  doc: cargo doc "$@"
  lint: cargo clippy "$@"
  test: cargo test "$@"
```

This demonstrates a basic Rust build workflow with persistent toolchain and
module caching.

---

## Using the SDK

### Prerequisites, project layout

No prerequisite SDKs are required.
Your Rust project (with a `Cargo.toml` file) should be in your project
directory:

```bash
git clone <YOUR_REPO_URL>
```

### Build the project

Once the workshop is ready:

```bash
workshop shell
cargo build
```

The first build downloads the Rust toolchain and project dependencies. Both
are cached via host mounts, so subsequent builds reuse them.

To see where mount data is stored on the host:

```bash
workshop info
```

### Test and run

From within the workshop shell:

```bash
workshop shell
cargo test
cargo run
```

Use standard Cargo commands; the toolchain behaves exactly as it would in a
native Rust installation.

### Toolchain configuration

Rustup settings persist across workshop updates via the
`rustup-and-toolchains` mount:

```bash
workshop shell
rustup default nightly
```

The SDK checks for `rust-toolchain.toml` or `rust-toolchain` (the legacy
format) on launch; if found, rustup's override mechanism selects the specified
toolchain. If neither file exists, the SDK defaults to `stable`.

---

## Plugs (resources this SDK consumes)

### `rustup-and-toolchains`

- Interface: `mount`
- Workshop target: `/home/workshop/.rustup`
- Purpose: Persists installed Rust toolchains, components, and rustup
  configuration between workshop updates.

### `cargo-global-config`

- Interface: `mount`
- Workshop target: `/home/workshop/.cargo`
- Purpose: Persists Cargo global configuration and registry caches between
  workshop updates.

## Slots (resources this SDK provides)

This SDK doesn't define any slots.

---

## Documentation and guidance

- [Rust official documentation](https://doc.rust-lang.org/)
- [Cargo book](https://doc.rust-lang.org/cargo/)
- [Workshop documentation](https://ubuntu.com/workshop/docs/)

---

## Community and support

- Rust community: [Rust Users Forum](https://users.rust-lang.org/)
- Workshop forum:
  [Discourse](https://discourse.ubuntu.com/)
- Please review our
  [Code of Conduct](https://ubuntu.com/community/ethos/code-of-conduct) before
  participating.

---

## Contributions

All contributions, including code, documentation updates, and issue reports,
are welcome!

- See `CONTRIBUTING.md` for guidelines.
- Open issues or pull requests on the official repository.

---

## License and copyright

Copyright 2025 Canonical Ltd.

This program is free software: you can redistribute it and/or modify it under
the terms of the
[GNU Lesser General Public License version 2.1 (LGPLv2.1)](https://www.gnu.org/licenses/old-licenses/lgpl-2.1.html)
as published by the Free Software Foundation.

This program is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE. See the GNU Lesser General Public License for more details.

The Rust toolchain is dual-licensed under
[Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0) and
[MIT License](https://opensource.org/licenses/MIT).

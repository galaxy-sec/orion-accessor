# Repository Guidelines

## Project Structure & Module Organization
Source lives in `src/`, with `addr/` managing HTTP, Git, and local lookups plus redirect rules, `tpl/` for templating, `vars/` for variable resolution, and `update/` for download flows; shared helpers sit alongside as `archive.rs`, `timeout.rs`, `types.rs`, and `tools.rs`. Integration tests and fixtures reside in `tests/` (e.g., `git_test.rs`, `data/`), runnable examples are in `examples/` such as `compress_demo.rs`, longer-form notes live in `docs/`, and planning artefacts are kept in `tasks/`.

## Build, Test, and Development Commands
Use `cargo build` for debug builds and `cargo build --release` before benchmarking or publishing. Run `cargo test -- --test-threads=1` to mirror CI behaviour, keeping network-heavy scenarios stable. Lint and formatting gates are `cargo fmt --all -- --check` plus `cargo clippy --all-targets --all-features -- -D warnings`. For coverage snapshots, run `cargo llvm-cov --all-features --workspace -- --test-threads=1`. Inspect example behaviour with `cargo run --example compress_demo`.

## Coding Style & Naming Conventions
Stick to Rust edition settings in `Cargo.toml` and apply `rustfmt`’s 4-space indentation. Modules and files use `snake_case`, types and traits take `UpperCamelCase`, functions and variables stay `snake_case`, and constants are `SCREAMING_SNAKE_CASE`. Prefer `thiserror` for error enums, return `Result<_, orion_error::Error>` or `anyhow::Result`, avoid `unwrap`/`expect` outside tests and examples, and reach for `tokio` primitives when async coordination is required.

## Testing Guidelines
Inline unit tests live beside the code (`mod tests`), while broader scenarios belong in `tests/` named `*_test.rs`. Compose async tests with `tokio::test`, parameterise cases via `rstest`, and stub HTTP with `mockito`. Respect proxy variables (`HTTP_PROXY`, `HTTPS_PROXY`, `ALL_PROXY`) when exercising Git or HTTP paths, and rely on `tempfile` for ephemeral resources. Keep coverage meaningful by running `cargo llvm-cov` before substantial refactors.

## Commit & Pull Request Guidelines
Format commits imperatively and keep them focused (e.g., "Add Git accessor proxy support"); reference issues like `#123` when relevant. Before opening a PR, confirm `cargo fmt`, `cargo clippy`, and `cargo test -- --test-threads=1` succeed, document API-facing changes in `docs/` or `examples/`, and provide a summary that calls out impact, validation steps, and any follow-up TODOs. CI mirrors these checks on stable and beta toolchains across Linux and macOS.

## Security & Configuration Tips
Never commit secrets—CI supplies tokens such as `CRATES_IO_TOKEN` or `GITHUB_TOKEN`. When testing through restricted networks, set `HTTP(S)_PROXY` or `ALL_PROXY` to match local policy, and include proxy considerations in docs or PR notes when behaviour differs. Remove temporary credentials or debug logging before merging.

# AGENTS.md

## Hotfix Release Playbook

Current release behavior in this repo:
- Pushing a tag matching `vX.Y.Z` triggers `/Users/omerba/Workspace/evtx/.github/workflows/release.yml`.
- That workflow creates/updates the GitHub Release and uploads platform binaries.
- It does **not** publish crates to crates.io.

### Steps (security hotfix)
1. Prepare release commit on `master`:
   - Update `/Users/omerba/Workspace/evtx/Cargo.toml` version (for example `0.11.1`).
   - Update `/Users/omerba/Workspace/evtx/CHANGELOG.md` from `Unreleased` to the new version/date.
2. Validate locally:
   - `cargo test`
   - `cargo test --manifest-path utf16-simd/Cargo.toml`
   - `cargo publish --dry-run`
3. Publish crates to crates.io (manual, if needed):
   - Publish `/Users/omerba/Workspace/evtx/utf16-simd` first **only** if its version changed.
     - `cargo publish --manifest-path utf16-simd/Cargo.toml`
   - Then publish `/Users/omerba/Workspace/evtx`:
     - `cargo publish`
4. Create and push the tag:
   - `git tag vX.Y.Z`
   - `git push origin vX.Y.Z`
5. Verify release:
   - Confirm crates.io shows the new versions.
   - Confirm `/Users/omerba/Workspace/evtx/.github/workflows/release.yml` succeeds and assets are attached.

### Notes
- `evtx` depends on `utf16-simd` via `path + version`; if `evtx` starts requiring a new `utf16-simd` version, publish `utf16-simd` first.
- Do not use `/Users/omerba/Workspace/evtx/release.py` (removed; no longer reflects this flow).

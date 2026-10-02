# 06 - GitHub Releases and Release Automation

## 1. Anatomy of a GitHub Release

A **GitHub Release** wraps an immutable Git Tag with packaged release metadata:
- Auto-generated release notes (linking merged PRs and authors).
- Binary asset attachments (compiled Go/Rust binaries, Helm charts, Docker manifests).
- Cryptographic checksums (`SHA256SUMS`).

---

## 2. Automated Release with `semantic-release`

Modern CI/CD pipelines use tools like **`semantic-release`**:
1. Analyzes Conventional Commit messages since last release.
2. Determines the next SemVer version (`fix` -> Patch, `feat` -> Minor, `BREAKING CHANGE` -> Major).
3. Generates `CHANGELOG.md`.
4. Creates Git Tag, GitHub Release, and publishes packages automatically with zero human intervention.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - CODEOWNERS Architecture and Governance](./05-CODEOWNERS-Architecture-and-Governance.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |

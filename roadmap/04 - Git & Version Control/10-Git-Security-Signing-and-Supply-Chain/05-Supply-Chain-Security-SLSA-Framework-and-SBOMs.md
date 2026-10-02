# 05 - Supply Chain Security: SLSA Framework and SBOMs

## 1. What is SLSA (Supply-chain Levels for Software Artifacts)?

SLSA is a security framework created by Google and the OpenSSF to protect against tampering:
- **Source Integrity:** Verified, signed commits, mandatory multi-party code reviews.
- **Build Integrity:** Hermetic, isolated build runners (no network access during compilation).
- **Provenance:** Cryptographically proving that container image `app:v1.2.0` was built from exact Git commit `4a2b1c`.

---

## 2. Software Bill of Materials (SBOM)

An SBOM is a formal inventory of all third-party dependencies, licenses, and versions embedded within a release.
- Standards: **SPDX** and **CycloneDX**.
- Generated automatically in CI pipelines using tools like **Syft** and signed with **Cosign**.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Purging Secrets](./04-Purging-Leaked-Secrets-with-git-filter-repo.md) | [README](./README.md) | [06 - Repo Auditing](./06-Repository-Auditing-and-Access-Governance.md) |

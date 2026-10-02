# 04 - Basic Workflow: Staging, Committing, and Status

## 1. Staging Best Practices

Avoid blindly running `git add .` in large repositories. You risk staging temporary debug logs, `.env` secret files, or incomplete edits.

### Selective Patch Staging (`git add -p`)
`git add -p` allows interactive review and staging of individual code chunks (hunks):
```bash
git add -p src/main.py
```
Interactive prompt options:
- `y`: Stage this hunk.
- `n`: Do not stage this hunk.
- `s`: Split the hunk into smaller chunks.
- `e`: Manually edit the hunk.
- `q`: Quit interactive mode.

---

## 2. Conventional Commits Standard

High-performing DevOps teams enforce **Conventional Commits** for automated semantic versioning (`v1.2.3`) and changelog generation:

```
<type>[optional scope]: <short description>

[optional detailed body explaining WHY, not WHAT]

[optional footer(s): Breaking Changes, Issue references]
```

### Common Types:
- `feat:` A new feature for the user or API.
- `fix:` A bug fix.
- `docs:` Documentation changes only.
- `refactor:` Code changes that neither fix a bug nor add a feature.
- `ci:` Changes to CI/CD pipelines (GitHub Actions, GitLab CI).
- `chore:` Dependency updates, tooling adjustments.

---

## 3. Atomic Commits Principle
An **Atomic Commit** encapsulates a single logical change. If an issue requires a database migration and an API update, separate them into two distinct commits rather than combining them into one massive monolithic commit.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Git Configuration](./03-Git-Configuration-System-Global-and-Local.md) | [README](./README.md) | [05 - Diffing & Inspection](./05-Diffing-and-State-Inspection-Working-vs-Staged.md) |

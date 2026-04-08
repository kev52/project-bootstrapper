---
description: Generate CHANGELOG from conventional commits
allowed-tools: Read, Write, Bash(git log:*, git tag:*, git describe:*)
---

Generate or update CHANGELOG.md from conventional commits.

## Process

1. **Find last release tag** — `git describe --tags --abbrev=0` or fall back to first commit.
2. **Collect commits since tag** — `git log {tag}..HEAD --pretty=format:"%H %s"`.
3. **Parse conventional commit prefixes** and group:

```markdown
## [{version}] — {date}

### Added
- {feat: commits}

### Fixed
- {fix: commits}

### Changed
- {refactor: commits}

### Documentation
- {docs: commits}

### Testing
- {test: commits}

### Chores
- {chore: commits}
```

4. **Determine version bump** suggestion:
   - `feat:` → minor bump
   - `fix:` → patch bump
   - `BREAKING CHANGE` in body → major bump

5. **Write/append** to CHANGELOG.md in project root.
6. **Show diff** and confirm with user before finalizing.

## Usage

- `/gen-changelog` — unreleased changes since last tag
- `/gen-changelog v1.2.0` — label the unreleased section with this version
- `/gen-changelog --full` — regenerate entire changelog from all tags

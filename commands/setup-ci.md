---
description: Scaffold GitHub Actions CI/CD workflows for your stack
allowed-tools: Read, Glob, Grep, Write, Bash(git:*)
---

Generate a GitHub Actions CI workflow tailored to this project's stack and CLAUDE.md rules.

## Process

1. **Detect stack** from CLAUDE.md, package.json, pubspec.yaml, pyproject.toml, or requirements.txt
2. **Ask user** which workflow(s) to generate:
   - CI (lint + test + build on PR) — recommended for all
   - Deploy (staging/production) — if applicable
   - Release (tag-based) — if applicable

3. **Generate `.github/workflows/ci.yml`** with:

**Flutter:**
```yaml
- uses: subosito/flutter-action with channel: stable
- run: fvm install && fvm flutter pub get
- run: fvm flutter analyze
- run: fvm flutter test
- run: fvm flutter build (if applicable)
```

**TypeScript/Node:**
```yaml
- uses: actions/setup-node
- run: npm ci
- run: npm run lint
- run: npm run typecheck (if script exists)
- run: npm test
- run: npm run build
```

**Python:**
```yaml
- uses: actions/setup-python
- run: pip install -r requirements.txt
- run: ruff check .
- run: mypy . (if configured)
- run: pytest
```

4. **Add verification steps** matching CLAUDE.md:
   - If testing atom is active → fail on coverage threshold
   - If security atom is active → add `npm audit` / `pip audit` / `dart pub outdated` step
   - If observability atom is active → add health check smoke test

5. **Write file** and confirm with user before committing.

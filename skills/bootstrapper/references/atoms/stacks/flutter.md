## Flutter Stack

### Commands (Always Use FVM)

```bash
fvm flutter run                              # Run
fvm flutter pub get                          # Dependencies
fvm flutter pub run build_runner build       # Codegen
fvm flutter analyze                          # Lint (zero issues required)
fvm dart format .                            # Format
fvm flutter test                             # Test
```

### Patterns

- **CupertinoApp, not MaterialApp.** iOS-native look. Cupertino widgets only.
- **Riverpod for state.** Use `autoDispose` and `family` modifiers by default.
- **Services: abstract interface + concrete impl + mock.** Always all three.
- **Check `mounted` after every async gap** in StatefulWidget.
- **`const` constructors everywhere** they're possible.
- **Imports order:** Dart SDK → Flutter → third-party → project-relative.

### Code Generation

- **Slang** (i18n): `lib/i18n/*.i18n.json` → `strings.g.dart`. Base locale: `en`.
- **Envied** (env): `lib/env/env.dart` → `env.g.dart`.
- Never manually edit `*.g.dart` files. After changes → `fvm flutter pub run build_runner build`.

### i18n

- All 7 locale files must be updated: EN, DE, ES, FR, IT, KA, RU.
- Fallback strategy: `base_locale` (configured in `slang.yaml`).

### Verification (Stack-Specific)

1. `fvm flutter analyze` — zero issues
2. `fvm flutter test` — all pass
3. `fvm dart format .` — clean
4. i18n: all 7 locales updated
5. UI: Cupertino widgets, not Material

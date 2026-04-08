## TypeScript Stack

### Commands

```bash
npm run build                    # Build
npm run dev                      # Dev server
npm run test                     # Test
npm run lint                     # Lint
npm run lint:fix                 # Auto-fix
```

### Patterns

- **Strict mode always.** `strict: true` in tsconfig. No `any`, no `as` casts unless unavoidable (and commented why).
- **Prefer `interface` over `type`** for object shapes. Use `type` for unions and intersections.
- **Zod for runtime validation** at boundaries (API inputs, env vars, external data).
- **Barrel exports (`index.ts`) per feature folder.** Not per-file.
- **Async/await over raw Promises.** No `.then()` chains.
- **Enums: prefer `as const` objects** over TypeScript enums (better tree-shaking, type inference).

### Error Handling

- Custom error classes extending `Error` with typed `code` property.
- Never `catch (e: any)`. Always `catch (e: unknown)` and narrow.

### Verification (Stack-Specific)

1. `npm run build` — zero errors
2. `npm run test` — all pass
3. `npm run lint` — clean
4. No `any` types, no `@ts-ignore`

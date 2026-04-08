## Python Stack

### Commands

```bash
python -m pytest                 # Test
python -m mypy .                 # Type check
ruff check .                     # Lint
ruff format .                    # Format
```

### Patterns

- **Type hints everywhere.** All function params, return types, class attributes. Use `from __future__ import annotations`.
- **Pydantic for data models** at boundaries. Dataclasses for internal domain objects.
- **Never `Dict[str, Any]`** for structured data. Define a model.
- **Async with `asyncio`** for I/O-bound work. Sync for CPU-bound.
- **`pathlib.Path` over `os.path`.** Always.
- **f-strings over `.format()` or `%`.** Always.
- **Context managers** (`with`) for any resource that needs cleanup.

### Error Handling

- Custom exception classes with meaningful names. `OrderNotFoundError`, not `CustomError`.
- Never bare `except:`. Always `except SpecificError`.
- Use `raise ... from e` to preserve exception chains.

### Verification (Stack-Specific)

1. `python -m mypy .` — zero errors
2. `python -m pytest` — all pass
3. `ruff check .` — clean
4. No `Any` types, no `# type: ignore`

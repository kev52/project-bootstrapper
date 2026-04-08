## Security

- **Never trust input.** Validate and sanitize all user input at the boundary. Whitelist over blacklist. Reject early.
- **Parameterized queries only.** No string interpolation in SQL, NoSQL, or ORM queries. Ever.
- **No secrets in code.** API keys, tokens, passwords, connection strings go in environment variables or a secrets manager. Never in source, comments, or logs.
- **Pin dependencies.** Use lock files (`package-lock.json`, `pubspec.lock`, `poetry.lock`). Review dependency updates before merging.
- **Least privilege.** Services, API keys, and database users get the minimum permissions needed. No admin-by-default.
- **Encode output.** Escape HTML, SQL, and shell output at the rendering layer. Prevent XSS, injection, and command injection.
- **Auth boundaries are explicit.** Every endpoint/route must declare whether it requires authentication. No implicit "open by default."
- **Fail closed.** If a security check errors, deny access. Never fall through to "allowed."

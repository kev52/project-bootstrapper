## API Design

- **Nouns for resources, HTTP verbs for actions.** `GET /users/:id` not `GET /getUser`. `POST /orders` not `POST /createOrder`.
- **Consistent error shape.** Every error response follows one structure: `{ "error": { "code": "MACHINE_READABLE", "message": "Human readable", "details": [...] } }`. No surprises.
- **HTTP status codes mean what they say.** 200 OK, 201 Created, 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 409 Conflict, 422 Unprocessable, 500 Internal. No 200 with `{ "success": false }`.
- **Version your API.** URL prefix (`/v1/`) or header — pick one per project and stick with it.
- **Pagination from day one.** Any list endpoint returns `{ data: [], pagination: { total, page, per_page, next_cursor } }`. Never return unbounded arrays.
- **Validate at the boundary.** Every request body/query param gets schema validation (Zod, Pydantic, json_serializable) before touching business logic.
- **Idempotency for mutations.** POST/PUT/PATCH endpoints accept an idempotency key or are naturally idempotent. Retries must be safe.

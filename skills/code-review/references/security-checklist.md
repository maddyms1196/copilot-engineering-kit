# Security Review Checklist

Use the items relevant to the code under review. Each item is a question to answer with evidence from the code, not a box to tick.

## Input and injection
- Does untrusted input (request data, files, environment, messages, data from other services) reach a SQL or NoSQL query, shell command, file path, template, regex, XML parser, deserializer, `eval`-style call, or URL fetch without parameterization, allow-listing or escaping?
- Are file paths normalized and confined to an expected directory (no `../` traversal, no symlink escape)?
- Is output encoded for its context (HTML, attribute, JavaScript, SQL, shell, log line)?
- Are uploads checked for size, type and storage location?

## Authentication and authorization
- Is every new endpoint, handler or job covered by the same authentication as its neighbours?
- Is authorization checked on the server for the specific resource (object-level), not only by hiding UI?
- Are tokens, sessions and password-reset flows time-limited, single-use where appropriate, and compared in constant time?

## Secrets and sensitive data
- Are secrets, keys or credentials hard-coded, committed, logged, printed in errors, or sent to the client?
- Is sensitive or personal data minimized, masked in logs, and protected in transit and at rest according to project conventions?

## Cryptography and randomness
- Are standard library primitives used, rather than custom cryptography? Are deprecated algorithms (MD5, SHA-1 for security, ECB mode) avoided?
- Are security-relevant random values produced by a cryptographically secure generator?

## Web and network
- Are CSRF protections, CORS rules, cookie flags (`Secure`, `HttpOnly`, `SameSite`) and redirect targets handled safely?
- Can a user-supplied URL make the server fetch internal resources (SSRF)?
- Do outbound calls have timeouts, TLS verification, and bounded retries?

## Resource abuse
- Can a single request cause unbounded memory, CPU, file or query work (missing pagination, size limits, rate limits, catastrophic regex backtracking)?

## Dependencies and configuration
- Are new dependencies necessary, maintained, and pinned according to project practice?
- Do configuration changes weaken defaults (debug mode, permissive CORS, disabled TLS verification, broad permissions)?

## Concurrency and state
- Are there time-of-check/time-of-use races, unsafe shared mutable state, or non-atomic read-modify-write operations on security-relevant data?

## Agent-specific
- Does the code pass untrusted text into an LLM prompt or tool call that can trigger actions? Are tool permissions limited, and is model output validated before it is used?

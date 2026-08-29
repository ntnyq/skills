# Browser Runtime Boundaries

Use these rules when a Vue application depends on browser capabilities or logs
runtime failures.

## Capability Detection

- Identify whether the application must work on HTTP, embedded webviews, SSR, or
  other environments that may not provide a secure browser context.
- Before using camera, microphone, clipboard, Web Crypto, or another gated API,
  check both `window.isSecureContext` when relevant and the exact API's runtime
  availability.
- Treat capability checks as preconditions, not guarantees. Handle permission
  denial, missing devices, cancellation, browser policy, and runtime exceptions
  from the actual call.
- Give users actionable feedback that names the unavailable capability and, when
  relevant, explains the HTTPS or permission requirement. Do not fail silently.
- Do not assume `crypto.randomUUID()` exists in every deployment. When insecure
  contexts must be supported, use an already-installed portable identifier
  generator or the application's established ID wrapper. Do not add a dependency
  without checking the repository first.

## Runtime Logging And User Feedback

- Create scoped logger instances at module scope rather than on every function
  call. Name the scope after a stable route, feature, or responsibility.
- Keep fixed log messages consistent with the application's language. Pass the
  original error and structured context as separate arguments so stack and
  metadata remain inspectable.
- Determine whether the shared HTTP client already displays request failures. If
  it does, a catch block should normally log the failure without showing a second
  toast for the same error.
- When the product requires a custom message, explicitly suppress the transport
  client's default notification and let the caller own the one user-visible
  error message.
- Success messages, domain-result failures after a successful request, browser
  capability failures, and third-party SDK failures may need their own feedback
  because the HTTP client cannot represent them.

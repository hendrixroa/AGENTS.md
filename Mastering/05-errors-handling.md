# Error Handling

> **Scope:** Exception handling patterns, logging strategies, and failure recovery.
> **Objective:** Ensure system stability and observability.

## 1. General Philosophy

- **Fail Fast:** Do not process invalid data. If a precondition fails, stop immediately.
- **No Silent Failures:** NEVER swallow exceptions. An empty `catch` block is a critical violation.
- **Recoverability:** Distinguish between "Expected Failures" (e.g., User not found -> Return 404) and "Unexpected Bugs" (e.g., NullReference -> Throw 500).

## 2. Exception Handling Rules

### Do's

- **Use Specific Exceptions:** Catch `FileNotFoundException` or `SqlException`, not generic `Exception` (unless at the global app boundary).
- **Preserve Stack Trace:** When catching and re-throwing, pass the original exception as the inner exception.
  - Yes: `throw new ServiceException("Failed to process order", ex);`
  - No: `throw ex;` (Destroys the stack trace).
- **Clean Up Resources:** Always use `finally`, `using`, or `defer` blocks to close streams, connections, or files, regardless of success or failure.

### Don'ts

- **Control Flow via Exceptions:** Do not use exceptions for normal logic (e.g., do not use try-catch to check if a string is a number; use `TryParse`).
- **Catch All (Locally):** Do not catch `Exception` inside a low-level method just to log it and return null. Let it bubble up to a layer that knows how to handle it.

## 3. Logging Strategy

- **Context is King:** Log the *context* (User ID, Order ID, Input Params), not just the error message.
- **Levels:**
  - `ERROR`: System cannot continue current operation (DB down, NullRef).
  - `WARN`: System recovered but something was odd (Retry happened, deprecated API used).
  - `INFO`: High-level flow events (Job started, Order placed).
  - `DEBUG`: Developer details (Payload content).

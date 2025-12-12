# Code Style & Naming Conventions

## Naming Rules

- **General:** Use `camelCase` for variables/methods. Use `PascalCase` for Classes/Types.
- **Classes:** Must be substantives/nouns (e.g., `User`, `PaymentProcessor`). Avoid generic names like `Manager`, `Data`, `Info`, `Object`.
- **Methods:** Must be verbs (e.g., `calculateTotal`, `fetchUser`).
- **Booleans:** Must answer a question (e.g., `isValid`, `hasAccess`, `canEdit`).
- **Collections:** Use plural or specific suffix (e.g., `users`, `userList`).
- **Variables:**
  - NEVER use single letters (except `i, j, k` in short loops).
  - NEVER use Hungarian notation or type prefixes.
  - NEVER use obscure abbreviations. Use international acronyms only (e.g., `HTTP`, `ID`).
- **Constants:** Use `UPPER_SNAKE_CASE`.

## Commenting Rules

- **DO:**
  - Comments explaining syntax ("Declaring variable").
  - Author/Date headers (Use Git for that).
  - Commented-out code (Delete it).
  - Comments apologizing for bad names (Fix the name instead).
- **DO NOT:**
  - Explain the **WHY** (Business logic decisions).
  - Explain complex Regex patterns.
  - `TODO` comments must include context.

## Formatting

- One instruction per line.
- Attributes at the top of the class.
- Group related methods together.
- Use spaces, not tabs.

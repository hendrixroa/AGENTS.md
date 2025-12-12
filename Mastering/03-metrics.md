# Quality Metrics & Limits

The agent must validate generated code against these thresholds.

## Class Scope

- **Lines of Code (LOC):** 200 - 500 lines maximum.
- **Attributes:** 3 - 5 main attributes preferred, if more than 5, group them into Value Objects.
- **Methods:** 10 - 20 methods maximum.
- **Cohesion:** If a class manages disjointed data (e.g., User Auth AND PDF Generation), SPLIT IT.

## Method/Function Scope

- **Lines of Code:** 10 - 30 lines ideal. MAX 50 lines.
- **Parameters:** Max 3. If > 3, encapsulate in a Parameter Object / DTO.
- **Cyclomatic Complexity:** Max 5. Minimize nested `if/else` or loops.
- **Return Statements:** Avoid deeply nested returns. Use Guard Clauses (`if (!valid) return;`) at the beginning.

## Inheritance

- **Depth:** Max 3 levels deep.
- **Strategy:** Favor Composition over Inheritance.

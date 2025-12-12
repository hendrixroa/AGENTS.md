# Architecture & Design Principles

## Anti-Patterns to Avoid

1. **Rigidity:** Hard to change because it affects too many other parts.
2. **Fragility:** Changes break unrelated parts.
3. **Immobility:** Code cannot be reused/ported because it carries too much baggage.
4. **Viscosity:** It is easier to do the "wrong thing" (hack) than the "right thing".

## Design Patterns & Principles

- **SOLID:** Strictly adhere to SRP, OCP, LSP, ISP, and DIP.
- **GRASP Principles:**
  - **Controller:** Use a controller for UI layer entry points.
  - **Information Expert:** Assign responsibility to the class that has the data.
  - **Low Coupling:** Minimize dependencies.
  - **High Cohesion:** Keep related things together.
  - **Creator:** A class B should create class A if B contains, records, or closely uses A.

## Detection Strategies

- **Abbott Approach:** Identify nouns (classes) and verbs (methods) from requirements.
- **Primitive Obsession:** If a class has many primitive fields (strings/ints) that belong together (e.g., address fields), extract them into a Value Object.
- **Surprise Minimum Principle:** A method name must describe EVERYTHING it does. If it does something not in the name, split it.
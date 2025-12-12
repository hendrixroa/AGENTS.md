# Testing Standards

> **Scope:** xUnit Patterns, Test Smells, and Anti-Patterns (Based on Gerard Meszaros).
> **Objective:** Create a maintenance-free, reliable, and fast test suite.
> **Enforcement:** Strict.

## 1. Core Philosophy

### Independence & Isolation

- **Atomic Tests:** Each test MUST verify **one single logical condition**. If a test fails, the reason should be obvious from the test name alone.
- **Total Independence:** Tests MUST be able to run in *any order*. NEVER rely on state left by a previous test (Chained Tests).
- **SUT Isolation:** The System Under Test (SUT) must be isolated from its environment. NEVER let a unit test talk to a real database, network, or file system. Use **Test Doubles**.

### State vs. Behavior

- **Prefer State Verification:** Assert that the final state is correct (e.g., `Assert.Equal(expected, actual)`).
- **Avoid Behavior Verification:** Use Mock interactions (e.g., `Verify(x => x.CallWasMade())`) ONLY when testing side effects (like sending an email) where state cannot be observed.

## 2. Test Fixture Patterns (Setup)

### DO

- **Fresh Fixture:** Create a brand new, clean fixture for EVERY test execution. Use `SetUp`/`BeforeEach` or helper methods to instantiate a fresh world.
- **Minimal Fixture:** Only configure the data strictly required for the test. If a property doesn't affect the outcome, don't set it explicitly.
- **Creation Methods:** Use Factory Methods or Builders with intent-revealing names to hide complex setup.
  - *Example:* `var user = CreateValidUserWithAdminRole();`

### DON'T

- **Shared Fixture:** NEVER reuse a mutable instance across tests. This leads to **Interacting Tests** and debugging nightmares.
- **General Fixture:** Avoid massive "setup everything" methods. They create **Obscure Tests** where the relationship between data and verification is unclear.
- **Mystery Guest:** Do not use external files or database rows containing "magic data" that is not visible within the test code.

## 3. Test Logic & Verification

### DO

- **AAA Pattern:** Visually separate **Arrange**, **Act**, and **Assert**.
- **Guard Assertions:** Use assertions to validate assumptions *before* the SUT acts. Fail fast if the setup is wrong.
- **Custom Assertions:** Encapsulate complex verification logic (e.g., verifying a JSON structure) into a reusable method with a clear name.
  - **Problem:** Repeated complex verification logic (e.g., checking a JSON schema or a complex object state) leads to code duplication and **Assertion Roulette**.
  - **Solution:** Extract logic into **Custom Assertion Methods**.
    - *Example:* Instead of writing 5 lines to check an address, write: `AssertAddressMatches(expectedAddress, actualUser.Address)`.
  - **Benefit:** Turns the test into a readable "Specification"

### DON'T

- **Conditional Logic:** NEVER use `if`, `switch`, or loops inside a test. A test must be a linear sequence. If you need logic, you likely need two separate tests.
- **Assertion Roulette:** Avoid multiple asserts without messages. If one fails, you won't know which one it was. Ideally, stick to one logical assert per test.
- **Test Hooks:** NEVER put logic in production code like `if (TESTING_MODE)`. Use Dependency Injection or Test-Specific Subclasses instead.

## 4. Double Strategy (Mocks & Stubs)

- **Stubs:** Use them to provide indirect **inputs** to the SUT (e.g., "When the repo is asked for ID 1, return User A").
- **Mocks:** Use them to verify indirect **outputs** of the SUT (e.g., "Verify that the EmailService.Send() was called once").
- **Fakes:** Use lightweight in-memory implementations (e.g., `InMemoryDatabase`, `FakeFileSystem`) to replace slow dependencies.

## 5. Integration & Database Strategy

When you MUST test with a database (Integration Tests):

- **Transaction Rollback Teardown:** Wrap the test in a transaction and roll it back in the `Teardown/After` phase. This ensures a **Fresh Fixture** instantly.
- **Sandbox Pattern:** Each developer/agent must use a dedicated DB instance. NEVER share a development database for running tests (**Test Run Wars**).
- **No Leftovers:** A test must never leave data behind.

## 6. Anti-Patterns Detection (The Red Flags)

The Agent must refuse to generate code that exhibits these smells:

1. **The Slow Poke:** Unit tests taking > 100ms. (Likely hitting I/O).
2. **The Fragile Test:** Tests that break when you change internal implementation details (Overspecified Mocks).
3. **The Liar:** A test that passes but verifies nothing (e.g., catching an Exception and doing nothing).
4. **The Humble Object Violation:** Trying to test complex logic through a UI or Controller. Extract the logic to a domain class.

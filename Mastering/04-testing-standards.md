##### Modern Standards for Test Antipatterns and Clean Automation

Here is the unified guide to test antipatterns and smells, combining the language-agnostic concepts from the *xUnit Test Patterns* book with the strict, highly-enforced rules and "Red Flags" established in 04-testing-standards.md.

##### 1. Obscure Tests, General Fixtures & Mystery Guests
**Summary:** Tests that are difficult to understand at a glance because they hide external dependencies, construct massive setups, or contain irrelevant information.
* **Do:** Visually structure tests using the **AAA Pattern** (Arrange, Act, Assert) to separate phases. Keep your fixtures **Minimal** by configuring only the data strictly required for the outcome. Hide complex configurations using **Creation Methods** with intent-revealing names (e.g., CreateValidUserWithAdminRole()).
* **Don't:** Rely on **Mystery Guests** like external files or database rows containing magic data not visible in the code. Build **General Fixtures** that configure massive "setup everything" methods, which obscures the relationship between the data and the verification.

##### 2. Conditional Test Logic
**Summary:** Tests that contain control structures, creating multiple execution paths that make the test non-linear and difficult to reliably verify.
* **Do:** Keep tests as a strictly linear sequence. Use **Guard Assertions** to validate assumptions and fail fast *before* the System Under Test (SUT) acts.
* **Don't:** NEVER use if, switch, or loops inside a test. If conditional logic feels necessary, you almost certainly need two separate tests instead.

##### 3. Assertion Roulette & "The Liar"
**Summary:** Tests where failures are impossible to trace to a specific assertion, or false-positive tests that pass but verify nothing.
* **Do:** Prefer **State Verification** (e.g., Assert.Equal) to assert that the final state is correct. Extract complex, repeated verification logic (e.g., validating a JSON structure) into clear **Custom Assertions** (e.g., AssertAddressMatches) to turn the test into a readable specification.
* **Don't:** Write **The Liar**, a test that passes but verifies nothing (like catching an Exception and doing nothing). Use multiple assertions without distinct messages, which leads to Assertion Roulette. Use Behavior Verification (Mocks) unless you are strictly testing side effects where state cannot be observed.

##### 4. Erratic Tests & Interacting Tests
**Summary:** Flaky tests that behave inconsistently or fail sporadically due to shared state, leftover data, or execution order.
* **Do:** Write **Atomic Tests** that verify one single logical condition. Use a **Fresh Fixture** to instantiate a clean world for every single test execution. When integration testing, ensure **No Leftovers** by keeping a clean environment.
* **Don't:** Rely on **Chained Tests** where a test assumes the state left by a previous test. NEVER reuse a mutable instance across tests, as this Shared Fixture approach leads to Interacting Tests.

##### 5. Slow Tests & "The Slow Poke"
**Summary:** Tests that take too long to run, mostly due to I/O operations, which ultimately discourages developers from running them.
* **Do:** Ensure total **SUT Isolation** using Test Doubles. Use lightweight **Fakes** (e.g., InMemoryDatabase or FakeFileSystem) to replace slow dependencies. For database tests, use **Transaction Rollback Teardown** (wrapping tests in a transaction and rolling it back in the teardown phase) to instantly reset the fixture.
* **Don't:** Write unit tests taking **> 100ms**, which is a Red Flag known as **The Slow Poke**. Allow unit tests to communicate with a real database, network, or file system.

##### 6. Fragile Tests & Overspecification
**Summary:** Tests that break due to internal implementation changes rather than behavioral changes, tightly coupling the test to the SUT.
* **Do:** Clearly separate your Test Doubles: Use **Stubs** to provide indirect *inputs* to the SUT, and use **Mocks** solely to verify indirect *outputs*.
* **Don't:** Create Overspecified Mocks that break when you change internal implementation details. Share a development database to run tests, which causes **Test Run Wars**; instead, each agent or developer must use a dedicated **Sandbox** instance.

##### 7. Test Logic in Production & The Humble Object Violation
**Summary:** Mingling testing-only logic and flags inside production code, or attempting to test complex business logic through an untestable context.
* **Do:** Use Dependency Injection or Test-Specific Subclasses to isolate behavior. Extract logic from untestable contexts into a clean domain class.
* **Don't:** Insert **Test Hooks** (e.g., if (TESTING_MODE)) into production code. Commit **The Humble Object Violation** by trying to test complex logic through a UI or Controller layer.

##### 8. Test Code Duplication
**Summary:** Repeating the exact same setup or assertion logic across multiple tests, massively increasing the test maintenance cost.
* **Do:** Encapsulate duplicated logic into **Creation Methods** and **Custom Assertions**. Visually separate code using the **AAA Pattern** to make duplication obvious to spot and extract.
* **Don't:** Create tests via Cut-and-Paste reuse, or repeat complex verification lines instead of extracting them into a reusable method.

[skill.txt](https://github.com/user-attachments/files/28175872/skill.txt)
---
name: unit-test-generator
description: >
  Generates complete, production-quality unit tests from a function or code snippet.
  Use this skill whenever the user wants to write tests, generate test cases, cover edge cases,
  test a function, improve test coverage, or says things like "write unit tests for this",
  "generate tests", "what should I test here?", "help me test this function", or "add test coverage".
  Also trigger if the user pastes code without an explicit request but the context implies they want tests.
  Supports Jest, pytest, JUnit, Vitest, Mocha, Go testing, and RSpec — infer from the language if not specified.
  Always use this skill rather than writing tests from scratch, even for small snippets.
---

# Unit Test Generator

Produces complete, runnable unit tests covering the happy path, edge cases, boundary values, type errors, and failure modes. Tests are ready to paste into a test file with zero modification.

---

## Step 1: Identify the Framework

Use the framework the user specifies. If none is given, infer from the language:

| Language       | Default Framework |
|----------------|-------------------|
| JavaScript/TS  | Jest (or Vitest if vite config detected) |
| Python         | pytest            |
| Java           | JUnit 5           |
| Go             | Go testing (`testing` package) |
| Ruby           | RSpec             |
| C#             | xUnit             |

If the language is ambiguous, make a reasonable guess and state it at the top of your output.

---

## Step 2: Analyze the Code

Before writing any tests, mentally extract:

- **Function signature**: name, parameters, return type
- **Expected behavior**: what it does on valid inputs
- **Dependencies**: external calls, I/O, randomness, time — these need mocking
- **Error handling**: does it throw, return null/None, or return an error value?

---

## Step 3: Generate Test Cases

Cover all of these categories — skip only those that genuinely don't apply:

### ✅ Happy Path
- Typical, valid inputs producing expected outputs
- At least 2–3 representative examples with concrete values

### 🔲 Edge Cases
- Empty inputs (empty string, empty array, zero)
- Single-element collections
- Very large inputs (if relevant)
- Case sensitivity (if strings are involved)

### 📐 Boundary Values
- Min/max numeric ranges
- Off-by-one: length - 1, length, length + 1
- Null/None/undefined inputs
- Zero, negative numbers (if numeric)

### 💥 Error / Failure Modes
- Invalid types (if the language is dynamically typed)
- Out-of-range values
- Null/undefined where not expected
- Async rejection / exception throwing

### 🔁 Idempotency / State (if applicable)
- Calling the function twice produces consistent results
- Mutations don't bleed across test cases

---

## Step 4: Write the Tests

Follow the conventions for the target framework. See `references/frameworks.md` for syntax details on each supported framework.

**Universal rules:**
- Each test has a single, clear assertion (or a tight group if they test one concept)
- Test names describe the scenario: `"returns empty array when input is null"` not `"test1"`
- No shared mutable state between tests — each test is self-contained
- Mock all external dependencies (network, filesystem, DB, time, randomness)
- Use `beforeEach`/`setUp`/fixtures to avoid repetitive setup
- Group related tests with `describe`/nested classes/modules

**Output format:**
- Output a single complete file, ready to paste
- Include all necessary imports at the top
- Add a one-line comment above each test group explaining what's being tested
- Do NOT truncate — output every single test case

---

## Step 5: Closing Notes (brief)

After the code block, add a short section (3–5 bullets) noting:
- Which framework and version assumptions were made
- Any mocks or test doubles needed and why
- Anything the user should configure (test runner setup, env vars, etc.)
- Suggested additions if the code has untestable parts (e.g., needs DI refactor)

---

## Reference

For framework-specific syntax, imports, and patterns, read:
👉 `references/frameworks.md`

Load this file when writing tests — it has concrete examples for every supported framework.

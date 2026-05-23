# Framework Reference

Concrete syntax patterns for each supported testing framework.

---

## Jest (JavaScript / TypeScript)

```javascript
import { myFunction } from '../src/myFunction';

describe('myFunction', () => {
  // Happy path
  it('returns the correct result for valid input', () => {
    expect(myFunction(2, 3)).toBe(5);
  });

  // Edge case
  it('returns 0 when both inputs are zero', () => {
    expect(myFunction(0, 0)).toBe(0);
  });

  // Error / throws
  it('throws TypeError when input is null', () => {
    expect(() => myFunction(null, 1)).toThrow(TypeError);
  });

  // Async
  it('resolves with correct data', async () => {
    const result = await myAsyncFunction('id-123');
    expect(result).toEqual({ id: 'id-123', name: 'Test' });
  });

  // Mocking
  it('calls the API once', () => {
    const mockFetch = jest.fn().mockResolvedValue({ ok: true });
    global.fetch = mockFetch;
    myFunction();
    expect(mockFetch).toHaveBeenCalledTimes(1);
  });
});
```

**Setup**: `npm install --save-dev jest` | Run: `npx jest`

---

## Vitest (JavaScript / TypeScript)

Same API as Jest; change imports:

```javascript
import { describe, it, expect, vi, beforeEach } from 'vitest';
import { myFunction } from '../src/myFunction';

describe('myFunction', () => {
  it('returns expected value', () => {
    expect(myFunction('hello')).toBe('HELLO');
  });

  it('mocks a dependency', () => {
    const spy = vi.fn().mockReturnValue(42);
    expect(spy()).toBe(42);
  });
});
```

**Setup**: `npm install --save-dev vitest` | Run: `npx vitest`

---

## Mocha + Chai (JavaScript)

```javascript
const { expect } = require('chai');
const { myFunction } = require('../src/myFunction');

describe('myFunction', () => {
  it('returns correct result', () => {
    expect(myFunction(1, 2)).to.equal(3);
  });

  it('throws on invalid input', () => {
    expect(() => myFunction(null)).to.throw(TypeError);
  });
});
```

**Setup**: `npm install --save-dev mocha chai` | Run: `npx mocha`

---

## pytest (Python)

```python
import pytest
from mymodule import my_function

class TestMyFunction:
    # Happy path
    def test_returns_correct_result(self):
        assert my_function(2, 3) == 5

    # Edge case: empty input
    def test_empty_string_returns_empty(self):
        assert my_function("") == ""

    # Boundary: None input
    def test_none_input_raises_type_error(self):
        with pytest.raises(TypeError):
            my_function(None)

    # Parametrize for multiple cases
    @pytest.mark.parametrize("a,b,expected", [
        (1, 1, 2),
        (0, 5, 5),
        (-1, 1, 0),
    ])
    def test_addition_cases(self, a, b, expected):
        assert my_function(a, b) == expected

    # Mocking
    def test_calls_external_service(self, mocker):
        mock_call = mocker.patch("mymodule.external_service")
        my_function("input")
        mock_call.assert_called_once_with("input")
```

**Setup**: `pip install pytest pytest-mock` | Run: `pytest`

---

## JUnit 5 (Java)

```java
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

class MyFunctionTest {

    private MyClass subject;

    @BeforeEach
    void setUp() {
        subject = new MyClass();
    }

    // Happy path
    @Test
    void returnsCorrectResultForValidInput() {
        assertEquals(5, subject.myFunction(2, 3));
    }

    // Exception
    @Test
    void throwsIllegalArgumentForNullInput() {
        assertThrows(IllegalArgumentException.class,
            () -> subject.myFunction(null));
    }

    // Multiple cases
    @ParameterizedTest
    @CsvSource({"1,1,2", "0,5,5", "-1,1,0"})
    void additionCases(int a, int b, int expected) {
        assertEquals(expected, subject.myFunction(a, b));
    }
}
```

**Setup**: Add `junit-jupiter` to `pom.xml` or `build.gradle` | Run: `./mvnw test` or `./gradlew test`

---

## Go testing (Go)

```go
package mypackage

import (
    "testing"
)

// Happy path
func TestMyFunction_ValidInput(t *testing.T) {
    result := MyFunction(2, 3)
    if result != 5 {
        t.Errorf("expected 5, got %d", result)
    }
}

// Table-driven tests
func TestMyFunction_Cases(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        expected int
    }{
        {"positive numbers", 2, 3, 5},
        {"zeros", 0, 0, 0},
        {"negative", -1, 1, 0},
    }
    for _, tc := range tests {
        t.Run(tc.name, func(t *testing.T) {
            got := MyFunction(tc.a, tc.b)
            if got != tc.expected {
                t.Errorf("MyFunction(%d, %d) = %d; want %d", tc.a, tc.b, got, tc.expected)
            }
        })
    }
}

// Error case
func TestMyFunction_ReturnsError(t *testing.T) {
    _, err := MyFunctionWithError(-1)
    if err == nil {
        t.Error("expected error for negative input, got nil")
    }
}
```

**Run**: `go test ./...`

---

## RSpec (Ruby)

```ruby
require 'spec_helper'
require_relative '../lib/my_module'

RSpec.describe MyModule::MyFunction do
  # Happy path
  it 'returns the correct result for valid input' do
    expect(described_class.call(2, 3)).to eq(5)
  end

  # Edge case
  it 'returns nil for empty string input' do
    expect(described_class.call('')).to be_nil
  end

  # Exception
  it 'raises ArgumentError for nil input' do
    expect { described_class.call(nil) }.to raise_error(ArgumentError)
  end

  # Multiple cases
  [
    [1, 1, 2],
    [0, 5, 5],
    [-1, 1, 0],
  ].each do |a, b, expected|
    it "returns #{expected} for #{a} + #{b}" do
      expect(described_class.call(a, b)).to eq(expected)
    end
  end

  # Mocking
  it 'calls the external service' do
    service = instance_double(ExternalService)
    allow(service).to receive(:call).and_return(true)
    expect(service).to receive(:call).once
    described_class.call('input', service: service)
  end
end
```

**Setup**: `gem install rspec` | Run: `bundle exec rspec`
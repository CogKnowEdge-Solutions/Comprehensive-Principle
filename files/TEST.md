# Comprehensive Testing Guide

This guide provides a complete framework for testing any lab, pipeline, API, or application—following Test-Driven Development (TDD) principles and practical testing methodologies.

---

## Table of Contents

1. [General Test Case Design Framework](#general-test-case-design-framework)
2. [Test-Driven Development Cycle](#test-driven-development-cycle)
3. [TDD Best Practices](#tdd-best-practices)
4. [Running Tests](#running-tests)
5. [Development Workflow](#development-workflow)
6. [Troubleshooting & Debugging](#troubleshooting--debugging)

---

## General Test Case Design Framework

### Overview

This framework applies to testing any dev-built lab, pipeline, API, or application—ML pipelines, web apps, notebooks, backend services, etc. Not every category applies to every project; select the ones relevant to what you're testing.

**Key Principle**: For any code, ask: *"What does this code assume that isn't actually guaranteed?"* Each unverified assumption becomes one test case. Aim for 10-15 test cases that surface real risk, rather than padding the count.

---

### 1. Boundary & Limit Conditions

Test any hardcoded threshold, size limit, count, or cutoff in the code.

**Test Cases:**
- Exactly at the limit
- One below the limit
- One above the limit
- Zero / empty case
- Maximum / very large case

**Example:**
```python
def test_at_limit(self):
    """Test value exactly at the limit (e.g., 100)"""
    result = process_data(100)
    assert result is not None

def test_below_limit(self):
    """Test value one below limit"""
    result = process_data(99)
    assert result is not None

def test_above_limit(self):
    """Test value one above limit"""
    with pytest.raises(ValueError):
        process_data(101)

def test_zero_or_empty(self):
    """Test zero/empty case"""
    result = process_data(0)
    assert result is None
```

---

### 2. Input Validity & Variation

Test what a real user might actually type or send in.

**Test Cases:**
- Empty / blank / null input
- Extremely long or oversized input
- Wrong type/format (text where number expected, wrong language, special characters)
- Malicious or adversarial input (injection attempts, unexpected commands)
- Valid but unusual/edge-of-scope input (off-topic, ambiguous)

**Example:**
```python
def test_empty_input(self):
    """Test with empty string"""
    with pytest.raises(ValueError):
        process_query("")

def test_null_input(self):
    """Test with None"""
    with pytest.raises(TypeError):
        process_query(None)

def test_oversized_input(self):
    """Test with extremely long input"""
    huge_input = "a" * 100000
    with pytest.raises(ValueError):
        process_query(huge_input)

def test_wrong_type(self):
    """Test with wrong data type"""
    with pytest.raises(TypeError):
        process_number("not_a_number")

def test_malicious_input(self):
    """Test injection attempt"""
    with pytest.raises(ValueError):
        process_query("'; DROP TABLE users; --")
```

---

### 3. Control Flow & Logic

Test loops, conditionals, retries, and state transitions.

**Test Cases:**
- Every branch gets exercised at least once
- Loops terminate correctly (not infinite, not cut off early)
- Counters/flags/state update correctly at each step

**Example:**
```python
def test_all_branches_executed(self):
    """Ensure if/else branches both execute"""
    result_true = process_with_flag(True)
    result_false = process_with_flag(False)
    assert result_true != result_false

def test_loop_terminates(self):
    """Ensure loop terminates, not infinite"""
    start = time.time()
    result = process_list([1, 2, 3, 4, 5])
    elapsed = time.time() - start
    assert elapsed < 1.0  # Should complete quickly
    assert len(result) == 5

def test_state_updates_correctly(self):
    """Ensure state transitions happen correctly"""
    agent = SimpleAgent()
    assert agent.state == "idle"
    agent.start()
    assert agent.state == "running"
    agent.stop()
    assert agent.state == "stopped"
```

---

### 4. Output Correctness & Format

Test the structure and semantics of output.

**Test Cases:**
- Output is structurally correct (expected fields/format/schema)
- Output is semantically correct, not just present
- Handling of malformed upstream responses

**Example:**
```python
def test_output_structure(self):
    """Verify output has expected fields"""
    result = get_user_data(123)
    assert "id" in result
    assert "name" in result
    assert "email" in result

def test_output_semantic_correctness(self):
    """Verify output values are correct"""
    result = calculate_total([10, 20, 30])
    assert result == 60  # Not just that result exists

def test_malformed_upstream_response(self):
    """Test handling of bad input from dependencies"""
    with patch('requests.get', return_value={"invalid": "response"}):
        with pytest.raises(ValueError):
            process_external_data()
```

---

### 5. Error Handling & Failure Modes

Deliberately break dependencies the code relies on.

**Test Cases:**
- Invalid credentials / auth failures
- Network or external service failures (timeout, downtime, rate limits)
- Fails fast with a clear message vs. silent failure vs. raw crash

**Example:**
```python
def test_invalid_credentials(self):
    """Test behavior with bad auth"""
    with pytest.raises(AuthenticationError):
        api_client = APIClient(api_key="invalid-key")
        api_client.authenticate()

def test_network_timeout(self):
    """Test handling of network timeout"""
    with patch('requests.get', side_effect=requests.Timeout):
        with pytest.raises(ConnectionError):
            fetch_data()

def test_rate_limit_handling(self):
    """Test behavior when rate-limited"""
    with patch('api.call', side_effect=RateLimitError):
        with pytest.raises(RateLimitError) as exc_info:
            api_call()
        assert "rate limit" in str(exc_info.value).lower()

def test_clear_error_messages(self):
    """Ensure error messages are helpful"""
    try:
        process_file("nonexistent.txt")
    except FileNotFoundError as e:
        assert "nonexistent.txt" in str(e)
        assert len(str(e)) > 10  # Message is not empty
```

---

### 6. Consistency & Reproducibility

Test that identical inputs produce consistent results.

**Test Cases:**
- Same input, run twice = same result (especially if determinism is claimed)
- Logic-level consistency (decisions/branches taken) vs. exact output text

**Example:**
```python
def test_deterministic_output(self):
    """Verify same input produces same output"""
    input_data = [5, 3, 1, 4, 2]
    result1 = sort_data(input_data)
    result2 = sort_data(input_data)
    assert result1 == result2

def test_consistency_across_runs(self):
    """Verify consistency without exact string match"""
    query = "What is 5 + 3?"
    response1 = agent.process(query)
    response2 = agent.process(query)
    # Check semantic equivalence, not string equality
    assert "8" in response1 and "8" in response2
```

---

### 7. Correctness of Core Logic

Test that the code produces the right answer, not just an answer.

**Test Cases:**
- Produces the correct result
- Avoids false confidence (e.g., answering when it shouldn't)

**Example:**
```python
def test_correct_calculation(self):
    """Verify calculation is correct"""
    result = calculate_compound_interest(principal=1000, rate=5, years=2)
    assert abs(result - 1102.5) < 0.01  # Known correct value

def test_knows_when_to_decline(self):
    """Verify doesn't provide answers when uncertain"""
    result = agent.answer("What is the meaning of life?")
    if "don't know" in result.lower() or "uncertain" in result.lower():
        assert True  # Correctly declined to answer
    else:
        assert "42" in result  # Or has reasonable answer
```

---

### 8. Security & Configuration Hygiene

Test for exposed secrets and improper configuration.

**Test Cases:**
- No hardcoded secrets/credentials in code
- Sensitive config is externalized (env vars, secrets manager)

**Example:**
```python
def test_no_hardcoded_secrets(self):
    """Ensure secrets aren't hardcoded"""
    with open('agent.py', 'r') as f:
        content = f.read()
    assert 'sk-' not in content  # No API keys
    assert 'password=' not in content.lower()

def test_uses_environment_variables(self):
    """Verify config uses env vars"""
    with patch.dict(os.environ, {'API_KEY': 'test-key'}):
        config = load_config()
        assert config.api_key == 'test-key'

def test_missing_config_fails_gracefully(self):
    """Test missing config doesn't expose secrets"""
    with patch.dict(os.environ, {}, clear=True):
        with pytest.raises(ConfigError) as exc_info:
            load_config()
        assert "API_KEY" in str(exc_info.value)
```

---

### 9. Resource & Environment Constraints

Test rate limits, quotas, and missing dependencies.

**Test Cases:**
- Rate limits and quotas are respected
- Missing/incompatible dependencies are detected

**Example:**
```python
def test_respects_rate_limit(self):
    """Verify API respects rate limits"""
    for i in range(5):
        result = api.call()
        assert result is not None
    # 6th call within window should fail
    with pytest.raises(RateLimitError):
        api.call()

def test_detects_missing_dependency(self):
    """Test missing required library"""
    with patch.dict(sys.modules, {'required_lib': None}):
        with pytest.raises(ImportError):
            import_required_module()

def test_version_compatibility(self):
    """Test version requirements"""
    import anthropic
    # Ensure minimum version
    assert parse_version(anthropic.__version__) >= parse_version("0.25.0")
```

---

### 10. Execution Order / Environment Independence

Test that code works on a fresh run without prior state.

**Test Cases:**
- Works on a fresh run (clean restart)
- Doesn't silently depend on leftover state from a previous run

**Example:**
```python
def test_fresh_initialization(self):
    """Test code works on clean startup"""
    # Ensure no leftover state
    if os.path.exists("cache.pkl"):
        os.remove("cache.pkl")
    
    agent = SimpleAgent()
    result = agent.process("test query")
    assert result is not None

def test_no_global_state_dependency(self):
    """Test doesn't depend on previous execution"""
    result1 = process_independent_query("query1")
    result2 = process_independent_query("query2")
    # Results shouldn't depend on order
    assert result1 != result2
```

---

### 11. Documentation vs. Actual Behavior

Test that written spec matches actual behavior.

**Test Cases:**
- Documented behavior matches actual implementation
- Examples in documentation work as written

**Example:**
```python
def test_doc_example_works(self):
    """Verify documentation example executes correctly"""
    # From docs: "Example: calculator.add(5, 3) returns 8"
    calculator = Calculator()
    result = calculator.add(5, 3)
    assert result == 8

def test_signature_matches_docs(self):
    """Verify function signature matches documentation"""
    sig = inspect.signature(process_data)
    params = list(sig.parameters.keys())
    assert params == ["input_data", "threshold", "timeout"]
```

---

### 12. Cleanup & Side Effects

Test that code cleans up after itself.

**Test Cases:**
- Temp files, cached data, connections are cleaned up
- Safe to re-run without conflicts

**Example:**
```python
def test_cleanup_temp_files(self):
    """Verify temp files are cleaned up"""
    temp_dir = tempfile.mkdtemp()
    process_with_temp(temp_dir)
    assert not os.path.exists(temp_dir) or len(os.listdir(temp_dir)) == 0

def test_safe_to_rerun(self):
    """Test can be safely re-executed"""
    output_file = "output.txt"
    
    # Clean up first
    if os.path.exists(output_file):
        os.remove(output_file)
    
    # Run once
    generate_output(output_file)
    assert os.path.exists(output_file)
    
    # Should be safe to run again (overwrite, not error)
    generate_output(output_file)  # Should not raise error
    assert os.path.exists(output_file)
```

---

## Test-Driven Development Cycle

### Overview

TDD follows a three-phase cycle: RED → GREEN → REFACTOR

```
┌─────────────────────────────────────────┐
│         TDD Development Cycle           │
├─────────────────────────────────────────┤
│                                         │
│  1. RED: Write failing tests            │
│     └─> Run tests, see them fail        │
│                                         │
│  2. GREEN: Write minimal code           │
│     └─> Run tests, see them pass        │
│                                         │
│  3. REFACTOR: Improve code              │
│     └─> Run tests to ensure quality     │
│                                         │
│  4. REPEAT: For each feature            │
│                                         │
└─────────────────────────────────────────┘
```

---

### Phase 1: RED - Write Failing Tests

#### Step 1: Define What You Want to Build

Before writing code, write a test that describes the desired behavior.

**Example: Calculator Tests**

```python
# File: test_agent.py

import pytest
from agent import Calculator, SimpleAgent

class TestCalculatorTool:
    """Tests for the Calculator tool"""
    
    def test_add_two_numbers(self):
        """Addition should return correct sum"""
        calculator = Calculator()
        result = calculator.add(5, 3)
        assert result == 8  # This will FAIL initially
    
    def test_subtract_two_numbers(self):
        """Subtraction should return correct difference"""
        calculator = Calculator()
        result = calculator.subtract(10, 3)
        assert result == 7
    
    def test_multiply_two_numbers(self):
        """Multiplication should return correct product"""
        calculator = Calculator()
        result = calculator.multiply(4, 5)
        assert result == 20
    
    def test_divide_two_numbers(self):
        """Division should return correct quotient"""
        calculator = Calculator()
        result = calculator.divide(20, 4)
        assert result == 5.0
    
    def test_divide_by_zero_raises_error(self):
        """Division by zero should raise ValueError"""
        calculator = Calculator()
        with pytest.raises(ValueError):
            calculator.divide(10, 0)
```

#### Step 2: Run the Failing Tests

```bash
pytest test_agent.py::TestCalculatorTool -v
```

**Expected Output:**
```
FAILED test_agent.py::TestCalculatorTool::test_add_two_numbers
NameError: name 'Calculator' is not defined

FAILED test_agent.py::TestCalculatorTool::test_subtract_two_numbers
NameError: name 'Calculator' is not defined

... (all tests fail)
```

---

### Phase 2: GREEN - Write Minimal Code

#### Step 1: Implement Just Enough Code to Pass Tests

```python
# File: agent.py

class Calculator:
    """A simple calculator tool for arithmetic operations."""
    
    def add(self, a: float, b: float) -> float:
        """Add two numbers and return the result."""
        return a + b
    
    def subtract(self, a: float, b: float) -> float:
        """Subtract two numbers and return the result."""
        return a - b
    
    def multiply(self, a: float, b: float) -> float:
        """Multiply two numbers and return the result."""
        return a * b
    
    def divide(self, a: float, b: float) -> float:
        """Divide two numbers and return the result."""
        if b == 0:
            raise ValueError("Division by zero is not allowed")
        return a / b
```

#### Step 2: Run Tests Again

```bash
pytest test_agent.py::TestCalculatorTool -v
```

**Expected Output:**
```
PASSED test_agent.py::TestCalculatorTool::test_add_two_numbers
PASSED test_agent.py::TestCalculatorTool::test_subtract_two_numbers
PASSED test_agent.py::TestCalculatorTool::test_multiply_two_numbers
PASSED test_agent.py::TestCalculatorTool::test_divide_two_numbers
PASSED test_agent.py::TestCalculatorTool::test_divide_by_zero_raises_error

======================== 5 passed in 0.32s ========================
```

---

### Phase 3: REFACTOR - Improve Code

#### Step 1: Improve Code Quality While Keeping Tests Green

**Before:**
```python
def execute_tool(self, tool_name, tool_input):
    if tool_name == "Calculator":
        calc = Calculator()
        # ...hardcoded logic...
        return result
```

**After:**
```python
def get_tool_definition(self, tool_name: str) -> dict:
    """Get the definition and schema of a tool."""
    if tool_name == "Calculator":
        return {
            "name": "Calculator",
            "description": "Perform arithmetic operations",
            "input_schema": {
                "type": "object",
                "properties": {
                    "operation": {
                        "type": "string",
                        "enum": ["add", "subtract", "multiply", "divide"]
                    },
                    "a": {"type": "number"},
                    "b": {"type": "number"}
                },
                "required": ["operation", "a", "b"]
            }
        }

def execute_tool(self, tool_name: str, tool_input: dict) -> Any:
    """Execute a tool with given input."""
    tool_def = self.get_tool_definition(tool_name)
    if not tool_def:
        raise ValueError(f"Unknown tool: {tool_name}")
    
    operation = tool_input.get("operation")
    a = tool_input.get("a")
    b = tool_input.get("b")
    
    calculator = Calculator()
    if operation == "add":
        return calculator.add(a, b)
    # ...etc...
```

#### Step 2: Run All Tests to Ensure Refactoring Didn't Break Anything

```bash
pytest test_agent.py -v
```

**Expected Output:**
```
======================== 12 passed in 0.45s ========================
```

---

## TDD Best Practices

### 1. One Assertion Per Test

**✓ GOOD:**
```python
def test_add_two_numbers(self):
    calculator = Calculator()
    result = calculator.add(5, 3)
    assert result == 8
```

**✗ BAD:**
```python
def test_calculator(self):
    calculator = Calculator()
    assert calculator.add(5, 3) == 8
    assert calculator.subtract(10, 5) == 5
    assert calculator.multiply(3, 4) == 12
    assert calculator.divide(20, 4) == 5.0
```

**Why?** If one assertion fails, you don't know which one. Makes debugging harder.

---

### 2. Descriptive Test Names

**✓ GOOD:**
```python
def test_divide_by_zero_raises_value_error(self):
def test_add_positive_and_negative_numbers(self):
def test_agent_initialization_with_api_key(self):
```

**✗ BAD:**
```python
def test_divide(self):
def test_add(self):
def test_agent(self):
```

**Why?** Test names should explain what's being tested and what's expected. They serve as documentation.

---

### 3. Arrange-Act-Assert Pattern

Structure tests clearly with three parts:

```python
def test_add_two_numbers(self):
    # ARRANGE: Set up the test
    calculator = Calculator()
    
    # ACT: Execute the function
    result = calculator.add(5, 3)
    
    # ASSERT: Verify the result
    assert result == 8
```

---

### 4. Test Independence

Tests should not depend on each other or their execution order.

**✓ GOOD:**
```python
def test_add_positive_numbers(self):
    assert Calculator().add(5, 3) == 8

def test_add_negative_numbers(self):
    assert Calculator().add(-5, -3) == -8
```

**✗ BAD:**
```python
def test_calculator(self):
    global calculator
    calculator = Calculator()
    # Depends on global state

def test_add_uses_global(self):
    # Depends on test_calculator running first
    assert calculator.add(5, 3) == 8
```

---

### 5. Test Edge Cases

```python
# Normal cases
def test_add_positive_numbers(self):
    assert Calculator().add(5, 3) == 8

# Edge cases
def test_add_with_zero(self):
    assert Calculator().add(0, 5) == 5

def test_add_negative_numbers(self):
    assert Calculator().add(-5, -3) == -8

def test_add_large_numbers(self):
    assert Calculator().add(1e308, 1e308) is not None

# Error cases
def test_add_with_none_raises_error(self):
    with pytest.raises(TypeError):
        Calculator().add(None, 5)
```

---

### 6. Mocking and Unit Testing

Isolate the code being tested by mocking external dependencies.

```python
from unittest.mock import patch, MagicMock
import pytest

class TestSimpleAgent:
    """Tests for SimpleAgent with mocked API"""
    
    def test_agent_process_query_with_addition(self):
        """Test agent handles tool use correctly"""
        agent = SimpleAgent(api_key="test-key")
        
        # Mock the Claude API response
        mock_response = MagicMock()
        mock_response.content = [MagicMock(
            type="text",
            text="The result of adding 5 and 3 is 8."
        )]
        
        with patch.object(agent.client, 'messages.create', return_value=mock_response):
            result = agent.process_query("What is 5 + 3?")
            assert result is not None
            assert "8" in result
    
    def test_agent_handles_api_error(self):
        """Test agent handles API errors gracefully"""
        agent = SimpleAgent(api_key="test-key")
        
        with patch.object(agent.client, 'messages.create', 
                         side_effect=Exception("API Error")):
            with pytest.raises(Exception):
                agent.process_query("What is 5 + 3?")
```

**Why Mock?**
- Avoid calling real APIs during tests
- Tests run faster
- Tests are deterministic
- No API costs or external dependencies

---

## Running Tests

### Prerequisites

```bash
# Install dependencies
pip install -r requirements.txt

# Verify installation
pip list | grep -E "anthropic|pytest"
```

Expected output:
```
anthropic           0.25.0  (or later)
pytest              7.4.0   (or later)
pytest-asyncio      0.21.0  (or later)
```

---

### Basic Test Execution

#### Run All Tests

```bash
pytest test_agent.py -v
```

**Output:**
```
test_agent.py::TestCalculatorTool::test_add_two_numbers PASSED
test_agent.py::TestCalculatorTool::test_subtract_two_numbers PASSED
test_agent.py::TestCalculatorTool::test_multiply_two_numbers PASSED
test_agent.py::TestCalculatorTool::test_divide_two_numbers PASSED
test_agent.py::TestCalculatorTool::test_divide_by_zero_raises_error PASSED
test_agent.py::TestSimpleAgent::test_agent_initialization PASSED
test_agent.py::TestSimpleAgent::test_agent_has_calculator_tool PASSED
test_agent.py::TestSimpleAgent::test_agent_process_query_with_addition PASSED
test_agent.py::TestSimpleAgent::test_agent_handles_tool_use PASSED
test_agent.py::TestSimpleAgent::test_agent_get_tool_definition PASSED
test_agent.py::TestAgentIntegration::test_calculator_tool_workflow PASSED
test_agent.py::TestAgentIntegration::test_agent_initialization_with_tools PASSED

======================== 12 passed in 2.45s ========================
```

#### Run Specific Test Class

```bash
# Test only Calculator
pytest test_agent.py::TestCalculatorTool -v

# Test only Agent
pytest test_agent.py::TestSimpleAgent -v

# Test only Integration tests
pytest test_agent.py::TestAgentIntegration -v
```

#### Run Specific Test

```bash
pytest test_agent.py::TestCalculatorTool::test_add_two_numbers -v
```

---

### Advanced Test Options

#### Run with Coverage Report

```bash
# Generate coverage report
pytest test_agent.py --cov=agent --cov-report=term-missing

# Generate HTML coverage report
pytest test_agent.py --cov=agent --cov-report=html

# View HTML report in browser
open htmlcov/index.html
```

**Output example:**
```
Name      Stmts   Miss  Cover   Missing
---------------------------------------
agent.py     85      4    95%    42, 65, 78, 92
---------------------------------------
TOTAL        85      4    95%
```

#### Run Tests Matching a Pattern

```bash
# All tests with "calculator" in name
pytest test_agent.py -k calculator -v

# All tests with "divide" in name
pytest test_agent.py -k divide -v

# All tests NOT matching "integration"
pytest test_agent.py -k "not integration" -v
```

#### Stop on First Failure

```bash
# Useful for debugging
pytest test_agent.py -x

# With verbose output
pytest test_agent.py -x -v
```

#### Show Print Statements

```bash
# Capture output from test
pytest test_agent.py -v -s

# Show local variables on failure
pytest test_agent.py -v -l
```

#### Generate Detailed Report

```bash
# Full traceback on failures
pytest test_agent.py -v --tb=long

# Short traceback
pytest test_agent.py -v --tb=short

# No traceback
pytest test_agent.py -v --tb=no

# Line-by-line
pytest test_agent.py -v --tb=line
```

#### Run Tests Multiple Times

```bash
# Install pytest-repeat
pip install pytest-repeat

# Repeat 5 times
pytest test_agent.py --count=5

# Randomize test order (requires pytest-randomly)
pip install pytest-randomly
pytest test_agent.py -p no:randomly
```

#### Parallel Testing

```bash
# Install pytest-xdist
pip install pytest-xdist

# Run 4 tests in parallel
pytest test_agent.py -n 4
```

#### Watch Mode

```bash
# Install pytest-watch
pip install pytest-watch

# Run tests in watch mode - re-runs on file changes
ptw test_agent.py

# With coverage
ptw test_agent.py -- --cov=agent
```

---

### Common Test Commands

#### Quick Test (for CI/CD)

```bash
pytest test_agent.py -q --tb=short
```

#### Full Test (for development)

```bash
pytest test_agent.py -v --tb=long --cov=agent
```

#### Safe Test (for production)

```bash
pytest test_agent.py -v --tb=line -s --strict-markers
```

#### TDD Test (for rapid development)

```bash
ptw test_agent.py -- --cov=agent
```

---

## Development Workflow

### Example: Adding a New Feature (Power Operation)

#### Step 1: Write Failing Test (RED)

```python
# In test_agent.py
class TestCalculatorTool:
    # ... existing tests ...
    
    def test_power_operation(self):
        """Test exponentiation: 2^3 = 8"""
        calculator = Calculator()
        result = calculator.power(2, 3)
        assert result == 8
```

Run the test:
```bash
pytest test_agent.py::TestCalculatorTool::test_power_operation -v
```

**Result: FAILED ✗**
```
FAILED - AttributeError: 'Calculator' object has no attribute 'power'
```

---

#### Step 2: Implement Minimal Code (GREEN)

```python
# In agent.py, add to Calculator class:
def power(self, a: float, b: float) -> float:
    """Calculate a raised to the power of b."""
    return a ** b
```

Run the test again:
```bash
pytest test_agent.py::TestCalculatorTool::test_power_operation -v
```

**Result: PASSED ✓**

---

#### Step 3: Verify All Tests Still Pass (REFACTOR)

```bash
pytest test_agent.py -v
```

**Result: All 13 tests passed**

Now refactor if needed:

```python
# Update get_tool_definition to include power operation
def get_tool_definition(self, tool_name: str) -> dict:
    if tool_name == "Calculator":
        return {
            "name": "Calculator",
            "description": "Perform arithmetic operations",
            "input_schema": {
                "type": "object",
                "properties": {
                    "operation": {
                        "enum": ["add", "subtract", "multiply", "divide", "power"]
                    },
                    # ... rest of schema ...
                }
            }
        }

# Update execute_tool to handle power
def execute_tool(self, tool_name: str, tool_input: dict) -> Any:
    operation = tool_input.get("operation")
    a = tool_input.get("a")
    b = tool_input.get("b")
    
    calculator = Calculator()
    if operation == "add":
        return calculator.add(a, b)
    elif operation == "subtract":
        return calculator.subtract(a, b)
    elif operation == "multiply":
        return calculator.multiply(a, b)
    elif operation == "divide":
        return calculator.divide(a, b)
    elif operation == "power":
        return calculator.power(a, b)
    else:
        raise ValueError(f"Unknown operation: {operation}")
```

Run all tests again:
```bash
pytest test_agent.py -v
```

**Result: All tests passed, including new feature**

---

## Troubleshooting & Debugging

### Tests Won't Run

```bash
# Check if pytest is installed
pip list | grep pytest

# Reinstall if needed
pip install -r requirements.txt

# Check Python path
python -c "import agent; print('OK')"

# Run from project directory if import errors occur
cd project-directory
```

### Import Errors

```bash
# Verify module exists
python -c "import agent; print('OK')"

# Check PYTHONPATH
export PYTHONPATH="${PYTHONPATH}:$(pwd)"

# Run from project root directory
```

### Async Test Errors

```bash
# Ensure pytest-asyncio is installed
pip install pytest-asyncio

# Verify pytest.ini has proper configuration
cat pytest.ini
# Should contain: asyncio_mode = auto
```

### Mocking Errors

```bash
# Ensure unittest.mock is available (builtin in Python 3.6+)
python -c "from unittest.mock import patch; print('OK')"

# For async mocking, ensure AsyncMock is available (Python 3.8+)
python -c "from unittest.mock import AsyncMock; print('OK')"
```

### Add Print Statements for Debugging

```python
def test_add_two_numbers(self):
    calculator = Calculator()
    print("Calculator created")  # Use with -s flag
    result = calculator.add(5, 3)
    print(f"Result: {result}")
    assert result == 8
```

Run with:
```bash
pytest test_agent.py::TestCalculatorTool::test_add_two_numbers -v -s
```

### Use Python Debugger

```python
def test_add_two_numbers(self):
    import pdb
    calculator = Calculator()
    pdb.set_trace()  # Drops into debugger here
    result = calculator.add(5, 3)
    assert result == 8
```

Run with:
```bash
pytest test_agent.py::TestCalculatorTool::test_add_two_numbers -v -s
# Interactive debugger will start
```

### Inspect Failure Details

```bash
# Full traceback with context
pytest test_agent.py -v --tb=long

# Show local variables
pytest test_agent.py -v -l

# Show all assertion details
pytest test_agent.py -v --assert=plain
```

---

### Measure Test Performance

```bash
# Show slowest tests
pytest test_agent.py -v --durations=5

# Example output:
# test_agent_process_query PASSED [98%] 0.45s
# test_agent_handles_tool_use PASSED [99%] 0.23s
```

### Count Tests

```bash
# List all tests
pytest test_agent.py --collect-only -q

# Count total
pytest test_agent.py --collect-only -q | wc -l
```

---

## Integration with Git

### Add Test Running to Git Hooks

Create `.git/hooks/pre-commit`:

```bash
#!/bin/bash
# Run tests before committing

pytest test_agent.py -q
if [ $? -ne 0 ]; then
  echo "❌ Tests failed. Commit aborted."
  exit 1
fi
echo "✅ All tests passed. Commit accepted."
```

Make it executable:
```bash
chmod +x .git/hooks/pre-commit
```

---

## Continuous Integration Example

### GitHub Actions

Create `.github/workflows/test.yml`:

```yaml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.8, 3.9, "3.10"]
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: ${{ matrix.python-version }}
      
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
      
      - name: Run tests
        run: pytest test_agent.py -v --cov=agent --cov-report=xml
      
      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          file: ./coverage.xml
```

---

## Summary

### Quick Reference: Test Commands

| Command | Purpose |
|---------|---------|
| `pytest test_agent.py -v` | Run all tests with verbose output |
| `pytest test_agent.py::TestClass::test_method -v` | Run specific test |
| `pytest test_agent.py -k pattern -v` | Run tests matching pattern |
| `pytest test_agent.py --cov=agent` | Run with coverage report |
| `pytest test_agent.py -x` | Stop on first failure |
| `pytest test_agent.py -s` | Show print statements |
| `pytest test_agent.py --tb=long` | Full traceback on error |
| `ptw test_agent.py` | Run in watch mode (auto-rerun) |

### Test Metrics

- **Total Tests**: 12+ (project dependent)
- **Unit Tests**: 10+
- **Integration Tests**: 2+
- **Expected Time**: 2-3 seconds
- **Coverage Target**: 95%+

### When to Run Tests

✅ Before committing code
✅ Before pushing to main/master
✅ After any code changes
✅ In CI/CD pipeline
✅ During code review
✅ Before releasing to production

### TDD Workflow Tips

1. **Write tests first** - Define behavior before implementation
2. **Run tests frequently** - After each change, at minimum
3. **Keep tests independent** - No shared state or dependencies
4. **Use descriptive names** - Tests serve as documentation
5. **Test edge cases** - Boundaries, errors, unusual inputs
6. **Mock external calls** - Keep tests fast and deterministic
7. **Refactor with confidence** - Tests catch regressions
8. **Aim for coverage** - 95%+ is a good target

---

## Additional Resources

### Pytest Documentation
- https://docs.pytest.org/

### Testing Best Practices
- [Google Testing Blog](https://testing.googleblog.com/)
- [Real Python Testing](https://realpython.com/pytest-python-testing/)

### TDD Principles
- [Test-Driven Development: By Example - Kent Beck](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530)
- [Growing Object-Oriented Software, Guided by Tests](https://www.amazon.com/Growing-Object-Oriented-Software-Guided-Tests/dp/0321503627)

---

## Next Steps

1. **Set up your test environment** with the prerequisites above
2. **Write tests for your features** using the general outline as a guide
3. **Follow the TDD cycle**: RED → GREEN → REFACTOR
4. **Run tests regularly** during development
5. **Maintain high coverage** (target 95%+)
6. **Document test purposes** with clear test names and docstrings
7. **Integrate with CI/CD** to run tests automatically on push

Happy Testing! 🚀

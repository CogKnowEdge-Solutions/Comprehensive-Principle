# Test File Format

This file defines the one required format for any test cases a lesson or its
supporting code ships with. It does not prescribe what to test; the lesson's
validation workflow (the five gates) lives in `AGENTS.md`.

---

## Mandatory Test File Format

**MUST — Test cases are written as pytest `.py` files.** All test cases MUST be authored in a standalone Python test file (e.g., `test_agent.py`) using the pytest framework, and MUST be run with the `pytest` command.

**MUST NOT — Test cases are never written or executed in notebooks.** Test cases MUST NOT be authored, stored, or executed inside Jupyter notebooks (`.ipynb`) — not as notebook cells, not as an `.ipynb`-based test runner, and not as a `!pytest` invocation in a notebook cell. A notebook may *demonstrate* interactive testing for teaching, but the authoritative test cases for the project always live in `.py` files.

**Why:** Notebook tests silently accumulate state, skip or reorder cells, and cannot run headlessly in continuous integration (CI). `.py` pytest files are deterministic, independent, repeatable, and CI-friendly — the mandatory format for every test case.

### Shape of a test file

- **File name:** `test_<module>.py` (pytest discovers files starting with `test_`).
- **Test functions:** named `test_<what_it_checks>`, optionally grouped in a `Test<Thing>` class.
- **Assertions:** plain `assert` statements.

```python
# test_calculator.py
from calculator import add


def test_add_two_positive_numbers():
    assert add(2, 3) == 5


def test_add_negative_number():
    assert add(2, -3) == -1
```

Run it with:

```bash
pytest test_calculator.py -v
```

Test results are read from the pytest output only — they are not saved to any
separate file or format.

### If you find tests inside a notebook

Move them into a `test_<module>.py` file, import the code under test from a
`.py` module (not from the notebook), delete the test cells from the notebook,
and run them with `pytest`.

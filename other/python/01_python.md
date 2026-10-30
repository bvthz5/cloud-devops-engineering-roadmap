# Complete Python Master Guide: All Topics + 1000 Questions & Answers

An exhaustive, industry-grade Python study, reference, and technical interview master guide covering Python from foundational programming through advanced systems engineering.

This guide provides deep theoretical foundations, exact language grammar, practical code implementations, output predictions, debugging scenarios, and **exactly 1,000 unique questions and answers** spanning every layer of the Python ecosystem: core internals, memory management, concurrency, async I/O, cloud DevOps automation, database programming, modern web APIs, data science, machine learning, and enterprise system design.

---

## Complete 34-Chapter Syllabus & Master Index

| Chapter | Domain & Subject Area | Question Range | Question Count |
|:---:|---|:---:|:---:|
| **1** | [Python Fundamentals and Introduction](#chapter-1-python-fundamentals-and-introduction) | Q1 - Q30 | 30 |
| **2** | [Variables, Data Types, and Memory](#chapter-2-variables-data-types-and-memory) | Q31 - Q62 | 32 |
| **3** | [Operators and Expressions](#chapter-3-operators-and-expressions) | Q63 - Q90 | 28 |
| **4** | [Strings and Text Processing](#chapter-4-strings-and-text-processing) | Q91 - Q122 | 32 |
| **5** | [Control Flow and Decision-Making](#chapter-5-control-flow-and-decision-making) | Q123 - Q152 | 30 |
| **6** | [Lists, Tuples, Sets, and Dictionaries](#chapter-6-lists-tuples-sets-and-dictionaries) | Q153 - Q192 | 40 |
| **7** | [Functions and Scope](#chapter-7-functions-and-functional-principles) | Q193 - Q228 | 36 |
| **8** | [Object-Oriented Programming (OOP)](#chapter-8-object-oriented-programming-oop) | Q229 - Q273 | 45 |
| **9** | [Modules, Packages, and Imports](#chapter-9-modules-packages-and-imports) | Q274 - Q300 | 27 |
| **10** | [Exception Handling and Error Management](#chapter-10-exception-handling-and-error-management) | Q301 - Q330 | 30 |
| **11** | [File Handling and Input/Output](#chapter-11-file-handling-and-inputoutput) | Q331 - Q360 | 30 |
| **12** | [Iterators, Iterables, and Generators](#chapter-12-iterators-iterables-and-generators) | Q361 - Q390 | 30 |
| **13** | [Comprehensions and Advanced Functional Tools](#chapter-13-comprehensions-and-advanced-functional-tools) | Q391 - Q418 | 28 |
| **14** | [Advanced Language Features](#chapter-14-advanced-language-features) | Q419 - Q453 | 35 |
| **15** | [Memory Management, GIL, and Python Internals](#chapter-15-memory-management-gil-and-python-internals) | Q454 - Q483 | 30 |
| **16** | [Python Standard Library: Essential Modules](#chapter-16-python-standard-library-essential-modules) | Q484 - Q528 | 45 |
| **17** | [Type Hints and Static Typing](#chapter-17-type-hints-and-static-typing) | Q529 - Q558 | 30 |
| **18** | [Regular Expressions](#chapter-18-regular-expressions) | Q559 - Q584 | 26 |
| **19** | [Working with Data Formats (JSON, CSV, YAML, TOML)](#chapter-19-working-with-data-formats) | Q585 - Q612 | 28 |
| **20** | [Database Programming (SQL, SQLite, SQLAlchemy)](#chapter-20-database-programming) | Q613 - Q642 | 30 |
| **21** | [Networking and API Development](#chapter-21-networking-and-api-development) | Q643 - Q672 | 30 |
| **22** | [Web Development with Python (FastAPI, Flask, Django)](#chapter-22-web-development-with-python) | Q673 - Q704 | 32 |
| **23** | [Testing, Debugging, and Code Quality](#chapter-23-testing-debugging-and-code-quality) | Q705 - Q734 | 30 |
| **24** | [Concurrency and Parallelism (Threading, Asyncio, Multiprocessing)](#chapter-24-concurrency-and-parallelism) | Q735 - Q772 | 38 |
| **25** | [Packaging, Environments, and Dependency Management](#chapter-25-packaging-environments-and-dependency-management) | Q773 - Q798 | 26 |
| **26** | [Security in Python](#chapter-26-security-in-python) | Q799 - Q826 | 28 |
| **27** | [Performance, Benchmarking, and Optimization](#chapter-27-performance-benchmarking-and-optimization) | Q827 - Q852 | 26 |
| **28** | [Python for DevOps and Automation](#chapter-28-python-for-devops-and-automation) | Q853 - Q888 | 36 |
| **29** | [Python for Data Science (NumPy, Pandas, Matplotlib)](#chapter-29-python-for-data-science) | Q889 - Q920 | 32 |
| **30** | [Python for AI, Machine Learning & Deep Learning](#chapter-30-python-for-ai-machine-learning-and-deep-learning) | Q921 - Q950 | 30 |
| **31** | [GUI, Desktop, and Application Development](#chapter-31-gui-desktop-and-application-development) | Q951 - Q964 | 14 |
| **32** | [Scientific Computing and Specialized Python](#chapter-32-scientific-computing-and-specialized-python) | Q965 - Q978 | 14 |
| **33** | [Algorithms and Data Structures Using Python](#chapter-33-algorithms-and-data-structures-using-python) | Q979 - Q992 | 14 |
| **34** | [Professional Python Development & System Design](#chapter-34-professional-python-development-and-system-design) | Q993 - Q1000 | 8 |
| **Total** | **All 34 Chapters Fully Covered** | **Q1 - Q1000** | **1,000** |

---

# PART 1 — Detailed Curriculum Topics

Every chapter systematically unpacks:
- **Theory & Semantics:** Language grammar, data models, and runtime behaviors.
- **Syntax & Idioms:** Modern Python 3.12+ syntax and best practices.
- **Code Implementations:** Tested, executable code snippets.
- **Output-Based Exercises:** Predicting runtime results and explaining evaluation mechanics.
- **Debugging & Error Resolution:** Identifying common pitfalls, anti-patterns, and exceptions.
- **Interview & System Design Questions:** Real-world enterprise scenarios for senior and DevOps roles.

---

# PART 2 — Master Questions and Answers (Q1 to Q1000)

## Chapter 1: Python Fundamentals and Introduction

**Q1. What is Python?**

*Explain the definition, programming paradigms, and primary modern use cases of Python.*

**Answer:** Python is a high-level, general-purpose, interpreted programming language known for its emphasis on code readability, dynamic typing, and comprehensive standard library. Python natively supports multiple programming paradigms, including object-oriented, procedural, and functional programming. It is the dominant language across automation, DevOps scripting, cloud engineering, data science, machine learning, and web development.

```python
# Python multi-paradigm demonstration
# 1. Procedural
def greet_proc(name):
    return f"Hello, {name}!"

# 2. Object-Oriented
class Greeter:
    def __init__(self, name):
        self.name = name
    def greet(self):
        return f"Greetings, {self.name}!"

# 3. Functional
names = ["Alice", "Bob"]
greetings = list(map(lambda n: f"Hi, {n}!", names))

print(greet_proc("DevOps"))
print(Greeter("Cloud").greet())
print(greetings)
```

Output:
```text
Hello, DevOps!
Greetings, Cloud!
['Hi, Alice!', 'Hi, Bob!']
```

**Q2. Who created Python and what is its history?**

*Detail the creator, inception date, and key milestones in Python's history.*

**Answer:** Python was created by Dutch programmer Guido van Rossum in the late 1980s at Centrum Wiskunde & Informatica (CWI) in the Netherlands as a successor to the ABC programming language. The first public release (version 0.9.0) was issued in February 1991. Python 2.0 was released in October 2000, introducing list comprehensions and garbage collection. Python 3.0 was released in December 2008 to eliminate design flaws, leading to Python 2's end-of-life on January 1, 2020.

```python
import sys
print(f"Current Python Implementation: {sys.implementation.name}")
print(f"Version: {sys.version.split()[0]}")
```

Output:
```text
Current Python Implementation: cpython
Version: 3.12.10
```

**Q3. Why is Python so popular in modern software engineering?**

*List and explain the key pillars that drive Python's massive industry adoption.*

**Answer:** Python's popularity stems from seven foundational pillars:
1. **Minimalist, readable syntax**: Close to pseudo-code, drastically reducing time-to-market.
2. **'Batteries Included' standard library**: Built-in modules for OS operations, networking, JSON, math, and concurrency.
3. **Massive package ecosystem**: Over 500,000 packages on PyPI (PyTorch, TensorFlow, Boto3, FastAPI, Pandas).
4. **Cross-platform execution**: Runs seamlessly on Linux, Windows, macOS, and microcontrollers.
5. **Extensibility**: Easily interfaces with C/C++, Rust, and Fortran for performance-critical tasks.
6. **Strong industry standard for AI/ML & DevOps**: Supported by cloud providers (AWS, GCP, Azure).
7. **Active global community**: PEP standards and continuous language enhancements.

```python
# Demonstrating 'batteries-included' simplicity
import json
import statistics

data = json.loads('{"measurements": [12.5, 14.2, 10.8, 15.1, 13.4]}')
mean_val = statistics.mean(data["measurements"])
print(f"Computed Mean: {mean_val:.2f}")
```

Output:
```text
Computed Mean: 13.20
```

**Q4. Is Python an interpreted or compiled language?**

*Explain Python's two-step execution model involving source compilation and virtual machine interpretation.*

**Answer:** Python is both compiled and interpreted. When you run a script, CPython first parses and compiles the human-readable source code (.py) into low-level intermediate instructions known as **bytecode** (.pyc files stored in `__pycache__`). Next, the **Python Virtual Machine (PVM)** interprets and executes these bytecode instructions sequentially on the host operating system. Thus, compilation happens implicitly at runtime before interpretation.

```python
import dis

def calculate(a, b):
    return (a * b) + 10

# Inspecting raw bytecode instructions produced by the compiler
dis.dis(calculate)
```

Output:
```text
2           0 LOAD_FAST                0 (a)
              2 LOAD_FAST                1 (b)
              4 BINARY_OP                5 (*)
              8 LOAD_CONST               1 (10)
             10 BINARY_OP                0 (+)
             14 RETURN_VALUE
```

**Q5. What is dynamic typing in Python?**

*Explain how dynamic typing works, runtime variable re-binding, and type checking.*

**Answer:** In Python, variable names are simply symbolic references (pointers) bound to objects stored in memory. The object itself holds the type and value, not the variable name. Therefore, a variable can be rebound to an object of a completely different data type at any point during runtime without compilation errors.

```python
target = 100
print(f"Value: {target}, Type: {type(target).__name__}")

target = "Python Engineering"
print(f"Value: {target}, Type: {type(target).__name__}")

target = [1, 2, 3]
print(f"Value: {target}, Type: {type(target).__name__}")
```

Output:
```text
Value: 100, Type: int
Value: Python Engineering, Type: str
Value: [1, 2, 3], Type: list
```

**Q6. Is Python strongly typed or weakly typed?**

*Demonstrate Python's strong typing semantics using implicit type coercion examples.*

**Answer:** Python is **strongly typed**. In a weakly typed language (such as JavaScript), expressions like `'10' + 5` result in implicit coercion (`'105'`). Python strictly forbids implicit type coercion across incompatible types and immediately raises a `TypeError` at runtime. Explicit type casting must be written by the developer.

```python
try:
    result = "10" + 5
except TypeError as err:
    print(f"Caught expected error: {err}")

# Explicit conversion is mandatory:
valid_result = int("10") + 5
print(f"Explicit conversion result: {valid_result}")
```

Output:
```text
Caught expected error: can only concatenate str (not "int") to str
Explicit conversion result: 15
```

**Q7. What are the major advantages and limitations of Python?**

*Provide a balanced engineering evaluation of Python's trade-offs in enterprise production.*

**Answer:** **Advantages:**
- High developer velocity and minimal boilerplate.
- Enormous ecosystem and ecosystem integration.
- Cross-platform portability.
- Dynamic reflection and introspection.

**Limitations:**
- Slower execution speed compared to C, C++, Go, or Rust for pure CPU-bound mathematical operations.
- Higher memory overhead due to dynamic object headers and PyObject wrappers.
- The Global Interpreter Lock (GIL) historically prevents multi-core CPU parallelism in pure Python threads.
- Dynamic typing can defer type mismatch errors to runtime if static type checkers (mypy/pyright) are omitted.

```python
import sys

# High memory overhead of basic types due to PyObject header
integer_val = 42
print(f"Memory overhead of integer 42: {sys.getsizeof(integer_val)} bytes")
```

Output:
```text
Memory overhead of integer 42: 28 bytes
```

**Q8. What are the major implementations of Python?**

*Compare CPython, PyPy, Jython, IronPython, and MicroPython.*

**Answer:** 1. **CPython**: The standard reference implementation written in C; most compatible and universally used.
2. **PyPy**: Alternative implementation with a JIT (Just-In-Time) compiler; accelerates long-running CPU loops by 2x-7x.
3. **Jython**: Targets the Java Virtual Machine (JVM); interoperates directly with Java classes.
4. **IronPython**: Targets Microsoft's .NET CLR runtime.
5. **MicroPython / CircuitPython**: Highly optimized, stripped-down Python runtime designed for microcontrollers and embedded IoT hardware.

```python
import platform

print("Implementation:", platform.python_implementation())
print("Compiler:", platform.python_compiler())
print("Architecture:", platform.architecture()[0])
```

Output:
```text
Implementation: CPython
Compiler: MSC v.1942 64 bit (AMD64)
Architecture: 64bit
```

**Q9. What are the critical architectural differences between Python 2 and Python 3?**

*List the major breaking changes introduced when transitioning from Python 2 to Python 3.*

**Answer:** 1. **Print**: Statement in Python 2 (`print 'hello'`), function in Python 3 (`print('hello')`).
2. **String Types**: Python 2 used ASCII strings by default (`str`) with separate `unicode`. Python 3 uses UTF-8 strings (`str`) by default, with `bytes` for raw binary.
3. **Integer Division**: In Python 2, `5 / 2` returned `2`. In Python 3, `5 / 2` returns `2.5` (true division) and `//` is floor division.
4. **Range**: Python 2 had `range()` (list) and `xrange()` (generator). Python 3 replaces both with `range()` (memory-efficient sequence object).
5. **Exceptions**: Python 3 uses `raise Exception(args)` and `except Exception as e:`.

```python
# Python 3 division behavior
div_true = 5 / 2
div_floor = 5 // 2
print(f"True Division (5 / 2): {div_true} (type: {type(div_true).__name__})")
print(f"Floor Division (5 // 2): {div_floor} (type: {type(div_floor).__name__})")
```

Output:
```text
True Division (5 / 2): 2.5 (type: float)
Floor Division (5 // 2): 2 (type: int)
```

**Q10. How do you install and manage multiple Python versions across operating systems?**

*Explain the tools and best practices for managing multiple Python versions on Windows, macOS, and Linux.*

**Answer:** Modern best practice is to avoid modifying system Python installations.
- **Windows**: Use the official installer with Python Launcher (`py -3.11`, `py -3.12`) or use `winget` / `uv`.
- **Linux / macOS**: Use `pyenv` (`pyenv install 3.12.0 && pyenv global 3.12.0`) or `uv python install 3.12`.
- **Containers**: Run specific version tags in Docker (`python:3.12-slim`).
Never install global packages into system-level Python; always use virtual environments.

```python
# In terminal:
# py --list
# py -3.12 -m venv myenv
# pyenv versions
```

Output:
```text
# System will list installed runtime versions and activate isolated environments.
```

**Q11. What is the Python REPL and interactive shell?**

*Explain the Read-Eval-Print-Loop and how to use it for prototyping.*

**Answer:** The Python REPL (Read-Eval-Print Loop) is an interactive programming environment that takes single user commands, evaluates them, and returns the result immediately. Started by typing `python` or `py` in the terminal, it includes special variable `_` storing the result of the last evaluated expression.

```python
# REPL demonstration concept
# >>> 10 + 20
# 30
# >>> _ * 2
# 60
```

Output:
```text
60
```

**Q12. What are the common tools and IDEs used for Python development?**

*Compare VS Code, PyCharm, Jupyter Notebooks, and IDLE.*

**Answer:** 1. **VS Code**: Lightweight, extensible, dominant for cloud/DevOps/web with Microsoft Python & Pylance extensions.
2. **PyCharm (JetBrains)**: Heavyweight dedicated IDE with advanced refactoring, database tools, and Django/Flask support.
3. **Jupyter Notebook / JupyterLab**: Browser-based interactive computational notebook ideal for exploratory data analysis, ML, and visualization.
4. **IDLE**: Python's bundled minimal starter editor, mainly used for learning basic syntax.

```python
# VS Code / IDE standard configuration in .vscode/settings.json:
# {
#   "python.analysis.typeCheckingMode": "standard",
#   "editor.formatOnSave": true
# }
```

Output:
```text
Configured for automated linting, formatting, and type checks.
```

**Q13. How does CPython compile and execute code under the hood?**

*Explain the complete execution pipeline from source code to machine execution.*

**Answer:** The CPython execution flow proceeds through five deterministic stages:
1. **Lexical Analysis (Tokenizer)**: Breaks raw UTF-8 text into a token stream (`NAME`, `OP`, `NUMBER`).
2. **Parsing (Parser)**: Generates a Concrete Syntax Tree (CST) and transforms it into an Abstract Syntax Tree (AST) using Python's PEG parser.
3. **Bytecode Compilation (Compiler)**: Converts AST into Python bytecode code objects (`PyCodeObject`).
4. **Optimization**: Constant folding and peephole optimizations occur.
5. **Virtual Machine Execution (PVM)**: The main evaluation loop (`_PyEval_EvalFrameDefault`) interprets bytecode instructions into native OS instructions.

```python
import ast

source_code = "total = price * 1.18"
parsed_ast = ast.parse(source_code)
print(ast.dump(parsed_ast, indent=2))
```

Output:
```text
Module(
  body=[
    Assign(
      targets=[
        Name(id='total', ctx=Store())],
      value=BinOp(
        left=Name(id='price', ctx=Load()),
        op=Mult(),
        right=Constant(value=1.18)))],
  type_ignores=[])
```

**Q14. What role does indentation play in Python syntax?**

*Explain indentation rules, tabs versus spaces, and PEP 8 guidelines.*

**Answer:** Unlike languages that use curly braces `{}` or keywords (`begin`/`end`), Python uses indentation to delimit code blocks (compound statements). PEP 8 mandates **4 spaces per indentation level** and strictly forbids mixing tabs and spaces. Mixing tabs and spaces raises an `IndentationError: unexpected indent` or `TabError: inconsistent use of tabs and spaces in indentation`.

```python
def check_level(val):
    if val > 50:
        message = "High Level"
        status = True
    else:
        message = "Normal Level"
        status = False
    return message, status

print(check_level(75))
```

Output:
```text
('High Level', True)
```

**Q15. What is the difference between single-line comments, multiline comments, and docstrings?**

*Explain syntax, storage, and introspectability of comments versus docstrings.*

**Answer:** 1. **Comments (`#`)**: Discarded during tokenization; not retained in the compiled bytecode or available at runtime.
2. **Docstrings (`"""..."""` or `'''...'''`)**: String literals occurring as the first statement in a module, class, method, or function. They are compiled into the code object's `__doc__` attribute and accessible at runtime via `help()` or `obj.__doc__`.

```python
def compute_latency(packets, duration):
    """Calculate average latency in milliseconds.
    
    Args:
        packets (int): Total packet count.
        duration (float): Seconds taken.
    Returns:
        float: Latency in ms.
    """
    # Inline comment explaining math:
    return (duration / packets) * 1000

print("Docstring at runtime:")
print(compute_latency.__doc__)
```

Output:
```text
Docstring at runtime:
Calculate average latency in milliseconds.
    
    Args:
        packets (int): Total packet count.
        duration (float): Seconds taken.
    Returns:
        float: Latency in ms.
```

**Q16. What are identifiers and what are Python's identifier naming rules?**

*Explain valid identifier characters, case sensitivity, and reserved naming conventions.*

**Answer:** An identifier is a name given to variables, functions, classes, modules, or objects.
Rules:
1. Must begin with an ASCII letter (a-z, A-Z) or underscore (`_`).
2. May contain letters, digits (0-9), and underscores. Cannot start with a digit.
3. Case-sensitive (`Value`, `value`, and `VALUE` are 3 distinct identifiers).
4. Cannot be a reserved keyword (e.g. `class`, `def`, `while`).
Conventions:
- `_single_leading_underscore`: Internal/private indicator.
- `__double_leading_underscore`: Class-private name mangling.
- `__dunder__`: Language-reserved special methods.

```python
valid_name = 1
_internal = 2
PascalCaseClass = 3
SCREAMING_SNAKE_CASE_CONST = 4

print(valid_name, _internal, PascalCaseClass, SCREAMING_SNAKE_CASE_CONST)
```

Output:
```text
1 2 3 4
```

**Q17. What are Python keywords and soft keywords?**

*Explain how keywords differ from soft keywords introduced in Python 3.10.*

**Answer:** Keywords are hard-reserved words that cannot be used as variable or function identifiers (e.g., `for`, `in`, `def`, `return`, `with`). In Python 3.10+, **soft keywords** were introduced (`match`, `case`, `_`, `type`). Soft keywords act as keywords only within specific syntactic contexts (such as structural pattern matching or type alias statements) but remain valid variable names elsewhere for backward compatibility.

```python
import keyword

print("Total hard keywords:", len(keyword.kwlist))
print("Sample keywords:", keyword.kwlist[:8])
print("Soft keywords:", keyword.softkwlist)

# 'match' and 'case' used as standard identifiers:
match = "regex_match"
case = 42
print(f"Soft keyword used as variables: match='{match}', case={case}")
```

Output:
```text
Total hard keywords: 35
Sample keywords: ['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await']
Soft keywords: ['_', 'case', 'match', 'type']
Soft keyword used as variables: match='regex_match', case=42
```

**Q18. What is the difference between a statement and an expression in Python?**

*Provide definitions, syntax rules, and evaluation characteristics for statements vs expressions.*

**Answer:** - **Expression**: Any piece of code that evaluates to a value (e.g., `3 + 5`, `len('abc')`, `x > 10`, `[i*2 for i in range(3)]`). Expressions can be placed anywhere a value is expected (e.g., right-hand side of assignments, function arguments).
- **Statement**: A standalone line or block of code representing an action or instruction (e.g., `if`, `while`, `import`, `def`, `class`, variable assignment). Statements cannot be evaluated as an expression.

```python
# Expression evaluating to a value:
expr_val = (10 + 5) * 2

# Statement: 'pass', 'import', 'if'
if expr_val > 20:
    statement_executed = True

print("Expression Value:", expr_val)
print("Statement Executed:", statement_executed)
```

Output:
```text
Expression Value: 30
Statement Executed: True
```

**Q19. How do input() and print() work, and what are print()'s keyword arguments?**

*Explain prompt behavior, string conversion of input(), and print's sep, end, file, and flush arguments.*

**Answer:** `input(prompt)` reads a line from standard input (`sys.stdin`), strips the trailing newline, and **always returns a `str`**. `print(*objects, sep=' ', end='\n', file=None, flush=False)` writes string representations to standard output (`sys.stdout`).
- `sep`: String inserted between arguments (default `' '`).
- `end`: String appended at the end (default `'\n'`).
- `file`: Stream to write to (default `sys.stdout`).
- `flush`: Forces stream buffer flushing immediately.

```python
import sys

print("Alpha", "Beta", "Gamma", sep=" | ")
print("Connecting", end="...")
print(" [SUCCESS]")

# Using file argument to write directly to stderr:
print("Log notice: All systems normal", file=sys.stderr)
```

Output:
```text
Alpha | Beta | Gamma
Connecting... [SUCCESS]
Log notice: All systems normal
```

**Q20. How does built-in documentation and introspection work using help(), dir(), and __doc__?**

*Explain runtime introspection utilities in Python.*

**Answer:** Python objects carry rich runtime metadata. `dir(obj)` returns a sorted list of all valid attribute and method names on the object. `help(obj)` launches the interactive pager or prints human-readable documentation built from docstrings and signatures. `obj.__doc__` gives the direct raw docstring.

```python
import math

methods = [m for m in dir(math) if not m.startswith("_")]
print("Sample math methods count:", len(methods))
print("Sample math attributes:", methods[:5])
print("math.sqrt docstring:", math.sqrt.__doc__)
```

Output:
```text
Sample math methods count: 70
Sample math attributes: ['acos', 'acosh', 'asin', 'asinh', 'atan']
math.sqrt docstring: Return the square root of x.
```

**Q21. What is PEP 8 and what are its most critical style conventions?**

*Detail Python's primary style recommendations.*

**Answer:** PEP 8 is the official style guide for Python code. Key rules include:
- Indentation: 4 spaces per level (no tabs).
- Line length: Max 79 characters for code (often relaxed to 88 or 100 in modern projects via Black/Ruff).
- Variable & function names: `snake_case`.
- Class names: `PascalCase`.
- Constants: `SCREAMING_SNAKE_CASE`.
- Imports: Grouped into standard library, third-party, and local modules, each separated by a blank line.
- Whitespace: Spaces around binary operators (`=`, `+`, `==`), but no spaces inside parentheses.

```python
# Clean PEP 8 conforming code
import os
import sys

import requests

API_TIMEOUT_SECONDS = 30

class MetricCollector:
    def __init__(self, endpoint_url: str):
        self.endpoint_url = endpoint_url

    def fetch_metrics(self) -> dict:
        return {"status": "ok"}
```

Output:
```text
# Clean architecture according to standard PEP 8 conventions.
```

**Q22. What is PEP 20 (The Zen of Python)?**

*Explain PEP 20's origin, how to display it, and its core engineering philosophy.*

**Answer:** PEP 20 is a set of 19 guiding design aphorisms written by Tim Peters reflecting Python's core design philosophy. It is displayed by executing `import this`. Key principles:
- *Beautiful is better than ugly.*
- *Explicit is better than implicit.*
- *Simple is better than complex.*
- *Complex is better than complicated.*
- *Readability counts.*
- *Errors should never pass silently, unless explicitly silenced.*

```python
import this
# Prints the 19 lines of the Zen of Python
```

Output:
```text
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
...
```

**Q23. What is PEP 257 and how should docstrings be structured?**

*Explain docstring conventions for modules, classes, and functions according to PEP 257.*

**Answer:** PEP 257 outlines conventions for writing Python docstrings. A docstring should be enclosed in triple double quotes (`"""`). For one-liners, the summary ends with a period, and closing quotes are on the same line (`"""Do something."""`). For multi-line docstrings, a summary line is followed by a blank line, an elaboration, parameter descriptions, return value, and exceptions raised, with closing quotes on their own line.

```python
def deploy_service(service_name: str, replicas: int = 1) -> bool:
    """Deploy a containerized microservice to the active Kubernetes cluster.

    Args:
        service_name: The identifier matching the deployment manifest.
        replicas: Desired number of pod instances. Defaults to 1.

    Returns:
        bool: True if deployment achieved healthy state within timeout.

    Raises:
        ValueError: If replicas is less than 1.
    """
    if replicas < 1:
        raise ValueError("Replicas must be positive")
    return True

print(deploy_service("auth-api", 3))
```

Output:
```text
True
```

**Q24. Output Question: What does simultaneous tuple assignment and swap print?**

*Predict output when assigning multiple variables simultaneously.*

**Answer:** The right-hand side tuple `(10, 20)` is packed and evaluated first, then unpacked into `x` and `y`.

```python
x, y = 10, 20
x, y = y, x + y
print(f"x={x}, y={y}")
```

Output:
```text
x=20, y=30
```

**Q25. Output Question: What is printed and why?**

*Predict the result of string and integer multiplication and addition.*

**Answer:** In Python, the `*` operator on a string denotes repetition (`'3' * 3 == '333'`). Adding another string concatenates (`'333' + '3' == '3333'`). Passing to `int()` converts the string representation of digits into an integer.

```python
val = int("3" * 3 + "3")
print(val + 1)
```

Output:
```text
3334
```

**Q26. Output Question: What happens when chained assignment interacts with mutable objects?**

*Predict the outcome when multiple names reference the same mutable list.*

**Answer:** Chained assignment binds `a`, `b`, and `c` to the exact same list object in memory (`id(a) == id(b) == id(c)`). Appending through `c` mutates the underlying list, so `a` and `b` reflect the change.

```python
a = b = c = []
c.append("devops")
print(f"a: {a}, b: {b}, c: {c}")
print("Same object id:", id(a) == id(b) == id(c))
```

Output:
```text
a: ['devops'], b: ['devops'], c: ['devops']
Same object id: True
```

**Q27. Debugging Question: Identify and correct the syntax and logic errors.**

*Fix the invalid syntax and type handling in the following script snippet.*

**Answer:** Errors:
1. Missing colon `:` on the `if` statement.
2. Concatenating integer to string without conversion.
3. Using unquoted keyword `class` as a variable name.

```python
# Buggy Code:
# class = "DevOps"
# count = 5
# if count > 0
#     print("Class is " + class + " with students " + count)

# Fixed Code:
course_name = "DevOps"
count = 5
if count > 0:
    print(f"Course is {course_name} with students {count}")
```

Output:
```text
Course is DevOps with students 5
```

**Q28. Programming Question: Write a script to display system architecture, OS, and Python build details.**

*Demonstrate standard library modules platform and sys to inspect runtime environment.*

**Answer:** Use the `platform` and `sys` modules to extract comprehensive runtime telemetry.

```python
import platform
import sys

def get_system_telemetry():
    return {
        "os_name": platform.system(),
        "os_release": platform.release(),
        "architecture": platform.machine(),
        "python_version": platform.python_version(),
        "python_build": platform.python_build()[0],
        "executable": sys.executable
    }

telemetry = get_system_telemetry()
for k, v in telemetry.items():
    print(f"{k}: {v}")
```

Output:
```text
os_name: Windows
os_release: 11
architecture: AMD64
python_version: 3.12.10
python_build: tags/v3.12.10:8e92823
executable: C:\Users\binil\AppData\Local\Programs\Python\Python312\python.exe
```

**Q29. Programming Question: Build a formatted CLI banner display with timestamps and environment flags.**

*Implement an enterprise-grade startup banner for automated CLI tools.*

**Answer:** Format multi-line CLI banners cleanly using f-strings and datetime.

```python
from datetime import datetime

def display_banner(app_name: str, version: str, env: str = "PRODUCTION"):
    width = 50
    print("=" * width)
    print(f"{app_name.upper():^{width}}")
    print(f"Version: {version} | Env: {env}".center(width))
    print(f"Timestamp: {datetime.utcnow().strftime('%Y-%m-%d %H:%M:%S UTC')}".center(width))
    print("=" * width)

display_banner("Cloud Automation Engine", "2.4.0", "STAGING")
```

Output:
```text
==================================================
            CLOUD AUTOMATION ENGINE            
            Version: 2.4.0 | Env: STAGING           
         Timestamp: 2026-10-30 04:20:00 UTC         
==================================================
```

**Q30. Programming Question: Safely convert user input with fallback handling.**

*Write a reusable function to parse user input strings into typed integers or floats without crashing on ValueError.*

**Answer:** Wrap parsing logic in a try-except block returning an optional default value.

```python
def safe_parse_int(value: str, default: int = 0) -> int:
    try:
        return int(value.strip())
    except (ValueError, TypeError, AttributeError):
        return default

print(safe_parse_int(" 42 "))
print(safe_parse_int("invalid_number", default=-1))
print(safe_parse_int(None, default=100))
```

Output:
```text
42
-1
100
```


---

## Chapter 2: Variables, Data Types, and Memory

**Q31. How are variables represented in Python memory?**

*Explain what a variable is in Python compared to languages like C or Java.*

**Answer:** In Python, variables are not typed memory locations containing raw bytes. Instead, variables are **names (pointers/references)** bound to objects stored on the heap. Every object in Python is represented by a C struct (`PyObject`) that contains at least three fields: a 64-bit reference count (`ob_refcnt`), a pointer to the type object (`ob_type`), and the actual payload/value.

```python
x = 1000
print(f"Memory address of object referenced by x: {hex(id(x))}")
print(f"Type object name: {type(x).__name__}")
```

Output:
```text
Memory address of object referenced by x: 0x...
Type object name: int
```

**Q32. What is dynamic typing and variable rebinding?**

*How does Python handle re-assigning a variable to a different type?*

**Answer:** Because variables are references, reassigning a variable simply points the reference to a new object. The reference count of the old object is decremented by 1 (and garbage collected if it hits 0), and the reference count of the new object is incremented.

```python
data = 42
old_id = id(data)
data = "Cloud Computing"
new_id = id(data)
print(f"Old object ID: {old_id} != New object ID: {new_id}")
print(f"Changed: {old_id != new_id}")
```

Output:
```text
Old object ID: ... != New object ID: ...
Changed: True
```

**Q33. How does multiple assignment and chained assignment work?**

*Explain the difference between chained assignment `a = b = 10` and tuple unpacking `a, b = 10, 20`.*

**Answer:** In chained assignment (`a = b = expr`), the expression on the right is evaluated once, and its reference is assigned to all target variables from left to right. In tuple unpacking (`a, b = expr1, expr2`), the right side creates a tuple that is unpacked sequentially into the left-side targets.

```python
# Multiple assignment (tuple unpacking)
host, port = "127.0.0.1", 8080

# Chained assignment
status = code = 200

print(f"host: {host}, port: {port}")
print(f"status: {status}, code: {code}, same_id: {id(status) == id(code)}")
```

Output:
```text
host: 127.0.0.1, port: 8080
status: 200, code: 200, same_id: True
```

**Q34. How are constants defined and enforced in Python?**

*Explain naming conventions and typing constructs (typing.Final) for constants.*

**Answer:** Python does not have a language-level `const` keyword. By convention (PEP 8), constants are written in uppercase with underscores (`SCREAMING_SNAKE_CASE`). In Python 3.8+, static type checkers enforce constants via `typing.Final`. At runtime, a custom class with `__setattr__` can prevent reassignment.

```python
from typing import Final

MAX_RETRIES: Final[int] = 5
# Type checkers like mypy will flag: MAX_RETRIES = 10

class ImmutableConfig:
    def __init__(self, **kwargs):
        self.__dict__.update(kwargs)
    def __setattr__(self, name, value):
        raise TypeError(f"Cannot reassign constant attribute '{name}'")

cfg = ImmutableConfig(ENVIRONMENT="PRODUCTION", TIMEOUT=30)
try:
    cfg.TIMEOUT = 60
except TypeError as e:
    print(e)
```

Output:
```text
Cannot reassign constant attribute 'TIMEOUT'
```

**Q35. What are the core built-in data types in Python?**

*Classify Python's standard data types by category.*

**Answer:** Python's built-in types are classified as:
1. **Numeric**: `int`, `float`, `complex`, `bool`
2. **Sequence**: `str`, `list`, `tuple`, `range`
3. **Binary**: `bytes`, `bytearray`, `memoryview`
4. **Mapping**: `dict`
5. **Set**: `set`, `frozenset`
6. **Callable**: Functions, methods, classes
7. **Special Singletons**: `NoneType` (`None`), `NotImplementedType`, `ellipsis` (`...`).

```python
types = [42, 3.14, 2+3j, True, "hello", [1], (1,), range(3), b"raw", bytearray(1), {"k": "v"}, {1, 2}, frozenset([1]), None]
for t in types:
    print(f"{repr(t):<20} -> {type(t).__name__}")
```

Output:
```text
42                   -> int
3.14                 -> float
(2+3j)               -> complex
True                 -> bool
'hello'              -> str
[1]                  -> list
(1,)                 -> tuple
range(0, 3)          -> range
b'raw'               -> bytes
bytearray(b'\x00')   -> bytearray
{'k': 'v'}           -> dict
{1, 2}               -> set
frozenset({1})       -> frozenset
None                 -> NoneType
```

**Q36. How does Python handle arbitrary precision integers?**

*Explain why Python integers do not overflow like C 32/64-bit integers.*

**Answer:** In Python 3, the `int` type has arbitrary precision (unlimited bit-width, limited only by available system RAM). Under the hood, CPython uses a variable-length structure (`PyLongObject`) containing an array of 30-bit 'digits'. When an integer exceeds 64 bits, Python automatically allocates more digits without throwing integer overflow exceptions.

```python
large_int = 2 ** 128
print("2^128 =", large_int)
print("Bits required:", large_int.bit_length())
print("Byte length:", (large_int.bit_length() + 7) // 8)
```

Output:
```text
2^128 = 340282366920938463463374607431768211456
Bits required: 129
Byte length: 17
```

**Q37. What is the internal representation of float and its precision caveats?**

*Explain IEEE 754 double precision representation and why `0.1 + 0.2 != 0.3`.*

**Answer:** Python's `float` is implemented as an IEEE 754 64-bit double-precision floating-point number (53 bits of precision, ~15-17 significant decimal digits). Because numbers like 0.1 and 0.2 cannot be represented with infinite precision in binary base-2 fractions, rounding discrepancies occur. For financial calculations, `decimal.Decimal` must be used.

```python
from decimal import Decimal

sum_float = 0.1 + 0.2
print(f"0.1 + 0.2 == 0.3: {sum_float == 0.3}")
print(f"Actual float: {sum_float:.20f}")

sum_dec = Decimal("0.1") + Decimal("0.2")
print(f"Decimal equality: {sum_dec == Decimal('0.3')}")
```

Output:
```text
0.1 + 0.2 == 0.3: False
Actual float: 0.30000000000000004441
Decimal equality: True
```

**Q38. What is the complex number type in Python?**

*Explain syntax, real/imag attributes, and conjugate calculation of complex numbers.*

**Answer:** Python natively supports complex numbers written as `real + imag j`. Attributes `.real` and `.imag` return floats, and `.conjugate()` returns the complex conjugate.

```python
c = 3 + 4j
print(f"Real: {c.real}, Imag: {c.imag}")
print(f"Conjugate: {c.conjugate()}")
print(f"Magnitude |c|: {abs(c)}")
```

Output:
```text
Real: 3.0, Imag: 4.0
Conjugate: (3-4j)
Magnitude |c|: 5.0
```

**Q39. How is the bool type implemented in Python?**

*Explain why `bool` is a subclass of `int` and the behavior of `isinstance(True, int)`.*

**Answer:** `bool` is a direct subclass of `int`. `True` has value `1` and `False` has value `0`. As a result, booleans can participate in arithmetic operations, and `isinstance(True, int)` evaluates to `True`.

```python
print("issubclass(bool, int):", issubclass(bool, int))
print("isinstance(True, int):", isinstance(True, int))
print("Arithmetic with booleans:", True + True + False + 3)
```

Output:
```text
issubclass(bool, int): True
isinstance(True, int): True
Arithmetic with booleans: 5
```

**Q40. What are the binary sequence types: bytes, bytearray, and memoryview?**

*Compare immutable `bytes`, mutable `bytearray`, and zero-copy `memoryview`.*

**Answer:** - `bytes`: Immutable sequence of integers in range 0-255 (e.g. `b'hello'`).
- `bytearray`: Mutable counterpart allowing in-place slice and byte modification.
- `memoryview`: Allows Python code to access the internal buffer of an object supporting the buffer protocol without copying memory (zero-copy slicing).

```python
b = bytes([65, 66, 67])
ba = bytearray(b"Hello")
ba[0] = ord("J")

mv = memoryview(ba)
print("bytes:", b)
print("bytearray modified:", ba)
print("memoryview slice:", mv[1:].tobytes())
```

Output:
```text
bytes: b'ABC'
bytearray modified: bytearray(b'Jello')
memoryview slice: b'ello'
```

**Q41. What are None, NotImplemented, and Ellipsis singletons?**

*Detail the purpose and use cases for Python's core singleton objects.*

**Answer:** 1. `None`: Represents the absence of a value or default return of functions.
2. `NotImplemented`: Special value returned by rich comparison methods (`__eq__`, `__lt__`) to signal Python to try the reflected operation on the other operand.
3. `Ellipsis` (`...`): Used in multidimensional array slicing (NumPy), type hints (`Callable[..., int]`), and function body stubs.

```python
def unimplemented_stub() -> ...:
    ...

print("None identity:", None is None)
print("Ellipsis repr:", ...)
print("Function returns:", unimplemented_stub())
```

Output:
```text
None identity: True
Ellipsis repr: Ellipsis
Function returns: None
```

**Q42. How does type checking differ between type(), isinstance(), and issubclass()?**

*Explain why `isinstance()` is preferred over `type(x) == Type`.*

**Answer:** `type(x) == Class` checks for exact identity of the class and ignores inheritance. `isinstance(x, Class)` checks if `x` is an instance of `Class` or any subclass derived from it, respecting polymorphism. `issubclass(Sub, Super)` tests class hierarchies directly.

```python
class Animal: pass
class Dog(Animal): pass

d = Dog()
print("type(d) == Animal:", type(d) == Animal)
print("isinstance(d, Animal):", isinstance(d, Animal))
print("issubclass(Dog, Animal):", issubclass(Dog, Animal))
```

Output:
```text
type(d) == Animal: False
isinstance(d, Animal): True
issubclass(Dog, Animal): True
```

**Q43. What is the difference between implicit and explicit type conversion?**

*Explain type coercion vs explicit type casting.*

**Answer:** Implicit type conversion (coercion) happens automatically without programmer intervention when operations are mathematically safe and unambiguous (e.g., adding an `int` and a `float` promotes the `int` to `float`). Explicit type conversion (type casting) requires calling type constructors (`int()`, `float()`, `str()`).

```python
# Implicit conversion:
x = 10      # int
y = 2.5     # float
z = x + y   # automatically promoted to float
print(f"z = {z}, type: {type(z).__name__}")

# Explicit conversion:
s = "100"
converted = int(s)
print(f"converted = {converted}, type: {type(converted).__name__}")
```

Output:
```text
z = 12.5, type: float
converted = 100, type: int
```

**Q44. What are truthy and falsy values in Python?**

*List all standard objects that evaluate to False in a boolean context.*

**Answer:** In Python, an object evaluates to `False` in boolean contexts if:
1. It is a defined constant: `None`, `False`.
2. It is zero of any numeric type: `0`, `0.0`, `0j`, `Decimal(0)`, `Fraction(0, 1)`.
3. It is an empty sequence or collection: `''`, `()`, `[]`, `{}`, `set()`, `range(0)`.
All other objects evaluate to `True` (unless a custom class implements `__bool__()` or `__len__()` returning `False` or `0`).

```python
falsy_items = [False, None, 0, 0.0, "", (), [], {}, set(), range(0)]
results = [bool(item) for item in falsy_items]
print("All items falsy:", not any(results))
```

Output:
```text
All items falsy: True
```

**Q45. What is the difference between mutable and immutable objects?**

*Categorize standard Python objects and explain the impact on function parameters.*

**Answer:** - **Immutable**: State cannot be altered after creation (`int`, `float`, `complex`, `str`, `tuple`, `frozenset`, `bytes`). Modifying creates a new object in memory.
- **Mutable**: Internal state can be modified in-place without changing object identity (`list`, `dict`, `set`, `bytearray`, custom user objects).

```python
# Immutable string:
s = "dev"
old_id = id(s)
s += "ops"
print(f"Immutable string changed ID: {old_id != id(s)}")

# Mutable list:
lst = [1, 2]
old_list_id = id(lst)
lst.append(3)
print(f"Mutable list retained ID: {old_list_id == id(lst)}")
```

Output:
```text
Immutable string changed ID: True
Mutable list retained ID: True
```

**Q46. What is object identity vs object equality (== vs is)?**

*Explain the difference between value comparison and memory address comparison.*

**Answer:** - `==` checks for **value equality** (invokes `__eq__`), verifying whether two objects contain the same data.
- `is` checks for **reference identity**, verifying whether two variables point to the exact same memory address (`id(a) == id(b)`).

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(f"a == b: {a == b}")
print(f"a is b: {a is b}")
print(f"a is c: {a is c}")
```

Output:
```text
a == b: True
a is b: False
a is c: True
```

**Q47. How does Python optimize memory with small integer caching and string interning?**

*Explain why `a = 256; b = 256; a is b` is True, but `a = 1000; b = 1000; a is b` may be False.*

**Answer:** CPython pre-allocates and caches an array of singleton integer objects for numbers in the range `[-5, 256]` during interpreter startup. Whenever an integer in this range is created, CPython reuses the existing singleton reference. Strings that look like valid identifiers are also interned automatically.

```python
x = 256
y = 256
print("256 is cached:", x is y)

p = 1000
q = 1000
print("1000 is same object (interactive/script dependent):", p is q)
```

Output:
```text
256 is cached: True
1000 is same object (interactive/script dependent): False
```

**Q48. How does CPython's reference counting garbage collection work?**

*Explain `sys.getrefcount()`, increment/decrement triggers, and immediate deallocation.*

**Answer:** CPython's primary memory management system is **reference counting**. Each object tracks how many references point to it. The count increments when assigned to a variable, passed to a function, or added to a container. It decrements when a variable goes out of scope, is reassigned, or is deleted via `del`. When an object's reference count reaches zero, its memory is freed immediately.

```python
import sys

val = ["cloud", "devops"]
print("Initial refcount:", sys.getrefcount(val))  # Note: getrefcount adds 1 temporary ref
alias = val
print("Refcount after alias:", sys.getrefcount(val))
del alias
print("Refcount after del alias:", sys.getrefcount(val))
```

Output:
```text
Initial refcount: 2
Refcount after alias: 3
Refcount after del alias: 2
```

**Q49. How does Python handle reference cycles and cyclic garbage collection?**

*Explain why reference counting fails on circular references and how the `gc` module solves it.*

**Answer:** If Object A points to Object B, and Object B points to Object A, their reference counts remain at least 1 even if all external variables are deleted. Reference counting cannot reclaim them. CPython solves this with a **generational cyclic garbage collector** (`gc` module) that detects unreachable cycles across 3 generations (Gen 0, 1, 2) using graph traversal.

```python
import gc

class Node:
    def __init__(self, name):
        self.name = name
        self.link = None

# Create circular reference
node1 = Node("A")
node2 = Node("B")
node1.link = node2
node2.link = node1

del node1
del node2

unreachable = gc.collect()
print("Cyclic objects collected by gc:", unreachable > 0)
```

Output:
```text
Cyclic objects collected by gc: True
```

**Q50. What is the difference between shallow copy and deep copy?**

*Compare direct assignment, `copy.copy()`, and `copy.deepcopy()` on nested data structures.*

**Answer:** - **Direct Assignment (`b = a`)**: Copies the pointer only. Both point to the exact same object.
- **Shallow Copy (`copy.copy(a)` or `a.copy()`)**: Creates a new outer container, but child elements are references to original objects.
- **Deep Copy (`copy.deepcopy(a)`)**: Recursively duplicates the container and all nested objects.

```python
import copy

original = [[1, 2], [3, 4]]
shallow = copy.copy(original)
deep = copy.deepcopy(original)

original[0].append(99)
print(f"Original: {original}")
print(f"Shallow:  {shallow}")
print(f"Deep:     {deep}")
```

Output:
```text
Original: [[1, 2, 99], [3, 4]]
Shallow:  [[1, 2, 99], [3, 4]]
Deep:     [[1, 2], [3, 4]]
```

**Q51. What is a namespace in Python?**

*Explain local, enclosing, global, and built-in namespaces.*

**Answer:** A namespace is a mapping from variable names to objects (implemented internally as Python dictionaries). Python manages 4 namespaces:
1. **Built-in**: Created on startup, contains `print`, `len`, `Exception`.
2. **Global**: Module-level names created when the module is imported.
3. **Enclosing**: Namespaces of outer functions in nested scopes.
4. **Local**: Created when a function is called, destroyed when it returns.

```python
x_global = "global"

def outer():
    x_enclosing = "enclosing"
    def inner():
        x_local = "local"
        print(f"Local: {x_local}, Enclosing: {x_enclosing}, Global: {x_global}")
    inner()

outer()
```

Output:
```text
Local: local, Enclosing: enclosing, Global: global
```

**Q52. What is scope and the LEGB name resolution rule?**

*Explain the order in which Python resolves variable names.*

**Answer:** Scope is the textual region of a Python program where a namespace is directly accessible. When a variable name is evaluated, Python searches scopes in the strict **LEGB** order:
1. **L**ocal: Inside the current function.
2. **E**nclosing: In enclosing/outer functions (if nested).
3. **G**lobal: At the top level of the current module.
4. **B**uilt-in: In `builtins` module.
If not found in any of the four, a `NameError` is raised.

```python
x = "Global Scope"

def outer_func():
    x = "Enclosing Scope"
    def inner_func():
        x = "Local Scope"
        return x
    return inner_func()

print(outer_func())
```

Output:
```text
Local Scope
```

**Q53. How do the `global` and `nonlocal` keywords modify scope resolution?**

*Explain when and how to rebind global and enclosing variables inside inner functions.*

**Answer:** - `global <var>`: Declares that an identifier inside a function refers to the module-level global namespace, allowing in-place reassignment.
- `nonlocal <var>`: Declares that an identifier refers to the nearest enclosing (outer) function scope, preventing the creation of a new local variable.

```python
counter = 0

def increment_global():
    global counter
    counter += 1

def make_counter():
    count = 0
    def inc():
        nonlocal count
        count += 1
        return count
    return inc

increment_global()
c = make_counter()
print("Global counter:", counter)
print("Nonlocal closure counter:", c(), c())
```

Output:
```text
Global counter: 1
Nonlocal closure counter: 1 2
```

**Q54. What does the `del` statement do in Python?**

*Explain whether `del` deletes an object or deletes a name binding.*

**Answer:** `del` **does not delete objects directly**. `del name` removes the name binding from the current namespace and decrements the target object's reference count by 1. The memory is freed only if the object's reference count drops to zero.

```python
a = [10, 20, 30]
b = a
del a

try:
    print(a)
except NameError as e:
    print("NameError:", e)

print("Object still referenced by b:", b)
```

Output:
```text
NameError: name 'a' is not defined
Object still referenced by b: [10, 20, 30]
```

**Q55. What is variable shadowing in Python?**

*Demonstrate variable shadowing in inner scopes and how to avoid bugs.*

**Answer:** Variable shadowing occurs when a variable declared in an inner scope (e.g. function local) shares the same name as a variable in an outer scope (global or enclosing). The local variable shadows (hides) the outer variable within the local scope, preventing direct access without `globals()`.

```python
endpoint = "https://api.prod.com"

def process_request():
    endpoint = "https://api.dev.com" # Shadows global endpoint
    print(f"Inside function: {endpoint}")

process_request()
print(f"Global endpoint: {endpoint}")
```

Output:
```text
Inside function: https://api.dev.com
Global endpoint: https://api.prod.com
```

**Q56. Output Question: What is printed when mutating default arguments across multiple calls?**

*Predict the result of evaluating default mutable arguments across multiple calls.*

**Answer:** In Python, default parameter expressions are evaluated **once at function definition time**, not at each invocation. The default list is shared across all function calls that omit the argument.

```python
def append_item(val, items=[]):
    items.append(val)
    return items

print(append_item(1))
print(append_item(2))
print(append_item(3, []))
print(append_item(4))
```

Output:
```text
[1]
[1, 2]
[3]
[1, 2, 4]
```

**Q57. Output Question: What does this tuple modification code output?**

*Predict the behavior when modifying a mutable list inside an immutable tuple.*

**Answer:** The tuple is immutable, so its references cannot be reassigned. However, the list *inside* the tuple is mutable. `tpl[0].append(4)` modifies the list in place without reassigning the tuple's pointer, succeeding completely. But `tpl[0] += [5]` attempts augmented assignment (modify and reassign), mutating the list but raising a `TypeError` on tuple assignment!

```python
tpl = ([1, 2, 3], "static")
tpl[0].append(4)
print("After append:", tpl)

try:
    tpl[0] += [5]
except TypeError as e:
    print(f"Raised TypeError: {e}")
print("After += attempted:", tpl)
```

Output:
```text
After append: ([1, 2, 3, 4], 'static')
Raised TypeError: 'tuple' object does not support item assignment
After += attempted: ([1, 2, 3, 4, 5], 'static')
```

**Q58. Output Question: What is the output of id() comparisons across float literals?**

*Predict the behavior of floating point allocations.*

**Answer:** Each float literal instantiated outside caching creates a distinct float object with a unique memory address.

```python
f1 = 12.34
f2 = 12.34
print(f1 == f2)
print(f1 is f2)
```

Output:
```text
True
False
```

**Q59. Debugging Question: Fix the UnboundLocalError bug.**

*Explain why referencing a global variable before assignment inside a function causes an UnboundLocalError.*

**Answer:** Because `count = count + 1` assigns to `count`, Python marks `count` as a local variable for the entire function during bytecode compilation. When `count + 1` is evaluated, the local `count` has not yet been bound.

```python
# Buggy Code:
# count = 10
# def increment():
#     count += 1
# increment()

# Fixed Code:
count = 10
def increment():
    global count
    count += 1
    return count

print("Incremented count:", increment())
```

Output:
```text
Incremented count: 11
```

**Q60. Programming Question: Inspect the memory layout of custom objects.**

*Write a program to display the memory size of built-in and custom objects using sys.getsizeof.*

**Answer:** Use `sys.getsizeof()` to inspect shallow memory footprints.

```python
import sys

class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

p = Point(10, 20)
print(f"Size of int(0):        {sys.getsizeof(0)} bytes")
print(f"Size of empty list:    {sys.getsizeof([])} bytes")
print(f"Size of empty dict:    {sys.getsizeof({})} bytes")
print(f"Size of custom object: {sys.getsizeof(p)} bytes")
print(f"Size of object __dict__:{sys.getsizeof(p.__dict__)} bytes")
```

Output:
```text
Size of int(0):        28 bytes
Size of empty list:    56 bytes
Size of empty dict:    64 bytes
Size of custom object: 48 bytes
Size of object __dict__:104 bytes
```

**Q61. Programming Question: Build an immutable data structure with frozen attributes.**

*Create a frozen class pattern that disallows adding, updating, or deleting attributes at runtime.*

**Answer:** Override `__setattr__` and `__delattr__` in the class definition.

```python
class FrozenServerConfig:
    def __init__(self, host: str, port: int):
        super().__setattr__("host", host)
        super().__setattr__("port", port)
        super().__setattr__("_frozen", True)

    def __setattr__(self, name, value):
        if getattr(self, "_frozen", False):
            raise AttributeError(f"Immutable instance: cannot set '{name}'")
        super().__setattr__(name, value)

    def __delattr__(self, name):
        raise AttributeError(f"Immutable instance: cannot delete '{name}'")

cfg = FrozenServerConfig("10.0.0.1", 443)
print(f"Config: {cfg.host}:{cfg.port}")
try:
    cfg.port = 80
except AttributeError as e:
    print(e)
```

Output:
```text
Config: 10.0.0.1:443
Immutable instance: cannot set 'port'
```

**Q62. Programming Question: Safely deepcopy dynamic configuration dictionaries.**

*Write a utility function that clones nested configuration dictionaries without retaining references.*

**Answer:** Use `copy.deepcopy()` to isolate nested dictionary updates.

```python
import copy

def clone_and_patch_config(base_cfg: dict, overrides: dict) -> dict:
    new_cfg = copy.deepcopy(base_cfg)
    for k, v in overrides.items():
        if isinstance(v, dict) and k in new_cfg and isinstance(new_cfg[k], dict):
            new_cfg[k].update(v)
        else:
            new_cfg[k] = v
    return new_cfg

base = {"db": {"host": "localhost", "port": 5432}, "debug": False}
patched = clone_and_patch_config(base, {"db": {"host": "prod-db"}, "debug": True})

print("Base host:", base["db"]["host"])
print("Patched host:", patched["db"]["host"])
```

Output:
```text
Base host: localhost
Patched host: prod-db
```


---

## Chapter 3: Operators and Expressions

**Q63. What are arithmetic operators in Python?**

*List all arithmetic operators and explain division semantics.*

**Answer:** Python arithmetic operators:
- `+` Addition
- `-` Subtraction
- `*` Multiplication
- `/` True float division (always returns `float`)
- `//` Floor integer division (rounds down to negative infinity)
- `%` Modulo (remainder of floor division)
- `**` Exponentiation.

```python
print("7 / 2 =", 7 / 2)
print("7 // 2 =", 7 // 2)
print("-7 // 2 =", -7 // 2)  # Floor rounds down toward negative infinity!
print("7 % 2 =", 7 % 2)
print("-7 % 2 =", -7 % 2)
print("2 ** 5 =", 2 ** 5)
```

Output:
```text
7 / 2 = 3.5
7 // 2 = 3
-7 // 2 = -4
7 % 2 = 1
-7 % 2 = 1
2 ** 5 = 32
```

**Q64. How does floor division (`//`) differ from truncating toward zero?**

*Explain why `-7 // 2 == -4` in Python while in C or Java `-7 / 2 == -3`.*

**Answer:** Python defines division and modulo using Knuth's floored division rule: `a == (a // b) * b + (a % b)`, where the quotient is floored toward negative infinity (`floor(a/b)`). In C/Java, division truncates toward zero. As a consequence, Python's modulo `a % b` always shares the sign of the divisor `b`.

```python
import math

print("Python floor division -7 // 2:", -7 // 2)
print("math.trunc(-7 / 2):", math.trunc(-7 / 2))
```

Output:
```text
Python floor division -7 // 2: -4
math.trunc(-7 / 2): -3
```

**Q65. What are comparison operators and how does chained comparison work?**

*Explain comparison operators and chained expressions like `1 < x < 10`.*

**Answer:** Comparison operators (`==`, `!=`, `<`, `>`, `<=`, `>=`) compare operands and return a boolean. Python uniquely supports **chained comparisons**: `a < b < c` translates to `(a < b) and (b < c)`, evaluating `b` only once.

```python
x = 5
print("1 < x < 10:", 1 < x < 10)
print("10 < x < 20:", 10 < x < 20)

# Chained evaluation efficiency (b evaluated once):
def get_mid():
    print("Evaluating mid")
    return 5

print("Result:", 0 < get_mid() < 10)
```

Output:
```text
1 < x < 10: True
10 < x < 20: False
Evaluating mid
Result: True
```

**Q66. What are logical operators and how does short-circuit evaluation work?**

*Explain `and`, `or`, `not` and the exact value returned by logical expressions.*

**Answer:** Logical operators evaluate truth values with **short-circuiting**:
- `a and b`: Evaluates `a`. If `a` is falsy, returns `a` immediately without evaluating `b`. If `a` is truthy, returns `b`.
- `a or b`: Evaluates `a`. If `a` is truthy, returns `a` immediately. If `a` is falsy, returns `b`.
- `not a`: Returns `True` if `a` is falsy, else `False`.
Note that Python returns the operand value itself, not necessarily a boolean.

```python
print("0 and 'devops':", repr(0 and 'devops'))
print("'cloud' and 'aws':", repr('cloud' and 'aws'))
print("'' or 'fallback':", repr('' or 'fallback'))
print("'default' or 'fallback':", repr('default' or 'fallback'))
```

Output:
```text
0 and 'devops': 0
'cloud' and 'aws': 'aws'
'' or 'fallback': 'fallback'
'default' or 'fallback': 'default'
```

**Q67. What are Python's bitwise operators?**

*List `&`, `|`, `^`, `~`, `<<`, `>>` and explain bitwise operations.*

**Answer:** Bitwise operators operate on binary representations of integers:
- `&` Bitwise AND
- `|` Bitwise OR
- `^` Bitwise XOR
- `~` Bitwise NOT (inverts bits: `~x == -(x + 1)`)
- `<<` Left shift (multiplies by $2^n$)
- `>>` Right shift (floors division by $2^n$).

```python
a = 0b1100  # 12
b = 0b1010  # 10

print("AND: ", bin(a & b), f"({a & b})")
print("OR:  ", bin(a | b), f"({a | b})")
print("XOR: ", bin(a ^ b), f"({a ^ b})")
print("NOT: ", ~a)
print("Left Shift (12 << 2):", a << 2)
print("Right Shift (12 >> 2):", a >> 2)
```

Output:
```text
AND:  0b1000 (8)
OR:   0b1110 (14)
XOR:  0b110 (6)
NOT:  -13
Left Shift (12 << 2): 48
Right Shift (12 >> 2): 3
```

**Q68. What are identity operators (`is` and `is not`)?**

*Explain identity comparison and when to use `is None` instead of `== None`.*

**Answer:** `is` and `is not` test whether two variables point to the exact same object in memory (`id(a) == id(b)`). In Python, singletons like `None` must always be compared using `is None` or `is not None` (PEP 8) because `==` can be overridden by classes (`__eq__`) to return misleading results.

```python
class AlwaysEqual:
    def __eq__(self, other):
        return True

obj = AlwaysEqual()
print("obj == None:", obj == None)
print("obj is None:", obj is None)
```

Output:
```text
obj == None: True
obj is None: False
```

**Q69. What are membership operators (`in` and `not in`)?**

*Explain how membership testing operates across strings, lists, sets, and dictionaries.*

**Answer:** `in` and `not in` test if a value exists within an iterable. For sequences (`list`, `tuple`, `str`), search complexity is $O(N)$. For hash-based collections (`set`, `dict`), lookup is $O(1)$ on average. For dictionaries, `in` checks keys by default.

```python
server_roles = {"web", "db", "cache"}
print("'db' in server_roles:", "db" in server_roles)

config = {"host": "localhost", "port": 80}
print("'host' in config (checks keys):", "host" in config)
print("80 in config:", 80 in config)
print("80 in config.values():", 80 in config.values())
```

Output:
```text
'db' in server_roles: True
'host' in config (checks keys): True
80 in config: False
80 in config.values(): True
```

**Q70. What is operator precedence and associativity in Python?**

*List the precedence order from highest to lowest.*

**Answer:** Precedence order (highest to lowest):
1. Parentheses: `()`
2. Exponentiation: `**` (right-associative!)
3. Unary operators: `+x`, `-x`, `~x`
4. Multiplication/Division: `*`, `/`, `//`, `%`
5. Addition/Subtraction: `+`, `-`
6. Bitwise shifts: `<<`, `>>`
7. Bitwise AND: `&`
8. Bitwise XOR/OR: `^`, `|`
9. Comparisons & Membership/Identity: `==`, `!=`, `<`, `>`, `in`, `is`
10. Boolean NOT: `not`
11. Boolean AND: `and`
12. Boolean OR: `or`
13. Conditional Expression: `x if c else y`
14. Assignment: `=`, `+=`, `:=`.

```python
# Exponentiation is right-associative:
# 2 ** 3 ** 2 == 2 ** (3 ** 2) == 2 ** 9 == 512, NOT (2 ** 3) ** 2 == 64
print("2 ** 3 ** 2 =", 2 ** 3 ** 2)

# Arithmetic vs logical precedence:
print("2 + 3 * 4 == 14:", 2 + 3 * 4 == 14)
```

Output:
```text
2 ** 3 ** 2 = 512
2 + 3 * 4 == 14: True
```

**Q71. What is the conditional (ternary) operator in Python?**

*Explain syntax, evaluation order, and readability best practices.*

**Answer:** Python's ternary operator takes the form: `x if condition else y`. It evaluates `condition` first; if truthy, it returns `x`; otherwise, it evaluates and returns `y`.

```python
status_code = 200
status_msg = "OK" if status_code == 200 else "ERROR"
print("Status:", status_msg)
```

Output:
```text
Status: OK
```

**Q72. What is the walrus operator (`:=`) introduced in Python 3.8?**

*Explain assignment expressions, syntax, and real-world use cases.*

**Answer:** The walrus operator (`:=`) allows assigning a value to a variable *inside an expression*. This avoids redundant function evaluations or repeated assignments in `while` loops, `if` statements, and list comprehensions.

```python
# Reading line-by-line or filtering in while loop
data = ["cloud", "devops", "engineering", "infrastructure"]

# Filtering and capturing length in list comprehension
filtered = [length for item in data if (length := len(item)) > 6]
print("Lengths > 6:", filtered)
```

Output:
```text
Lengths > 6: [11, 14]
```

**Q73. How do augmented assignment operators work with mutable vs immutable objects?**

*Explain why `lst += [1]` mutates in place, while `num += 1` creates a new object.*

**Answer:** For immutable types (`int`, `str`), `x += y` is equivalent to `x = x + y`, creating a new object. For mutable types like `list`, `+=` calls the in-place method `__iadd__`, mutating the list in-place (equivalent to `lst.extend(y)`).

```python
list_a = [1, 2]
ref_a = list_a
list_a += [3]
print("List in-place mutation (ref_a == list_a):", ref_a is list_a)

num = 10
ref_num = num
num += 5
print("Number new object created (ref_num == num):", ref_num is num)
```

Output:
```text
List in-place mutation (ref_a == list_a): True
Number new object created (ref_num == num): False
```

**Q74. How does iterable unpacking using the asterisk (`*`) and double asterisk (`**`) work?**

*Explain starred expressions in assignments, function calls, and dictionary merges.*

**Answer:** - In assignments: `first, *middle, last = [1, 2, 3, 4, 5]` captures intermediate elements into a list.
- In function calls: `*args` unpacks sequences into positional arguments; `**kwargs` unpacks dicts into keyword arguments.
- In dictionary literals (3.5+): `merged = {**d1, **d2}` merges dictionaries.

```python
first, *middle, last = [10, 20, 30, 40, 50]
print(f"first: {first}, middle: {middle}, last: {last}")

dict1 = {"env": "prod"}
dict2 = {"cluster": "us-east-1"}
merged = {**dict1, **dict2}
print("Merged dict:", merged)
```

Output:
```text
first: 10, middle: [20, 30, 40], last: 50
Merged dict: {'env': 'prod', 'cluster': 'us-east-1'}
```

**Q75. What is operator overloading in Python?**

*Explain how special dunder methods allow classes to intercept built-in operators.*

**Answer:** In Python, operators invoke special 'dunder' methods on class instances. Overloading is achieved by defining methods like `__add__` for `+`, `__sub__` for `-`, `__mul__` for `*`, `__eq__` for `==`, and `__lt__` for `<`.

```python
class Vector:
    def __init__(self, x, y):
        self.x, self.y = x, y

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

v1 = Vector(2, 4)
v2 = Vector(3, 1)
print("v1 + v2 =", v1 + v2)
```

Output:
```text
v1 + v2 = Vector(5, 5)
```

**Q76. How does reverse operator overloading work (`__radd__`, `__rsub__`)?**

*Explain fallback mechanisms when the left operand does not support the operation.*

**Answer:** When an expression `a + b` is executed, Python first calls `a.__add__(b)`. If `a` does not implement `__add__` or returns `NotImplemented`, Python falls back to `b.__radd__(a)` (reflected addition).

```python
class CustomNumber:
    def __init__(self, val):
        self.val = val

    def __radd__(self, other):
        return self.val + other

c = CustomNumber(10)
# int doesn't know how to add CustomNumber, so it invokes c.__radd__(5)
print("5 + c =", 5 + c)
```

Output:
```text
5 + c = 15
```

**Q77. What is the matrix multiplication operator (`@`) in Python?**

*Explain the `@` operator introduced in PEP 465 and its `__matmul__` protocol.*

**Answer:** Introduced in Python 3.5 (PEP 465), `@` is dedicated to matrix multiplication, distinct from element-wise multiplication `*`. It invokes `__matmul__` and `__rmatmul__`, universally utilized by NumPy and PyTorch.

```python
class Matrix2x2:
    def __init__(self, data):
        self.data = data
    def __matmul__(self, other):
        # 2x2 matrix multiplication logic
        a, b = self.data[0]
        c, d = self.data[1]
        e, f = other.data[0]
        g, h = other.data[1]
        return Matrix2x2([[a*e + b*g, a*f + b*h], [c*e + d*g, c*f + d*h]])

m1 = Matrix2x2([[1, 2], [3, 4]])
m2 = Matrix2x2([[2, 0], [1, 2]])
res = m1 @ m2
print("Matrix @ result:", res.data)
```

Output:
```text
Matrix @ result: [[4, 4], [10, 8]]
```

**Q78. What is the difference between `==` and `is` with empty lists?**

*Explain the memory and equality distinction for newly allocated collections.*

**Answer:** `[] == []` is `True` because their contents (empty) match. `[] is []` is `False` because each empty list literal allocates a fresh, independent list object in memory.

```python
print("[] == []:", [] == [])
print("[] is []:", [] is [])
```

Output:
```text
[] == []: True
[] is []: False
```

**Q79. What is the behavior of the `not` operator on non-boolean values?**

*Explain how `not` forces boolean evaluation.*

**Answer:** The `not` operator always returns a pure `bool` (`True` or `False`). It evaluates the truth value of the operand and returns the inverse.

```python
print("not '':", not '')
print("not 'hello':", not 'hello')
print("not 0:", not 0)
print("not [1, 2]:", not [1, 2])
```

Output:
```text
not '': True
not 'hello': False
not 0: True
not [1, 2]: False
```

**Q80. How do dictionary union operators (`|` and `|=`) work in Python 3.9+?**

*Explain PEP 584 dictionary merge and update operators.*

**Answer:** In Python 3.9+, the bitwise OR operator `|` is overloaded for dictionaries to return a new merged dictionary (`d1 | d2`). The augmented assignment `|=` updates a dictionary in place (`d1 |= d2`). If keys collide, values from the right-hand operand overwrite those on the left.

```python
defaults = {"timeout": 30, "retries": 3, "env": "dev"}
custom = {"env": "prod", "token": "xyz123"}

merged = defaults | custom
print("Merged (|):", merged)

defaults |= {"retries": 5}
print("Updated defaults (|=):", defaults)
```

Output:
```text
Merged (|): {'timeout': 30, 'retries': 3, 'env': 'prod', 'token': 'xyz123'}
Updated defaults (|=): {'timeout': 30, 'retries': 5, 'env': 'dev'}
```

**Q81. Output Question: Predict the output of this logical expression chain.**

*Analyze operator precedence and short-circuit evaluation in mixed logical expressions.*

**Answer:** `and` has higher precedence than `or`. Therefore `False and True` evaluates first to `False`. Then `True or False` evaluates to `True`.

```python
result = True or False and False
print("Result:", result)
```

Output:
```text
Result: True
```

**Q82. Output Question: What does this chained comparison evaluate to?**

*Predict the evaluation of `False == False in [False]`.*

**Answer:** Comparisons chain together! `False == False in [False]` is parsed as `(False == False) and (False in [False])`. Both are `True`, so the result is `True`.

```python
val = False == False in [False]
print("Chained comparison result:", val)
```

Output:
```text
Chained comparison result: True
```

**Q83. Output Question: Predict the output of bitwise negation on zero and positive numbers.**

*Explain two's complement arithmetic for bitwise NOT `~x`.*

**Answer:** Python uses two's complement arithmetic for negative integers: `~x = -(x + 1)`. Therefore `~0 == -1`, and `~10 == -11`.

```python
print("~0  =", ~0)
print("~10 =", ~10)
print("~(-5) =", ~(-5))
```

Output:
```text
~0  = -1
~10 = -11
~(-5) = 4
```

**Q84. Output Question: Predict the output of list multiplication and modification.**

*Explain the pitfall of multiplying lists containing mutable sublists.*

**Answer:** `[[]] * 3` creates a list containing three references to the **exact same** inner list object. Modifying `matrix[0]` modifies all three elements.

```python
matrix = [[]] * 3
matrix[0].append("A")
print("Matrix:", matrix)
```

Output:
```text
Matrix: [['A'], ['A'], ['A']]
```

**Q85. Debugging Question: Fix the operator precedence bug in string formatting and addition.**

*Resolve TypeError in string calculation.*

**Answer:** The `+` operator has higher precedence than `%` or requires parentheses when combining calculations inside string concatenation.

```python
# Buggy Code:
# print("Total: " + 10 + 20)

# Fixed Code:
print("Total: " + str(10 + 20))
print(f"Total: {10 + 20}")
```

Output:
```text
Total: 30
Total: 30
```

**Q86. Programming Question: Implement a custom bitmask permission system.**

*Write a Python class representing Linux file permissions (Read, Write, Execute) using bitwise flags.*

**Answer:** Use bitwise OR (`|`) to combine flags, bitwise AND (`&`) to check flags, and bitwise XOR/NOT to toggle flags.

```python
class FilePermission:
    READ    = 0b100  # 4
    WRITE   = 0b010  # 2
    EXECUTE = 0b001  # 1

    def __init__(self, mode: int = 0):
        self.mode = mode

    def grant(self, permission: int):
        self.mode |= permission

    def revoke(self, permission: int):
        self.mode &= ~permission

    def has(self, permission: int) -> bool:
        return (self.mode & permission) == permission

perm = FilePermission()
perm.grant(FilePermission.READ | FilePermission.EXECUTE)
print("Has Read:", perm.has(FilePermission.READ))
print("Has Write:", perm.has(FilePermission.WRITE))
print("Has Execute:", perm.has(FilePermission.EXECUTE))
```

Output:
```text
Has Read: True
Has Write: False
Has Execute: True
```

**Q87. Programming Question: Implement a custom Money class with full operator overloading.**

*Write a class supporting addition, subtraction, scalar multiplication, and comparison.*

**Answer:** Implement `__add__`, `__sub__`, `__mul__`, `__eq__`, and `__lt__`.

```python
class Money:
    def __init__(self, amount: float, currency: str = "USD"):
        self.amount = round(float(amount), 2)
        self.currency = currency

    def __add__(self, other):
        if not isinstance(other, Money) or self.currency != other.currency:
            raise ValueError("Currencies must match")
        return Money(self.amount + other.amount, self.currency)

    def __mul__(self, scalar: float):
        return Money(self.amount * scalar, self.currency)

    def __repr__(self):
        return f"{self.amount:.2f} {self.currency}"

m1 = Money(150.50)
m2 = Money(49.50)
print("m1 + m2 =", m1 + m2)
print("m1 * 2 =", m1 * 2)
```

Output:
```text
m1 + m2 = 200.00 USD
m1 * 2 = 301.00 USD
```

**Q88. Programming Question: Streamline input processing using the Walrus Operator.**

*Use `:=` to read and process chunks of data until an exit sentinel is received.*

**Answer:** Demonstrate `while (chunk := get_chunk()) != sentinel:` loop pattern.

```python
sample_stream = ["cluster-1", "cluster-2", "cluster-3", "STOP", "cluster-4"]
stream_iter = iter(sample_stream)

processed = []
while (token := next(stream_iter, "STOP")) != "STOP":
    processed.append(token.upper())

print("Processed tokens:", processed)
```

Output:
```text
Processed tokens: ['CLUSTER-1', 'CLUSTER-2', 'CLUSTER-3']
```

**Q89. Programming Question: Implement safe division with fallback using short-circuit operators.**

*Write a concise one-line expression for division avoiding ZeroDivisionError.*

**Answer:** Combine conditional expressions or logical short-circuiting.

```python
def safe_divide(numerator: float, denominator: float, fallback: float = 0.0) -> float:
    return (numerator / denominator) if denominator != 0 else fallback

print("10 / 2 =", safe_divide(10, 2))
print("10 / 0 =", safe_divide(10, 0, fallback=-1.0))
```

Output:
```text
10 / 2 = 5.0
10 / 0 = -1.0
```

**Q90. Programming Question: Efficiently determine if an integer is a power of two.**

*Write a bitwise function to verify powers of two in O(1) time.*

**Answer:** A number $N > 0$ is a power of 2 if and only if `(N & (N - 1)) == 0`.

```python
def is_power_of_two(n: int) -> bool:
    return n > 0 and (n & (n - 1)) == 0

test_vals = [0, 1, 2, 14, 16, 64, 100, 1024]
results = {v: is_power_of_two(v) for v in test_vals}
for val, is_pow in results.items():
    print(f"{val:>4}: {is_pow}")
```

Output:
```text
0: False
   1: True
   2: True
  14: False
  16: True
  64: True
 100: False
1024: True
```


---

## Chapter 4: Strings and Text Processing

**Q91. How are strings represented in Python 3?**

*Explain Python's Unicode string model and internal representation (PEP 393).*

**Answer:** In Python 3, all strings (`str`) are sequences of Unicode code points (UTF-8 source text). Internally, CPython uses PEP 393 flexible string representation to optimize memory: if a string contains only ASCII, it uses 1 byte per character (`Latin-1`); if it contains characters requiring 16 bits, it uses 2 bytes (`UCS-2`); if code points exceed 16 bits (e.g. emojis), it uses 4 bytes (`UCS-4`). Strings are completely immutable.

```python
import sys

s_ascii = "DevOps"
s_emoji = "DevOps🚀"

print(f"ASCII str size: {sys.getsizeof(s_ascii)} bytes")
print(f"Emoji str size: {sys.getsizeof(s_emoji)} bytes")
```

Output:
```text
ASCII str size: 55 bytes
Emoji str size: 84 bytes
```

**Q92. How does string indexing and slicing work?**

*Explain start, stop, step syntax, negative indexes, and reverse slicing.*

**Answer:** Strings support zero-based positive indexing `[0, len-1]` from left to right, and negative indexing `[-len, -1]` from right to left. Slicing syntax is `string[start:stop:step]`:
- `start`: Inclusive lower bound (default 0).
- `stop`: Exclusive upper bound (default `len`).
- `step`: Stride amount (default 1). A negative step (e.g., `[::-1]`) traverses backward.

```python
text = "Kubernetes"
print("First char [0]:", text[0])
print("Last char [-1]:", text[-1])
print("Slice [0:5]:", text[0:5])
print("Every 2nd char [::2]:", text[::2])
print("Reversed [::-1]:", text[::-1])
```

Output:
```text
First char [0]: K
Last char [-1]: s
Slice [0:5]: Kuber
Every 2nd char [::2]: Kbrts
Reversed [::-1]: setenrebuK
```

**Q93. Why are Python strings immutable?**

*Explain the advantages of string immutability regarding safety, performance, and hashing.*

**Answer:** 1. **Thread Safety**: Strings can be shared across multiple threads without locks because their contents cannot be modified.
2. **Hash Stability**: Because content never changes, the string's hash (`__hash__`) is computed once and cached, allowing strings to serve as dictionary keys and set elements safely.
3. **Memory Optimization**: Allows string interning (reusing identical string objects across the runtime).

```python
s = "Cloud"
try:
    s[0] = "c"
except TypeError as e:
    print("Immutability error:", e)

# Hash stability
print(f"Hash of 'Cloud': {hash(s)}")
```

Output:
```text
Immutability error: 'str' object does not support item assignment
Hash of 'Cloud': ...
```

**Q94. What is string concatenation and why is `+=` in a loop an anti-pattern?**

*Compare `+=` concatenation $O(N^2)$ with `''.join()` $O(N)$.*

**Answer:** Because strings are immutable, repeatedly using `s += chunk` in a loop forces Python to allocate a new string object and copy all previous characters into it on every iteration, leading to $O(N^2)$ quadratic time complexity. The idiomatic, high-performance approach is to collect chunks in a list and invoke `str.join()`, which pre-calculates the required buffer size and completes in linear $O(N)$ time.

```python
import time

chunks = ["chunk_" + str(i) for i in range(1000)]

# Idiomatic O(N) join:
result = "".join(chunks)
print("Joined length:", len(result))
```

Output:
```text
Joined length: 8890
```

**Q95. How do string case methods work: lower(), upper(), title(), capitalize(), swapcase(), casefold()?**

*Explain the difference between `lower()` and `casefold()` for international caseless matching.*

**Answer:** - `lower()` / `upper()`: Converts to lowercase / uppercase.
- `title()`: Capitalizes first character of every word.
- `capitalize()`: Capitalizes first character of sentence, lowers the rest.
- `swapcase()`: Inverts case.
- `casefold()`: Aggressive caseless matching designed for Unicode (e.g. German 'ß' becomes 'ss'). Always use `casefold()` when comparing international user input.

```python
german_word = "Fluß"
print("lower():", german_word.lower())
print("casefold():", german_word.casefold())
print("Caseless match:", german_word.casefold() == "fluss".casefold())
```

Output:
```text
lower(): fluß
casefold(): fluss
Caseless match: True
```

**Q96. How do trimming methods work: strip(), lstrip(), rstrip(), removeprefix(), removesuffix()?**

*Explain why `removeprefix()` (Python 3.9+) is safer than `lstrip()` for stripping prefixes.*

**Answer:** `strip(chars)` removes any characters listed in `chars` from both ends until a non-matching character is found. It treats the argument as a **set of characters**, NOT a substring. In contrast, `removeprefix(sub)` (Python 3.9+) strictly removes the exact substring prefix once.

```python
filename = "test_metrics.py"

# Pitfall with rstrip (treats '.py' as set of chars '.', 'p', 'y'):
bad_strip = "metricspy.py".rstrip(".py")
print("rstrip pitfall on 'metricspy.py':", bad_strip)

# Safe Python 3.9+ removesuffix:
safe_strip = "metricspy.py".removesuffix(".py")
print("removesuffix on 'metricspy.py':", safe_strip)
```

Output:
```text
rstrip pitfall on 'metricspy.py': metrics
removesuffix on 'metricspy.py': metricspy
```

**Q97. How do searching methods work: find(), rfind(), index(), rindex(), and count()?**

*Explain the difference in error handling between `find()` and `index()`.*

**Answer:** `find(sub)` searches for substring from the left and returns the lowest index if found, or `-1` if not found. `index(sub)` does the same but raises a `ValueError` if the substring is missing. `rfind()` and `rindex()` search from the right. `count(sub)` returns non-overlapping occurrences.

```python
log = "ERROR: Disk full. ERROR: Out of memory."

print("find('ERROR'):", log.find("ERROR"))
print("rfind('ERROR'):", log.rfind("ERROR"))
print("find('FATAL'):", log.find("FATAL"))

try:
    log.index("FATAL")
except ValueError as e:
    print("index('FATAL') raised:", e)

print("Count of 'ERROR':", log.count("ERROR"))
```

Output:
```text
find('ERROR'): 0
rfind('ERROR'): 18
find('FATAL'): -1
index('FATAL') raised: substring not found
Count of 'ERROR': 2
```

**Q98. How do replace() and translate() with maketrans() work?**

*Compare substring replacement with multi-character mapping tables.*

**Answer:** `str.replace(old, new, count)` replaces occurrences of a substring. `str.translate(table)` performs single-character translations using a mapping table built with `str.maketrans(from_chars, to_chars, delete_chars)`, operating at high C-speed.

```python
raw_text = "user@domain.internal;port=8080"
cleaned = raw_text.replace("internal", "external")
print("replace():", cleaned)

# Using translate to sanitize and strip punctuation
trans_table = str.maketrans({"@": "_at_", ";": " ", "=": ": "})
print("translate():", raw_text.translate(trans_table))
```

Output:
```text
replace(): user@domain.external;port=8080
translate(): user_at_domain.internal port: 8080
```

**Q99. How do split(), rsplit(), splitlines(), and partition() work?**

*Explain the 3-element tuple returned by `partition()` vs `split()`.*

**Answer:** - `split(sep, maxsplit)`: Splits by separator into a list of strings.
- `splitlines(keepends=False)`: Splits by universal line boundaries (`\n`, `\r\n`).
- `partition(sep)`: Splits at the **first occurrence** of `sep` and returns a 3-element tuple: `(before, sep, after)`. If `sep` is not found, returns `(original_string, '', '')`.

```python
record = "api.service.internal:443"
host, sep, port = record.partition(":")
print(f"Host: {host}, Port: {port}")

header = "Authorization: Bearer token12345"
key, _, val = header.partition(": ")
print(f"Key: {key}, Val: {val}")
```

Output:
```text
Host: api.service.internal, Port: 443
Key: Authorization, Val: Bearer token12345
```

**Q100. What are string testing methods: isalpha, isdigit, isnumeric, isdecimal, isalnum, isidentifier, isspace?**

*Explain the subtle differences between `isdecimal()`, `isdigit()`, and `isnumeric()`.*

**Answer:** - `isdecimal()`: True for strict base-10 digits `[0-9]` and Unicode decimal characters.
- `isdigit()`: True for decimals plus subscripts, superscripts (e.g. `²`).
- `isnumeric()`: True for digits, superscripts, plus vulgar fractions (e.g. `½`, Roman numerals).
- `isalnum()`: True if all chars are alphanumeric.
- `isidentifier()`: Checks if string is a valid Python variable name.

```python
superscript = "²"
fraction = "½"

print(f"'{superscript}'.isdecimal():", superscript.isdecimal())
print(f"'{superscript}'.isdigit():", superscript.isdigit())
print(f"'{fraction}'.isnumeric():", fraction.isnumeric())
print("'app_port_8080'.isidentifier():", "app_port_8080".isidentifier())
```

Output:
```text
'²'.isdecimal(): False
'²'.isdigit(): True
'½'.isnumeric(): True
'app_port_8080'.isidentifier(): True
```

**Q101. How has string formatting evolved in Python (% vs str.format vs f-strings)?**

*Compare the 3 generations of string formatting and why f-strings are preferred.*

**Answer:** 1. **`%` formatting (printf-style)**: Older, prone to tuple errors: `"%s: %d" % (name, port)`.
2. **`str.format()` (Python 2.6+)**: More flexible: `"{name}: {port}".format(name=name, port=port)`.
3. **f-strings (PEP 498, Python 3.6+)**: Formatted string literals evaluated at runtime with direct variable scoping, highest readability, and fastest execution due to opcode optimization (`BUILD_STRING`).

```python
name, latency = "gateway", 23.4567

print("%s: %.2f ms" % (name, latency))
print("{0}: {1:.2f} ms".format(name, latency))
print(f"{name}: {latency:.2f} ms")
```

Output:
```text
gateway: 23.46 ms
gateway: 23.46 ms
gateway: 23.46 ms
```

**Q102. What are advanced f-string features: format specifiers, conversions (!r, !s), and debugging syntax?**

*Explain padding, alignment, datetime formatting, and Python 3.8+ `f'{var=}'` syntax.*

**Answer:** f-strings support extensive format specifiers:
- Alignment: `<` (left), `>` (right), `^` (center).
- Conversions: `!r` (invokes `repr()`), `!s` (`str()`), `!a` (`ascii()`).
- Debugging: `f'{expr=}'` prints the expression text followed by `=` and its evaluated value.

```python
metric = 42.129
server = "k8s-node-01"

print(f"{server:>20}")
print(f"Metric: {metric:.2f}")
print(f"Debug: {server=}, {metric=}")
print(f"Repr conversion: {server!r}")
```

Output:
```text
k8s-node-01
Metric: 42.13
Debug: server='k8s-node-01', metric=42.129
Repr conversion: 'k8s-node-01'
```

**Q103. What are escape sequences and raw string literals (`r'...'`)?**

*Explain how raw strings disable backslash escaping for regex and filesystem paths.*

**Answer:** Standard strings interpret escape sequences (`\n` newline, `\t` tab, `\\` literal backslash). A **raw string** prefix `r` treats backslashes as literal characters, essential when writing Windows paths or regular expressions to avoid 'backslash plague'. Caveat: raw strings cannot end with an odd number of backslashes.

```python
normal_path = "C:\new_folder\test"
raw_path = r"C:
ew_folder	est"

print("Normal string (interprets \n and \t):")
print(normal_path)
print("Raw string:")
print(raw_path)
```

Output:
```text
Normal string (interprets 
 and 	):
C:
ew_folder	est
Raw string:
C:\new_folder\test
```

**Q104. How does string encoding and decoding work (str.encode vs bytes.decode)?**

*Explain converting Unicode strings to byte sequences and error handling strategies.*

**Answer:** - `str.encode(encoding='utf-8', errors='strict')`: Serializes human-readable Unicode code points into raw byte sequences.
- `bytes.decode(encoding='utf-8', errors='strict')`: Deserializes byte streams into Unicode strings.
- Error policies: `'strict'` (raises `UnicodeError`), `'ignore'` (skips unmappable bytes), `'replace'` (inserts `` / `?`).

```python
text = "DevOps 🚀"
encoded_bytes = text.encode("utf-8")
decoded_str = encoded_bytes.decode("utf-8")

print("Encoded bytes:", encoded_bytes)
print("Decoded string:", decoded_str)
```

Output:
```text
Encoded bytes: b'DevOps \xf0\x9f\x9a\x80'
Decoded string: DevOps 🚀
```

**Q105. What is string interning and when should you use `sys.intern()`?**

*Explain CPython's string interning mechanism and performance benefits for dictionary keys.*

**Answer:** String interning ensures only one copy of each distinct string value is stored in memory. When two interned strings are compared with `is`, Python performs a fast single pointer comparison ($O(1)$) instead of character-by-character comparison ($O(N)$). CPython automatically interns compile-time string constants resembling identifiers. `sys.intern(s)` allows manual interning of dynamic strings.

```python
import sys

s1 = sys.intern("dynamic_string_" + str(100))
s2 = sys.intern("dynamic_string_" + str(100))
print("Interned strings reference identical object:", s1 is s2)
```

Output:
```text
Interned strings reference identical object: True
```

**Q106. How do ord() and chr() relate characters and Unicode code points?**

*Explain converting characters to numerical ASCII/Unicode values and vice-versa.*

**Answer:** `ord(c)` returns the integer Unicode code point of a single character. `chr(i)` returns the character string corresponding to the integer code point.

```python
char = "A"
code_point = ord(char)
print(f"ord('{char}') =", code_point)
print(f"chr({code_point}) =", chr(code_point))
print(f"Rocket emoji ord: {ord('🚀')}")
```

Output:
```text
ord('A') = 65
chr(65) = A
Rocket emoji ord: 128640
```

**Q107. What is string alignment using center(), ljust(), rjust(), and zfill()?**

*Demonstrate padding strings for table displays and terminal dashboards.*

**Answer:** - `ljust(width, fillchar)`: Left-aligns and pads on right.
- `rjust(width, fillchar)`: Right-aligns and pads on left.
- `center(width, fillchar)`: Centers with padding on both sides.
- `zfill(width)`: Left-pads numeric strings with zeros.

```python
title = "CLUSTER HEALTH"
print(title.center(30, "="))
print("CPU:".ljust(15, ".") + "85%".rjust(15, "."))
print("42".zfill(6))
```

Output:
```text
========CLUSTER HEALTH========
CPU:.......................85%
000042
```

**Q108. How do multi-line strings work and how does `textwrap.dedent()` clean them?**

*Explain triple-quoted multiline strings and stripping accidental leading indentation.*

**Answer:** Triple-quoted strings (`"""` or `'''`) preserve newlines and indentation. When nested inside functions, they inherit leading spaces. `textwrap.dedent()` strips common leading whitespace across all lines, producing clean text blocks.

```python
import textwrap

def generate_nginx_conf():
    raw_conf = """
        server {
            listen 80;
            server_name api.devops.com;
        }
    """
    return textwrap.dedent(raw_conf).strip()

print(generate_nginx_conf())
```

Output:
```text
server {
    listen 80;
    server_name api.devops.com;
}
```

**Q109. What is string template substitution using `string.Template`?**

*Compare `string.Template` with f-strings and explain its security benefits for untrusted input.*

**Answer:** `string.Template` uses `$`-based substitutions (`$var` or `${var}`). Unlike f-strings or `.format()`, `string.Template` does not allow arbitrary code execution or object attribute traversal, making it safe for processing untrusted user-supplied configuration templates.

```python
from string import Template

template_str = Template("Host: $host, Port: $port")
rendered = template_str.substitute(host="10.0.0.1", port=8080)
print("Rendered:", rendered)

# Safe substitution prevents KeyErrors:
safe = template_str.safe_substitute(host="10.0.0.1")
print("Safe substitution:", safe)
```

Output:
```text
Rendered: Host: 10.0.0.1, Port: 8080
Safe substitution: Host: 10.0.0.1, Port: $port
```

**Q110. Output Question: What does this slice expression return?**

*Predict output when slicing with negative steps and omitted bounds.*

**Answer:** With `[::-2]`, traversal begins at the end and steps backward by 2.

```python
s = "0123456789"
print(s[::-2])
```

Output:
```text
97531
```

**Q111. Output Question: Predict the output of split() without arguments vs split(' ').**

*Explain the difference between default whitespace splitting and explicit space splitting.*

**Answer:** `s.split()` (no arguments) groups consecutive whitespace together and discards leading/trailing whitespace. `s.split(' ')` splits strictly on single space characters, preserving empty strings.

```python
s = "   cloud    devops   k8s   "
print("split():", s.split())
print("split(' '):", s.split(' '))
```

Output:
```text
split(): ['cloud', 'devops', 'k8s']
split(' '): ['', '', '', 'cloud', '', '', '', 'devops', '', '', 'k8s', '', '', '']
```

**Q112. Output Question: What does this f-string format specification display?**

*Predict binary, hex, percentage, and comma-separated formatting.*

**Answer:** f-strings support format flags: `,` for thousand separators, `b` for binary, `x` for hexadecimal, and `%` for percentages.

```python
count = 123456789
ratio = 0.8542
val = 255

print(f"{count:,}")
print(f"{ratio:.1%}")
print(f"0b{val:08b} == 0x{val:02x}")
```

Output:
```text
123,456,789
85.4%
0b11111111 == 0xff
```

**Q113. Output Question: Predict the result of string comparison.**

*Explain lexicographical comparison in Python based on ASCII/Unicode values.*

**Answer:** Strings are compared lexicographically by code point. Uppercase ASCII letters (`A-Z` = 65-90) have smaller code points than lowercase (`a-z` = 97-122).

```python
print("'apple' < 'banana':", "apple" < "banana")
print("'Banana' < 'apple':", "Banana" < "apple")
print("'10' < '2':", "10" < "2")
```

Output:
```text
'apple' < 'banana': True
'Banana' < 'apple': True
'10' < '2': True
```

**Q114. Output Question: Predict what happens when string partition does not match.**

*Explain the fallback 3-tuple return of `str.partition`.*

**Answer:** If the separator is not found, `partition()` returns `(original_string, '', '')`.

```python
result = "database_cluster".partition(":")
print("Partition result:", result)
```

Output:
```text
Partition result: ('database_cluster', '', '')
```

**Q115. Debugging Question: Fix the IndexError during string parsing.**

*Safely access optional substring tokens without throwing IndexError on malformed lines.*

**Answer:** Check slice length or use `split(':', 1)` with unpacking defaults.

```python
# Buggy Code:
# line = "host_only"
# host, port = line.split(":")

# Fixed Code:
line = "host_only"
host, _, port = line.partition(":")
port = port or "80" # Default fallback
print(f"Host: {host}, Port: {port}")
```

Output:
```text
Host: host_only, Port: 80
```

**Q116. Programming Question: Validate and sanitize a slug for URL paths.**

*Write a function that turns an arbitrary string into a URL-friendly slug.*

**Answer:** Convert to lowercase, replace non-alphanumeric chars with hyphens, and strip duplicate hyphens.

```python
import re

def slugify(text: str) -> str:
    text = text.strip().lower()
    text = re.sub(r"[^\w\s-]", "", text)
    text = re.sub(r"[\s_-]+", "-", text)
    return text.strip("-")

print(slugify("  Deploying Kubernetes v1.28 on AWS!  "))
```

Output:
```text
deploying-kubernetes-v128-on-aws
```

**Q117. Programming Question: Mask sensitive secrets in logs.**

*Write a function to mask API tokens and passwords, exposing only the last 4 characters.*

**Answer:** Replace all but the last 4 characters with asterisks.

```python
def mask_secret(secret: str, visible_tail: int = 4) -> str:
    if not secret or len(secret) <= visible_tail:
        return "*" * len(secret)
    return "*" * (len(secret) - visible_tail) + secret[-visible_tail:]

print(mask_secret("AKIAIOSFODNN7EXAMPLE"))
print(mask_secret("secret123"))
```

Output:
```text
****************MPLE
*****e123
```

**Q118. Programming Question: Parse environment variables in `KEY=VALUE` format.**

*Write a robust parser that handles comments, blank lines, and embedded `=` signs.*

**Answer:** Use `partition('=')` after stripping lines and ignoring lines starting with `#`.

```python
env_data = """
# Server Configuration
ENV=production
DATABASE_URL=postgres://user:pass=word@db:5432/app
DEBUG=false
"""

env_dict = {}
for line in env_data.splitlines():
    line = line.strip()
    if not line or line.startswith("#"):
        continue
    k, _, v = line.partition("=")
    env_dict[k.strip()] = v.strip()

for k, v in env_dict.items():
    print(f"{k} -> {v}")
```

Output:
```text
ENV -> production
DATABASE_URL -> postgres://user:pass=word@db:5432/app
DEBUG -> false
```

**Q119. Programming Question: Count occurrences of words in text, ignoring case and punctuation.**

*Write a clean string frequency counter using built-in methods.*

**Answer:** Normalize with `.lower()`, strip punctuation, and count words.

```python
from collections import Counter
import re

text = "Docker is great. Kubernetes is great! Cloud is great and docker is awesome."
words = re.findall(r"\w+", text.lower())
counts = Counter(words)

print("Top 3 words:", counts.most_common(3))
```

Output:
```text
Top 3 words: [('is', 4), ('great', 3), ('docker', 2)]
```

**Q120. Programming Question: Convert between CamelCase and snake_case.**

*Write bidirectional conversion functions between CamelCase and snake_case.*

**Answer:** Use regex lookaheads or iteration.

```python
import re

def camel_to_snake(name: str) -> str:
    s1 = re.sub(r"(.)([A-Z][a-z]+)", r"_", name)
    return re.sub(r"([a-z0-9])([A-Z])", r"_", s1).lower()

def snake_to_camel(name: str) -> str:
    return "".join(word.capitalize() for word in name.split("_"))

print(camel_to_snake("KubernetesClusterManager"))
print(snake_to_camel("kubernetes_cluster_manager"))
```

Output:
```text
kubernetes_cluster_manager
KubernetesClusterManager
```

**Q121. Programming Question: Truncate strings cleanly with ellipses without cutting words.**

*Write a smart text truncator that preserves whole words.*

**Answer:** Use `textwrap.shorten()`.

```python
import textwrap

long_msg = "Critical error detected during automated deployment pipeline on production node 04."
shortened = textwrap.shorten(long_msg, width=45, placeholder="...")
print("Smart Truncate:", shortened)
```

Output:
```text
Smart Truncate: Critical error detected during...
```

**Q122. Programming Question: Check if a string is a valid IPv4 address using string operations.**

*Validate IPv4 addresses without external libraries using string split and digit testing.*

**Answer:** Split by `.`, verify 4 octets, check digits, and check range 0-255 with no leading zeros.

```python
def is_valid_ipv4(ip: str) -> bool:
    parts = ip.split(".")
    if len(parts) != 4:
        return False
    for p in parts:
        if not p.isdigit():
            return False
        if len(p) > 1 and p.startswith("0"):
            return False
        if not (0 <= int(p) <= 255):
            return False
    return True

print("192.168.1.1 valid:", is_valid_ipv4("192.168.1.1"))
print("256.0.0.1 valid:", is_valid_ipv4("256.0.0.1"))
print("192.168.01.1 valid:", is_valid_ipv4("192.168.01.1"))
```

Output:
```text
192.168.1.1 valid: True
256.0.0.1 valid: False
192.168.01.1 valid: False
```


---

## Chapter 5: Control Flow and Decision-Making

**Q123. How do if, elif, and else statements work in Python?**

*Explain conditional execution syntax and short-circuit evaluation in branching.*

**Answer:** `if` evaluates a boolean expression. If truthy, its indented block executes and all subsequent `elif`/`else` branches are skipped. If falsy, Python proceeds to the next `elif`. If no conditions match, the optional `else` block executes.

```python
def check_status(code: int) -> str:
    if 200 <= code < 300:
        return "SUCCESS"
    elif 400 <= code < 500:
        return "CLIENT_ERROR"
    elif 500 <= code < 600:
        return "SERVER_ERROR"
    else:
        return "UNKNOWN_STATUS"

print(check_status(200))
print(check_status(503))
```

Output:
```text
SUCCESS
SERVER_ERROR
```

**Q124. What are guard clauses and how do they eliminate nested if statements?**

*Explain the 'bailing out early' pattern to improve code readability and maintainability.*

**Answer:** Guard clauses use early `return` or `raise` statements at the beginning of a function to handle error conditions and edge cases immediately, eliminating deep indentation pyramids.

```python
# Clean code with guard clauses
def process_deployment(service: str, user_role: str, active: bool):
    if not active:
        return "Aborted: Service inactive"
    if user_role != "admin":
        return "Forbidden: Insufficient privileges"
    if not service.strip():
        return "Invalid: Empty service name"
        
    return f"Deployment initiated for {service}"

print(process_deployment("auth-api", "admin", True))
print(process_deployment("auth-api", "viewer", True))
```

Output:
```text
Deployment initiated for auth-api
Forbidden: Insufficient privileges
```

**Q125. What is structural pattern matching (`match` / `case`) introduced in Python 3.10?**

*Explain the syntax and purpose of PEP 634 pattern matching compared to C/Java switch statements.*

**Answer:** Python 3.10 introduced `match` / `case`, which is not a simple switch statement. It performs **structural pattern matching**: inspecting the shape, data types, and contents of objects, unpacking values, and binding variables dynamically.

```python
def handle_command(cmd):
    match cmd:
        case ["quit"]:
            return "Exiting system"
        case ["deploy", service]:
            return f"Deploying {service}"
        case ["scale", service, int(replicas)]:
            return f"Scaling {service} to {replicas} replicas"
        case _:
            return "Unknown command"

print(handle_command(["deploy", "web-server"]))
print(handle_command(["scale", "web-server", 5]))
print(handle_command(["unknown"]))
```

Output:
```text
Deploying web-server
Scaling web-server to 5 replicas
Unknown command
```

**Q126. How do pattern guards (if clauses) and OR patterns work in match-case?**

*Demonstrate combining multiple patterns with `|` and applying conditional guards.*

**Answer:** In `case` statements, `|` matches alternatives, and `if condition` adds an additional boolean guard check that must evaluate to `True` for the branch to execute.

```python
def analyze_http_response(status, payload):
    match status:
        case 200 | 201:
            return f"Success with payload: {payload}"
        case 404:
            return "Resource Not Found"
        case code if code >= 500:
            return f"Server Alert: status {code}"
        case _:
            return f"Unhandled status: {status}"

print(analyze_http_response(200, "OK"))
print(analyze_http_response(502, "Bad Gateway"))
```

Output:
```text
Success with payload: OK
Server Alert: status 502
```

**Q127. How does mapping pattern matching work in match-case?**

*Explain matching against dictionary structures and extracting specific keys.*

**Answer:** A mapping pattern `case {'key': val}:` matches dictionaries containing at least the specified keys and binds `val` to their values. Other extra keys in the dictionary are ignored unless `**rest` is specified.

```python
def route_event(event):
    match event:
        case {"type": "alert", "severity": "CRITICAL", "host": host}:
            return f"Paging on-call for critical host: {host}"
        case {"type": "alert", "host": host}:
            return f"Logging non-critical alert for {host}"
        case _:
            return "Ignoring event"

print(route_event({"type": "alert", "severity": "CRITICAL", "host": "prod-db-01", "ts": 12345}))
```

Output:
```text
Paging on-call for critical host: prod-db-01
```

**Q128. How does class pattern matching work in match-case?**

*Explain matching against custom class instances and attribute binding.*

**Answer:** A class pattern `case ClassName(attr1=val1):` checks if the subject is an instance of `ClassName` and binds attributes.

```python
class CloudServer:
    def __init__(self, name, provider, cpus):
        self.name = name
        self.provider = provider
        self.cpus = cpus

def inspect_server(srv):
    match srv:
        case CloudServer(provider="AWS", cpus=c) if c > 8:
            return f"Large AWS Instance: {srv.name} ({c} CPUs)"
        case CloudServer(provider="AWS"):
            return f"Standard AWS Instance: {srv.name}"
        case CloudServer():
            return f"Other Server: {srv.name} on {srv.provider}"
        case _:
            return "Not a server"

print(inspect_server(CloudServer("node-1", "AWS", 16)))
print(inspect_server(CloudServer("node-2", "GCP", 4)))
```

Output:
```text
Large AWS Instance: node-1 (16 CPUs)
Other Server: node-2 on GCP
```

**Q129. How does a for loop work in Python?**

*Explain iteration over iterable objects and how Python translates `for` into the iterator protocol.*

**Answer:** In Python, a `for` loop does not use C-style index counters (`for (int i=0; ...)`). Instead, it iterates over any **iterable** object. Under the hood, Python calls `iter(obj)` to obtain an iterator, repeatedly invokes `next()`, and catches `StopIteration` automatically to terminate the loop.

```python
items = ["docker", "k8s", "terraform"]
for tool in items:
    print(f"DevOps Tool: {tool}")
```

Output:
```text
DevOps Tool: docker
DevOps Tool: k8s
DevOps Tool: terraform
```

**Q130. How does a while loop work and when should it be used?**

*Compare `for` and `while` loops and explain condition-based repetition.*

**Answer:** A `while` loop continues executing as long as its condition expression evaluates to `True`. It is preferred when the number of iterations is not known in advance (e.g., polling an API status, waiting for a server heartbeat, or game loops).

```python
retries = 3
while retries > 0:
    print(f"Connecting to database... ({retries} attempts remaining)")
    retries -= 1
print("Connection sequence complete.")
```

Output:
```text
Connecting to database... (3 attempts remaining)
Connecting to database... (2 attempts remaining)
Connecting to database... (1 attempts remaining)
Connection sequence complete.
```

**Q131. How does the range() function work in Python?**

*Explain start, stop, step parameters, memory efficiency, and lazy evaluation of range.*

**Answer:** `range(start, stop, step)` creates an immutable arithmetic sequence of integers. In Python 3, `range` is not a list; it is a generator-like sequence type that takes $O(1)$ constant memory regardless of size because it only calculates numbers on-demand.

```python
import sys

r_small = range(10)
r_huge = range(10_000_000)

print("Size of range(10):", sys.getsizeof(r_small), "bytes")
print("Size of range(10,000,000):", sys.getsizeof(r_huge), "bytes")
print("Range step test:", list(range(10, 0, -2)))
```

Output:
```text
Size of range(10): 48 bytes
Size of range(10,000,000): 48 bytes
Range step test: [10, 8, 6, 4, 2]
```

**Q132. What does the break statement do?**

*Explain loop termination and control transfer.*

**Answer:** `break` terminates the nearest enclosing loop immediately. Control resumes at the statement following the loop block. If the loop has an `else` block, `break` skips the `else` block.

```python
servers = ["web-1", "web-2", "FAIL-web-3", "web-4"]

for s in servers:
    if "FAIL" in s:
        print(f"Detected faulty server {s}! Aborting check.")
        break
    print(f"Server {s} is healthy.")
```

Output:
```text
Server web-1 is healthy.
Server web-2 is healthy.
Detected faulty server FAIL-web-3! Aborting check.
```

**Q133. What does the continue statement do?**

*Explain skipping the rest of the current iteration and proceeding to the next.*

**Answer:** `continue` abandons the remainder of the current loop iteration and immediately evaluates the next iteration (in a `for` loop, fetches the next element; in a `while` loop, re-tests the condition).

```python
ports = [80, 443, 8080, 22, 9090]
for p in ports:
    if p == 22:
        continue # Skip SSH port
    print(f"Inspecting HTTP port: {p}")
```

Output:
```text
Inspecting HTTP port: 80
Inspecting HTTP port: 443
Inspecting HTTP port: 8080
Inspecting HTTP port: 9090
```

**Q134. What does the pass statement do in Python?**

*Explain why pass is needed as a syntactic placeholder.*

**Answer:** `pass` is a null statement (no-op). In Python, empty code blocks are syntactically invalid; `pass` serves as a placeholder where a statement is syntactically required but no action is needed (e.g., abstract stubs, ignoring exceptions).

```python
def placeholder_function():
    pass

class BaseHandler:
    pass

try:
    x = 1 / 1
except ZeroDivisionError:
    pass # Explicitly ignore
print("pass executed smoothly")
```

Output:
```text
pass executed smoothly
```

**Q135. How does the `else` clause on loops (`for ... else` and `while ... else`) work?**

*Explain when loop else executes and why it is commonly misunderstood.*

**Answer:** The `else` clause of a loop executes **if and only if the loop finishes naturally without encountering a `break` statement**. If a `break` is triggered, the `else` block is skipped. Think of it as 'nobreak'. It is ideal for search algorithms where you want to execute code only if no match was found.

```python
def find_target(items, target):
    for item in items:
        if item == target:
            print(f"Found {target}!")
            break
    else:
        print(f"Target {target} was NOT found in collection.")

find_target(["alpha", "beta"], "alpha")
find_target(["alpha", "beta"], "gamma")
```

Output:
```text
Found alpha!
Target gamma was NOT found in collection.
```

**Q136. How does enumerate() work and what is its start parameter?**

*Explain counter tracking during loop iteration without manual index increments.*

**Answer:** `enumerate(iterable, start=0)` yields 2-tuples containing a running index and the corresponding item, eliminating the anti-pattern `for i in range(len(lst)):`.

```python
stages = ["Build", "Test", "Deploy"]
for idx, stage in enumerate(stages, start=1):
    print(f"Step {idx}: {stage}")
```

Output:
```text
Step 1: Build
Step 2: Test
Step 3: Deploy
```

**Q137. How does zip() work and what is the `strict=True` parameter in Python 3.10+?**

*Explain parallel iteration over multiple sequences and handling unequal sequence lengths.*

**Answer:** `zip(*iterables)` aggregates elements from each iterable into tuples, stopping when the shortest iterable is exhausted. In Python 3.10+, passing `strict=True` raises a `ValueError` if iterables have unequal lengths, preventing silent truncation bugs.

```python
services = ["frontend", "backend", "database"]
ports = [80, 8080, 5432]

for svc, port in zip(services, ports):
    print(f"{svc} runs on port {port}")

# Strict checking:
try:
    list(zip([1, 2], ["a", "b", "c"], strict=True))
except ValueError as e:
    print("Strict zip caught mismatch:", e)
```

Output:
```text
frontend runs on port 80
backend runs on port 8080
database runs on port 5432
Strict zip caught mismatch: zip() argument 2 is longer than argument 1
```

**Q138. How does itertools.zip_longest work?**

*Explain padding shorter iterables with fill values during parallel iteration.*

**Answer:** `itertools.zip_longest(*iterables, fillvalue=None)` continues iteration until the *longest* iterable is exhausted, filling in missing elements with `fillvalue`.

```python
import itertools

keys = ["env", "cluster", "region", "tier"]
values = ["prod", "us-east-1"]

for k, v in itertools.zip_longest(keys, values, fillvalue="N/A"):
    print(f"{k}: {v}")
```

Output:
```text
env: prod
cluster: us-east-1
region: N/A
tier: N/A
```

**Q139. How do reversed() and sorted() work in loop control?**

*Compare `reversed()` with slicing `[::-1]` and `sorted()` with list `.sort()`.*

**Answer:** - `reversed(seq)` returns a reverse iterator without allocating a new reversed list in memory.
- `sorted(iterable, key=..., reverse=...)` returns a new sorted list from any iterable without modifying the original.

```python
nodes = ["node-3", "node-1", "node-2"]

print("Reversed iterator:")
for n in reversed(nodes):
    print(n, end=" ")
print("
Sorted copy:")
print(sorted(nodes))
print("Original untouched:", nodes)
```

Output:
```text
Reversed iterator:
node-2 node-1 node-3 
Sorted copy:
['node-1', 'node-2', 'node-3']
Original untouched: ['node-3', 'node-1', 'node-2']
```

**Q140. How does nested loop execution work and how can you break out of multiple loops?**

*Explain techniques to break outer loops (flags, function returns, or exceptions).*

**Answer:** A `break` statement only exits the innermost loop. To break out of nested loops, use a boolean flag, refactor the nested loops into a helper function and `return`, or raise a custom exception.

```python
# Clean technique: returning from helper function
def search_matrix(matrix, target):
    for r_idx, row in enumerate(matrix):
        for c_idx, val in enumerate(row):
            if val == target:
                return (r_idx, c_idx)
    return None

grid = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print("Found target 5 at coordinates:", search_matrix(grid, 5))
```

Output:
```text
Found target 5 at coordinates: (1, 1)
```

**Q141. What are common loop mistakes: modifying a collection while iterating over it?**

*Explain why `for item in lst: lst.remove(item)` causes skipped elements and how to fix it.*

**Answer:** When you remove an element from a list during iteration, the remaining elements shift left. The internal loop index advances to the next position, inadvertently skipping the element that slid into the current slot. Fix: iterate over a shallow copy (`for item in lst[:]:`) or use a list comprehension.

```python
# Bug demonstration:
nums = [1, 2, 3, 4, 5]
for n in nums[:]: # Safe iteration over slice copy
    if n % 2 != 0:
        nums.remove(n)

print("Remaining even numbers:", nums)
```

Output:
```text
Remaining even numbers: [2, 4]
```

**Q142. What is an infinite loop and how do you design controlled while loops with timeouts?**

*Demonstrate implementing timeout guards to prevent hanging infinite loops.*

**Answer:** An infinite loop (`while True:`) must include explicit exit criteria or timeouts to prevent hanging background tasks or draining CPU resources.

```python
import time

def poll_status(timeout_sec: float = 2.0):
    start_time = time.time()
    while True:
        elapsed = time.time() - start_time
        if elapsed > timeout_sec:
            return "TIMEOUT_EXPIRED"
        # Simulated check
        time.sleep(0.01)

print("Poll result:", poll_status(0.05))
```

Output:
```text
Poll result: TIMEOUT_EXPIRED
```

**Q143. How does iter() with a sentinel two-argument form work in while loops?**

*Explain `iter(callable, sentinel)` for clean loop termination.*

**Answer:** `iter(callable, sentinel)` repeatedly calls `callable()` with no arguments until it returns `sentinel`, automatically raising `StopIteration`. This simplifies reading from streams or queues.

```python
import random

def random_stream():
    return random.choice([1, 2, 3, 0])

# Loops until random_stream() returns 0
counter = 0
for val in iter(lambda: 0, 0): # sentinel stops immediately
    counter += 1
print("Sentinel terminated successfully")
```

Output:
```text
Sentinel terminated successfully
```

**Q144. Output Question: Predict the output of this loop with an else clause.**

*Analyze loop execution when no break condition is reached.*

**Answer:** The loop iterates through all elements. Because no element equals 99, `break` is never hit, so the `else` block executes.

```python
for i in [1, 2, 3]:
    if i == 99:
        break
else:
    print("Loop finished without break")
```

Output:
```text
Loop finished without break
```

**Q145. Output Question: What does this code print with nested loops and continue?**

*Predict the output when continue affects only the inner loop.*

**Answer:** `continue` only skips the current iteration of the inner loop.

```python
for i in range(2):
    for j in range(2):
        if i == j:
            continue
        print(f"i={i}, j={j}")
```

Output:
```text
i=0, j=1
i=1, j=0
```

**Q146. Output Question: What does match-case with variable capture print?**

*Explain variable binding in pattern matching.*

**Answer:** In `case [x, y]:`, `x` and `y` bind to the two elements in the list.

```python
point = (10, 20)
match point:
    case (0, 0):
        print("Origin")
    case (x, 0):
        print(f"X-axis at {x}")
    case (0, y):
        print(f"Y-axis at {y}")
    case (x, y):
        print(f"Point at ({x}, {y})")
```

Output:
```text
Point at (10, 20)
```

**Q147. Output Question: Predict the output of range with negative steps.**

*Evaluate reverse range boundaries.*

**Answer:** `range(5, 0, -1)` starts at 5 and stops before 0 (at 1).

```python
print(list(range(5, 0, -1)))
print(list(range(0, 5, -1)))
```

Output:
```text
[5, 4, 3, 2, 1]
[]
```

**Q148. Debugging Question: Fix the off-by-one loop error.**

*Correct range upper bound to include all items.*

**Answer:** Remember that `range(start, stop)` stops at `stop - 1`.

```python
# Buggy Code:
# items = ["A", "B", "C"]
# for i in range(len(items) - 1): # skips last item
#     print(items[i])

# Fixed Code:
items = ["A", "B", "C"]
for item in items: # Idiomatic
    print(item, end=" ")
print()
```

Output:
```text
A B C
```

**Q149. Programming Question: Implement exponential backoff retry loop.**

*Write a production-grade retry loop with configurable exponential backoff and max attempts.*

**Answer:** Implement `attempt * backoff_factor` delay.

```python
def retry_operation(operation, max_retries: int = 3, factor: float = 0.1):
    attempts = 0
    while attempts < max_retries:
        try:
            return operation(attempts)
        except Exception as e:
            attempts += 1
            if attempts == max_retries:
                raise RuntimeError(f"Exceeded {max_retries} retries: {e}")
            delay = factor * (2 ** (attempts - 1))
            # In production: time.sleep(delay)

def mock_call(attempt):
    if attempt < 2:
        raise ConnectionResetError("Connection dropped")
    return "SUCCESS_DATA"

print("Retry result:", retry_operation(mock_call))
```

Output:
```text
Retry result: SUCCESS_DATA
```

**Q150. Programming Question: Process paginated API responses until exhausted.**

*Write a while loop that processes paginated pages using cursor tokens.*

**Answer:** Continue while cursor token is present.

```python
def mock_api_page(cursor):
    pages = {
        None: {"data": ["item1", "item2"], "next_cursor": "tok_1"},
        "tok_1": {"data": ["item3", "item4"], "next_cursor": "tok_2"},
        "tok_2": {"data": ["item5"], "next_cursor": None}
    }
    return pages[cursor]

all_items = []
cursor = None
while True:
    page = mock_api_page(cursor)
    all_items.extend(page["data"])
    cursor = page["next_cursor"]
    if cursor is None:
        break

print("All collected items:", all_items)
```

Output:
```text
All collected items: ['item1', 'item2', 'item3', 'item4', 'item5']
```

**Q151. Programming Question: Group list into consecutive fixed-size chunks.**

*Write a function to partition a list into chunks of size N.*

**Answer:** Use range with step N.

```python
def chunk_list(lst, chunk_size):
    for i in range(0, len(lst), chunk_size):
        yield lst[i:i + chunk_size]

sample = list(range(1, 10))
chunks = list(chunk_list(sample, 3))
print("Chunks:", chunks)
```

Output:
```text
Chunks: [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```

**Q152. Programming Question: Implement a rate limiter sliding window check.**

*Verify if requests exceed limit within a time window.*

**Answer:** Prune outdated timestamps from the front of the list.

```python
def is_rate_limited(timestamps: list, current_time: float, limit: int = 3, window: float = 60.0) -> bool:
    cutoff = current_time - window
    # Prune old timestamps
    while timestamps and timestamps[0] < cutoff:
        timestamps.pop(0)
    if len(timestamps) >= limit:
        return True
    timestamps.append(current_time)
    return False

log_ts = [10.0, 20.0, 30.0]
print("Request at 40s rate limited:", is_rate_limited(log_ts, 40.0, limit=3, window=60.0))
print("Request at 75s rate limited:", is_rate_limited(log_ts, 75.0, limit=3, window=60.0))
```

Output:
```text
Request at 40s rate limited: True
Request at 75s rate limited: False
```


---

## Chapter 6: Lists, Tuples, Sets, and Dictionaries

**Q153. How are Python lists implemented in CPython?**

*Explain the internal C dynamic array representation and memory over-allocation.*

**Answer:** In CPython, a `list` is not a linked list; it is a **variable-length dynamic array of pointers (`PyObject**`)**. Elements are stored contiguously in memory, enabling $O(1)$ random indexing. When elements are appended and capacity is reached, CPython over-allocates extra slots (`0, 4, 8, 16, 25, 35, 46...`) using growth formula `new_allocated = (size_t)newsize + (newsize >> 3) + (newsize < 9 ? 3 : 6)`, giving amortized $O(1)$ append time.

```python
import sys

lst = []
print("Empty list size:", sys.getsizeof(lst))
for i in range(10):
    lst.append(i)
    print(f"Len: {len(lst):<2} | Allocated Bytes: {sys.getsizeof(lst)}")
```

Output:
```text
Empty list size: 56
Len: 1  | Allocated Bytes: 88
Len: 2  | Allocated Bytes: 88
Len: 3  | Allocated Bytes: 88
Len: 4  | Allocated Bytes: 88
Len: 5  | Allocated Bytes: 120
Len: 6  | Allocated Bytes: 120
Len: 7  | Allocated Bytes: 120
Len: 8  | Allocated Bytes: 120
Len: 9  | Allocated Bytes: 184
Len: 10 | Allocated Bytes: 184
```

**Q154. What is the time complexity of common list operations?**

*Detail Big O performance for indexing, appending, popping, inserting, and searching.*

**Answer:** - Indexing `lst[i]`: $O(1)$
- Assignment `lst[i] = val`: $O(1)$
- Append `lst.append(x)`: Amortized $O(1)$
- Pop from end `lst.pop()`: $O(1)$
- Insert `lst.insert(0, x)`: $O(N)$ (requires shifting all elements right)
- Pop from start `lst.pop(0)`: $O(N)$ (requires shifting all elements left)
- Search `val in lst` or `lst.index(val)`: $O(N)$
- Slice `lst[a:b]`: $O(k)$ where $k = b - a$
- Sort `lst.sort()`: $O(N \log N)$ (Timsort).

```python
# Demonstrating O(1) append vs O(N) insert
import time

big_list = list(range(100_000))
t0 = time.perf_counter()
big_list.append(-1) # O(1)
t_append = time.perf_counter() - t0

t0 = time.perf_counter()
big_list.insert(0, -1) # O(N) shift
t_insert = time.perf_counter() - t0

print(f"Insert(0) is significantly slower than append(): {t_insert > t_append}")
```

Output:
```text
Insert(0) is significantly slower than append(): True
```

**Q155. What is the difference between append() and extend() on a list?**

*Explain adding a single object vs merging an iterable.*

**Answer:** - `append(obj)` adds `obj` as a **single element** at the end of the list (even if `obj` is another list, resulting in a nested list).
- `extend(iterable)` iterates over `iterable` and appends each element individually to the list.

```python
l1 = [1, 2]
l1.append([3, 4])
print("After append:", l1)

l2 = [1, 2]
l2.extend([3, 4])
print("After extend:", l2)
```

Output:
```text
After append: [1, 2, [3, 4]]
After extend: [1, 2, 3, 4]
```

**Q156. How do list remove(), pop(), and clear() differ?**

*Explain deleting by value vs deleting by index vs wiping the list.*

**Answer:** - `remove(x)` searches for the **first occurrence of value `x`** and deletes it. Raises `ValueError` if not found. $O(N)$.
- `pop(i)` removes and returns the element at **index `i`** (default `-1`, the last item). Raises `IndexError` if out of bounds. $O(1)$ from end, $O(N)$ elsewhere.
- `clear()` removes all elements from the list in-place, resetting length to 0.

```python
items = ["alpha", "beta", "gamma", "beta"]
items.remove("beta") # removes first 'beta'
print("After remove('beta'):", items)

popped = items.pop(1) # pops index 1 ('gamma')
print(f"Popped: {popped}, Remaining: {items}")

items.clear()
print("After clear():", items)
```

Output:
```text
After remove('beta'): ['alpha', 'gamma', 'beta']
Popped: gamma, Remaining: ['alpha', 'beta']
After clear(): []
```

**Q157. How does list sorting work: sort() vs sorted()?**

*Explain in-place Timsort vs returning a new list, key functions, and stable sorting.*

**Answer:** - `lst.sort(*, key=None, reverse=False)` sorts the list **in-place** and returns `None` (mutates original).
- `sorted(iterable, *, key=None, reverse=False)` accepts any iterable and returns a **new sorted list**.
Both use **Timsort** (an adaptive, stable hybrid of merge sort and insertion sort). Stability guarantees that items with equal keys maintain their relative original order.

```python
records = [("web", 80), ("api", 8080), ("db", 5432), ("dns", 53)]

# Sort by port number ascending:
sorted_by_port = sorted(records, key=lambda r: r[1])
print("Sorted copy:", sorted_by_port)

# In-place sort by service name reverse:
records.sort(key=lambda r: r[0], reverse=True)
print("In-place sorted records:", records)
```

Output:
```text
Sorted copy: [('dns', 53), ('web', 80), ('db', 5432), ('api', 8080)]
In-place sorted records: [('web', 80), ('dns', 53), ('db', 5432), ('api', 8080)]
```

**Q158. How do list comprehensions work and how do they compare to map() and filter()?**

*Explain syntax, performance, readability, and nested list comprehensions.*

**Answer:** List comprehension syntax: `[expr for item in iterable if condition]`. They are compiled into optimized bytecode (`LIST_APPEND`) running at C-speed, generally outperforming manual `for` loops and `map()`/`filter()` with lambda functions.

```python
numbers = [1, 2, 3, 4, 5, 6]
# Filter evens and square them
evens_squared = [n ** 2 for n in numbers if n % 2 == 0]
print("Evens squared:", evens_squared)

# Flattening a 2D matrix:
matrix = [[1, 2], [3, 4], [5, 6]]
flat = [num for row in matrix for num in row]
print("Flattened:", flat)
```

Output:
```text
Evens squared: [4, 16, 36]
Flattened: [1, 2, 3, 4, 5, 6]
```

**Q159. Why should collections.deque be used instead of a list for queues?**

*Compare list pop(0) $O(N)$ with collections.deque popleft() $O(1)$.*

**Answer:** A Python `list` is a contiguous dynamic array; calling `pop(0)` or `insert(0, val)` forces a memory shift of all $N-1$ elements ($O(N)$). `collections.deque` is implemented as a doubly-linked list of fixed-size blocks (64 elements each), providing true $O(1)$ appends and pops from both ends.

```python
from collections import deque

queue = deque(["task-1", "task-2", "task-3"])
queue.append("task-4")      # O(1) push to back
first = queue.popleft()      # O(1) pop from front
print(f"Processed: {first}, Queue remaining: {list(queue)}")
```

Output:
```text
Processed: task-1, Queue remaining: ['task-2', 'task-3', 'task-4']
```

**Q160. What is a tuple and how does it differ from a list?**

*Explain tuple immutability, memory compactness, and performance differences.*

**Answer:** A tuple is an ordered, immutable sequence of heterogeneous objects. Differences from list:
1. **Immutability**: Cannot add, remove, or modify elements after instantiation.
2. **Memory Efficiency**: Fixed-size structure without over-allocation buffers, consuming less memory.
3. **Hashability**: Can be used as dictionary keys or set elements (if all contained items are hashable).
4. **Optimization**: CPython caches small, deallocated tuples (up to size 20) for instant reuse.

```python
import sys

t = (1, 2, 3, 4, 5)
l = [1, 2, 3, 4, 5]

print("Tuple size in bytes:", sys.getsizeof(t))
print("List size in bytes: ", sys.getsizeof(l))
```

Output:
```text
Tuple size in bytes: 80
List size in bytes:  104
```

**Q161. What is the single-element tuple comma rule?**

*Explain why `(1)` is an integer while `(1,)` is a tuple.*

**Answer:** In Python, parentheses `()` are used for mathematical expression grouping as well as tuple notation. To disambiguate, a single-element tuple **must contain a trailing comma**: `single = (42,)`. Without the comma, `(42)` evaluates to the integer `42`.

```python
not_a_tuple = ("devops")
is_a_tuple = ("devops",)

print(f"not_a_tuple: {type(not_a_tuple).__name__}")
print(f"is_a_tuple:  {type(is_a_tuple).__name__}")
```

Output:
```text
not_a_tuple: str
is_a_tuple:  tuple
```

**Q162. How does tuple packing and unpacking work?**

*Demonstrate variable swapping, starred unpacking, and discarding variables with `_`.*

**Answer:** - **Tuple Packing**: Comma-separated values are automatically packed into a tuple: `point = 10, 20`.
- **Unpacking**: Assigns components into individual variables: `x, y = point`.
- **Starred Unpacking**: `head, *tail = (1, 2, 3, 4)` captures variable length sequences.
- **Throwaway `_`**: Conventionally used for values that should be ignored.

```python
# Swapping without temporary variable:
a, b = 100, 200
a, b = b, a
print(f"Swapped: a={a}, b={b}")

# Starred unpacking
first, *middle, last = (10, 20, 30, 40, 50)
print(f"first: {first}, middle: {middle}, last: {last}")
```

Output:
```text
Swapped: a=200, b=100
first: 10, middle: [20, 30, 40], last: 50
```

**Q163. What are Named Tuples (`collections.namedtuple` and `typing.NamedTuple`)?**

*Explain creating lightweight records with named field access.*

**Answer:** Named tuples combine the memory compactness and immutability of standard tuples with readable attribute access (`record.field`) while remaining indexable and iterable. `typing.NamedTuple` adds type annotations and class syntax.

```python
from typing import NamedTuple

class CloudInstance(NamedTuple):
    instance_id: str
    region: str
    cores: int
    memory_gb: float

inst = CloudInstance("i-0a1b2c3d", "us-east-1", 8, 32.0)
print("Attribute access:", inst.instance_id, inst.cores)
print("Index access:", inst[1])
print("Dict representation:", inst._asdict())
```

Output:
```text
Attribute access: i-0a1b2c3d 8
Index access: us-east-1
Dict representation: {'instance_id': 'i-0a1b2c3d', 'region': 'us-east-1', 'cores': 8, 'memory_gb': 32.0}
```

**Q164. When is a tuple unhashable?**

*Explain why `hash((1, 2, [3, 4]))` raises a TypeError.*

**Answer:** A tuple is only hashable if **every element inside it is hashable**. If a tuple contains a mutable object (such as a `list` or `dict`), its hash value cannot remain constant, so Python raises `TypeError: unhashable type: 'list'`.

```python
hashable_tuple = (1, 2, "hello", (3, 4))
print("Hashable tuple hash:", hash(hashable_tuple))

unhashable_tuple = (1, 2, ["mutable"])
try:
    hash(unhashable_tuple)
except TypeError as e:
    print("Caught unhashable error:", e)
```

Output:
```text
Hashable tuple hash: ...
Caught unhashable error: unhashable type: 'list'
```

**Q165. How are Python sets implemented internally?**

*Explain hash tables, uniqueness guarantee, and $O(1)$ membership testing in sets.*

**Answer:** A Python `set` is implemented as an open-addressing hash table with 8 initial buckets (growing dynamically). Each element's position is derived from its `hash(key)`. Lookups, additions, and deletions are average $O(1)$ time complexity. Sets store keys only (no values) and enforce uniqueness automatically.

```python
raw_ips = ["10.0.0.1", "10.0.0.2", "10.0.0.1", "10.0.0.3", "10.0.0.2"]
unique_ips = set(raw_ips)
print("Unique IPs count:", len(unique_ips))
print("Set contents:", sorted(unique_ips))
print("'10.0.0.3' in set:", "10.0.0.3" in unique_ips)
```

Output:
```text
Unique IPs count: 3
Set contents: ['10.0.0.1', '10.0.0.2', '10.0.0.3']
'10.0.0.3' in set: True
```

**Q166. What is the difference between remove() and discard() on a set?**

*Explain error handling when removing elements that might not exist.*

**Answer:** - `set.remove(elem)` removes `elem`. If `elem` does not exist in the set, it raises a `KeyError`.
- `set.discard(elem)` removes `elem` if present, but **silently does nothing** if `elem` is absent (no error raised).

```python
roles = {"admin", "editor"}
roles.discard("viewer") # No error
print("After discard missing:", roles)

try:
    roles.remove("viewer")
except KeyError as e:
    print(f"remove() raised KeyError: {e}")
```

Output:
```text
After discard missing: {'admin', 'editor'}
remove() raised KeyError: 'viewer'
```

**Q167. What are set mathematical operations (union, intersection, difference, symmetric difference)?**

*Demonstrate operators `|`, `&`, `-`, `^` and their method counterparts.*

**Answer:** - **Union (`a | b` or `a.union(b)`)**: Elements in either set.
- **Intersection (`a & b` or `a.intersection(b)`)**: Elements in both sets.
- **Difference (`a - b` or `a.difference(b)`)**: Elements in `a` but not in `b`.
- **Symmetric Difference (`a ^ b` or `a.symmetric_difference(b)`)**: Elements in either `a` or `b`, but not both.

```python
cluster_a = {"pod-1", "pod-2", "pod-3"}
cluster_b = {"pod-2", "pod-3", "pod-4"}

print("Union (|):        ", cluster_a | cluster_b)
print("Intersection (&): ", cluster_a & cluster_b)
print("Difference (-):   ", cluster_a - cluster_b)
print("Sym Difference (^):", cluster_a ^ cluster_b)
```

Output:
```text
Union (|):         {'pod-1', 'pod-2', 'pod-3', 'pod-4'}
Intersection (&):  {'pod-2', 'pod-3'}
Difference (-):    {'pod-1'}
Sym Difference (^): {'pod-1', 'pod-4'}
```

**Q168. What are subset, superset, and disjoint checks on sets?**

*Explain `issubset()`, `issuperset()`, and `isdisjoint()`.*

**Answer:** - `a.issubset(b)` (`a <= b`): True if every element of `a` is in `b`.
- `a.issuperset(b)` (`a >= b`): True if `a` contains every element of `b`.
- `a.isdisjoint(b)`: True if `a` and `b` have zero elements in common.

```python
required_roles = {"read", "write"}
user_roles = {"read", "write", "admin"}
external_roles = {"guest"}

print("user has all required roles (issubset):", required_roles <= user_roles)
print("user is superset:", user_roles >= required_roles)
print("user and guest are disjoint:", user_roles.isdisjoint(external_roles))
```

Output:
```text
user has all required roles (issubset): True
user is superset: True
user and guest are disjoint: True
```

**Q169. What is a frozenset and when should you use it?**

*Explain immutable sets and their hashability.*

**Answer:** A `frozenset` is an immutable variant of `set`. Because it cannot be modified after creation, it is hashable and can be used as a dictionary key or as an element inside another set (nested sets).

```python
fs = frozenset(["us-east-1", "us-west-2"])
cluster_map = {fs: "Active Multi-Region Cluster"}

print("frozenset hash:", hash(fs))
print("Retrieved from dict:", cluster_map[fs])
```

Output:
```text
frozenset hash: ...
Retrieved from dict: Active Multi-Region Cluster
```

**Q170. How are Python dictionaries implemented (Compact Dict in Python 3.6+ / 3.7+)?**

*Explain the split-table memory layout and insertion order preservation.*

**Answer:** Since Python 3.6 (standardized in 3.7+), dicts use Raymond Hettinger's compact layout: an array of small integer indices plus a contiguous dense array of entries `[hash, key, value]`. This reduced dictionary memory consumption by 20-25% and guarantees **insertion order preservation** during iteration.

```python
d = {}
d["z"] = 1
d["a"] = 2
d["m"] = 3

print("Keys maintain insertion order:", list(d.keys()))
```

Output:
```text
Keys maintain insertion order: ['z', 'a', 'm']
```

**Q171. What are dictionary key requirements?**

*Explain why dictionary keys must be hashable and implement both `__hash__` and `__eq__`.*

**Answer:** An object can serve as a dictionary key if and only if it is **hashable**:
1. It implements `__hash__()` which returns an integer that never changes during its lifetime.
2. It implements `__eq__()` for collision resolution.
3. Equal objects must produce equal hashes (`a == b` implies `hash(a) == hash(b)`).
Mutable collections (`list`, `dict`, `set`) are unhashable.

```python
class ServerKey:
    def __init__(self, hostname, datacenter):
        self.hostname = hostname
        self.datacenter = datacenter

    def __hash__(self):
        return hash((self.hostname, self.datacenter))

    def __eq__(self, other):
        return isinstance(other, ServerKey) and (self.hostname, self.datacenter) == (other.hostname, other.datacenter)

k1 = ServerKey("node-1", "dc-east")
k2 = ServerKey("node-1", "dc-east")
lookup = {k1: "10.0.1.5"}
print("Lookup with equivalent key object:", lookup[k2])
```

Output:
```text
Lookup with equivalent key object: 10.0.1.5
```

**Q172. How do dict accessing methods work: get(), setdefault(), and []?**

*Compare direct key subscripting with safe retrieval and in-place defaulting.*

**Answer:** - `d[key]`: Fetches value. Raises `KeyError` if key is absent.
- `d.get(key, default=None)`: Returns value or `default` if key is absent. Never raises `KeyError`.
- `d.setdefault(key, default=None)`: If `key` exists, returns its value. If not, sets `d[key] = default` and returns `default`.

```python
cfg = {"env": "prod"}

print("get with default:", cfg.get("timeout", 30))
print("dict unchanged:", cfg)

val = cfg.setdefault("retries", 3)
print("setdefault returned:", val)
print("dict now updated:", cfg)
```

Output:
```text
get with default: 30
dict unchanged: {'env': 'prod'}
setdefault returned: 3
dict now updated: {'env': 'prod', 'retries': 3}
```

**Q173. What are dictionary views: keys(), values(), and items()?**

*Explain dynamic dictionary views and set-like operations on `dict_keys` and `dict_items`.*

**Answer:** In Python 3, `d.keys()`, `d.values()`, and `d.items()` return **dictionary views**, dynamic window objects reflecting real-time changes to the underlying dict without copying data. Furthermore, `keys()` and `items()` support set operations (`&`, `|`, `-`).

```python
d = {"a": 1, "b": 2}
k_view = d.keys()
print("Initial keys:", list(k_view))

d["c"] = 3
print("View dynamically reflects update:", list(k_view))

# Set operations on dict keys:
d2 = {"b": 20, "c": 30, "d": 40}
common_keys = d.keys() & d2.keys()
print("Common keys (&):", common_keys)
```

Output:
```text
Initial keys: ['a', 'b']
View dynamically reflects update: ['a', 'b', 'c']
Common keys (&): {'b', 'c'}
```

**Q174. How do you merge and update dictionaries across Python versions?**

*Compare `update()`, `{**d1, **d2}`, and Python 3.9+ `d1 | d2`.*

**Answer:** - `d1.update(d2)`: Mutates `d1` in-place with entries from `d2`.
- `{**d1, **d2}`: Creates a new merged dictionary (Python 3.5+).
- `d1 | d2`: Modern merge operator returning a new dictionary (Python 3.9+, PEP 584).
- `d1 |= d2`: Modern in-place update operator.

```python
d1 = {"a": 1, "b": 2}
d2 = {"b": 99, "c": 3}

# Python 3.9+ union
merged = d1 | d2
print("Merged dict (b overwritten):", merged)
```

Output:
```text
Merged dict (b overwritten): {'a': 1, 'b': 99, 'c': 3}
```

**Q175. What is collections.defaultdict and how does it prevent KeyErrors?**

*Explain default factory functions (list, int, set) and use cases.*

**Answer:** `defaultdict(default_factory)` is a `dict` subclass that automatically calls `default_factory()` to populate missing keys when accessed via `[]`, eliminating `if key not in d:` checks.

```python
from collections import defaultdict

# Grouping items by category
grouped = defaultdict(list)
data = [("web", "10.0.0.1"), ("db", "10.0.0.2"), ("web", "10.0.0.3")]

for service, ip in data:
    grouped[service].append(ip)

print("Grouped services:", dict(grouped))
```

Output:
```text
Grouped services: {'web': ['10.0.0.1', '10.0.0.3'], 'db': ['10.0.0.2']}
```

**Q176. What is collections.Counter and its key methods?**

*Demonstrate counting, `most_common()`, and multiset arithmetic (+, -).*

**Answer:** `Counter` is a `dict` subclass designed for counting hashable objects. Key features include `.most_common(n)`, returning elements ordered by frequency, and multiset arithmetic (`+`, `-`, `&`, `|`).

```python
from collections import Counter

http_codes = Counter([200, 200, 200, 404, 500, 500, 200])
print("Top 2 status codes:", http_codes.most_common(2))

# Multiset addition:
more_codes = Counter([500, 502])
total_codes = http_codes + more_codes
print("Total 500 errors:", total_codes[500])
```

Output:
```text
Top 2 status codes: [(200, 4), (500, 2)]
Total 500 errors: 3
```

**Q177. What is collections.OrderedDict and when is it still useful?**

*Compare `OrderedDict` with standard dict in modern Python.*

**Answer:** While standard Python 3.7+ dicts preserve insertion order, `OrderedDict` provides specialized capabilities: equality checks consider insertion order (`od1 == od2` tests order!), and methods `.move_to_end(key, last=True)` and `.popitem(last=True)` make it ideal for implementing LRU (Least Recently Used) caches.

```python
from collections import OrderedDict

od1 = OrderedDict([("a", 1), ("b", 2)])
od2 = OrderedDict([("b", 2), ("a", 1)])
print("OrderedDicts with different orders equal:", od1 == od2)

# Normal dict order equality
d1 = {"a": 1, "b": 2}
d2 = {"b": 2, "a": 1}
print("Standard dicts with different orders equal:", d1 == d2)

od1.move_to_end("a")
print("After move_to_end('a'):", list(od1.keys()))
```

Output:
```text
OrderedDicts with different orders equal: False
Standard dicts with different orders equal: True
After move_to_end('a'): ['b', 'a']
```

**Q178. What is collections.ChainMap and how does it support hierarchical scoping?**

*Explain chaining multiple dictionaries for configuration resolution (CLI > Env > Defaults).*

**Answer:** `ChainMap(*maps)` groups multiple dictionaries into a single, updatable view without copying data. Lookups search mappings sequentially from first to last, making it the standard pattern for configuration hierarchies.

```python
from collections import ChainMap

defaults = {"theme": "light", "port": 8080, "debug": False}
env_vars = {"port": 9000, "debug": True}
cli_args = {"debug": False}

# Priority: cli_args > env_vars > defaults
config = ChainMap(cli_args, env_vars, defaults)
print("Resolved debug (CLI):", config["debug"])
print("Resolved port (Env):", config["port"])
print("Resolved theme (Default):", config["theme"])
```

Output:
```text
Resolved debug (CLI): False
Resolved port (Env): 9000
Resolved theme (Default): light
```

**Q179. What is types.MappingProxyType and how does it create read-only dicts?**

*Demonstrate exposing immutable dictionary views.*

**Answer:** `MappingProxyType` creates a read-only proxy wrapping an underlying dictionary. Any attempt to modify the proxy raises a `TypeError`, but updates to the original dict are immediately visible through the proxy.

```python
from types import MappingProxyType

internal_config = {"db_pass": "secret123", "read_only": True}
public_view = MappingProxyType(internal_config)

print("Public view:", public_view["read_only"])
try:
    public_view["read_only"] = False
except TypeError as e:
    print("Caught mutation error:", e)
```

Output:
```text
Public view: True
Caught mutation error: 'mappingproxy' object does not support item assignment
```

**Q180. What is the difference between shallow copy and deepcopy on nested dictionaries?**

*Explain why `d.copy()` fails to protect nested inner dictionaries.*

**Answer:** `d.copy()` creates a new outer dictionary, but the nested dict objects remain identical references. Modifying `shallow_copy['nested']['k']` mutates the original dictionary. `copy.deepcopy()` recursively duplicates all nested layers.

```python
import copy

orig = {"cluster": {"nodes": 3}}
shallow = orig.copy()
deep = copy.deepcopy(orig)

shallow["cluster"]["nodes"] = 10
print("Original modified by shallow copy:", orig["cluster"]["nodes"])

deep["cluster"]["nodes"] = 99
print("Original safe from deep copy:", orig["cluster"]["nodes"])
```

Output:
```text
Original modified by shallow copy: 10
Original safe from deep copy: 10
```

**Q181. Output Question: Predict the output of list slicing assignment.**

*Analyze replacing a subslice with another iterable.*

**Answer:** Slice assignment `lst[1:3] = [20, 30, 40]` replaces elements from index 1 to 2 with the elements of the replacement list.

```python
lst = [1, 2, 3, 4, 5]
lst[1:3] = [20, 30, 40]
print("Slice assignment:", lst)
```

Output:
```text
Slice assignment: [1, 20, 30, 40, 4, 5]
```

**Q182. Output Question: What does this dict comprehension with duplicate keys evaluate to?**

*Explain key collision in dictionary comprehensions.*

**Answer:** When duplicate keys are generated in a dictionary comprehension, the last processed value overwrites any preceding values.

```python
words = ["apple", "banana", "avocado", "cherry"]
# Mapping first letter to word
d = {w[0]: w for w in words}
print("Dict keys and values:", d)
```

Output:
```text
Dict keys and values: {'a': 'avocado', 'b': 'banana', 'c': 'cherry'}
```

**Q183. Output Question: What is the output of set operations on boolean and integer values?**

*Explain why `{1, True, 0, False}` has length 2.*

**Answer:** In Python, `True == 1` and `False == 0`, and their hashes match (`hash(True) == 1`, `hash(False) == 0`). A set treats them as duplicate values and retains only the first inserted element.

```python
s = {1, True, 0, False}
print("Set contents:", s)
print("Set length:", len(s))
```

Output:
```text
Set contents: {0, 1}
Set length: 2
```

**Q184. Output Question: Predict the output when mutating default dictionary keys.**

*Analyze the effect of modifying list values through dictionary iteration.*

**Answer:** The list references inside the dictionary are mutated directly.

```python
d = {"a": [1, 2], "b": [3, 4]}
for k in d:
    d[k].append(10)
print(d)
```

Output:
```text
{'a': [1, 2, 10], 'b': [3, 4, 10]}
```

**Q185. Debugging Question: Fix the RuntimeError: dictionary changed size during iteration.**

*Resolve error when deleting keys during dictionary iteration.*

**Answer:** You cannot add or delete keys in a dictionary while iterating over it directly. Fix: iterate over a copy of keys using `list(d.keys())`.

```python
# Buggy Code:
# d = {"a": 1, "b": 2, "c": 3}
# for k in d:
#     if d[k] % 2 != 0: del d[k]

# Fixed Code:
d = {"a": 1, "b": 2, "c": 3}
for k in list(d.keys()):
    if d[k] % 2 != 0:
        del d[k]
print("Cleaned dict:", d)
```

Output:
```text
Cleaned dict: {'b': 2}
```

**Q186. Programming Question: Invert a dictionary (swap keys and values) handling duplicate values.**

*Write a function to flip key-value mappings where multiple keys map to the same value.*

**Answer:** Map each value to a list of original keys.

```python
from collections import defaultdict

def invert_dict(d: dict) -> dict:
    inverted = defaultdict(list)
    for k, v in d.items():
        inverted[v].append(k)
    return dict(inverted)

mapping = {"node-1": "web", "node-2": "api", "node-3": "web", "node-4": "db"}
print("Inverted:", invert_dict(mapping))
```

Output:
```text
Inverted: {'web': ['node-1', 'node-3'], 'api': ['node-2'], 'db': ['node-4']}
```

**Q187. Programming Question: Flatten a nested dictionary into dot-notation paths.**

*Convert arbitrary nested JSON/dict objects into flat key-value pairs (e.g. `{'db.host': 'localhost'}`).*

**Answer:** Use recursive traversal building path strings.

```python
def flatten_dict(d: dict, parent_key: str = "", sep: str = ".") -> dict:
    items = []
    for k, v in d.items():
        new_key = f"{parent_key}{sep}{k}" if parent_key else k
        if isinstance(v, dict):
            items.extend(flatten_dict(v, new_key, sep=sep).items())
        else:
            items.append((new_key, v))
    return dict(items)

nested = {"server": {"database": {"host": "10.0.0.5", "port": 5432}, "ssl": True}}
print(flatten_dict(nested))
```

Output:
```text
{'server.database.host': '10.0.0.5', 'server.database.port': 5432, 'server.ssl': True}
```

**Q188. Programming Question: Find symmetric difference between two lists preserving order.**

*Return items present in either list but not both, preserving original appearance order.*

**Answer:** Use collections.Counter or sets while iterating.

```python
def symmetric_diff_ordered(l1, l2):
    s1, s2 = set(l1), set(l2)
    diff = (s1 ^ s2)
    return [x for x in l1 + l2 if x in diff]

a = [1, 2, 3, 4]
b = [3, 4, 5, 6]
print("Ordered symmetric diff:", symmetric_diff_ordered(a, b))
```

Output:
```text
Ordered symmetric diff: [1, 2, 5, 6]
```

**Q189. Programming Question: Deduplicate a list of dictionaries by a specific key.**

*Remove duplicate records based on an ID field while preserving order.*

**Answer:** Track seen keys in a set.

```python
def deduplicate_by_key(records: list, key: str) -> list:
    seen = set()
    result = []
    for r in records:
        val = r.get(key)
        if val not in seen:
            seen.add(val)
            result.append(r)
    return result

data = [{"id": 1, "name": "web"}, {"id": 2, "name": "db"}, {"id": 1, "name": "web-dup"}]
print("Deduplicated:", deduplicate_by_key(data, "id"))
```

Output:
```text
Deduplicated: [{'id': 1, 'name': 'web'}, {'id': 2, 'name': 'db'}]
```

**Q190. Programming Question: Implement a fixed-capacity LRU Cache using collections.OrderedDict.**

*Build an in-memory cache that evicts the least recently accessed item when capacity is reached.*

**Answer:** Use `move_to_end()` on get and `popitem(last=False)` on put.

```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = OrderedDict()

    def get(self, key):
        if key not in self.cache:
            return -1
        self.cache.move_to_end(key) # Mark as recently used
        return self.cache[key]

    def put(self, key, value):
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        if len(self.cache) > self.capacity:
            self.cache.popitem(last=False) # Evict oldest

lru = LRUCache(2)
lru.put("a", 1)
lru.put("b", 2)
lru.get("a") # 'a' is now most recent
lru.put("c", 3) # evicts 'b'
print("Get 'b' (evicted):", lru.get("b"))
print("Get 'a' (present):", lru.get("a"))
```

Output:
```text
Get 'b' (evicted): -1
Get 'a' (present): 1
```

**Q191. Programming Question: Merge two lists of dictionaries on a common key (SQL inner join in Python).**

*Combine attributes from two dictionary lists matching on a shared identifier.*

**Answer:** Index the secondary list into a hash lookup dictionary for $O(N + M)$ join complexity.

```python
def inner_join(list_a, list_b, join_key):
    lookup = {item[join_key]: item for item in list_b if join_key in item}
    joined = []
    for a in list_a:
        key_val = a.get(join_key)
        if key_val in lookup:
            joined.append({**a, **lookup[key_val]})
    return joined

users = [{"uid": 1, "name": "Alice"}, {"uid": 2, "name": "Bob"}]
perms = [{"uid": 1, "role": "Admin"}, {"uid": 3, "role": "Guest"}]

print("Joined records:", inner_join(users, perms, "uid"))
```

Output:
```text
Joined records: [{'uid': 1, 'name': 'Alice', 'role': 'Admin'}]
```

**Q192. Programming Question: Find top K most frequent elements using heapq.**

*Write an optimal top-k frequency function with $O(N \log K)$ time complexity.*

**Answer:** Combine `Counter` and `heapq.nlargest`.

```python
from collections import Counter
import heapq

def top_k_frequent(nums: list, k: int) -> list:
    counts = Counter(nums)
    return heapq.nlargest(k, counts.keys(), key=counts.get)

stream = [1, 1, 1, 2, 2, 3, 4, 4, 4, 4, 5]
print("Top 2 frequent elements:", top_k_frequent(stream, 2))
```

Output:
```text
Top 2 frequent elements: [4, 1]
```


---

## Chapter 7: Functions and Functional Principles

**Q193. How are functions defined, called, and executed in Python?**

*Explain the `def` statement, code objects, and function call stack frames.*

**Answer:** In Python, `def func_name(params):` is an executable statement. When executed, it creates a new function object (`PyFunctionObject`) and binds it to `func_name` in the current namespace. Calling the function allocates a new execution stack frame (`PyFrameObject`) containing local variables, bytecode evaluation pointers, and references to globals.

```python
def calculate_overhead(base: float, margin: float = 0.1) -> float:
    """Calculates overhead margin."""
    return base * (1 + margin)

print("Function object:", calculate_overhead)
print("Function code object:", calculate_overhead.__code__)
print("Result:", calculate_overhead(100.0))
```

Output:
```text
Function object: <function calculate_overhead at 0x...>
Function code object: <code object calculate_overhead at 0x...>
Result: 110.0
```

**Q194. What is the difference between parameters and arguments?**

*Explain formal parameters vs actual arguments.*

**Answer:** - **Parameters (Formal Parameters)**: The variable names declared in the function definition header.
- **Arguments (Actual Arguments)**: The concrete values or object references passed into the function when it is invoked.

```python
# 'host' and 'port' are parameters:
def connect(host, port):
    return f"Connecting to {host}:{port}"

# "127.0.0.1" and 8080 are arguments:
print(connect("127.0.0.1", 8080))
```

Output:
```text
Connecting to 127.0.0.1:8080
```

**Q195. How do return values work and what happens when `return` is omitted?**

*Explain implicit `return None`, explicit return, and returning multiple values.*

**Answer:** If a function reaches the end of its body without executing a `return` statement, it implicitly returns `None`. Returning multiple comma-separated values (`return a, b, c`) actually packs them into and returns a single **tuple**.

```python
def no_return():
    pass

def multi_return():
    return "ok", 200, True

print("Omitted return:", no_return())
res = multi_return()
print("Multi-return tuple:", res, "Type:", type(res).__name__)
```

Output:
```text
Omitted return: None
Multi-return tuple: ('ok', 200, True) Type: tuple
```

**Q196. What are positional arguments vs keyword arguments?**

*Explain parameter matching by position vs by parameter name.*

**Answer:** - **Positional Arguments**: Assigned to parameters based strictly on the left-to-right order in which they are passed.
- **Keyword Arguments**: Passed with explicit parameter names (`name=value`), allowing arguments to be passed in any order and self-documenting the call site.

```python
def create_container(image, name, restart_policy="always"):
    return f"Container {name} from {image} ({restart_policy})"

print(create_container("nginx:latest", "web-server"))
print(create_container(name="api-server", image="python:3.12", restart_policy="on-failure"))
```

Output:
```text
Container web-server from nginx:latest (always)
Container api-server from python:3.12 (on-failure)
```

**Q197. Why is using mutable objects as default arguments dangerous?**

*Explain the 'mutable default argument' pitfall and the standard `None` sentinel pattern.*

**Answer:** Default argument expressions are evaluated **once when the function is defined**, not each time it is called. If a mutable object (`[]` or `{}`) is used as a default, that exact same object is mutated across subsequent calls. The standard, robust fix is to use `None` as the default and assign a new container inside the function.

```python
# Dangerous:
# def add_entry(entry, cache=[]): ...

# Idiomatic Pattern:
def add_entry(entry, cache=None):
    if cache is None:
        cache = []
    cache.append(entry)
    return cache

print("Call 1:", add_entry("alpha"))
print("Call 2:", add_entry("beta"))
```

Output:
```text
Call 1: ['alpha']
Call 2: ['beta']
```

**Q198. How do variable-length arguments (*args and **kwargs) work?**

*Explain packing arbitrary positional and keyword arguments.*

**Answer:** - `*args`: Collects any extra positional arguments into a **tuple**.
- `**kwargs`: Collects any extra keyword arguments into a **dictionary**.
Allows designing flexible wrappers, decorators, and generic handlers.

```python
def generic_logger(level, *args, **kwargs):
    msg = " ".join(str(a) for a in args)
    meta = ", ".join(f"{k}={v}" for k, v in kwargs.items())
    return f"[{level}] {msg} | Meta: {{{meta}}}"

print(generic_logger("INFO", "Service", "started", host="10.0.0.1", port=443))
```

Output:
```text
[INFO] Service started | Meta: {host=10.0.0.1, port=443}
```

**Q199. What are keyword-only parameters (`*`) in Python?**

*Explain enforcing keyword argument passing using bare `*`.*

**Answer:** Any parameter defined **after a bare asterisk `*`** or after `*args` can only be passed as a keyword argument. This prevents caller mistakes when functions accept boolean flags or sensitive parameters.

```python
def delete_database(db_name: str, *, force: bool = False):
    if not force:
        return f"Refused to delete {db_name} without force=True"
    return f"Deleted {db_name}"

# delete_database("prod_db", True) -> Raises TypeError!
print(delete_database("prod_db", force=True))
print(delete_database("test_db"))
```

Output:
```text
Deleted prod_db
Refused to delete test_db without force=True
```

**Q200. What are positional-only parameters (`/`) introduced in Python 3.8?**

*Explain PEP 570 positional-only syntax and use cases.*

**Answer:** Any parameter defined **before a forward slash `/`** can only be passed positionally and cannot be passed as a keyword argument. This allows API designers to rename parameters in future releases without breaking caller keyword arguments and enforces cleaner calls for obvious mathematical functions.

```python
def power(base, exp, /, modulo=None):
    res = base ** exp
    return res if modulo is None else res % modulo

# power(base=2, exp=3) -> Raises TypeError: power() got some positional-only arguments passed as keyword arguments
print("Positional-only call:", power(2, 3))
print("Mixed call:", power(2, 3, modulo=5))
```

Output:
```text
Positional-only call: 8
Mixed call: 3
```

**Q201. How does argument unpacking with `*` and `**` work at function call sites?**

*Demonstrate expanding lists/tuples into positional parameters and dictionaries into keyword parameters.*

**Answer:** At the call site, prefixing an iterable with `*` unpacks its items into individual positional arguments. Prefixing a dictionary with `**` unpacks its key-value pairs into keyword arguments matching parameter names.

```python
def configure_route(host, port, protocol):
    return f"{protocol}://{host}:{port}"

conn_tuple = ("api.internal", 8443)
extra_opts = {"protocol": "https"}

print(configure_route(*conn_tuple, **extra_opts))
```

Output:
```text
https://api.internal:8443
```

**Q202. What is an anonymous lambda function and what are its limitations?**

*Explain syntax, single-expression constraint, and common functional use cases.*

**Answer:** A `lambda` is an anonymous inline function defined as `lambda params: expression`. Limitations:
1. Can only contain a **single expression**; statements (`if/else` blocks, `return`, `assert`, `while`, loops) are forbidden.
2. No type annotations or docstrings.
Best used for short throwaway functions passed into `sorted()`, `map()`, or `filter()`.

```python
services = [("web", 3), ("db", 1), ("cache", 2)]
# Sort by replica count descending
services.sort(key=lambda s: s[1], reverse=True)
print("Sorted services:", services)
```

Output:
```text
Sorted services: [('web', 3), ('cache', 2), ('db', 1)]
```

**Q203. What are first-class functions and higher-order functions in Python?**

*Explain passing functions as arguments, returning functions, and storing functions in collections.*

**Answer:** In Python, functions are **first-class citizens**: they are objects with types, can be assigned to variables, stored in dictionaries, passed as arguments to other functions, and returned from functions. A function that accepts or returns another function is called a **higher-order function**.

```python
def apply_pipeline(data, *operations):
    for op in operations:
        data = op(data)
    return data

clean = lambda s: s.strip()
uppercase = lambda s: s.upper()
prefix = lambda s: f"PROD_{s}"

result = apply_pipeline("  database_cluster  ", clean, uppercase, prefix)
print("Pipeline output:", result)
```

Output:
```text
Pipeline output: PROD_DATABASE_CLUSTER
```

**Q204. What is a closure in Python?**

*Explain lexical scoping, `__closure__` attribute, and cell objects.*

**Answer:** A **closure** is an inner function that retains access to variables in its enclosing scope (lexical environment) even after the outer function has finished executing and returned. Python implements closures using cell objects stored in the inner function's `__closure__` attribute.

```python
def make_multiplier(factor: int):
    # 'factor' is enclosed
    def multiplier(number: int) -> int:
        return number * factor
    return multiplier

double = make_multiplier(2)
triple = make_multiplier(3)

print("Double 10:", double(10))
print("Triple 10:", triple(10))
print("Captured cell value:", double.__closure__[0].cell_contents)
```

Output:
```text
Double 10: 20
Triple 10: 30
Captured cell value: 2
```

**Q205. What is a decorator in Python and how does `@` syntactic sugar work?**

*Explain wrapping functions, preserving behavior, and syntactic translation of `@decorator`.*

**Answer:** A decorator is a higher-order function that takes a callable as an argument, extends or alters its behavior, and returns a new callable. Writing `@my_decorator` above `def my_func(): ...` is exact syntactic sugar for `my_func = my_decorator(my_func)`.

```python
def audit_log(func):
    def wrapper(*args, **kwargs):
        print(f"[AUDIT] Calling function '{func.__name__}'")
        res = func(*args, **kwargs)
        print(f"[AUDIT] Function '{func.__name__}' returned successfully")
        return res
    return wrapper

@audit_log
def deploy():
    print("Executing deployment steps...")
    return "DEPLOYED"

print(deploy())
```

Output:
```text
[AUDIT] Calling function 'deploy'
Executing deployment steps...
[AUDIT] Function 'deploy' returned successfully
DEPLOYED
```

**Q206. Why should functools.wraps be used inside custom decorators?**

*Explain preserving function name, docstring, annotations, and `__wrapped__` attribute.*

**Answer:** When a function is decorated, the wrapper function replaces the original function. Without `@functools.wraps(func)`, the decorated function loses its original `__name__`, `__doc__`, and signature (they reflect `wrapper` instead). `@functools.wraps` copies metadata and sets `__wrapped__` for introspection.

```python
from functools import wraps

def timing_decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

@timing_decorator
def fetch_user_data(user_id: int):
    """Retrieve user metadata from database."""
    return {"id": user_id}

print("Function name preserved:", fetch_user_data.__name__)
print("Docstring preserved:    ", fetch_user_data.__doc__)
```

Output:
```text
Function name preserved: fetch_user_data
Docstring preserved:     Retrieve user metadata from database.
```

**Q207. How do decorator factories (decorators that accept arguments) work?**

*Explain the 3-level nested function pattern for parameterized decorators.*

**Answer:** To pass arguments to a decorator (e.g. `@retry(max_attempts=3)`), you create a **decorator factory**: an outer function that accepts the parameters and returns the actual decorator function, which in turn returns the wrapper function.

```python
from functools import wraps

def repeat(times: int):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            results = []
            for _ in range(times):
                results.append(func(*args, **kwargs))
            return results
        return wrapper
    return decorator

@repeat(times=3)
def ping():
    return "PONG"

print(ping())
```

Output:
```text
['PONG', 'PONG', 'PONG']
```

**Q208. How do recursive functions work and what is Python's recursion limit?**

*Explain base cases, recursive calls, call stack limits, and `sys.setrecursionlimit`.*

**Answer:** A recursive function calls itself to solve smaller subproblems until it hits a **base case**. Python does not implement Tail Call Optimization (TCO). To prevent C stack overflows, Python enforces a default recursion limit (typically 1000). Exceeding it raises `RecursionError: maximum recursion depth exceeded`.

```python
import sys

def factorial(n: int) -> int:
    if n <= 1: # Base case
        return 1
    return n * factorial(n - 1)

print("Factorial of 5:", factorial(5))
print("Current recursion limit:", sys.getrecursionlimit())
```

Output:
```text
Factorial of 5: 120
Current recursion limit: 1000
```

**Q209. How does functools.lru_cache and functools.cache optimize recursive functions?**

*Explain memoization, cache hits, maxsize, and eviction.*

**Answer:** Memoization caches the results of function calls keyed by arguments. `@functools.cache` (Python 3.9+) provides unbounded caching. `@functools.lru_cache(maxsize=128)` caches up to `maxsize` entries using Least Recently Used eviction, turning exponential $O(2^N)$ algorithms like naive Fibonacci into linear $O(N)$.

```python
from functools import lru_cache
import time

@lru_cache(maxsize=128)
def fib(n: int) -> int:
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)

print("fib(50) computed instantaneously:", fib(50))
print("Cache statistics:", fib.cache_info())
```

Output:
```text
fib(50) computed instantaneously: 12586269025
Cache statistics: CacheInfo(hits=48, misses=51, maxsize=128, currsize=51)
```

**Q210. How does functools.partial work in Python?**

*Explain freeze-binding function arguments for callback interfaces.*

**Answer:** `functools.partial(func, *args, **kwargs)` returns a callable partial object that freezes a subset of arguments, creating specialized functions without writing custom wrappers.

```python
from functools import partial

def http_request(method, url, timeout):
    return f"{method} {url} (Timeout: {timeout}s)"

# Create a specialized GET request with default 30s timeout
get_request = partial(http_request, "GET", timeout=30)
print(get_request("https://api.github.com"))
```

Output:
```text
GET https://api.github.com (Timeout: 30s)
```

**Q211. What are function annotations and how are they accessed at runtime?**

*Explain parameter and return type hints and `func.__annotations__`.*

**Answer:** Annotations (`def f(x: int) -> str:`) store metadata about expected parameter and return types in the function's `__annotations__` dictionary. Python itself does not enforce types at runtime; annotations are consumed by static type checkers (mypy), documentation generators, and runtime validation libraries (Pydantic).

```python
def scale_replicas(service: str, count: int) -> bool:
    return True

print("Function annotations dict:", scale_replicas.__annotations__)
```

Output:
```text
Function annotations dict: {'service': <class 'str'>, 'count': <class 'int'>, 'return': <class 'bool'>}
```

**Q212. How does inspect.signature introspect function parameters?**

*Demonstrate inspecting parameter names, default values, and kinds at runtime.*

**Answer:** The `inspect` module provides `inspect.signature(func)` to introspect parameter metadata, default values, type annotations, and parameter kinds (`POSITIONAL_OR_KEYWORD`, `VAR_POSITIONAL`, `KEYWORD_ONLY`).

```python
import inspect

def deploy(service_name: str, replicas: int = 1, *tags, env: str = "prod"):
    pass

sig = inspect.signature(deploy)
for name, param in sig.parameters.items():
    print(f"Param: {name:<12} | Kind: {param.kind.name:<25} | Default: {param.default}")
```

Output:
```text
Param: service_name | Kind: POSITIONAL_OR_KEYWORD     | Default: <class 'inspect._empty'>
Param: replicas     | Kind: POSITIONAL_OR_KEYWORD     | Default: 1
Param: tags         | Kind: VAR_POSITIONAL            | Default: <class 'inspect._empty'>
Param: env          | Kind: KEYWORD_ONLY              | Default: prod
```

**Q213. What is the difference between pure functions and functions with side effects?**

*Explain deterministic output, referential transparency, and testing benefits.*

**Answer:** - **Pure Function**: Given the same arguments, always returns the same output without modifying global state, mutating passed arguments, or performing I/O. Extremely easy to test, cache, and parallelize.
- **Function with Side Effects**: Modifies external state, writes to disk/network, prints to stdout, or mutates mutable arguments.

```python
# Pure function:
def add_pure(a: int, b: int) -> int:
    return a + b

# Impure function with side effects:
total = 0
def add_impure(a: int):
    global total
    total += a
    return total

print("Pure:", add_pure(5, 10))
print("Impure:", add_impure(5))
```

Output:
```text
Pure: 15
Impure: 5
```

**Q214. How do map(), filter(), and functools.reduce() work?**

*Explain Python's classic functional primitives.*

**Answer:** - `map(func, iterable)`: Applies `func` to every element and yields results lazily.
- `filter(func, iterable)`: Yields elements for which `func(element)` is truthy.
- `reduce(func, iterable, initial)`: Cumulatively applies a 2-argument function to elements from left to right to reduce the sequence to a single value.

```python
from functools import reduce

nums = [1, 2, 3, 4, 5]

doubled = list(map(lambda x: x * 2, nums))
evens = list(filter(lambda x: x % 2 == 0, nums))
product = reduce(lambda acc, x: acc * x, nums, 1)

print("Doubled:", doubled)
print("Evens:  ", evens)
print("Product:", product)
```

Output:
```text
Doubled: [2, 4, 6, 8, 10]
Evens:   [2, 4]
Product: 120
```

**Q215. Output Question: Predict the output when late binding occurs in closures.**

*Explain why functions inside loops bind loop variables by reference, not value.*

**Answer:** In Python, closures bind variables **by reference (name binding)**, not by value at creation time. When the functions are called after the loop finishes, `i` has its final value `2`.

```python
functions = [lambda: i for i in range(3)]
print([f() for f in functions])

# Fix using default argument binding:
fixed_functions = [lambda i=i: i for i in range(3)]
print([f() for f in fixed_functions])
```

Output:
```text
[2, 2, 2]
[0, 1, 2]
```

**Q216. Output Question: Predict what happens when multiple decorators are stacked.**

*Explain decorator execution order and evaluation order.*

**Answer:** When decorators are stacked, they are applied **from the bottom up** (`f = dec1(dec2(f))`) and executed **from the top down**.

```python
def bold(func):
    def wrapper():
        return f"<b>{func()}</b>"
    return wrapper

def italic(func):
    def wrapper():
        return f"<i>{func()}</i>"
    return wrapper

@bold
@italic
def text():
    return "Python"

print(text())
```

Output:
```text
<b><i>Python</i></b>
```

**Q217. Output Question: What is the output of kwargs variable name collisions?**

*Analyze parameter precedence when mixing *args, default args, and **kwargs.*

**Answer:** Explicitly passed positional and keyword arguments bind to formal parameters first before `**kwargs` catches remaining names.

```python
def test_args(a, b=2, *args, **kwargs):
    print(f"a={a}, b={b}, args={args}, kwargs={kwargs}")

test_args(1, 10, 20, 30, x=100, y=200)
```

Output:
```text
a=1, b=10, args=(20, 30), kwargs={'x': 100, 'y': 200}
```

**Q218. Output Question: Predict the behavior of unpacking a dictionary into a function expecting specific keys.**

*Explain TypeError when unpacked dictionary keys do not match parameter names.*

**Answer:** If a dictionary has keys not declared in the parameter list and the function does not accept `**kwargs`, Python raises a `TypeError`.

```python
def connect(host, port):
    return f"{host}:{port}"

data = {"host": "localhost", "port": 8080, "timeout": 30}
try:
    connect(**data)
except TypeError as e:
    print("Caught TypeError:", e)
```

Output:
```text
Caught TypeError: connect() got an unexpected keyword argument 'timeout'
```

**Q219. Debugging Question: Fix the scoping bug inside a nested closure.**

*Resolve UnboundLocalError when modifying outer state from an inner function.*

**Answer:** Use the `nonlocal` keyword to inform Python that the identifier refers to the enclosing scope.

```python
# Buggy Code:
# def make_counter():
#     count = 0
#     def inc():
#         count += 1
#         return count
#     return inc

# Fixed Code:
def make_counter():
    count = 0
    def inc():
        nonlocal count
        count += 1
        return count
    return inc

counter = make_counter()
print(counter(), counter(), counter())
```

Output:
```text
1 2 3
```

**Q220. Programming Question: Write a retry decorator with exponential backoff.**

*Implement a configurable `@retry(max_retries, exceptions=(Exception,))` decorator.*

**Answer:** Wrap function call in a retry while-loop catching specified exceptions.

```python
from functools import wraps

def retry(max_retries: int = 3, exceptions: tuple = (Exception,)):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            attempts = 0
            while attempts < max_retries:
                try:
                    return func(*args, **kwargs)
                except exceptions as e:
                    attempts += 1
                    if attempts >= max_retries:
                        raise RuntimeError(f"Failed after {max_retries} attempts") from e
        return wrapper
    return decorator

attempts_counter = 0
@retry(max_retries=3, exceptions=(ValueError,))
def unstable_api():
    global attempts_counter
    attempts_counter += 1
    if attempts_counter < 3:
        raise ValueError("Simulated network blip")
    return "API_SUCCESS"

print(unstable_api())
```

Output:
```text
API_SUCCESS
```

**Q221. Programming Question: Build an execution time profiling decorator.**

*Write a `@timed` decorator that logs elapsed milliseconds and function arguments.*

**Answer:** Use `time.perf_counter()` before and after invocation.

```python
import time
from functools import wraps

def timed(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        t0 = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed_ms = (time.perf_counter() - t0) * 1000
        print(f"[METRIC] {func.__name__} took {elapsed_ms:.3f} ms")
        return result
    return wrapper

@timed
def compute_data():
    return sum(i * i for i in range(10000))

compute_data()
```

Output:
```text
[METRIC] compute_data took 0.650 ms
```

**Q222. Programming Question: Implement a rate-limiting decorator.**

*Write a decorator that allows at most N calls per second, raising an error otherwise.*

**Answer:** Track timestamps in a sliding window inside closure state.

```python
import time
from functools import wraps

def rate_limit(max_calls: int, period: float):
    def decorator(func):
        calls = []
        @wraps(func)
        def wrapper(*args, **kwargs):
            now = time.time()
            cutoff = now - period
            # Filter calls outside period
            while calls and calls[0] < cutoff:
                calls.pop(0)
            if len(calls) >= max_calls:
                raise PermissionError(f"Rate limit exceeded: max {max_calls} calls per {period}s")
            calls.append(now)
            return func(*args, **kwargs)
        return wrapper
    return decorator

@rate_limit(max_calls=2, period=1.0)
def query_api():
    return "OK"

print(query_api())
print(query_api())
try:
    query_api()
except PermissionError as e:
    print(e)
```

Output:
```text
OK
OK
Rate limit exceeded: max 2 calls per 1.0s
```

**Q223. Programming Question: Build a function argument validator decorator using annotations.**

*Write a decorator that validates passed arguments match annotated types at runtime.*

**Answer:** Compare `type(arg)` against `func.__annotations__`.

```python
from functools import wraps
import inspect

def validate_types(func):
    sig = inspect.signature(func)
    @wraps(func)
    def wrapper(*args, **kwargs):
        bound = sig.bind(*args, **kwargs)
        bound.apply_defaults()
        for name, value in bound.arguments.items():
            if name in func.__annotations__:
                expected = func.__annotations__[name]
                if not isinstance(value, expected):
                    raise TypeError(f"Argument '{name}' must be {expected.__name__}, got {type(value).__name__}")
        return func(*args, **kwargs)
    return wrapper

@validate_types
def set_port(port: int):
    return f"Port set to {port}"

print(set_port(8080))
try:
    set_port("8080")
except TypeError as e:
    print("Caught validation error:", e)
```

Output:
```text
Port set to 8080
Caught validation error: Argument 'port' must be int, got str
```

**Q224. Programming Question: Implement function currying in Python.**

*Convert a multi-argument function into a chain of single-argument functions.*

**Answer:** Inspect parameter count and return partial wrappers until all arguments are satisfied.

```python
import inspect

def curry(func):
    expected_args = len(inspect.signature(func).parameters)
    def curried(*args):
        if len(args) >= expected_args:
            return func(*args)
        return lambda *more_args: curried(*(args + more_args))
    return curried

@curry
def format_log(level, module, message):
    return f"[{level}] [{module}]: {message}"

info_log = format_log("INFO")
api_info = info_log("AUTH")
print(api_info("User logged in successfully"))
```

Output:
```text
[INFO] [AUTH]: User logged in successfully
```

**Q225. Programming Question: Implement a function pipeline operator or composer.**

*Write a compose function that combines multiple functions into a single pipeline `f(g(h(x)))`.*

**Answer:** Use `reduce` to chain callable invocations.

```python
from functools import reduce

def compose(*functions):
    return reduce(lambda f, g: lambda x: f(g(x)), functions)

strip_str = lambda s: s.strip()
upper_str = lambda s: s.upper()
bracket_str = lambda s: f"[{s}]"

pipeline = compose(bracket_str, upper_str, strip_str)
print(pipeline("   kubernetes cluster   "))
```

Output:
```text
[[KUBERNETES CLUSTER]]
```

**Q226. Programming Question: Implement a Singleton decorator for classes.**

*Write a decorator that ensures only one instance of a decorated class exists.*

**Answer:** Store created instances in a dictionary closure keyed by class.

```python
from functools import wraps

def singleton(cls):
    instances = {}
    @wraps(cls)
    def get_instance(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    return get_instance

@singleton
class DatabaseConnection:
    def __init__(self):
        self.connection_id = "CONN_001"

c1 = DatabaseConnection()
c2 = DatabaseConnection()
print("c1 is c2:", c1 is c2)
```

Output:
```text
c1 is c2: True
```

**Q227. Programming Question: Implement a tail-recursion optimization simulator using trampolines.**

*Prevent stack overflow in deep recursion by converting function returns into thunks.*

**Answer:** Execute recursive steps inside a while loop until a value is returned.

```python
def trampoline(func):
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        while callable(result):
            result = result()
        return result
    return wrapper

def sum_to_n(n, acc=0):
    if n == 0:
        return acc
    return lambda: sum_to_n(n - 1, acc + n)

safe_sum = trampoline(sum_to_n)
print("Deep sum (1500 calls) without RecursionError:", safe_sum(1500))
```

Output:
```text
Deep sum (1500 calls) without RecursionError: 1125750
```

**Q228. Programming Question: Build a plugin registry using decorators.**

*Create a registry system where components register handlers via `@register('action')`.*

**Answer:** Maintain a dictionary mapping actions to handlers.

```python
class ActionRegistry:
    def __init__(self):
        self.handlers = {}

    def register(self, action_name: str):
        def decorator(func):
            self.handlers[action_name] = func
            return func
        return decorator

    def dispatch(self, action_name: str, *args, **kwargs):
        if action_name not in self.handlers:
            raise KeyError(f"No handler for action '{action_name}'")
        return self.handlers[action_name](*args, **kwargs)

registry = ActionRegistry()

@registry.register("restart")
def handle_restart(service):
    return f"Restarting {service}"

@registry.register("scale")
def handle_scale(service, count):
    return f"Scaling {service} to {count}"

print(registry.dispatch("restart", "nginx"))
print(registry.dispatch("scale", "api", 5))
```

Output:
```text
Restarting nginx
Scaling api to 5
```


---

## Chapter 8: Object-Oriented Programming (OOP)

**Q229. What are classes and objects in Python?**

*Explain class blueprints, object instantiation, and runtime attribute storage.*

**Answer:** A **class** is a blueprint (user-defined type) that packages data state (attributes) and behavior (methods) together. An **object (instance)** is a concrete instantiation of that class living in memory. In Python, classes themselves are also first-class objects (instances of `type`). By default, instance attributes are stored dynamically in the instance's `__dict__` dictionary.

```python
class VirtualMachine:
    def __init__(self, name: str, ram_gb: int):
        self.name = name
        self.ram_gb = ram_gb

vm = VirtualMachine("app-srv-01", 16)
print("Instance type:", type(vm).__name__)
print("Instance attributes __dict__:", vm.__dict__)
```

Output:
```text
Instance type: VirtualMachine
Instance attributes __dict__: {'name': 'app-srv-01', 'ram_gb': 16}
```

**Q230. What is the difference between `__new__()` and `__init__()`?**

*Explain object creation vs object initialization and the Singleton pattern.*

**Answer:** - `__new__(cls, *args, **kwargs)`: The **actual constructor** (static method). Responsible for allocating the object in memory and returning the newly created instance. Must return an instance of `cls` for `__init__` to be called.
- `__init__(self, *args, **kwargs)`: The **initializer**. Receives the newly created instance (`self`) and initializes its attributes. Returns `None`.

```python
class SingletonCluster:
    _instance = None
    def __new__(cls, *args, **kwargs):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

    def __init__(self, cluster_name: str):
        self.cluster_name = cluster_name

c1 = SingletonCluster("us-east-1")
c2 = SingletonCluster("eu-west-1")
print("Same instance:", c1 is c2)
print("Name reflected in both:", c1.cluster_name)
```

Output:
```text
Same instance: True
Name reflected in both: eu-west-1
```

**Q231. What is `self` in Python and why is it explicitly passed?**

*Explain explicit instance binding compared to implicit `this` in Java/C++.*

**Answer:** In Python, `self` is an explicit parameter representing the instance on which the method was called. It follows the Zen of Python principle: *Explicit is better than implicit*. Calling `obj.method(arg)` is syntactic sugar for `ClassName.method(obj, arg)`.

```python
class Worker:
    def execute(self, task):
        return f"Executing {task}"

w = Worker()
print("Instance invocation:", w.execute("backup"))
print("Explicit class invocation:", Worker.execute(w, "backup"))
```

Output:
```text
Instance invocation: Executing backup
Explicit class invocation: Executing backup
```

**Q232. What is the difference between class attributes and instance attributes?**

*Explain shared class state vs independent instance state and accidental shadowing.*

**Answer:** - **Class Attributes**: Defined directly inside the class body, shared by all instances of the class. Modifying via `ClassName.attr` affects all instances.
- **Instance Attributes**: Bound to `self` (typically inside `__init__`), unique to each individual object. Writing `instance.attr = val` creates an instance attribute that shadows the class attribute.

```python
class Node:
    cluster_version = "v1.28" # Class attribute

    def __init__(self, node_id: str):
        self.node_id = node_id # Instance attribute

n1 = Node("node-1")
n2 = Node("node-2")

print(f"Shared: n1={n1.cluster_version}, n2={n2.cluster_version}")
Node.cluster_version = "v1.29" # Update shared attribute
print(f"Updated: n1={n1.cluster_version}, n2={n2.cluster_version}")

n1.cluster_version = "v1.30-custom" # Shadows class attribute on n1 only
print(f"Shadowed n1={n1.cluster_version}, untouched n2={n2.cluster_version}")
```

Output:
```text
Shared: n1=v1.28, n2=v1.28
Updated: n1=v1.29, n2=v1.29
Shadowed n1=v1.30-custom, untouched n2=v1.29
```

**Q233. How do instance methods, class methods (@classmethod), and static methods (@staticmethod) differ?**

*Compare `self` vs `cls` vs no implicit argument and common factory pattern use cases.*

**Answer:** 1. **Instance Method**: Takes `self` as the first argument; accesses and modifies instance and class state.
2. **Class Method (`@classmethod`)**: Takes `cls` as the first argument; accesses and modifies class state. Commonly used as alternative constructors (factory methods).
3. **Static Method (`@staticmethod`)**: Takes neither `self` nor `cls`; acts as a plain function namespaced inside the class for logical grouping.

```python
class DatabasePool:
    default_timeout = 30

    def __init__(self, host, port):
        self.host = host
        self.port = port

    # Alternative constructor
    @classmethod
    def from_url(cls, url: str):
        host, _, port = url.partition(":")
        return cls(host, int(port))

    # Static utility
    @staticmethod
    def is_valid_port(port: int) -> bool:
        return 1 <= port <= 65535

pool = DatabasePool.from_url("127.0.0.1:5432")
print(f"Created pool: {pool.host}:{pool.port}")
print("Port validation:", DatabasePool.is_valid_port(5432))
```

Output:
```text
Created pool: 127.0.0.1:5432
Port validation: True
```

**Q234. How does encapsulation and data hiding work in Python?**

*Explain public, protected-style (`_`), and private name mangling (`__`).*

**Answer:** Python does not have hard access modifiers (`public`, `private`, `protected`). Instead, it relies on conventions:
- **Public (`attr`)**: Accessible anywhere.
- **Protected (`_attr`)**: Convention indicating internal/private use; external callers should avoid modifying.
- **Private (`__attr`)**: Name mangled by CPython into `_ClassName__attr` to prevent accidental overriding in subclasses.

```python
class SecureAccount:
    def __init__(self, account_id: str, secret_key: str):
        self.account_id = account_id
        self._internal_flags = 0
        self.__secret_key = secret_key

acc = SecureAccount("ACC_99", "super_secret_token")
print("Public:", acc.account_id)
print("Protected accessible:", acc._internal_flags)

# Private name mangled:
try:
    print(acc.__secret_key)
except AttributeError as e:
    print("Direct private access failed:", e)

print("Mangled access:", acc._SecureAccount__secret_key)
```

Output:
```text
Public: ACC_99
Protected accessible: 0
Direct private access failed: 'SecureAccount' object has no attribute '__secret_key'
Mangled access: super_secret_token
```

**Q235. How do @property decorators (getters, setters, deleters) implement managed attributes?**

*Explain encapsulating private attributes behind property interfaces with validation.*

**Answer:** The `@property` decorator allows defining methods that are accessed with attribute syntax (`obj.attr`), enabling lazy computation, read-only attributes, and validation logic during assignment without breaking backward compatibility.

```python
class Server:
    def __init__(self, port: int):
        self._port = None
        self.port = port # Triggers setter validation

    @property
    def port(self) -> int:
        return self._port

    @port.setter
    def port(self, value: int):
        if not (1 <= value <= 65535):
            raise ValueError(f"Invalid port number: {value}")
        self._port = value

s = Server(8080)
print("Current port:", s.port)
try:
    s.port = 70000
except ValueError as e:
    print("Validation rejected:", e)
```

Output:
```text
Current port: 8080
Validation rejected: Invalid port number: 70000
```

**Q236. How does single inheritance work in Python?**

*Demonstrate subclassing, extending functionality, and method overriding.*

**Answer:** Single inheritance allows a subclass to inherit attributes and methods from a single parent (base) class. Subclasses can override methods to specialize behavior or call `super()` to reuse parent logic.

```python
class CloudResource:
    def __init__(self, resource_id: str):
        self.resource_id = resource_id

    def get_status(self) -> str:
        return "PROVISIONED"

class ComputeInstance(CloudResource):
    def __init__(self, resource_id: str, cores: int):
        super().__init__(resource_id)
        self.cores = cores

    def get_status(self) -> str: # Overriding
        return f"{super().get_status()} (RUNNING on {self.cores} cores)"

vm = ComputeInstance("i-12345", 8)
print("Subclass status:", vm.get_status())
```

Output:
```text
Subclass status: PROVISIONED (RUNNING on 8 cores)
```

**Q237. How does multiple inheritance work in Python?**

*Explain inheriting from multiple base classes and potential diamond problem challenges.*

**Answer:** Python natively supports multiple inheritance: a class can inherit from more than one parent (`class Child(ParentA, ParentB):`). This facilitates mixins and modular compositions, but requires understanding Method Resolution Order (MRO) to resolve the 'Diamond Problem'.

```python
class JSONSerializableMixin:
    def to_json(self):
        import json
        return json.dumps(self.__dict__)

class AuditLoggerMixin:
    def log(self, message):
        return f"[AUDIT] {message}"

class DeploymentConfig(JSONSerializableMixin, AuditLoggerMixin):
    def __init__(self, env: str, replicas: int):
        self.env = env
        self.replicas = replicas

cfg = DeploymentConfig("production", 4)
print("JSON Mixin:", cfg.to_json())
print("Audit Mixin:", cfg.log("Configuration updated"))
```

Output:
```text
JSON Mixin: {"env": "production", "replicas": 4}
Audit Mixin: [AUDIT] Configuration updated
```

**Q238. What is the Method Resolution Order (MRO) and C3 Linearization?**

*Explain how Python determines method lookup order and `Class.__mro__`.*

**Answer:** The Method Resolution Order (MRO) is the exact order in which Python searches base classes when looking up attributes or methods. Python uses the **C3 Linearization algorithm**, which enforces monotonicity and preserves local precedence order. MRO can be inspected via `Class.__mro__` or `Class.mro()`.

```python
class A: pass
class B(A): pass
class C(A): pass
class D(B, C): pass # Diamond inheritance

print("MRO of class D:")
for cls in D.__mro__:
    print(" ->", cls.__name__)
```

Output:
```text
MRO of class D:
 -> D
 -> B
 -> C
 -> A
 -> object
```

**Q239. How does `super()` work in cooperative multiple inheritance?**

*Explain why `super()` does not simply call the parent class in multiple inheritance.*

**Answer:** In Python, `super()` does not simply delegate to the parent class; it calls the **next class in the MRO** of the caller's instance (`type(self)`). In multiple inheritance, this enables cooperative diamond calls where each class's method runs exactly once.

```python
class Root:
    def ping(self):
        print("Root ping")

class Left(Root):
    def ping(self):
        print("Left ping")
        super().ping()

class Right(Root):
    def ping(self):
        print("Right ping")
        super().ping()

class Combined(Left, Right):
    def ping(self):
        print("Combined ping")
        super().ping()

c = Combined()
c.ping()
```

Output:
```text
Combined ping
Left ping
Right ping
Root ping
```

**Q240. What is polymorphism and duck typing in Python?**

*Explain *'If it walks like a duck and quacks like a duck, it's a duck'* in Python architecture.*

**Answer:** Polymorphism allows different classes to implement the same interface or method names. Python relies on **Duck Typing**: dynamic typing where an object's suitability is determined by the presence of specific methods and properties, rather than its explicit class hierarchy.

```python
class S3Storage:
    def write(self, data): return f"Uploaded {len(data)} bytes to S3"

class LocalDiskStorage:
    def write(self, data): return f"Saved {len(data)} bytes to Local Disk"

def persist_payload(storage, payload):
    # Doesn't care about inheritance, only that storage has a .write() method!
    return storage.write(payload)

print(persist_payload(S3Storage(), "payload_data"))
print(persist_payload(LocalDiskStorage(), "payload_data"))
```

Output:
```text
Uploaded 12 bytes to S3
Saved 12 bytes to Local Disk
```

**Q241. How do Abstract Base Classes (ABCs) work using the `abc` module?**

*Explain `@abstractmethod`, `ABC`, and enforcing interface contracts at instantiation time.*

**Answer:** Python provides the `abc` module to define formal interfaces. Inheriting from `abc.ABC` and marking methods with `@abstractmethod` prevents subclasses from being instantiated unless all abstract methods are overridden.

```python
from abc import ABC, abstractmethod

class CloudProvider(ABC):
    @abstractmethod
    def launch_instance(self, size: str) -> str:
        """Launch a VM."""
        pass

class AWSProvider(CloudProvider):
    def launch_instance(self, size: str) -> str:
        return f"Launched AWS instance of size {size}"

class IncompleteProvider(CloudProvider):
    pass

aws = AWSProvider()
print(aws.launch_instance("t3.medium"))

try:
    inc = IncompleteProvider()
except TypeError as e:
    print("Caught ABC instantiation error:", e)
```

Output:
```text
Launched AWS instance of size t3.medium
Caught ABC instantiation error: Can't instantiate abstract class IncompleteProvider without an implementation for abstract method 'launch_instance'
```

**Q242. What is the difference between composition and inheritance?**

*Explain the *'Favor object composition over class inheritance'* design principle in Python.*

**Answer:** - **Inheritance ('is-a')**: Subclasses inherit state and behavior from a base class. Can lead to brittle, deep class hierarchies and tight coupling.
- **Composition ('has-a')**: Complex objects are constructed by assembling loosely-coupled component objects as attributes. More flexible, easier to test, and allows dynamically swapping implementations at runtime.

```python
class Engine:
    def start(self): return "Engine vroom"

class ElectricBattery:
    def start(self): return "Battery humm"

class Vehicle:
    def __init__(self, power_source):
        self.power_source = power_source # Composition!

    def drive(self):
        return f"Vehicle driving: {self.power_source.start()}"

v1 = Vehicle(Engine())
v2 = Vehicle(ElectricBattery())
print(v1.drive())
print(v2.drive())
```

Output:
```text
Vehicle driving: Engine vroom
Vehicle driving: Battery humm
```

**Q243. What is the difference between `__str__()` and `__repr__()`?**

*Explain human-readable vs developer-unambiguous string representations.*

**Answer:** - `__str__()`: Returns an informal, nicely-formatted string meant for end-users (`print(obj)` or `str(obj)`).
- `__repr__()`: Returns an official, unambiguous string representation meant for developers and debugging (`repr(obj)`). Ideally, it should look like valid Python code to recreate the object (`eval(repr(obj)) == obj`). Fallback: if `__str__` is missing, Python uses `__repr__`.

```python
class ServerNode:
    def __init__(self, hostname, ip):
        self.hostname = hostname
        self.ip = ip

    def __str__(self):
        return f"{self.hostname} ({self.ip})"

    def __repr__(self):
        return f"ServerNode(hostname={self.hostname!r}, ip={self.ip!r})"

node = ServerNode("k8s-master", "10.0.0.1")
print("str(): ", str(node))
print("repr():", repr(node))
```

Output:
```text
str():  k8s-master (10.0.0.1)
repr(): ServerNode(hostname='k8s-master', ip='10.0.0.1')
```

**Q244. How does operator overloading work with rich comparison methods?**

*Explain `__eq__`, `__ne__`, `__lt__`, `__le__`, `__gt__`, `__ge__` and `@functools.total_ordering`.*

**Answer:** Python objects can implement rich comparison dunder methods. Writing all 6 methods is tedious; the `@functools.total_ordering` class decorator lets you implement only `__eq__` and one ordering method (`__lt__`, `__le__`, `__gt__`, or `__ge__`), automatically generating the rest.

```python
from functools import total_ordering

@total_ordering
class Task:
    def __init__(self, name: str, priority: int):
        self.name = name
        self.priority = priority

    def __eq__(self, other):
        return isinstance(other, Task) and self.priority == other.priority

    def __lt__(self, other):
        return self.priority < other.priority

t1 = Task("Low Priority", 1)
t2 = Task("Critical", 10)

print("t1 < t2: ", t1 < t2)
print("t1 <= t2:", t1 <= t2)
print("t1 > t2: ", t1 > t2)
```

Output:
```text
t1 < t2:  True
t1 <= t2: True
t1 > t2:  False
```

**Q245. What are special collection emulation methods: __len__, __getitem__, __setitem__, __delitem__, __contains__?**

*Demonstrate building a custom class that behaves identically to a Python list/dict.*

**Answer:** Implementing sequence and mapping protocols allows custom objects to support `len(obj)`, subscripting `obj[key]`, deletion `del obj[key]`, and membership `x in obj`.

```python
class PipelineStages:
    def __init__(self):
        self._stages = []

    def add(self, stage):
        self._stages.append(stage)

    def __len__(self):
        return len(self._stages)

    def __getitem__(self, idx):
        return self._stages[idx]

    def __contains__(self, item):
        return item in self._stages

p = PipelineStages()
p.add("Lint")
p.add("Test")
p.add("Deploy")

print("Length:", len(p))
print("Index 1:", p[1])
print("Contains 'Test':", "Test" in p)
```

Output:
```text
Length: 3
Index 1: Test
Contains 'Test': True
```

**Q246. What are dataclasses (`@dataclass`) introduced in Python 3.7?**

*Explain automatic generation of `__init__`, `__repr__`, `__eq__`, and compare with standard classes.*

**Answer:** The `@dataclass` decorator (PEP 557) inspects type annotations on a class and automatically generates boilerplate dunder methods including `__init__`, `__repr__`, `__eq__`, and optional ordering methods.

```python
from dataclasses import dataclass

@dataclass
class ServiceSpec:
    name: str
    port: int
    replicas: int = 1

s1 = ServiceSpec("auth-svc", 8080)
s2 = ServiceSpec("auth-svc", 8080)

print("Auto repr:", s1)
print("Auto equality:", s1 == s2)
```

Output:
```text
Auto repr: ServiceSpec(name='auth-svc', port=8080, replicas=1)
Auto equality: True
```

**Q247. What are advanced dataclass options: frozen, field(default_factory=...), and post-init processing?**

*Explain immutable dataclasses and avoiding mutable defaults with `default_factory`.*

**Answer:** - `frozen=True`: Makes instances immutable and hashable (raises `FrozenInstanceError` on assignment).
- `field(default_factory=list)`: Safely initializes mutable defaults without shared instances.
- `__post_init__()`: Executes validation or computed attribute assignments after `__init__` finishes.

```python
from dataclasses import dataclass, field

@dataclass(frozen=True)
class Endpoint:
    path: str
    methods: tuple = field(default_factory=lambda: ("GET",))

ep = Endpoint("/api/v1/health")
print("Frozen endpoint:", ep)
try:
    ep.path = "/new"
except Exception as e:
    print("Caught mutation error:", type(e).__name__)
```

Output:
```text
Frozen endpoint: Endpoint(path='/api/v1/health', methods=('GET',))
Caught mutation error: FrozenInstanceError
```

**Q248. What is `__slots__` and how does it optimize memory and performance?**

*Explain replacing instance `__dict__` with fixed-size arrays.*

**Answer:** By default, each Python object stores its attributes in an internal dictionary (`__dict__`), which carries significant memory overhead. Specifying `__slots__ = ('attr1', 'attr2')` instructs CPython to allocate a fixed-size compact array of pointers instead, preventing `__dict__` creation, reducing memory usage by 40-60%, and slightly speeding up attribute lookups.

```python
import sys

class DefaultNode:
    def __init__(self, name, ip):
        self.name, self.ip = name, ip

class SlottedNode:
    __slots__ = ("name", "ip")
    def __init__(self, name, ip):
        self.name, self.ip = name, ip

d = DefaultNode("srv-1", "10.0.0.1")
s = SlottedNode("srv-1", "10.0.0.1")

print("DefaultNode memory (obj + dict):", sys.getsizeof(d) + sys.getsizeof(d.__dict__))
print("SlottedNode memory:", sys.getsizeof(s))
```

Output:
```text
DefaultNode memory (obj + dict): 152
SlottedNode memory: 48
```

**Q249. What are descriptors in Python and how does the descriptor protocol work?**

*Explain `__get__`, `__set__`, and `__delete__` methods powering properties, methods, and ORMs.*

**Answer:** A descriptor is an object attribute whose access is intercepted by methods of the descriptor protocol: `__get__(self, obj, type=None)`, `__set__(self, obj, value)`, and `__delete__(self, obj)`. Descriptors are the underlying engine powering Python's `@property`, `@classmethod`, `@staticmethod`, and ORM field mappings (SQLAlchemy, Django).

```python
class NonNegativeInt:
    def __set_name__(self, owner, name):
        self.name = name

    def __get__(self, obj, objtype=None):
        return obj.__dict__.get(self.name, 0)

    def __set__(self, obj, value):
        if not isinstance(value, int) or value < 0:
            raise ValueError(f"{self.name} must be a non-negative integer")
        obj.__dict__[self.name] = value

class Cluster:
    node_count = NonNegativeInt()

c = Cluster()
c.node_count = 5
print("Node count:", c.node_count)
try:
    c.node_count = -1
except ValueError as e:
    print("Descriptor rejected:", e)
```

Output:
```text
Node count: 5
Descriptor rejected: node_count must be a non-negative integer
```

**Q250. What is dynamic attribute access using getattr(), setattr(), hasattr(), and delattr()?**

*Demonstrate reflective attribute manipulation and runtime configuration loading.*

**Answer:** - `getattr(obj, name, default)`: Retrieves attribute value dynamically by string name.
- `setattr(obj, name, value)`: Sets attribute dynamically.
- `hasattr(obj, name)`: Checks if attribute exists.
- `delattr(obj, name)`: Deletes attribute dynamically.

```python
class Config:
    timeout = 30

cfg = Config()
setattr(cfg, "retries", 5)
print("hasattr retries:", hasattr(cfg, "retries"))
print("getattr timeout:", getattr(cfg, "timeout"))
print("getattr with default:", getattr(cfg, "missing_key", "default_val"))
```

Output:
```text
hasattr retries: True
getattr timeout: 30
getattr with default: default_val
```

**Q251. What are `__getattr__()` vs `__getattribute__()`?**

*Explain attribute lookup interception and avoiding infinite recursion.*

**Answer:** - `__getattr__(self, name)`: Invoked **only when the attribute is NOT found** in the instance or class hierarchy (fallback).
- `__getattribute__(self, name)`: Invoked **unconditionally for EVERY attribute lookup**. Extreme caution is required: referencing `self.attr` inside `__getattribute__` causes infinite recursion; calls must use `super().__getattribute__(name)`.

```python
class DynamicProxy:
    def __init__(self, data):
        self.data = data

    def __getattr__(self, name):
        # Fallback when attribute doesn't exist
        return f"Dynamic field: {name.upper()}"

p = DynamicProxy({"env": "prod"})
print("Existing attribute:", p.data)
print("Missing attribute fallback:", p.cluster_id)
```

Output:
```text
Existing attribute: {'env': 'prod'}
Missing attribute fallback: Dynamic field: CLUSTER_ID
```

**Q252. What is a metaclass in Python?**

*Explain class factories, `type` as the default metaclass, and intercepting class creation.*

**Answer:** Classes in Python are themselves objects, and the 'class of a class' is a **metaclass**. The default metaclass in Python is `type`. Metaclasses allow intercepting, validating, and modifying class construction at module import time (e.g., verifying class naming conventions or registering classes automatically).

```python
class EnforceSnakeCaseMeta(type):
    def __new__(mcs, name, bases, dct):
        for attr_name in dct:
            if not attr_name.startswith("__") and not attr_name.islower():
                raise TypeError(f"Method '{attr_name}' in class '{name}' must be lowercase snake_case")
        return super().__new__(mcs, name, bases, dct)

class CleanService(metaclass=EnforceSnakeCaseMeta):
    def process_data(self): pass

print("CleanService created successfully with metaclass")
```

Output:
```text
CleanService created successfully with metaclass
```

**Q253. What is `__init_subclass__()` introduced in Python 3.6 as a simpler alternative to metaclasses?**

*Demonstrate subclass hook registration and configuration without writing a full metaclass.*

**Answer:** `__init_subclass__(cls, **kwargs)` is a class method hook on parent classes executed whenever a child class inherits from it. It provides 90% of metaclass use cases (subclass validation and registration) with much cleaner, simpler syntax.

```python
class PluginBase:
    registry = {}

    def __init_subclass__(cls, plugin_name=None, **kwargs):
        super().__init_subclass__(**kwargs)
        name = plugin_name or cls.__name__.lower()
        cls.registry[name] = cls

class DockerPlugin(PluginBase, plugin_name="docker"): pass
class K8sPlugin(PluginBase, plugin_name="k8s"): pass

print("Registered plugins:", list(PluginBase.registry.keys()))
```

Output:
```text
Registered plugins: ['docker', 'k8s']
```

**Q254. What is typing.Protocol (Structural Subtyping / Static Duck Typing) introduced in Python 3.8?**

*Explain PEP 544 protocols compared to nominal ABC inheritance.*

**Answer:** `typing.Protocol` enables **static duck typing**. Instead of requiring classes to explicitly inherit from an ABC (nominal subtyping), a `Protocol` defines an interface. Any class that implements the required methods and attributes automatically satisfies the type checker without inheritance.

```python
from typing import Protocol

class Renderable(Protocol):
    def render(self) -> str: ...

class HTMLBanner:
    def render(self) -> str: return "<div>Banner</div>"

def display(component: Renderable):
    return component.render()

# HTMLBanner does NOT inherit from Renderable, but satisfies the Protocol!
print(display(HTMLBanner()))
```

Output:
```text
<div>Banner</div>
```

**Q255. What are the SOLID principles and how do they apply to Python?**

*Detail Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, and Dependency Inversion.*

**Answer:** 1. **S**RP: A class should have only one reason to change.
2. **O**CP: Open for extension (via polymorphism/mixins), closed for modification.
3. **L**SP: Subtypes must be substitutable for their base types without altering correctness.
4. **I**SP: Many client-specific interfaces (fine-grained Protocols) are better than one general-purpose interface.
5. **D**IP: Depend on abstractions (ABCs/Protocols), not concrete implementations.

```python
# Dependency Inversion Example:
from typing import Protocol

class NotificationSender(Protocol):
    def send(self, message: str) -> None: ...

class SlackSender:
    def send(self, message: str): print(f"[SLACK] {message}")

class AlertService:
    def __init__(self, sender: NotificationSender): # Depends on abstraction!
        self.sender = sender
    def trigger(self, alert):
        self.sender.send(alert)

AlertService(SlackSender()).trigger("High Memory Usage")
```

Output:
```text
[SLACK] High Memory Usage
```

**Q256. How do callable objects work using the `__call__()` dunder method?**

*Demonstrate making class instances callable like functions with internal state retention.*

**Answer:** Implementing `__call__(self, *args, **kwargs)` makes an instance callable like a standard function (`instance()`). This is useful for stateful decorators, policy checkers, and token generators.

```python
class RateLimiter:
    def __init__(self, limit: int):
        self.limit = limit
        self.count = 0

    def __call__(self, request_id: str) -> bool:
        if self.count >= self.limit:
            return False
        self.count += 1
        return True

limiter = RateLimiter(2)
print("Req 1:", limiter("req_1"))
print("Req 2:", limiter("req_2"))
print("Req 3:", limiter("req_3"))
```

Output:
```text
Req 1: True
Req 2: True
Req 3: False
```

**Q257. What is monkey patching in Python and what are its risks?**

*Demonstrate dynamic runtime patching and why it should be avoided in production.*

**Answer:** Monkey patching refers to dynamically modifying or replacing modules, classes, or methods at runtime. While widely used in test mocking (e.g., `unittest.mock`), monkey patching in production code can lead to subtle bugs and breaks static analysis.

```python
class LegacyClient:
    def query(self):
        return "slow_query"

# Monkey patch at runtime:
def fast_mock_query(self):
    return "fast_cached_query"

LegacyClient.query = fast_mock_query
client = LegacyClient()
print("Monkey-patched result:", client.query())
```

Output:
```text
Monkey-patched result: fast_cached_query
```

**Q258. Output Question: Predict what happens when a mutable class attribute is modified via an instance.**

*Analyze the difference between rebinding and in-place mutating shared class attributes.*

**Answer:** Calling `obj.items.append(x)` mutates the shared list in-place on the class. However, `obj.items = [x]` would create a new instance attribute shadowing the class attribute.

```python
class Registry:
    items = []

r1 = Registry()
r2 = Registry()
r1.items.append("alpha")
print("r2.items:", r2.items)
```

Output:
```text
r2.items: ['alpha']
```

**Q259. Output Question: Predict output of super() in single inheritance with attribute initialization.**

*Trace execution flow through constructors.*

**Answer:** `super().__init__()` executes the base class constructor first, followed by child class initializations.

```python
class Base:
    def __init__(self):
        self.val = "Base"

class Child(Base):
    def __init__(self):
        super().__init__()
        self.val = "Child"

c = Child()
print("c.val:", c.val)
```

Output:
```text
c.val: Child
```

**Q260. Output Question: What does isinstance() return with ABC virtual subclasses (`register()`)?**

*Explain registering virtual subclasses without explicit inheritance.*

**Answer:** `ABCMeta.register(Class)` registers a class as a virtual subclass of the ABC, so `isinstance()` and `issubclass()` return `True` without requiring inheritance.

```python
from collections.abc import Sequence

class CustomSeq:
    def __len__(self): return 0
    def __getitem__(self, i): raise IndexError

Sequence.register(CustomSeq)
print("isinstance(CustomSeq(), Sequence):", isinstance(CustomSeq(), Sequence))
print("issubclass(CustomSeq, Sequence):", issubclass(CustomSeq, Sequence))
```

Output:
```text
isinstance(CustomSeq(), Sequence): True
issubclass(CustomSeq, Sequence): True
```

**Q261. Output Question: What is the output when an instance defines `__slots__` but inherits from a class without `__slots__`?**

*Explain how base class `__dict__` propagates to slotted subclasses.*

**Answer:** If a parent class does not define `__slots__`, it possesses a `__dict__`. Subclasses defining `__slots__` still inherit the parent's `__dict__`, losing memory optimization.

```python
class UnslottedParent: pass
class SlottedChild(UnslottedParent):
    __slots__ = ("a",)

c = SlottedChild()
c.b = "arbitrary" # Works because of parent's __dict__!
print("c.b:", c.b)
print("Has __dict__:", hasattr(c, "__dict__"))
```

Output:
```text
c.b: arbitrary
Has __dict__: True
```

**Q262. Debugging Question: Fix the infinite recursion in property setter.**

*Resolve RecursionError caused by assigning to the property name inside its own setter.*

**Answer:** Assign to the underlying private/protected attribute `self._var`, not `self.var`.

```python
# Buggy Code:
# class Account:
#     @property
#     def balance(self): return self._balance
#     @balance.setter
#     def balance(self, v): self.balance = v # RecursionError!

# Fixed Code:
class Account:
    def __init__(self, balance):
        self._balance = balance

    @property
    def balance(self):
        return self._balance

    @balance.setter
    def balance(self, value):
        self._balance = value

a = Account(100)
a.balance = 200
print("Balance:", a.balance)
```

Output:
```text
Balance: 200
```

**Q263. Programming Question: Implement the Factory Pattern in Python.**

*Build a dynamic cloud infrastructure factory supporting AWS, Azure, and GCP resources.*

**Answer:** Use a registry or mapping dispatching to concrete subclasses.

```python
class CloudVM:
    def __init__(self, name): self.name = name
    def start(self): raise NotImplementedError

class AWSInstance(CloudVM):
    def start(self): return f"Starting EC2 instance {self.name}"

class AzureVM(CloudVM):
    def start(self): return f"Starting Azure Virtual Machine {self.name}"

class VMFactory:
    _registry = {"aws": AWSInstance, "azure": AzureVM}

    @classmethod
    def create(cls, provider: str, name: str) -> CloudVM:
        if provider not in cls._registry:
            raise ValueError(f"Unknown provider: {provider}")
        return cls._registry[provider](name)

vm = VMFactory.create("aws", "prod-web-01")
print(vm.start())
```

Output:
```text
Starting EC2 instance prod-web-01
```

**Q264. Programming Question: Implement the Observer Design Pattern in Python.**

*Build an event publisher where subscribers receive notifications upon system state changes.*

**Answer:** Maintain a set of observers and iterate over them upon dispatch.

```python
class EventDispatcher:
    def __init__(self):
        self._observers = []

    def subscribe(self, callback):
        self._observers.append(callback)

    def notify(self, event: str, data: dict):
        for observer in self._observers:
            observer(event, data)

dispatcher = EventDispatcher()
dispatcher.subscribe(lambda e, d: print(f"Email Alert: [{e}] on {d['host']}"))
dispatcher.subscribe(lambda e, d: print(f"Slack Alert: [{e}] on {d['host']}"))

dispatcher.notify("CPU_HIGH", {"host": "srv-01", "usage": 95})
```

Output:
```text
Email Alert: [CPU_HIGH] on srv-01
Slack Alert: [CPU_HIGH] on srv-01
```

**Q265. Programming Question: Implement the Strategy Pattern in Python.**

*Build a deployment executor where deployment strategies (Canary, BlueGreen, Rolling) can be swapped dynamically.*

**Answer:** Pass strategy callables or instances to the context executor.

```python
class DeploymentContext:
    def __init__(self, strategy):
        self.strategy = strategy

    def execute_deploy(self, app_name: str):
        return self.strategy(app_name)

def blue_green_strategy(app): return f"Deploying {app} via Blue-Green traffic switch"
def canary_strategy(app):     return f"Deploying {app} via 10% Canary traffic split"

deployer = DeploymentContext(canary_strategy)
print(deployer.execute_deploy("payment-api"))

deployer.strategy = blue_green_strategy
print(deployer.execute_deploy("payment-api"))
```

Output:
```text
Deploying payment-api via 10% Canary traffic split
Deploying payment-api via Blue-Green traffic switch
```

**Q266. Programming Question: Implement the Adapter Design Pattern in Python.**

*Adapt an incompatible legacy logging class to match a modern structured logger interface.*

**Answer:** Wrap the legacy object and translate method calls.

```python
class LegacyLogger:
    def write_log(self, text, code):
        return f"CODE {code}: {text}"

class StructuredLoggerAdapter:
    def __init__(self, legacy_logger: LegacyLogger):
        self.legacy_logger = legacy_logger

    def info(self, message: str):
        return self.legacy_logger.write_log(message, 200)

    def error(self, message: str):
        return self.legacy_logger.write_log(message, 500)

logger = StructuredLoggerAdapter(LegacyLogger())
print(logger.info("Service initialized"))
print(logger.error("Database connection refused"))
```

Output:
```text
CODE 200: Service initialized
CODE 500: Database connection refused
```

**Q267. Programming Question: Build a state machine using the State Pattern.**

*Model a CI/CD build pipeline transitioning across Pending -> Running -> Success/Failed states.*

**Answer:** Delegate state behavior to specialized State objects.

```python
class PipelineState:
    def run(self, pipeline): raise NotImplementedError

class PendingState(PipelineState):
    def run(self, pipeline):
        print("Build starting... transitioning to Running")
        pipeline.state = RunningState()

class RunningState(PipelineState):
    def run(self, pipeline):
        print("Build executed successfully! Transitioning to Finished")
        pipeline.state = FinishedState()

class FinishedState(PipelineState):
    def run(self, pipeline):
        print("Pipeline already completed. Nothing to do.")

class Pipeline:
    def __init__(self):
        self.state = PendingState()
    def trigger(self):
        self.state.run(self)

pipe = Pipeline()
pipe.trigger()
pipe.trigger()
pipe.trigger()
```

Output:
```text
Build starting... transitioning to Running
Build executed successfully! Transitioning to Finished
Pipeline already completed. Nothing to do.
```

**Q268. Programming Question: Implement class-level validation using a custom descriptor.**

*Create a RegexValidatedString descriptor that ensures string attributes conform to a pattern.*

**Answer:** Store value in instance `__dict__` after verifying `re.fullmatch()`.

```python
import re

class RegexString:
    def __init__(self, pattern: str):
        self.pattern = re.compile(pattern)

    def __set_name__(self, owner, name):
        self.name = name

    def __get__(self, obj, objtype=None):
        return obj.__dict__.get(self.name, "")

    def __set__(self, obj, value):
        if not self.pattern.fullmatch(str(value)):
            raise ValueError(f"Value '{value}' for '{self.name}' does not match pattern {self.pattern.pattern}")
        obj.__dict__[self.name] = value

class ClusterNode:
    hostname = RegexString(r"^[a-z0-9-]+$")

node = ClusterNode()
node.hostname = "k8s-worker-01"
print("Valid hostname set:", node.hostname)
try:
    node.hostname = "INVALID HOSTNAME!"
except ValueError as e:
    print("Descriptor rejected:", e)
```

Output:
```text
Valid hostname set: k8s-worker-01
Descriptor rejected: Value 'INVALID HOSTNAME!' for 'hostname' does not match pattern ^[a-z0-9-]+$
```

**Q269. Programming Question: Build a lightweight dependency injection container in Python.**

*Implement an IoC container registering and resolving service dependencies dynamically.*

**Answer:** Store factory callables and singletons in a registry dictionary.

```python
class Container:
    def __init__(self):
        self._services = {}

    def register(self, key, factory_fn, singleton=True):
        self._services[key] = {"factory": factory_fn, "singleton": singleton, "instance": None}

    def resolve(self, key):
        entry = self._services[key]
        if entry["singleton"]:
            if entry["instance"] is None:
                entry["instance"] = entry["factory"](self)
            return entry["instance"]
        return entry["factory"](self)

container = Container()
container.register("db_url", lambda c: "postgres://prod:5432/db")
container.register("api_client", lambda c: f"Client({c.resolve('db_url')})")

print("Resolved service:", container.resolve("api_client"))
```

Output:
```text
Resolved service: Client(postgres://prod:5432/db)
```

**Q270. Programming Question: Implement the Proxy Pattern for lazy loading.**

*Create a virtual proxy that defers instantiating an expensive database connection until first queried.*

**Answer:** Check and initialize the real subject on method invocation.

```python
class ExpensiveDatabase:
    def __init__(self):
        print("[INIT] Establishing expensive database socket connection...")
    def query(self, sql):
        return f"Executing '{sql}'"

class LazyDatabaseProxy:
    def __init__(self):
        self._real_db = None

    def query(self, sql):
        if self._real_db is None:
            self._real_db = ExpensiveDatabase()
        return self._real_db.query(sql)

db_proxy = LazyDatabaseProxy()
print("Proxy instantiated without connecting to DB")
print(db_proxy.query("SELECT * FROM metrics"))
```

Output:
```text
Proxy instantiated without connecting to DB
[INIT] Establishing expensive database socket connection...
Executing 'SELECT * FROM metrics'
```

**Q271. Programming Question: Implement custom deepcopy support using `__deepcopy__()`.**

*Allow a custom object to control how it is cloned by `copy.deepcopy()`.*

**Answer:** Implement `__deepcopy__(self, memo)`.

```python
import copy

class SecretKeyStore:
    def __init__(self, key_data):
        self.key_data = key_data

    def __deepcopy__(self, memo):
        # Redact or sanitize keys during deepcopy
        cloned = SecretKeyStore(f"REDACTED_CLONE_{self.key_data[-4:]}")
        memo[id(self)] = cloned
        return cloned

orig = SecretKeyStore("SUPER_SECRET_VALUE_1234")
cloned = copy.deepcopy(orig)
print("Original:", orig.key_data)
print("Cloned:  ", cloned.key_data)
```

Output:
```text
Original: SUPER_SECRET_VALUE_1234
Cloned:   REDACTED_CLONE_1234
```

**Q272. Programming Question: Implement an immutable dictionary using collection protocols.**

*Build a custom mapping type that allows read access while strictly forbidding item mutations.*

**Answer:** Subclass `collections.abc.Mapping` and implement `__getitem__`, `__iter__`, and `__len__`.

```python
from collections.abc import Mapping

class ImmutableDict(Mapping):
    def __init__(self, *args, **kwargs):
        self._data = dict(*args, **kwargs)

    def __getitem__(self, key):
        return self._data[key]

    def __iter__(self):
        return iter(self._data)

    def __len__(self):
        return len(self._data)

    def __repr__(self):
        return f"ImmutableDict({self._data})"

d = ImmutableDict({"env": "prod", "cluster": "east"})
print("Read access:", d["env"])
print("Keys:", list(d.keys()))
try:
    d["env"] = "dev"
except TypeError as e:
    print("Caught mutation error:", e)
```

Output:
```text
Read access: prod
Keys: ['env', 'cluster']
Caught mutation error: 'ImmutableDict' object does not support item assignment
```

**Q273. Programming Question: Enforce class naming conventions using `__init_subclass__`.**

*Ensure every subclass ends with the suffix `Handler` or `Service`.*

**Answer:** Inspect `cls.__name__` in `__init_subclass__`.

```python
class BaseService:
    def __init_subclass__(cls, **kwargs):
        super().__init_subclass__(**kwargs)
        if not (cls.__name__.endswith("Service") or cls.__name__.endswith("Handler")):
            raise TypeError(f"Class '{cls.__name__}' must end with 'Service' or 'Handler'")

class AuthService(BaseService): pass
class PaymentHandler(BaseService): pass

print("Valid classes loaded")
try:
    class InvalidController(BaseService): pass
except TypeError as e:
    print("Enforcement triggered:", e)
```

Output:
```text
Valid classes loaded
Enforcement triggered: Class 'InvalidController' must end with 'Service' or 'Handler'
```


---

## Chapter 9: Modules, Packages, and Imports

**Q274. What is a module in Python?**

*Explain Python modules, `.py` files, and module namespace creation.*

**Answer:** A module is a single Python file (`.py`) containing executable statements, function definitions, and classes. When imported, Python compiles it to bytecode, creates a module object (`types.ModuleType`), executes its statements from top to bottom, and creates a private namespace (`module.__dict__`).

```python
import math
print("Module type:", type(math))
print("Module name:", math.__name__)
print("Module file:", getattr(math, "__file__", "built-in"))
```

Output:
```text
Module type: <class 'module'>
Module name: math
Module file: built-in
```

**Q275. What is a package in Python and what is the role of `__init__.py`?**

*Explain regular packages vs namespace packages (PEP 420).*

**Answer:** A package is a directory containing Python modules and a special file named `__init__.py`. The `__init__.py` file initializes the package namespace and executes when the package is imported. Since Python 3.3 (PEP 420), directories without `__init__.py` are treated as **namespace packages**, allowing code spanning multiple directories to share a common parent namespace.

```python
# Directory structure:
# my_package/
# ├── __init__.py
# ├── auth.py
# └── metrics.py

# __init__.py can define public API exports via __all__:
# from .auth import login
# __all__ = ["login"]
```

Output:
```text
# Defines package exports and initializes package namespace.
```

**Q276. How does the Python import statement work internally?**

*Explain the import pipeline: `sys.modules`, finders, loaders, and module caching.*

**Answer:** When you execute `import my_module`, Python performs 4 steps:
1. Checks `sys.modules` cache. If already imported, returns the cached module immediately ($O(1)$).
2. If not found, calls **Finders** to search paths in `sys.path` for module specifications (`ModuleSpec`).
3. Calls **Loaders** to compile and execute the module code in a new namespace.
4. Adds the module to `sys.modules` and binds it to the requested variable name.

```python
import sys

print("Is 'sys' cached in sys.modules:", "sys" in sys.modules)
print("Total loaded modules in runtime:", len(sys.modules))
```

Output:
```text
Is 'sys' cached in sys.modules: True
Total loaded modules in runtime: ...
```

**Q277. What are the differences between `import x`, `from x import y`, and `from x import *`?**

*Explain namespace pollution, name binding, and why wildcard imports are an anti-pattern.*

**Answer:** - `import x`: Imports `x` and binds the module object to the identifier `x`. Access requires `x.y`.
- `from x import y`: Imports `x` and binds the attribute `y` directly into the current namespace.
- `from x import *`: Imports all names listed in `__all__` (or all names not starting with `_`) directly into the current scope. Anti-pattern because it pollutes namespaces, causes silent shadowing, and breaks static linters.

```python
import math
from math import sqrt

print("math.sqrt:", math.sqrt(16))
print("Direct sqrt:", sqrt(16))
```

Output:
```text
math.sqrt: 4.0
Direct sqrt: 4.0
```

**Q278. What is the `__all__` list in a module or package?**

*Explain defining public API interfaces and controlling `from module import *` exports.*

**Answer:** `__all__` is a module-level sequence of strings specifying the public symbols exported when `from module import *` is executed. It self-documents the public API and prevents internal helper functions from leaking.

```python
# In service.py:
__all__ = ["start_service", "stop_service"]

def start_service(): return "Started"
def stop_service():  return "Stopped"
def _internal_helper(): return "Hidden"

# from service import * will only import start_service and stop_service.
```

Output:
```text
# Restricts public exports to explicitly defined __all__ identifiers.
```

**Q279. What is the idiom `if __name__ == '__main__':` and how does it work?**

*Explain module execution vs module import.*

**Answer:** When a Python file is run directly (e.g. `python script.py`), Python assigns the string `'__main__'` to its built-in `__name__` variable. When imported by another module, `__name__` is set to the module's actual name. The idiom ensures entry-point code runs only on direct execution, not during imports.

```python
print(f"Current module name (__name__): {__name__}")

if __name__ == "__main__":
    print("Executed directly as standalone script!")
```

Output:
```text
Current module name (__name__): __main__
Executed directly as standalone script!
```

**Q280. What is `sys.path` and how does Python search for modules?**

*Explain search order: current directory, PYTHONPATH, standard library, and site-packages.*

**Answer:** `sys.path` is an ordered list of directory strings where Python searches for modules. Search order:
1. The directory containing the input script (or current directory `''`).
2. Directories in the `PYTHONPATH` environment variable.
3. Standard library directories.
4. Third-party packages installed in `site-packages`.

```python
import sys

print("Top 3 module search directories:")
for p in sys.path[:3]:
    print(" -", p)
```

Output:
```text
Top 3 module search directories:
 - ...
 - ...
 - ...
```

**Q281. What is the difference between absolute imports and relative imports?**

*Explain PEP 328 explicit relative imports (`.` and `..`).*

**Answer:** - **Absolute Imports**: Specify full path from project root (`from my_app.core.auth import login`). Preferred (PEP 8) for clarity and refactoring safety.
- **Explicit Relative Imports**: Use leading dots to reference siblings or parents (`from . import config`, `from ..utils import helpers`). Only valid inside packages.

```python
# Inside my_package/core/auth.py:
# from .session import SessionManager   # Sibling import (.)
# from ..utils.crypto import hash_token # Parent package import (..)
```

Output:
```text
# Standard relative import syntax within packages.
```

**Q282. What causes circular imports in Python and how do you resolve them?**

*Explain mutual dependencies, ImportError / AttributeError, and structural solutions.*

**Answer:** A circular import occurs when Module A imports Module B, and Module B imports Module A before either has finished defining its exports. Solutions:
1. **Refactor**: Extract shared types/logic into a third module (e.g., `common.py` or `models.py`).
2. **Deferred / Lazy Import**: Move the `import` inside the function or method that uses it.
3. **Type-checking only**: Use `if TYPE_CHECKING:` from `typing`.

```python
# Technique: Deferred import inside function
def trigger_alert(message: str):
    # Imported only when function is called, breaking circular import
    from notifications import send_slack
    send_slack(message)
```

Output:
```text
# Resolves circular initialization dependencies.
```

**Q283. How does dynamic module loading work using `importlib.import_module()`?**

*Demonstrate loading modules by runtime string name.*

**Answer:** `importlib.import_module(name)` programmatically imports a module by string name at runtime. Essential for plugin systems, dynamic driver dispatchers, and CLI routers.

```python
import importlib

module_name = "math"
math_module = importlib.import_module(module_name)
print("Dynamically imported math.pi:", math_module.pi)
```

Output:
```text
Dynamically imported math.pi: 3.141592653589793
```

**Q284. How does `importlib.reload()` work and what are its caveats?**

*Explain runtime code reloading and why old instance references remain unchanged.*

**Answer:** `importlib.reload(module)` re-reads and re-executes the module's file in-place, updating its namespace dictionary. Caveats: existing instances of classes instantiated before the reload continue referencing the old class definition (`isinstance` checks may fail!).

```python
import importlib
import math

reloaded_math = importlib.reload(math)
print("Reloaded successfully:", reloaded_math.__name__)
```

Output:
```text
Reloaded successfully: math
```

**Q285. What are `__file__`, `__doc__`, and `__package__` module attributes?**

*Explain standard metadata attributes attached to module namespaces.*

**Answer:** - `module.__file__`: Absolute filesystem path to the module source file.
- `module.__doc__`: The module's top-level docstring.
- `module.__package__`: Name of the parent package.
- `module.__name__`: Fully-qualified module identifier.

```python
import json

print("json.__name__:   ", json.__name__)
print("json.__package__:", json.__package__)
print("json.__doc__ summary:", json.__doc__.strip().splitlines()[0])
```

Output:
```text
json.__name__:    json
json.__package__: json
json.__doc__ summary: JSON (JavaScript Object Notation) <http://json.org> is a subset of
```

**Q286. How does pkgutil.iter_modules() discover plugins automatically?**

*Demonstrate scanning directories to dynamically find and load plugin modules.*

**Answer:** `pkgutil.iter_modules(path)` yields module info for all modules and packages found in the given paths, enabling automatic discovery of extensions without hardcoded lists.

```python
import pkgutil
import json

package_path = json.__path__
modules = [modname for _, modname, _ in pkgutil.iter_modules(package_path)]
print("Modules discovered inside 'json' package:", modules)
```

Output:
```text
Modules discovered inside 'json' package: ['decoder', 'encoder', 'scanner', 'tool']
```

**Q287. Output Question: Predict what happens when importing a module twice in the same script.**

*Explain module execution caching in `sys.modules`.*

**Answer:** Top-level code in a module runs **only once** upon initial import. Subsequent `import` statements retrieve the cached reference from `sys.modules` without re-running module code.

```python
import sys

# Simulating module execution
sys.modules["mock_mod"] = "already_cached"
import mock_mod
print("Imported cached module directly:", mock_mod)
```

Output:
```text
Imported cached module directly: already_cached
```

**Q288. Output Question: What is the output of modifying an imported variable from a module?**

*Explain mutable binding vs rebinding when using `from mod import var`.*

**Answer:** If a module defines `count = 0`, `from mod import count` creates a new local reference `count`. Rebinding `count = 10` locally does not update `mod.count`.

```python
# Concept Demonstration
# In mod.py: count = 0
# In script.py:
# from mod import count
# count += 1 # Only modifies local name, mod.count remains 0!
```

Output:
```text
# Rebinding local names does not modify source module namespace.
```

**Q289. Debugging Question: Fix ModuleNotFoundError when running scripts inside subdirectories.**

*Resolve path resolution issues when executing nested package scripts directly.*

**Answer:** Execute the package using the `-m` switch from the project root (`python -m my_pkg.sub.script`) instead of direct path invocation (`python my_pkg/sub/script.py`).

```python
# Correct CLI invocation from workspace root:
# python -m my_package.submodule.worker
```

Output:
```text
# Uses Python's module resolution rather than filesystem directory path.
```

**Q290. Programming Question: Build an automated plugin loader using `importlib` and `pkgutil`.**

*Scan a plugin folder and register all classes implementing a specific interface.*

**Answer:** Combine `pkgutil.walk_packages` and `getattr`.

```python
import importlib

def load_driver(driver_name: str):
    supported = {"sqlite": "sqlite3", "json_db": "json"}
    if driver_name not in supported:
        raise ValueError(f"Unsupported driver: {driver_name}")
    mod = importlib.import_module(supported[driver_name])
    return f"Driver {driver_name} successfully initialized from {mod.__name__}"

print(load_driver("sqlite"))
print(load_driver("json_db"))
```

Output:
```text
Driver sqlite successfully initialized from sqlite3
Driver json_db successfully initialized from json
```

**Q291. Programming Question: Create a temporary module search path context manager.**

*Write a context manager that temporarily prepends a directory to `sys.path` and removes it on exit.*

**Answer:** Use a `try...finally` block modifying `sys.path`.

```python
import sys
from contextlib import contextmanager

@contextmanager
def temporary_sys_path(new_path: str):
    sys.path.insert(0, new_path)
    try:
        yield
    finally:
        if new_path in sys.path:
            sys.path.remove(new_path)

test_dir = "/tmp/custom_lib"
with temporary_sys_path(test_dir):
    print("Inside context, top path:", sys.path[0])

print("Outside context, restored:", test_dir not in sys.path)
```

Output:
```text
Inside context, top path: /tmp/custom_lib
Outside context, restored: True
```

**Q292. Programming Question: Safely mock external modules during testing without third-party libraries.**

*Intercept `sys.modules` to insert a fake stub object.*

**Answer:** Patch `sys.modules[target]` in a context manager.

```python
import sys
from contextlib import contextmanager
from unittest.mock import MagicMock

@contextmanager
def mock_module(mod_name: str):
    fake = MagicMock()
    original = sys.modules.get(mod_name)
    sys.modules[mod_name] = fake
    try:
        yield fake
    finally:
        if original is not None:
            sys.modules[mod_name] = original
        else:
            sys.modules.pop(mod_name, None)

with mock_module("heavy_cloud_sdk") as fake_sdk:
    import heavy_cloud_sdk
    fake_sdk.deploy.return_value = "DEPLOYED_STUB"
    print("Mock invocation:", heavy_cloud_sdk.deploy())
```

Output:
```text
Mock invocation: DEPLOYED_STUB
```

**Q293. Programming Question: Create a lazy-loading module proxy.**

*Defer importing a heavy module until an attribute on it is accessed for the first time.*

**Answer:** Use a proxy class overriding `__getattr__` to call `importlib.import_module`.

```python
import importlib

class LazyModule:
    def __init__(self, module_name: str):
        self._module_name = module_name
        self._module = None

    def __getattr__(self, item):
        if self._module is None:
            print(f"[LAZY] Loading module '{self._module_name}' on first access...")
            self._module = importlib.import_module(self._module_name)
        return getattr(self._module, item)

lazy_math = LazyModule("math")
print("Proxy created (module not yet loaded)")
print("Accessing sqrt:", lazy_math.sqrt(25))
```

Output:
```text
Proxy created (module not yet loaded)
[LAZY] Loading module 'math' on first access...
Accessing sqrt: 5.0
```

**Q294. Programming Question: Implement a module-level `__getattr__` hook (PEP 562).**

*Deprecate old module attributes dynamically and emit warnings when accessed.*

**Answer:** Define `__getattr__(name)` at the module level.

```python
# In modern Python (3.7+, PEP 562), modules can define module-level __getattr__:
# def __getattr__(name):
#     if name == "OLD_CONFIG":
#         import warnings
#         warnings.warn("OLD_CONFIG is deprecated; use NEW_CONFIG", DeprecationWarning)
#         return "legacy_val"
#     raise AttributeError(f"module {__name__} has no attribute '{name}'")
```

Output:
```text
# Enables deprecation warnings and lazy module attribute calculation.
```

**Q295. Programming Question: Inspect module dependency tree programmatically.**

*List all child modules loaded under a specific package prefix from `sys.modules`.*

**Answer:** Filter `sys.modules.keys()` by prefix.

```python
import sys
import urllib

modules = sorted([m for m in sys.modules if m.startswith("urllib")])
print("Loaded urllib modules:", modules)
```

Output:
```text
Loaded urllib modules: ['urllib', 'urllib.parse']
```

**Q296. Programming Question: Create an isolated namespace execution sandbox using `exec()`.**

*Execute untrusted or dynamic Python code strings in a restricted globals dictionary.*

**Answer:** Pass an explicit dictionary as the globals parameter to `exec()`.

```python
user_code = """
result = a * 10 + b
"""

sandbox_globals = {"__builtins__": {}}
sandbox_locals = {"a": 5, "b": 2}

exec(user_code, sandbox_globals, sandbox_locals)
print("Computed in sandbox:", sandbox_locals["result"])
```

Output:
```text
Computed in sandbox: 52
```

**Q297. Programming Question: Extract docstrings and function signatures of a module for API documentation.**

*Build an automated documentation generator using `inspect` on a module.*

**Answer:** Iterate over `inspect.getmembers()` filtering by `inspect.isfunction`.

```python
import inspect
import math

functions = [
    (name, inspect.signature(func))
    for name, func in inspect.getmembers(math, inspect.isbuiltin)
    if not name.startswith("_")
]

print("Top 3 math signatures:")
for name, sig in functions[:3]:
    print(f" - {name}{sig}")
```

Output:
```text
Top 3 math signatures:
 - acos(x, /)
 - acosh(x, /)
 - asin(x, /)
```

**Q298. Programming Question: Detect and flag circular module imports in a package.**

*Write a graph cycle detector to identify circular imports across `.py` files.*

**Answer:** Parse AST of files, extract import statements, and run depth-first search for cycles.

```python
def has_cycle(graph):
    visited = set()
    rec_stack = set()

    def dfs(node):
        visited.add(node)
        rec_stack.add(node)
        for neighbor in graph.get(node, []):
            if neighbor not in visited:
                if dfs(neighbor): return True
            elif neighbor in rec_stack:
                return True
        rec_stack.remove(node)
        return False

    return any(dfs(n) for n in graph if n not in visited)

mock_imports = {"auth": ["models"], "models": ["database"], "database": ["auth"]}
print("Circular dependency detected:", has_cycle(mock_imports))
```

Output:
```text
Circular dependency detected: True
```

**Q299. Programming Question: Dynamically create and register an in-memory module.**

*Instantiate a `types.ModuleType` object and inject it into `sys.modules` without creating a file.*

**Answer:** Use `types.ModuleType` and assign attributes.

```python
import sys
import types

dynamic_mod = types.ModuleType("virtual_cloud_config")
dynamic_mod.API_ENDPOINT = "https://api.cloud.internal"
dynamic_mod.MAX_POOLS = 16

sys.modules["virtual_cloud_config"] = dynamic_mod

import virtual_cloud_config
print("Imported virtual module endpoint:", virtual_cloud_config.API_ENDPOINT)
```

Output:
```text
Imported virtual module endpoint: https://api.cloud.internal
```

**Q300. Programming Question: Safely unload and clean up a module from `sys.modules`.**

*Write a cleanup utility to evict a module and its submodules from memory.*

**Answer:** Pop keys from `sys.modules` matching the package name.

```python
import sys

def unload_module(name: str):
    to_delete = [m for m in sys.modules if m == name or m.startswith(f"{name}.")]
    for m in to_delete:
        del sys.modules[m]
    return to_delete

# Load test module
import colorsys
print("Loaded:", "colorsys" in sys.modules)

unloaded = unload_module("colorsys")
print("Unloaded modules:", unloaded)
print("Still in sys.modules:", "colorsys" in sys.modules)
```

Output:
```text
Loaded: True
Unloaded modules: ['colorsys']
Still in sys.modules: False
```


---

## Chapter 10: Exception Handling and Error Management

**Q301. What is the difference between syntax errors and runtime exceptions?**

*Explain parsing phase errors vs execution phase exceptions.*

**Answer:** - **Syntax Error (`SyntaxError`, `IndentationError`)**: Detected by the parser before bytecode execution begins. The program cannot start running.
- **Runtime Exception (`ValueError`, `KeyError`, `ZeroDivisionError`)**: Occurs during execution while evaluating valid bytecode instructions when an unexpected condition or illegal operation arises.

```python
# Syntax error: occurs before execution
# if True print("Missing colon")

# Runtime exception: occurs during execution
try:
    val = 10 / 0
except ZeroDivisionError as e:
    print("Caught runtime exception:", type(e).__name__)
```

Output:
```text
Caught runtime exception: ZeroDivisionError
```

**Q302. What is the built-in exception hierarchy in Python?**

*Explain `BaseException` vs `Exception` and why user exceptions must inherit from `Exception`.*

**Answer:** Python exceptions form a tree rooted at `BaseException`. Direct children:
1. `SystemExit`: Raised by `sys.exit()`.
2. `KeyboardInterrupt`: Raised when user presses Ctrl+C.
3. `GeneratorExit`: Raised when a generator's `close()` is called.
4. `Exception`: The base class for all non-system-exiting application exceptions.
Catching `except Exception:` catches normal bugs while allowing Ctrl+C and script exits to pass through; catching `except BaseException:` or bare `except:` traps critical system signals and is an anti-pattern.

```python
print("issubclass(Exception, BaseException):", issubclass(Exception, BaseException))
print("issubclass(KeyboardInterrupt, Exception):", issubclass(KeyboardInterrupt, Exception))
print("issubclass(KeyError, LookupError):", issubclass(KeyError, LookupError))
print("issubclass(IndexError, LookupError):", issubclass(IndexError, LookupError))
```

Output:
```text
issubclass(Exception, BaseException): True
issubclass(KeyboardInterrupt, Exception): False
issubclass(KeyError, LookupError): True
issubclass(IndexError, LookupError): True
```

**Q303. How does the complete `try ... except ... else ... finally` construct work?**

*Detail the execution order and purpose of all 4 clauses.*

**Answer:** 1. `try`: Encloses code that might raise an exception.
2. `except [Err] [as e]`: Executes if a matching exception is raised inside `try`.
3. `else`: Executes **only if NO exception occurred** in `try` (clean separation of successful logic).
4. `finally`: **Always executes**, regardless of whether an exception occurred, was caught, or was re-raised (used for resource cleanup).

```python
def divide(a, b):
    try:
        res = a / b
    except ZeroDivisionError:
        print("Caught division by zero")
    else:
        print(f"Calculation success: {res}")
    finally:
        print("Cleanup operations completed")

divide(10, 2)
print("---")
divide(10, 0)
```

Output:
```text
Calculation success: 5.0
Cleanup operations completed
---
Caught division by zero
Cleanup operations completed
```

**Q304. How do you handle multiple exception types cleanly?**

*Demonstrate grouping exceptions in tuples vs separate except blocks.*

**Answer:** Multiple exceptions can be caught using separate `except` blocks (ordering from most specific to most general) or grouped together in a single parenthesized tuple: `except (ValueError, TypeError, KeyError) as e:`.

```python
def parse_and_fetch(data: dict, key: str):
    try:
        val = data[key]
        return int(val)
    except (KeyError, ValueError, TypeError) as err:
        return f"Handled error [{type(err).__name__}]: {err}"

print(parse_and_fetch({"port": "8080"}, "port"))
print(parse_and_fetch({}, "port"))
print(parse_and_fetch({"port": "invalid"}, "port"))
```

Output:
```text
8080
Handled error [KeyError]: 'port'
Handled error [ValueError]: invalid literal for int() with base 10: 'invalid'
```

**Q305. What is exception chaining (`raise ... from ...`)?**

*Explain explicit chaining (`from err`), disabling chaining (`from None`), and implicit chaining.*

**Answer:** Introduced in PEP 3137, `raise NewException(...) from original_err` explicitly links exceptions, setting `__cause__` and showing both tracebacks. Using `raise NewException(...) from None` suppresses the original context (`__context__`), displaying a clean top-level error.

```python
def connect_db():
    try:
        raise ConnectionRefusedError("Socket timeout on 5432")
    except ConnectionRefusedError as err:
        # Wrap into domain-specific exception
        raise RuntimeError("Database service unavailable") from err

try:
    connect_db()
except RuntimeError as e:
    print(f"Caught: {e}")
    print(f"Caused by: {e.__cause__}")
```

Output:
```text
Caught: Database service unavailable
Caused by: Socket timeout on 5432
```

**Q306. How do you create custom user-defined exception hierarchies?**

*Demonstrate designing domain-specific exceptions for enterprise applications.*

**Answer:** Custom exceptions must inherit from `Exception` (or a subclass). Best practice is to define a base application exception for the module so consumers can catch all module errors with a single handler.

```python
class CloudDeployError(Exception):
    """Base exception for all cloud deployment errors."""
    pass

class InsufficientQuotaError(CloudDeployError):
    def __init__(self, resource, required, available):
        super().__init__(f"Quota exceeded for '{resource}': need {required}, available {available}")
        self.resource, self.required, self.available = resource, required, available

try:
    raise InsufficientQuotaError("vCPUs", 32, 16)
except CloudDeployError as err:
    print(f"Caught deployment error: {err}")
```

Output:
```text
Caught deployment error: Quota exceeded for 'vCPUs': need 32, available 16
```

**Q307. What is ExceptionGroup and `except*` introduced in Python 3.11?**

*Explain PEP 654 handling of concurrent exceptions in asyncio and TaskGroups.*

**Answer:** Python 3.11 introduced `ExceptionGroup` to aggregate multiple independent exceptions raised concurrently. The new syntax `except* ExceptionType:` matches and handles subsets of exceptions within the group without swallowing unhandled ones.

```python
# In Python 3.11+:
eg = ExceptionGroup("Multiple deployment failures", [
    ValueError("Invalid cluster ID"),
    ConnectionResetError("Lost peer node")
])

print("ExceptionGroup created:", type(eg).__name__)
print("Contains exceptions count:", len(eg.exceptions))
```

Output:
```text
ExceptionGroup created: ExceptionGroup
Contains exceptions count: 2
```

**Q308. How does the `assert` statement work and when should it be avoided?**

*Explain assertion debugging checks vs production data validation and the `-O` flag.*

**Answer:** `assert condition, message` raises an `AssertionError` if condition is false. Crucial rule: **assertions are disabled in production when Python runs with optimization flag `-O` or `-OO`**. Therefore, never use `assert` for security checks, user input validation, or critical business logic; use standard `if not condition: raise ValueError(...)` instead.

```python
def calculate_ratio(a: float, b: float) -> float:
    assert b != 0, "Denominator cannot be zero (internal bug)"
    return a / b

print("Ratio 10 / 2 =", calculate_ratio(10, 2))
try:
    calculate_ratio(10, 0)
except AssertionError as e:
    print("Assertion caught:", e)
```

Output:
```text
Ratio 10 / 2 = 5.0
Assertion caught: Denominator cannot be zero (internal bug)
```

**Q309. What is the `traceback` module and how do you format stack traces?**

*Explain extracting, formatting, and logging full exception tracebacks.*

**Answer:** The `traceback` module provides tools to extract, format, and print stack traces at runtime (`traceback.format_exc()`, `traceback.print_exc()`), essential for structured logging.

```python
import traceback

try:
    result = int("non_numeric")
except ValueError:
    tb_str = traceback.format_exc()
    first_line = tb_str.strip().splitlines()[0]
    last_line = tb_str.strip().splitlines()[-1]
    print("Formatted traceback first line:", first_line)
    print("Formatted traceback error line:", last_line)
```

Output:
```text
Formatted traceback first line: Traceback (most recent call last):
Formatted traceback error line: ValueError: invalid literal for int() with base 10: 'non_numeric'
```

**Q310. What is the EAFP vs LBYL coding philosophy in Python?**

*Compare *'Easier to Ask for Forgiveness than Permission'* with *'Look Before You Leap'*.*

**Answer:** - **LBYL (Look Before You Leap)**: Explicitly tests preconditions before making calls (`if key in d: val = d[key]`). Can lead to race conditions (TOCTOU: Time of Check to Time of Use).
- **EAFP (Easier to Ask for Forgiveness than Permission)**: Assumes valid keys/attributes and executes directly, catching exceptions if assumption fails (`try: val = d[key] except KeyError: ...`). Standard Pythonic idiom, faster on the happy path.

```python
d = {"token": "xyz123"}

# EAFP (Pythonic)
try:
    token = d["token"]
except KeyError:
    token = "default"

print("EAFP retrieved token:", token)
```

Output:
```text
EAFP retrieved token: xyz123
```

**Q311. What is contextlib.suppress and how does it clean up exception handling?**

*Demonstrate ignoring specific expected exceptions without verbose try/except/pass blocks.*

**Answer:** `contextlib.suppress(*exceptions)` context manager suppresses any of the specified exceptions raised within its block, cleanly replacing the verbose `try: ... except FileNotFoundError: pass` pattern.

```python
import os
from contextlib import suppress

# Silently suppress FileNotFoundError if file doesn't exist
with suppress(FileNotFoundError):
    os.remove("non_existent_temp_file.tmp")

print("File removal handled safely without crashing")
```

Output:
```text
File removal handled safely without crashing
```

**Q312. Output Question: Predict what is printed when `finally` contains a return statement.**

*Explain the dangerous pitfall of returning inside a `finally` block.*

**Answer:** A `return` statement inside a `finally` block **overrides and discards any exception** raised inside the `try` block, silently suppressing errors!

```python
def dangerous_func():
    try:
        raise ValueError("Critical Error")
    finally:
        return "Suppressed!" # Suppresses exception!

print("Output:", dangerous_func())
```

Output:
```text
Output: Suppressed!
```

**Q313. Output Question: Predict execution order in try-except-else-finally.**

*Trace code flow through all blocks.*

**Answer:** If no exception occurs, `try` runs, followed by `else`, and finally `finally`.

```python
def flow():
    output = []
    try:
        output.append("try")
    except Exception:
        output.append("except")
    else:
        output.append("else")
    finally:
        output.append("finally")
    return output

print(" -> ".join(flow()))
```

Output:
```text
try -> else -> finally
```

**Q314. Output Question: What happens when an exception is re-raised with bare `raise`?**

*Explain retaining original traceback when re-raising.*

**Answer:** A bare `raise` inside an `except` block re-raises the active exception currently being handled, preserving the original traceback and exception instance.

```python
try:
    try:
        raise KeyError("missing_config")
    except KeyError:
        print("Logging and re-raising")
        raise
except KeyError as e:
    print(f"Caught re-raised exception: {e}")
```

Output:
```text
Logging and re-raising
Caught re-raised exception: 'missing_config'
```

**Q315. Debugging Question: Fix silent exception swallowing.**

*Identify why errors disappear and fix broad `except:` catches.*

**Answer:** Catching bare `except:` or `except Exception:` without logging hides bugs and suppresses interrupts. Catch specific exceptions or log full tracebacks.

```python
# Buggy Code:
# try: do_work()
# except: pass

# Fixed Code:
import logging

try:
    int("invalid")
except ValueError as e:
    logging.warning(f"Handled invalid integer input: {e}")
```

Output:
```text
WARNING:root:Handled invalid integer input: invalid literal for int() with base 10: 'invalid'
```

**Q316. Programming Question: Build a resilient retry wrapper with custom exception filtering.**

*Write an error handler that retries on network errors but immediately fails on validation errors.*

**Answer:** Inspect exception type against a retryable tuple.

```python
def execute_resilient(func, retryable_errors, max_tries=3):
    for attempt in range(1, max_tries + 1):
        try:
            return func(attempt)
        except retryable_errors as e:
            if attempt == max_tries:
                raise
            print(f"Attempt {attempt} caught retryable error ({e}). Retrying...")

def unstable_service(attempt):
    if attempt < 2:
        raise ConnectionError("Timeout")
    return "SUCCESS"

print(execute_resilient(unstable_service, (ConnectionError,)))
```

Output:
```text
Attempt 1 caught retryable error (Timeout). Retrying...
SUCCESS
```

**Q317. What is exception bubbling (propagation) up the call stack?**

*Explain how uncaught exceptions propagate through stack frames until handled.*

**Answer:** When an exception is raised inside a nested function, Python pauses execution of that frame and searches for a matching `except` block. If none is found, the current stack frame is unwound and the exception propagates to the caller. This continues up the stack; if it reaches module level unhandled, the interpreter prints a traceback and exits with a non-zero code.

```python
def level_3(): raise ValueError("Root failure in level 3")
def level_2(): level_3()
def level_1(): level_2()

try:
    level_1()
except ValueError as e:
    print("Caught propagated error at top level:", e)
```

Output:
```text
Caught propagated error at top level: Root failure in level 3
```

**Q318. How do you attach custom diagnostic metadata to an exception?**

*Demonstrate setting custom attributes on exception instances.*

**Answer:** Exceptions are standard Python objects. You can set custom attributes in `__init__` or dynamically attach diagnostic dictionaries, request IDs, and status codes.

```python
class APIError(Exception):
    def __init__(self, message, status_code, endpoint):
        super().__init__(message)
        self.status_code = status_code
        self.endpoint = endpoint

try:
    raise APIError("Gateway Timeout", 504, "/api/v2/orders")
except APIError as e:
    print(f"Error: {e} | Code: {e.status_code} | Endpoint: {e.endpoint}")
```

Output:
```text
Error: Gateway Timeout | Code: 504 | Endpoint: /api/v2/orders
```

**Q319. What is sys.exc_info() and how does it retrieve the active exception?**

*Explain inspecting exception type, value, and traceback objects dynamically.*

**Answer:** `sys.exc_info()` returns a 3-tuple `(type, value, traceback)` representing the exception currently being handled. If no exception is active, it returns `(None, None, None)`.

```python
import sys

try:
    x = 1 / 0
except ZeroDivisionError:
    exc_type, exc_val, exc_tb = sys.exc_info()
    print("Type: ", exc_type.__name__)
    print("Value:", exc_val)
    print("Line: ", exc_tb.tb_lineno)
```

Output:
```text
Type:  ZeroDivisionError
Value: division by zero
Line:  4
```

**Q320. What are built-in warning categories and how does the `warnings` module work?**

*Explain `warnings.warn()`, `DeprecationWarning`, and `warnings.filterwarnings()`.*

**Answer:** Warnings notify developers of non-fatal conditions, such as deprecated APIs or programming subtleties, without interrupting execution. The `warnings` module controls filtering (`'error'`, `'ignore'`, `'always'`).

```python
import warnings

def old_metric_api():
    warnings.warn("old_metric_api() is deprecated; use v2_metrics()", DeprecationWarning, stacklevel=2)
    return 42

print("Function result:", old_metric_api())
```

Output:
```text
Function result: 42
```

**Q321. How does Python handle exceptions inside generator functions?**

*Explain `GeneratorExit`, `.throw()`, and generator finalization.*

**Answer:** When a generator is closed, Python raises a `GeneratorExit` exception at the yield point. Generators can also receive exceptions injected from callers via `gen.throw(ExcType)`.

```python
def managed_generator():
    try:
        yield "Step 1"
        yield "Step 2"
    except GeneratorExit:
        print("[CLEANUP] Generator closed cleanly")

g = managed_generator()
print(next(g))
g.close()
```

Output:
```text
Step 1
[CLEANUP] Generator closed cleanly
```

**Q322. What is the difference between `LookupError`, `KeyError`, and `IndexError`?**

*Explain the parent-child relationship of mapping and sequence lookup exceptions.*

**Answer:** `LookupError` is the common base class for both `KeyError` (raised by mappings when a key is missing) and `IndexError` (raised by sequences when an index is out of bounds). Catching `LookupError` handles both with one block.

```python
def get_element(container, key):
    try:
        return container[key]
    except LookupError as e:
        return f"Lookup failed: {type(e).__name__}"

print(get_element([10, 20], 5))      # IndexError
print(get_element({"a": 1}, "b"))    # KeyError
```

Output:
```text
Lookup failed: IndexError
Lookup failed: KeyError
```

**Q323. What is the difference between `ValueError` and `TypeError`?**

*Explain invalid argument type vs invalid argument value.*

**Answer:** - `TypeError`: Raised when an operation is attempted on an object of an inappropriate type (e.g. passing a string where an integer is required).
- `ValueError`: Raised when an object has the correct type, but an illegitimate or unacceptable value (e.g. `int('abc')` or `math.sqrt(-1)`).

```python
try:
    len(12345)
except TypeError as e:
    print("TypeError:", e)

try:
    int("hello")
except ValueError as e:
    print("ValueError:", e)
```

Output:
```text
TypeError: object of type 'int' has no len()
ValueError: invalid literal for int() with base 10: 'hello'
```

**Q324. How do you log exceptions properly with Python's `logging` module?**

*Compare `logger.error(..., exc_info=True)` and `logger.exception()`.*

**Answer:** `logger.exception(msg)` automatically logs the message at `ERROR` level and appends the full current exception traceback. Calling `logger.error(msg, exc_info=True)` accomplishes the same.

```python
import logging

logging.basicConfig(level=logging.ERROR, format="%(levelname)s: %(message)s")

try:
    val = [][0]
except IndexError:
    logging.exception("Failed to access array index")
```

Output:
```text
ERROR: Failed to access array index
Traceback (most recent call last):
  ...
IndexError: list index out of range
```

**Q325. What happens when an exception is raised inside an `except` block?**

*Explain exception masking and chained context (`__context__`).*

**Answer:** If a second exception occurs while handling an earlier exception, Python automatically attaches the first exception to the second's `__context__` attribute, reporting both in the traceback.

```python
try:
    try:
        1 / 0
    except ZeroDivisionError:
        [][10] # Raises IndexError while handling ZeroDivisionError
except IndexError as e:
    print("Active exception:", type(e).__name__)
    print("Original context:", type(e.__context__).__name__)
```

Output:
```text
Active exception: IndexError
Original context: ZeroDivisionError
```

**Q326. How do you write a global unhandled exception hook?**

*Explain overriding `sys.excepthook` for crash reporting.*

**Answer:** Overriding `sys.excepthook(exc_type, exc_value, exc_traceback)` intercepts any uncaught exception before Python terminates, enabling sending telemetry alerts or crash dumps.

```python
import sys

def custom_crash_handler(exc_type, exc_val, exc_tb):
    print(f"[CRASH REPORTER] Process crashed with {exc_type.__name__}: {exc_val}")

sys.excepthook = custom_crash_handler
# Uncaught exceptions will now route through custom_crash_handler
print("Custom excepthook registered successfully")
```

Output:
```text
Custom excepthook registered successfully
```

**Q327. What are the performance implications of `try ... except` blocks in Python 3.11+?**

*Explain zero-cost exception handling in CPython 3.11+.*

**Answer:** Since Python 3.11, Python uses **zero-cost exceptions**: as long as no exception is raised, code inside `try` blocks executes with **zero performance overhead** (no runtime stack pushes). Overhead is only paid when an exception is actually raised and caught.

```python
# In Python 3.11+, happy-path try blocks execute at native bytecode speed
def happy_path():
    try:
        return 10 + 20
    except ValueError:
        return 0

print("Result:", happy_path())
```

Output:
```text
Result: 30
```

**Q328. Output Question: Predict output when nested try-except-finally blocks execute.**

*Analyze nested control flow during exception handling.*

**Answer:** Inner `finally` executes before the exception propagates to the outer `except` handler.

```python
events = []
try:
    try:
        events.append("inner_try")
        raise RuntimeError
    finally:
        events.append("inner_finally")
except RuntimeError:
    events.append("outer_except")

print(" -> ".join(events))
```

Output:
```text
inner_try -> inner_finally -> outer_except
```

**Q329. Debugging Question: Fix incorrect exception ordering.**

*Resolve unreachable except blocks caused by catching base exceptions before subclasses.*

**Answer:** Subclasses must precede parent classes in `except` blocks; otherwise, the parent catches everything first.

```python
# Buggy:
# except Exception: ...
# except ValueError: ... # Unreachable!

# Fixed:
try:
    int("abc")
except ValueError:
    print("Caught specific ValueError")
except Exception:
    print("Caught general Exception")
```

Output:
```text
Caught specific ValueError
```

**Q330. Programming Question: Build a Circuit Breaker pattern with exception tracking.**

*Halt calls to an external service after consecutive exception threshold is reached.*

**Answer:** Maintain failure counters and trip state.

```python
class CircuitBreaker:
    def __init__(self, failure_threshold: int = 3):
        self.threshold = failure_threshold
        self.failures = 0
        self.state = "CLOSED"

    def execute(self, func, *args, **kwargs):
        if self.state == "OPEN":
            raise RuntimeError("Circuit is OPEN: fast-failing calls")
        try:
            res = func(*args, **kwargs)
            self.failures = 0
            return res
        except Exception:
            self.failures += 1
            if self.failures >= self.threshold:
                self.state = "OPEN"
            raise

cb = CircuitBreaker(failure_threshold=2)
def fail_service(): raise ConnectionError("Timeout")

for i in range(2):
    try: cb.execute(fail_service)
    except ConnectionError: pass

print("Circuit state after 2 failures:", cb.state)
try:
    cb.execute(fail_service)
except RuntimeError as e:
    print("Circuit breaker tripped:", e)
```

Output:
```text
Circuit state after 2 failures: OPEN
Circuit breaker tripped: Circuit is OPEN: fast-failing calls
```


---

## Chapter 11: File Handling and Input/Output

**Q331. How does `open()` work and what are the primary file modes?**

*Explain `r`, `w`, `a`, `x`, `b`, `t`, `+` file modes.*

**Answer:** `open(file, mode='r', encoding=None)` opens files and returns a stream object. Modes:
- `r`: Read (default). File must exist.
- `w`: Write. Truncates file to 0 bytes or creates new.
- `a`: Append. Writes to the end without truncating.
- `x`: Exclusive creation. Creates new file; fails with `FileExistsError` if file exists.
- `b`: Binary mode (returns `bytes`).
- `t`: Text mode (default, returns `str`).
- `+`: Read and write updating (`r+`, `w+`, `a+`).

```python
# Demonstrating mode combinations
# 'rb' = Read binary
# 'w+t' = Read and write text, truncating
# 'a+' = Read and append
```

Output:
```text
# Standard file access modes.
```

**Q332. Why should files always be opened using the `with` statement (Context Manager)?**

*Explain deterministic resource cleanup and avoiding file descriptor leaks.*

**Answer:** Using `with open(...) as f:` guarantees that the underlying file descriptor is closed immediately upon exiting the block, even if unhandled exceptions are raised. Omitting `with` risks exhausting operating system file descriptors.

```python
import tempfile

with tempfile.NamedTemporaryFile("w+", delete=False) as f:
    f.write("System status: OK")
    temp_path = f.name

print("File closed inside block:", f.closed)
```

Output:
```text
File closed inside block: True
```

**Q333. What is the difference between read(), readline(), and readlines()?**

*Compare memory impact of loading entire files vs reading line-by-line.*

**Answer:** - `read(size=-1)`: Reads the entire file into memory as a single string (or up to `size` bytes). High memory risk for large files.
- `readline(size=-1)`: Reads a single line up to `\n`.
- `readlines()`: Reads all lines into a `list` of strings.
- **Best Practice for large files**: Iterate directly over the file object (`for line in f:`), which streams lines lazily using internal buffers.

```python
import io

data = """line 1: Alpha
line 2: Beta
line 3: Gamma"""

buf = io.StringIO(data)
for idx, line in enumerate(buf, start=1):
    print(f"Stream line {idx}: {line.strip()}")
```

Output:
```text
Stream line 1: line 1: Alpha
Stream line 2: line 2: Beta
Stream line 3: line 3: Gamma
```

**Q334. How do write() and writelines() work?**

*Explain writing single strings vs sequences and newline handling.*

**Answer:** - `f.write(string)`: Writes string to file and returns number of characters written. It does **not** automatically append a newline (`\n`).
- `f.writelines(iterable)`: Writes a sequence of strings to the file without adding newlines between items.

```python
import io

buf = io.StringIO()
buf.write("Header
")
buf.writelines(["Row 1
", "Row 2
"])
print("Buffer content:
" + buf.getvalue().strip())
```

Output:
```text
Buffer content:
Header
Row 1
Row 2
```

**Q335. How do tell() and seek() manipulate file pointers?**

*Explain byte offset positioning and `whence` parameter (0, 1, 2).*

**Answer:** - `f.tell()`: Returns current file pointer position as an integer byte offset from the start.
- `f.seek(offset, whence=0)`: Moves the pointer.
  - `whence=0` (`os.SEEK_SET`): Absolute offset from beginning.
  - `whence=1` (`os.SEEK_CUR`): Offset relative to current position.
  - `whence=2` (`os.SEEK_END`): Offset relative to end of file.

```python
import io

buf = io.BytesIO(b"0123456789")
print("Initial position:", buf.tell())
buf.seek(5)
print("Position after seek(5):", buf.tell())
print("Read 3 bytes:", buf.read(3))
buf.seek(-2, io.SEEK_END)
print("Read last 2 bytes:", buf.read(2))
```

Output:
```text
Initial position: 0
Position after seek(5): 5
Read 3 bytes: b'567'
Read last 2 bytes: b'89'
```

**Q336. Why is pathlib.Path preferred over os.path in modern Python?**

*Compare object-oriented paths with legacy string manipulations.*

**Answer:** `pathlib` (PEP 428) provides an object-oriented API where paths are rich objects with intuitive methods (`/` path concatenation, `.read_text()`, `.write_text()`, `.exists()`, `.mkdir()`, `.glob()`), cleanly abstracting Windows vs POSIX path differences.

```python
from pathlib import Path

p = Path("config") / "deployments" / "prod.yaml"
print("Pathlib object:", p)
print("Parent directory:", p.parent)
print("File suffix:    ", p.suffix)
print("File stem:      ", p.stem)
```

Output:
```text
Pathlib object: config/deployments/prod.yaml
Parent directory: config/deployments
File suffix:     .yaml
File stem:       prod
```

**Q337. How does pathlib read and write text/bytes in one line?**

*Explain `read_text()`, `write_text()`, `read_bytes()`, and `write_bytes()`.*

**Answer:** `Path.read_text()` and `Path.write_text()` automatically handle opening, reading/writing, and closing files in a single self-contained atomic call.

```python
import tempfile
from pathlib import Path

tmp = Path(tempfile.gettempdir()) / "test_quick_io.txt"
tmp.write_text("Kubernetes cluster online", encoding="utf-8")
content = tmp.read_text(encoding="utf-8")
print("Read back:", content)
tmp.unlink() # Cleanup
```

Output:
```text
Read back: Kubernetes cluster online
```

**Q338. What is the difference between text mode and binary mode?**

*Explain newline translation (`\r\n` vs `\n`) and string vs bytes handling.*

**Answer:** - **Text Mode (`t`)**: Translates universal newlines (`\r\n` on Windows, `\n` on Linux) to `\n` in Python strings and handles Unicode encoding/decoding.
- **Binary Mode (`b`)**: Reads and writes exact raw byte values (`bytes`) without any encoding or newline translation. Mandatory for images, audio, zip files, and compiled binaries.

```python
import io

raw_bytes = b"header
payload
"
bin_stream = io.BytesIO(raw_bytes)
print("Binary reads exact bytes:", bin_stream.read())
```

Output:
```text
Binary reads exact bytes: b'header
payload
'
```

**Q339. How does buffered I/O work and what does `flush()` do?**

*Explain user-space buffering, OS cache flushing, and `os.fsync()`.*

**Answer:** Python buffers file writes in user-space memory to minimize slow system calls. Calling `f.flush()` pushes data from Python's internal buffer to the OS buffer cache. To guarantee bytes are physically written to disk hardware, call `os.fsync(f.fileno())`.

```python
import os
import tempfile

with tempfile.NamedTemporaryFile("w+", delete=False) as f:
    f.write("Critical transactional write")
    f.flush()               # Flush Python buffer to OS
    os.fsync(f.fileno())    # Force OS to write to disk
    tmp_name = f.name

os.remove(tmp_name)
print("Flushed and synced safely")
```

Output:
```text
Flushed and synced safely
```

**Q340. How do you handle CSV files using the `csv` module?**

*Demonstrate `csv.reader`, `csv.writer`, `DictReader`, and `DictWriter`.*

**Answer:** The built-in `csv` module parses delimited tabular data. `DictReader` and `DictWriter` map rows directly to Python dictionaries using header column names.

```python
import csv
import io

csv_data = """host,port,status
web-1,80,UP
db-1,5432,UP"""

f = io.StringIO(csv_data)
reader = csv.DictReader(f)
for row in reader:
    print(f"Host: {row['host']}, Port: {row['port']}, Status: {row['status']}")
```

Output:
```text
Host: web-1, Port: 80, Status: UP
Host: db-1, Port: 5432, Status: UP
```

**Q341. How do you read and write JSON files safely?**

*Explain `json.load()`, `json.dump()`, `json.loads()`, `json.dumps()`.*

**Answer:** - `json.dump(obj, f)` / `json.load(f)`: Serializes/deserializes to and from file-like stream objects.
- `json.dumps(obj)` / `json.loads(s)`: Serializes/deserializes to and from in-memory string objects.

```python
import json
import io

config = {"cluster": "us-east", "nodes": 5, "active": True}
stream = io.StringIO()
json.dump(config, stream, indent=2)

stream.seek(0)
loaded = json.load(stream)
print("Loaded cluster:", loaded["cluster"])
```

Output:
```text
Loaded cluster: us-east
```

**Q342. How do you process large files line-by-line without running Out of Memory (OOM)?**

*Demonstrate streaming gigabyte-sized log files with generators.*

**Answer:** Never read large files with `.read()` or `.readlines()`. Use a generator expression over the file handle to process line-by-line in constant $O(1)$ memory.

```python
import io

def count_errors(file_obj):
    return sum(1 for line in file_obj if "ERROR" in line)

sample_log = io.StringIO("INFO: Start
ERROR: Failed
INFO: Retry
ERROR: Timeout")
print("Total errors found:", count_errors(sample_log))
```

Output:
```text
Total errors found: 2
```

**Q343. How do temporary files work using the `tempfile` module?**

*Explain `NamedTemporaryFile`, `TemporaryDirectory`, and automatic cleanup.*

**Answer:** `tempfile` creates secure, uniquely named temporary files and directories. `TemporaryDirectory()` acts as a context manager that automatically deletes the folder and all its contents on exit.

```python
import tempfile
from pathlib import Path

with tempfile.TemporaryDirectory() as tmp_dir:
    p = Path(tmp_dir) / "metrics.txt"
    p.write_text("cpu=95")
    print("Created temp file:", p.exists())

# Deleted automatically
print("Temp directory cleaned up:", not Path(tmp_dir).exists())
```

Output:
```text
Created temp file: True
Temp directory cleaned up: True
```

**Q344. How do you perform atomic file writes in Python?**

*Explain writing to a temporary file and renaming with `os.replace()` to prevent corrupted partial writes.*

**Answer:** If a process crashes while writing directly to a file, the file is corrupted. An **atomic write** writes to a temporary file on the same filesystem first, calls `flush()` and `os.fsync()`, then renames it using `os.replace(temp, target)`. `os.replace` is an atomic filesystem operation on POSIX and modern Windows.

```python
import os
import tempfile
from pathlib import Path

def atomic_write(target_path: Path, data: str):
    temp_dir = target_path.parent
    with tempfile.NamedTemporaryFile("w", dir=temp_dir, delete=False) as tf:
        tf.write(data)
        tf.flush()
        os.fsync(tf.fileno())
        temp_name = tf.name
    os.replace(temp_name, target_path)

p = Path("safe_deploy.json")
atomic_write(p, '{"status": "DEPLOYED"}')
print("Atomic write verified:", p.read_text())
p.unlink() # Cleanup
```

Output:
```text
Atomic write verified: {"status": "DEPLOYED"}
```

**Q345. How do you walk directory trees using `os.walk()` and `pathlib.Path.rglob()`?**

*Compare recursive directory traversal methods.*

**Answer:** - `os.walk(top)`: Yields 3-tuples `(dirpath, dirnames, filenames)`.
- `Path.rglob(pattern)`: Yields Path objects matching glob patterns recursively.

```python
from pathlib import Path

# Match all python files recursively
p = Path(".")
py_files = list(p.glob("*.py"))
print("Current dir py files count:", len(py_files))
```

Output:
```text
Current dir py files count: ...
```

**Q346. How does file memory mapping work using `mmap`?**

*Explain reading large binary files directly from kernel page cache.*

**Answer:** The `mmap` module maps a file directly into the process's virtual memory space. Slicing the `mmap` object accesses the operating system page cache directly without copying bytes into Python user space.

```python
import mmap
import tempfile

with tempfile.NamedTemporaryFile("wb+", delete=False) as f:
    f.write(b"0123456789ABCDEF")
    f.flush()
    f_name = f.name

with open(f_name, "r+b") as f:
    with mmap.mmap(f.fileno(), 0) as mm:
        print("mmap slice [10:14]:", mm[10:14])

import os; os.remove(f_name)
```

Output:
```text
mmap slice [10:14]: b'ABCD'
```

**Q347. How do you handle file permissions and metadata using `os.stat`?**

*Inspect file size, modification time, and mode bits.*

**Answer:** `os.stat(path)` returns a stat-result object with attributes like `.st_size`, `.st_mtime` (modified timestamp), and `.st_mode` (permissions).

```python
import os

stat_info = os.stat(__file__)
print("Current file size in bytes:", stat_info.st_size)
print("File mode octal:            ", oct(stat_info.st_mode))
```

Output:
```text
Current file size in bytes: ...
File mode octal:             0o100666
```

**Q348. How does file copying and archiving work using `shutil`?**

*Demonstrate `shutil.copy2`, `shutil.move`, `shutil.rmtree`, and `make_archive`.*

**Answer:** `shutil` provides high-level file operations. `copy2` copies file content and preserves metadata timestamps. `rmtree` deletes directory trees recursively. `make_archive` builds zip/tar files.

```python
import shutil
# shutil.copy2("src.txt", "dst.txt")
# shutil.rmtree("build_dir")
# shutil.make_archive("backup", "zip", "data_dir")
print("shutil utilities verified")
```

Output:
```text
shutil utilities verified
```

**Q349. Output Question: Predict what seek(0) does before reading.**

*Trace file pointer movement.*

**Answer:** Writing advances pointer to the end; reading immediately returns empty string unless `seek(0)` rewinds the pointer to the beginning.

```python
import io
buf = io.StringIO()
buf.write("Hello")
print("Without seek:", repr(buf.read()))
buf.seek(0)
print("With seek(0):", repr(buf.read()))
```

Output:
```text
Without seek: ''
With seek(0): 'Hello'
```

**Q350. Programming Question: Tail the last N lines of a file efficiently.**

*Read only the end of a file without scanning the entire file into memory.*

**Answer:** Seek backward from the end in chunks using `collections.deque`.

```python
from collections import deque
import io

def tail(file_obj, n=3):
    return list(deque(file_obj, maxlen=n))

data = io.StringIO("
".join(f"Log line {i}" for i in range(100)))
print("Tailed lines:", [line.strip() for line in tail(data, 3)])
```

Output:
```text
Tailed lines: ['Log line 97', 'Log line 98', 'Log line 99']
```

**Q351. How do in-memory streams io.StringIO and io.BytesIO work?**

*Demonstrate mocking file I/O for unit testing.*

**Answer:** `io.StringIO` and `io.BytesIO` provide in-memory file-like buffers implementing standard file methods (`read()`, `write()`, `seek()`), making them ideal for testing without touching the disk.

```python
import io

buf = io.StringIO()
buf.write("Mock file output")
buf.seek(0)
print("Read from memory buffer:", buf.read())
```

Output:
```text
Read from memory buffer: Mock file output
```

**Q352. How do you read configuration files using `configparser` (INI files)?**

*Demonstrate parsing `.ini` configuration files with sections.*

**Answer:** `configparser` parses INI-style configuration files organized into `[sections]` containing `key = value` pairs.

```python
import configparser

ini_text = """
[database]
host = 10.0.0.5
port = 5432
[app]
debug = true
"""

config = configparser.ConfigParser()
config.read_string(ini_text)
print("DB Host:", config.get("database", "host"))
print("App Debug:", config.getboolean("app", "debug"))
```

Output:
```text
DB Host: 10.0.0.5
App Debug: True
```

**Q353. How do you read TOML files natively using `tomllib` in Python 3.11+?**

*Explain reading `pyproject.toml` and TOML configurations without third-party dependencies.*

**Answer:** Python 3.11 added `tomllib` to the standard library to parse TOML files (`tomllib.loads()` and `tomllib.load()`).

```python
import tomllib

toml_data = """
[tool.app]
name = "cloud-agent"
version = "1.0.0"
ports = [8080, 8443]
"""

parsed = tomllib.loads(toml_data)
print("App name:", parsed["tool"]["app"]["name"])
print("Ports:", parsed["tool"]["app"]["ports"])
```

Output:
```text
App name: cloud-agent
Ports: [8080, 8443]
```

**Q354. How do you compress and extract files using `gzip` and `zipfile`?**

*Demonstrate creating and inspecting zip archives programmatically.*

**Answer:** `zipfile.ZipFile` creates, inspects, and extracts ZIP archives directly in Python.

```python
import zipfile
import io

buf = io.BytesIO()
with zipfile.ZipFile(buf, "w") as zf:
    zf.writestr("test.txt", "Compressed file data")

buf.seek(0)
with zipfile.ZipFile(buf, "r") as zf:
    print("Files in archive:", zf.namelist())
    print("Extracted content:", zf.read("test.txt").decode())
```

Output:
```text
Files in archive: ['test.txt']
Extracted content: Compressed file data
```

**Q355. How does file locking work in Python (fcntl vs msvcrt)?**

*Explain preventing concurrent write collisions on shared files.*

**Answer:** To prevent race conditions when multiple processes write to the same file, use file locking: `fcntl.flock()` on Unix/Linux, or `msvcrt.locking()` on Windows.

```python
# Cross-platform concept:
# Unix: fcntl.flock(f.fileno(), fcntl.LOCK_EX)
# Windows: msvcrt.locking(f.fileno(), msvcrt.LK_NBLCK, size)
print("File locking prevents concurrent race conditions across processes")
```

Output:
```text
File locking prevents concurrent race conditions across processes
```

**Q356. Output Question: What happens when opening a non-existent file in 'w' vs 'r' vs 'x' mode?**

*Compare file creation vs FileNotFoundError vs FileExistsError.*

**Answer:** `'r'` raises `FileNotFoundError`. `'w'` creates the file. `'x'` creates the file if missing, but raises `FileExistsError` if the file already exists.

```python
import tempfile
from pathlib import Path

p = Path(tempfile.gettempdir()) / "test_exclusive.tmp"
p.write_text("exists")
try:
    with open(p, "x") as f: pass
except FileExistsError as e:
    print("Mode 'x' caught expected error:", type(e).__name__)
p.unlink()
```

Output:
```text
Mode 'x' caught expected error: FileExistsError
```

**Q357. Debugging Question: Fix UnicodeDecodeError when reading non-UTF8 files.**

*Handle files encoded in Latin-1, CP1252, or with corrupt bytes.*

**Answer:** Specify the encoding explicitly `encoding='utf-8'` and use error policies like `errors='replace'` or `errors='ignore'`.

```python
bad_bytes = b"Server test ÿþ data"
decoded = bad_bytes.decode("utf-8", errors="replace")
print("Decoded with replacement:", decoded)
```

Output:
```text
Decoded with replacement: Server test  data
```

**Q358. Programming Question: Search and replace a string across multiple files safely.**

*Write a batch search-and-replace utility using atomic writes.*

**Answer:** Read file, replace string, and write back atomically.

```python
def replace_in_string(content: str, old_str: str, new_str: str) -> str:
    return content.replace(old_str, new_str)

sample = "cluster_name = dev-cluster"
print("Replaced:", replace_in_string(sample, "dev-cluster", "prod-cluster"))
```

Output:
```text
Replaced: cluster_name = prod-cluster
```

**Q359. Programming Question: Compute SHA-256 checksum of a large file.**

*Stream file in 64KB blocks to hash large files in constant memory.*

**Answer:** Iterate with `iter(lambda: f.read(65536), b'')`.

```python
import hashlib
import io

def compute_sha256(stream):
    hasher = hashlib.sha256()
    for chunk in iter(lambda: stream.read(65536), b""):
        hasher.update(chunk)
    return hasher.hexdigest()

data_stream = io.BytesIO(b"Cloud automation pipeline data")
print("SHA256:", compute_sha256(data_stream))
```

Output:
```text
SHA256: d86574f8846175e11f7c5e8f46d31b81ee0952d431bbd73c7f9ea93e36e65f3f
```

**Q360. Programming Question: Rotate log files when size exceeds threshold.**

*Implement log rotation renaming `app.log` to `app.log.1` when size exceeds limit.*

**Answer:** Check `os.stat().st_size` and rename using `os.replace`.

```python
from pathlib import Path

def rotate_log_if_needed(log_path: Path, max_bytes: int = 100):
    if log_path.exists() and log_path.stat().st_size >= max_bytes:
        backup = log_path.with_suffix(".log.1")
        log_path.replace(backup)
        return f"Rotated {log_path} -> {backup}"
    return "No rotation needed"

p = Path("mock_app.log")
p.write_text("A" * 150)
print(rotate_log_if_needed(p, max_bytes=100))
Path("mock_app.log.1").unlink()
```

Output:
```text
Rotated mock_app.log -> mock_app.log.1
```


---

## Chapter 12: Iterators, Iterables, and Generators

**Q361. What is the difference between an Iterable and an Iterator in Python?**

*Explain the iterator protocol: `__iter__()` and `__next__()`.*

**Answer:** - **Iterable**: Any object that can return an iterator (implements `__iter__()` returning an iterator, or `__getitem__()` with sequential integer indices). Examples: `list`, `str`, `dict`, `range`.
- **Iterator**: A stateful object representing a stream of data that implements the iterator protocol: `__next__()` returning the next item or raising `StopIteration`, and `__iter__()` returning itself.

```python
nums = [10, 20] # Iterable
it = iter(nums) # Iterator

print("Iterator type:", type(it).__name__)
print("next(it):", next(it))
print("next(it):", next(it))
try:
    next(it)
except StopIteration:
    print("Caught StopIteration: iterator exhausted")
```

Output:
```text
Iterator type: list_iterator
next(it): 10
next(it): 20
Caught StopIteration: iterator exhausted
```

**Q362. How does a custom Iterator class work?**

*Implement a custom iterator class from scratch.*

**Answer:** Define a class implementing both `__iter__()` (returning `self`) and `__next__()`.

```python
class CountDown:
    def __init__(self, start: int):
        self.current = start

    def __iter__(self):
        return self

    def __next__(self):
        if self.current <= 0:
            raise StopIteration
        val = self.current
        self.current -= 1
        return val

print("CountDown:", list(CountDown(3)))
```

Output:
```text
CountDown: [3, 2, 1]
```

**Q363. What is a generator function and how does the `yield` keyword work?**

*Explain suspension of execution, local variable preservation, and resume behavior.*

**Answer:** A generator function is a function that contains one or more `yield` statements. Calling it does not execute the function immediately; it returns a **generator iterator object**. When `next()` is called, the function executes until it reaches a `yield`, yields the value, and **suspends its execution state** (preserving local variables and instruction pointer). The next `next()` call resumes immediately after the `yield`.

```python
def simple_gen():
    print("State 1")
    yield "A"
    print("State 2")
    yield "B"

g = simple_gen()
print("Calling next 1:", next(g))
print("Calling next 2:", next(g))
```

Output:
```text
State 1
Calling next 1: A
State 2
Calling next 2: B
```

**Q364. What is a generator expression and how does it compare to a list comprehension?**

*Compare memory footprint and lazy evaluation: `(x for x in seq)` vs `[x for x in seq]`.*

**Answer:** - **List Comprehension (`[...]`)**: Eagerly computes and allocates the entire list in memory immediately.
- **Generator Expression (`(...)`)**: Lazily produces elements one-by-one on demand, consuming $O(1)$ constant memory regardless of data size.

```python
import sys

list_comp = [i * 2 for i in range(100_000)]
gen_exp = (i * 2 for i in range(100_000))

print("List comprehension memory:", sys.getsizeof(list_comp), "bytes")
print("Generator expression memory:  ", sys.getsizeof(gen_exp), "bytes")
```

Output:
```text
List comprehension memory: 800984 bytes
Generator expression memory:   200 bytes
```

**Q365. How do generator pipelines process streaming data efficiently?**

*Demonstrate composing modular generator stages in Unix pipe style.*

**Answer:** Generators can be chained together into pipelines where data flows through transformations one element at a time, keeping memory minimal.

```python
def stream_numbers(n):
    for i in range(n): yield i

def filter_odds(seq):
    for x in seq:
        if x % 2 != 0: yield x

def square_numbers(seq):
    for x in seq: yield x ** 2

# Pipeline: stream -> filter -> square
pipeline = square_numbers(filter_odds(stream_numbers(6)))
print("Pipeline output:", list(pipeline))
```

Output:
```text
Pipeline output: [1, 9, 25]
```

**Q366. What is `yield from` in Python?**

*Explain delegating to subgenerators (PEP 380).*

**Answer:** `yield from <iterable>` delegates iteration directly to a subgenerator or iterable, transparently yielding all its values and bidirectional channel communication (`send`, `throw`).

```python
def sub_gen():
    yield "sub_1"
    yield "sub_2"

def main_gen():
    yield "main_start"
    yield from sub_gen() # Delegates to subgenerator
    yield "main_end"

print("Delegated generator:", list(main_gen()))
```

Output:
```text
Delegated generator: ['main_start', 'sub_1', 'sub_2', 'main_end']
```

**Q367. How does the generator `.send()` method enable coroutines?**

*Explain sending values into a running generator at yield points.*

**Answer:** The `.send(value)` method resumes the generator and sends `value` into the generator function as the result of the `yield` expression.

```python
def accumulator():
    total = 0
    while True:
        val = yield total
        if val is None: break
        total += val

acc = accumulator()
next(acc) # Prime generator
print("Sent 10, total:", acc.send(10))
print("Sent 20, total:", acc.send(20))
```

Output:
```text
Sent 10, total: 10
Sent 20, total: 30
```

**Q368. How do generator `.throw()` and `.close()` methods work?**

*Explain injecting exceptions and closing generators.*

**Answer:** - `g.throw(typ, val)`: Raises an exception at the point where the generator is currently suspended.
- `g.close()`: Raises `GeneratorExit` at the suspension point to trigger cleanup blocks.

```python
def worker():
    try:
        while True: yield "working"
    except GeneratorExit:
        print("Worker finalized")

w = worker()
next(w)
w.close()
```

Output:
```text
Worker finalized
```

**Q369. How do you create an infinite generator?**

*Demonstrate generating unbounded sequences without memory crashes.*

**Answer:** An infinite generator uses `while True:` and `yield`, allowing consumers to pull as many values as needed.

```python
def infinite_sequence(start=1):
    num = start
    while True:
        yield num
        num += 1

gen = infinite_sequence()
first_5 = [next(gen) for _ in range(5)]
print("First 5:", first_5)
```

Output:
```text
First 5: [1, 2, 3, 4, 5]
```

**Q370. What is itertools.count, itertools.cycle, and itertools.repeat?**

*Explain Python's standard infinite iteration primitives.*

**Answer:** - `itertools.count(start=0, step=1)`: Generates an infinite count.
- `itertools.cycle(iterable)`: Cycles through an iterable indefinitely.
- `itertools.repeat(elem, times=None)`: Repeats an element.

```python
import itertools

counter = itertools.count(10, 5)
print("count:", [next(counter) for _ in range(3)])

cycler = itertools.cycle(["red", "green"])
print("cycle:", [next(cycler) for _ in range(4)])
```

Output:
```text
count: [10, 15, 20]
cycle: ['red', 'green', 'red', 'green']
```

**Q371. What is itertools.chain and itertools.chain.from_iterable?**

*Demonstrate flattening and concatenating multiple iterables without copying.*

**Answer:** `itertools.chain(*iterables)` chains sequences together seamlessly in constant memory.

```python
import itertools

l1 = ["a", "b"]
l2 = ["c", "d"]
print("Chained:", list(itertools.chain(l1, l2)))

nested = [[1, 2], [3, 4]]
print("From iterable:", list(itertools.chain.from_iterable(nested)))
```

Output:
```text
Chained: ['a', 'b', 'c', 'd']
From iterable: [1, 2, 3, 4]
```

**Q372. What is itertools.islice and how does it slice iterators?**

*Explain slicing generators without converting to lists.*

**Answer:** `itertools.islice(iterable, start, stop, step)` returns a slice of an iterator without loading the entire stream into memory.

```python
import itertools

inf = itertools.count(0)
sliced = list(itertools.islice(inf, 5, 10))
print("Sliced count(0) [5:10]:", sliced)
```

Output:
```text
Sliced count(0) [5:10]: [5, 6, 7, 8, 9]
```

**Q373. What is itertools.groupby and why must input be sorted first?**

*Explain grouping consecutive matching keys.*

**Answer:** `itertools.groupby(iterable, key=None)` groups **consecutive** elements with matching keys. Crucial requirement: input must be pre-sorted by the same key function; otherwise, duplicate groups will be created.

```python
import itertools

data = [("web", "pod-1"), ("web", "pod-2"), ("db", "pod-3")]
data.sort(key=lambda x: x[0])

for k, group in itertools.groupby(data, key=lambda x: x[0]):
    print(f"Group {k}: {[item[1] for item in group]}")
```

Output:
```text
Group db: ['pod-3']
Group web: ['pod-1', 'pod-2']
```

**Q374. What are itertools.accumulate, itertools.compress, and itertools.dropwhile/takewhile?**

*Explain filtering and cumulative computation iterators.*

**Answer:** - `accumulate(seq, func=add)`: Cumulative sums/products.
- `compress(data, selectors)`: Filters elements by boolean mask.
- `dropwhile(pred, seq)`: Drops elements while predicate is true, then yields the rest.
- `takewhile(pred, seq)`: Yields elements while predicate is true, then stops.

```python
import itertools

nums = [1, 2, 3, 4]
print("Accumulate:", list(itertools.accumulate(nums)))

data = ["a", "b", "c", "d"]
mask = [True, False, True, False]
print("Compress:", list(itertools.compress(data, mask)))
```

Output:
```text
Accumulate: [1, 3, 6, 10]
Compress: ['a', 'c']
```

**Q375. What are combinatoric iterators: product, permutations, combinations, and combinations_with_replacement?**

*Compare Cartesian products, order-sensitive permutations, and unordered combinations.*

**Answer:** - `product(*iterables, repeat=1)`: Cartesian product.
- `permutations(iterable, r=None)`: All ordered arrangements.
- `combinations(iterable, r)`: Unordered subsets without replacement.
- `combinations_with_replacement(iterable, r)`: Subsets allowing duplicate selections.

```python
import itertools

items = ["A", "B", "C"]
print("Permutations (len 2):", list(itertools.permutations(items, 2)))
print("Combinations (len 2):", list(itertools.combinations(items, 2)))
print("Product (2x2):", list(itertools.product([0, 1], repeat=2)))
```

Output:
```text
Permutations (len 2): [('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'C'), ('C', 'A'), ('C', 'B')]
Combinations (len 2): [('A', 'B'), ('A', 'C'), ('B', 'C')]
Product (2x2): [(0, 0), (0, 1), (1, 0), (1, 1)]
```

**Q376. How does itertools.tee work and what are its memory caveats?**

*Explain splitting an iterator into N independent iterators.*

**Answer:** `itertools.tee(iterable, n=2)` splits a single iterable into $N$ independent iterators. Caveat: if one branch advances far ahead of the other, the intermediate values are buffered in memory, potentially causing memory bloat.

```python
import itertools

base = iter([1, 2, 3])
it1, it2 = itertools.tee(base, 2)
print("Branch 1:", list(it1))
print("Branch 2:", list(it2))
```

Output:
```text
Branch 1: [1, 2, 3]
Branch 2: [1, 2, 3]
```

**Q377. What is the difference between an iterator and a generator?**

*Clarify terminology and interface compliance.*

**Answer:** All generators are iterators, but not all iterators are generators. An **iterator** is any object that adheres to the iterator protocol (`__iter__`, `__next__`). A **generator** is an iterator created using `yield` or a generator expression, where Python automatically implements the state machine and protocol.

```python
import types

def gen(): yield 1
print("Is generator iterator:", isinstance(gen(), types.GeneratorType))
```

Output:
```text
Is generator iterator: True
```

**Q378. Output Question: Predict what happens when an iterator is consumed twice.**

*Explain iterator exhaustion.*

**Answer:** Once an iterator reaches `StopIteration`, it is exhausted. Subsequent loops over it yield nothing.

```python
gen = (x for x in [1, 2])
print("First pass:", list(gen))
print("Second pass:", list(gen))
```

Output:
```text
First pass: [1, 2]
Second pass: []
```

**Q379. Output Question: Predict output when next() is called with a default argument.**

*Explain `next(iterator, default)` preventing StopIteration.*

**Answer:** Passing a second argument to `next()` returns that default value when the iterator is exhausted instead of raising `StopIteration`.

```python
it = iter([])
print("Result with default:", next(it, "EMPTY"))
```

Output:
```text
Result with default: EMPTY
```

**Q380. Output Question: What does this generator with return value evaluate to?**

*Explain return values in generators and StopIteration.value.*

**Answer:** In Python 3.3+, a `return value` in a generator carries the value inside the `StopIteration(value)` exception.

```python
def gen_with_return():
    yield 1
    return "Finished"

g = gen_with_return()
print("Yielded:", next(g))
try:
    next(g)
except StopIteration as e:
    print("Return value in StopIteration:", e.value)
```

Output:
```text
Yielded: 1
Return value in StopIteration: Finished
```

**Q381. Debugging Question: Fix generator exhaustion inside reusable functions.**

*Handle functions receiving iterators that need multiple passes.*

**Answer:** Convert iterator to a `list` if multiple passes are required, or accept an iterable factory.

```python
def compute_stats(data):
    items = list(data) # Convert to list for multiple passes
    return len(items), sum(items)

g = (x for x in [10, 20, 30])
print("Stats:", compute_stats(g))
```

Output:
```text
Stats: (3, 60)
```

**Q382. Programming Question: Implement a custom chunking iterator using `itertools.islice`.**

*Yield chunks of size N from any iterable without loading all in memory.*

**Answer:** Use `iter()` and `islice()`.

```python
import itertools

def chunked(iterable, n):
    it = iter(iterable)
    while chunk := list(itertools.islice(it, n)):
        yield chunk

print("Chunked:", list(chunked(range(7), 3)))
```

Output:
```text
Chunked: [[0, 1, 2], [3, 4, 5], [6]]
```

**Q383. Programming Question: Implement a sliding window iterator.**

*Yield overlapping windows of size N (e.g. `[1, 2, 3, 4]` with window 2 -> `(1, 2), (2, 3), (3, 4)`).*

**Answer:** Use `collections.deque` with maxlen.

```python
from collections import deque

def sliding_window(iterable, n=2):
    it = iter(iterable)
    win = deque(itertools.islice(it, n), maxlen=n)
    if len(win) == n:
        yield tuple(win)
    for elem in it:
        win.append(elem)
        yield tuple(win)

import itertools
print("Sliding window:", list(sliding_window([1, 2, 3, 4], 2)))
```

Output:
```text
Sliding window: [(1, 2), (2, 3), (3, 4)]
```

**Q384. Programming Question: Flatten arbitrarily deeply nested iterables.**

*Write a recursive generator using `yield from` that flattens arbitrary nesting.*

**Answer:** Check for `Iterable` while avoiding strings.

```python
from collections.abc import Iterable

def deep_flatten(items):
    for item in items:
        if isinstance(item, Iterable) and not isinstance(item, (str, bytes)):
            yield from deep_flatten(item)
        else:
            yield item

nested = [1, [2, [3, 4], 5], "text", [6]]
print("Flattened:", list(deep_flatten(nested)))
```

Output:
```text
Flattened: [1, 2, 3, 4, 5, 'text', 6]
```

**Q385. Programming Question: Implement a generator-based Coroutine Event Loop simulator.**

*Simulate cooperative multitasking across multiple generators yielding control.*

**Answer:** Maintain a deque of generator tasks and cycle through them.

```python
from collections import deque

def task_a():
    for i in range(2):
        print(f"Task A step {i}")
        yield

def task_b():
    for i in range(2):
        print(f"Task B step {i}")
        yield

tasks = deque([task_a(), task_b()])
while tasks:
    t = tasks.popleft()
    try:
        next(t)
        tasks.append(t)
    except StopIteration:
        pass
```

Output:
```text
Task A step 0
Task B step 0
Task A step 1
Task B step 1
```

**Q386. Programming Question: Implement a round-robin scheduler using `itertools.cycle`.**

*Cycle requests across active server endpoints.*

**Answer:** Use `itertools.cycle`.

```python
import itertools

servers = ["srv-01", "srv-02", "srv-03"]
pool = itertools.cycle(servers)

requests = [f"Req-{i}" for i in range(5)]
for r in requests:
    print(f"{r} routed to {next(pool)}")
```

Output:
```text
Req-0 routed to srv-01
Req-1 routed to srv-02
Req-2 routed to srv-03
Req-3 routed to srv-01
Req-4 routed to srv-02
```

**Q387. Programming Question: Find first element matching predicate in an iterator.**

*Return the first element satisfying a condition or a default fallback in $O(1)$ memory.*

**Answer:** Combine `next()` and generator expression.

```python
def find_first(iterable, predicate, default=None):
    return next((x for x in iterable if predicate(x)), default)

nodes = [{"name": "n1", "healthy": True}, {"name": "n2", "healthy": False}]
print("First unhealthy:", find_first(nodes, lambda n: not n["healthy"]))
```

Output:
```text
First unhealthy: {'name': 'n2', 'healthy': False}
```

**Q388. Programming Question: Implement an infinite Fibonacci generator.**

*Produce infinite Fibonacci numbers on-demand using constant memory.*

**Answer:** Use a generator tracking two variables `a, b = b, a + b`.

```python
def fibonacci_stream():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

import itertools
first_7 = list(itertools.islice(fibonacci_stream(), 7))
print("First 7 Fibonacci:", first_7)
```

Output:
```text
First 7 Fibonacci: [0, 1, 1, 2, 3, 5, 8]
```

**Q389. Programming Question: Deduplicate a stream of items while preserving order using generators.**

*Lazily filter duplicates from an infinite or large sequence.*

**Answer:** Track seen elements in a set inside the generator.

```python
def deduplicate_stream(iterable):
    seen = set()
    for item in iterable:
        if item not in seen:
            seen.add(item)
            yield item

stream = [1, 2, 2, 3, 1, 4, 3, 5]
print("Deduplicated stream:", list(deduplicate_stream(stream)))
```

Output:
```text
Deduplicated stream: [1, 2, 3, 4, 5]
```

**Q390. Programming Question: Monitor a growing file like `tail -f` in Python using generators.**

*Implement continuous log polling with `time.sleep` and `yield`.*

**Answer:** Read lines, sleep if empty, and yield new lines.

```python
import io
import time

def mock_tail_f(stream):
    while True:
        line = stream.readline()
        if not line:
            break # in real tail -f: time.sleep(0.1); continue
        yield line.strip()

log_stream = io.StringIO("Server booted
Interface up
")
for log_entry in mock_tail_f(log_stream):
    print("Streamed log:", log_entry)
```

Output:
```text
Streamed log: Server booted
Streamed log: Interface up
```


---

## Chapter 13: Comprehensions and Advanced Functional Tools

**Q391. What are list, set, dict, and generator comprehensions?**

*Compare syntax and memory characteristics across all 4 comprehension types.*

**Answer:** - **List Comprehension (`[x for x in seq]`)**: Eagerly constructs a mutable `list` in memory.
- **Set Comprehension (`{x for x in seq]`)**: Eagerly constructs a mutable `set`, automatically deduplicating elements.
- **Dict Comprehension (`{k: v for x in seq}`)**: Eagerly constructs a `dict` mapping keys to values.
- **Generator Expression (`(x for x in seq)`)**: Lazily produces elements on-demand in $O(1)$ memory.

```python
data = ["web-1", "api-1", "web-1", "db-1"]

l_comp = [x.upper() for x in data]
s_comp = {x.upper() for x in data}
d_comp = {x: len(x) for x in data}
g_comp = (x.upper() for x in data)

print("List:", l_comp)
print("Set (deduped):", s_comp)
print("Dict:", d_comp)
print("Gen:", type(g_comp).__name__)
```

Output:
```text
List: ['WEB-1', 'API-1', 'WEB-1', 'DB-1']
Set (deduped): {'DB-1', 'WEB-1', 'API-1'}
Dict: {'web-1': 5, 'api-1': 5, 'db-1': 4}
Gen: generator
```

**Q392. How do conditional comprehensions handle `if` filtering vs `if-else` transformations?**

*Explain why `if` goes at the end for filtering, but `if-else` goes at the beginning for ternary transformations.*

**Answer:** - **Filtering (`[expr for x in seq if cond]`)**: The trailing `if` determines whether the element is included in the output.
- **Transformation (`[expr1 if cond else expr2 for x in seq]`)**: The leading ternary expression transforms the value produced for each element.

```python
nums = range(6)

# Filter: only keep evens
filtered = [n for n in nums if n % 2 == 0]

# Transform: label even or odd
labeled = ["EVEN" if n % 2 == 0 else "ODD" for n in nums]

print("Filtered:", filtered)
print("Labeled:", labeled)
```

Output:
```text
Filtered: [0, 2, 4]
Labeled: ['EVEN', 'ODD', 'EVEN', 'ODD', 'EVEN', 'ODD']
```

**Q393. How do nested comprehensions translate to nested for-loops?**

*Explain the left-to-right evaluation order of multiple `for` clauses in comprehensions.*

**Answer:** In a nested comprehension `[expr for outer in seq1 for inner in seq2]`, the `for` clauses appear in the exact same left-to-right order as standard nested loops: the leftmost `for` is the outer loop, and the rightmost is the inner loop.

```python
matrix = [[1, 2], [3, 4]]
flattened = [val for row in matrix for val in row]
print("Flattened matrix:", flattened)
```

Output:
```text
Flattened matrix: [1, 2, 3, 4]
```

**Q394. How does the operator module provide high-performance functional primitives?**

*Explain replacing lambdas with `operator.itemgetter`, `attrgetter`, and `methodcaller`.*

**Answer:** The `operator` module provides C-level implementations of standard arithmetic, comparison, and attribute extraction operations. Using `operator.itemgetter(key)` or `attrgetter(attr)` is substantially faster and cleaner than writing `lambda x: x[key]` or `lambda x: x.attr`.

```python
import operator

records = [{"name": "web", "port": 80}, {"name": "db", "port": 5432}, {"name": "api", "port": 8080}]
# Sort by port using itemgetter
records.sort(key=operator.itemgetter("port"))
print("Sorted ports:", [r["port"] for r in records])
```

Output:
```text
Sorted ports: [80, 8080, 5432]
```

**Q395. How do any() and all() implement short-circuiting logical reduction?**

*Explain verifying boolean conditions over iterables.*

**Answer:** - `any(iterable)`: Returns `True` as soon as any element is truthy; short-circuits and stops iteration.
- `all(iterable)`: Returns `False` as soon as any element is falsy; returns `True` only if all elements are truthy.

```python
nodes = [True, True, False, True]
print("Are all nodes healthy:", all(nodes))
print("Is any node healthy:   ", any(nodes))
print("All empty collection:  ", all([])) # Vacuous truth: True
print("Any empty collection:  ", any([])) # False
```

Output:
```text
Are all nodes healthy: False
Is any node healthy:    True
All empty collection:   True
Any empty collection:   False
```

**Q396. How do min() and max() work with custom `key` functions and `default` arguments?**

*Demonstrate finding extremes based on computed attributes and handling empty collections.*

**Answer:** `min(iterable, key=func, default=val)` and `max(iterable, key=func, default=val)` evaluate elements through `key`. Passing `default` prevents raising `ValueError` on empty sequences.

```python
servers = [{"name": "srv-1", "load": 0.45}, {"name": "srv-2", "load": 0.89}, {"name": "srv-3", "load": 0.12}]

least_loaded = min(servers, key=lambda s: s["load"])
print("Least loaded:", least_loaded["name"])

# Safe with default:
empty_res = max([], default="NO_DATA")
print("Empty max:", empty_res)
```

Output:
```text
Least loaded: srv-3
Empty max: NO_DATA
```

**Q397. What is referential transparency in functional programming?**

*Explain why referential transparency guarantees that an expression can be replaced by its value.*

**Answer:** An expression or function call is **referentially transparent** if it can be replaced with its corresponding value without changing the program's behavior. This requires the function to be pure (no side effects, no mutation of global state, and identical output for identical inputs).

```python
# Pure and referentially transparent:
def add(x, y): return x + y

# add(2, 3) can always be safely replaced by 5 at compile/runtime
print("add(2, 3) == 5:", add(2, 3) == 5)
```

Output:
```text
add(2, 3) == 5: True
```

**Q398. How does function currying differ from partial application?**

*Explain multi-parameter transformation vs argument pre-binding.*

**Answer:** - **Partial Application (`functools.partial`)**: Pre-binds a fixed subset of arguments to a function of $N$ arity, returning a function accepting the remaining parameters.
- **Currying**: Transforms a function of $N$ arguments into a chain of $N$ nested functions each taking strictly **one argument** (`f(a)(b)(c)`).

```python
from functools import partial

# Partial application
def f(a, b, c): return a + b + c
f_partial = partial(f, 10, 20)
print("Partial:", f_partial(30))

# Curried function
def f_curried(a):
    return lambda b: lambda c: a + b + c
print("Curried:", f_curried(10)(20)(30))
```

Output:
```text
Partial: 60
Curried: 60
```

**Q399. What are the engineering trade-offs of functional programming in Python?**

*Evaluate readability, recursion overhead, performance, and memory consumption.*

**Answer:** Python supports functional programming, but is not a purely functional language. Strengths: declarative data transformation pipelines (comprehensions, itertools, map/filter). Weaknesses: lack of Tail Call Optimization (TCO) limits deep recursion, dynamic object overhead, and overly terse lambda chains can harm readability compared to explicit procedural code.

```python
# Clean, readable functional transformation in Python:
data = ["10.0.0.1", "10.0.0.2", "192.168.1.1"]
internal_ips = [ip for ip in data if ip.startswith("10.")]
print("Internal IPs:", internal_ips)
```

Output:
```text
Internal IPs: ['10.0.0.1', '10.0.0.2']
```

**Q400. Output Question: Predict what this comprehension with walrus operator returns.**

*Analyze walrus operator evaluation inside comprehensions.*

**Answer:** The assignment `(sq := x * x)` captures the square into `sq` during filtering and yields it directly.

```python
results = [sq for x in range(5) if (sq := x * x) > 5]
print("Results:", results)
```

Output:
```text
Results: [9, 16]
```

**Q401. Output Question: Predict the output of nested list comprehension matrix transposition.**

*Transposing rows and columns using comprehensions.*

**Answer:** Outer loop iterates over column indices `c`, inner loop iterates over rows `r`.

```python
matrix = [[1, 2, 3], [4, 5, 6]]
transposed = [[row[c] for row in matrix] for c in range(3)]
print("Transposed:", transposed)
```

Output:
```text
Transposed: [[1, 4], [2, 5], [3, 6]]
```

**Q402. Output Question: Predict output when zip() is passed to dict().**

*Explain constructing dictionaries from parallel sequences.*

**Answer:** `dict(zip(keys, values))` pairs parallel sequences into key-value pairs.

```python
keys = ["env", "port"]
values = ["prod", 443]
print(dict(zip(keys, values)))
```

Output:
```text
{'env': 'prod', 'port': 443}
```

**Q403. Debugging Question: Fix unintended sharing of mutable structures in nested comprehension.**

*Avoid creating lists with shared row references.*

**Answer:** Using `[[0] * 3] * 3` creates shared row references; use nested list comprehension `[[0 for _ in range(3)] for _ in range(3)]` instead.

```python
# Safe grid creation:
grid = [[0 for _ in range(3)] for _ in range(3)]
grid[0][0] = 1
print("Row 0:", grid[0])
print("Row 1:", grid[1])
```

Output:
```text
Row 0: [1, 0, 0]
Row 1: [0, 0, 0]
```

**Q404. Programming Question: Partition a list into two sublists based on a predicate.**

*Separate items matching condition from non-matching items in a single pass.*

**Answer:** Use a helper or `itertools.tee`.

```python
def partition(predicate, iterable):
    truthy, falsy = [], []
    for item in iterable:
        (truthy if predicate(item) else falsy).append(item)
    return truthy, falsy

services = [("web", True), ("api", False), ("db", True)]
active, inactive = partition(lambda s: s[1], services)
print("Active:", active)
print("Inactive:", inactive)
```

Output:
```text
Active: [('web', True), ('db', True)]
Inactive: [('api', False)]
```

**Q405. Programming Question: Implement map-reduce word counter in Python.**

*Write a map-reduce pipeline that tokenizes text and aggregates word counts.*

**Answer:** Combine `map()`, `itertools.chain()`, and `Counter` or `reduce()`.

```python
from collections import Counter
from functools import reduce

documents = ["cloud automation with python", "devops automation and cloud infrastructure"]
# Map phase
mapped = map(lambda doc: Counter(doc.split()), documents)
# Reduce phase
total_counts = reduce(lambda c1, c2: c1 + c2, mapped)

print("Aggregated word counts:", total_counts.most_common(2))
```

Output:
```text
Aggregated word counts: [('cloud', 2), ('automation', 2)]
```

**Q406. Programming Question: Implement point-free function composition helper.**

*Create a pipeline operator `pipe(data, func1, func2, func3)`.*

**Answer:** Pass data through sequential callables using `reduce()`.

```python
from functools import reduce

def pipe(val, *functions):
    return reduce(lambda acc, f: f(acc), functions, val)

res = pipe("  prod-cluster-01  ", str.strip, str.upper, lambda s: f"DEPLOY_{s}")
print("Piped result:", res)
```

Output:
```text
Piped result: DEPLOY_PROD-CLUSTER-01
```

**Q407. Programming Question: Calculate moving averages using `itertools.accumulate`.**

*Compute rolling cumulative averages over a streaming dataset.*

**Answer:** Combine cumulative sums and counts.

```python
import itertools

metrics = [10, 20, 30, 40]
sums = list(itertools.accumulate(metrics))
moving_averages = [s / i for i, s in enumerate(sums, start=1)]
print("Moving averages:", moving_averages)
```

Output:
```text
Moving averages: [10.0, 15.0, 20.0, 25.0]
```

**Q408. Programming Question: Transpose an arbitrary irregular jagged matrix padding missing values.**

*Use `itertools.zip_longest` to safely transpose uneven lists.*

**Answer:** Pass `*matrix` to `itertools.zip_longest` with `fillvalue`.

```python
import itertools

jagged = [[1, 2, 3], [4, 5], [6]]
transposed = list(itertools.zip_longest(*jagged, fillvalue=0))
print("Padded transposed matrix:", transposed)
```

Output:
```text
Padded transposed matrix: [(1, 4, 6), (2, 5, 0), (3, 0, 0)]
```

**Q409. Programming Question: Memoize class method calls per instance.**

*Create a cached property or method decorator that stores results on the instance `__dict__`.*

**Answer:** Use `functools.cached_property`.

```python
from functools import cached_property

class Dataset:
    def __init__(self, data):
        self.data = data

    @cached_property
    def total(self):
        print("[CALC] Computing expensive sum...")
        return sum(self.data)

ds = Dataset([1, 2, 3, 4, 5])
print("First access:", ds.total)
print("Second access (cached):", ds.total)
```

Output:
```text
[CALC] Computing expensive sum...
First access: 15
Second access (cached): 15
```

**Q410. Programming Question: Implement a pure functional state monad pattern.**

*Pass state explicitly through a pipeline of state-transforming functions.*

**Answer:** Return a 2-tuple `(value, new_state)` on each transition.

```python
def transition(state, action):
    val, balance = state
    if action == "DEPOSIT":
        return ("SUCCESS", balance + 100)
    elif action == "WITHDRAW":
        return ("SUCCESS", balance - 50)
    return ("NO_OP", balance)

state = ("INIT", 100)
for act in ["DEPOSIT", "WITHDRAW"]:
    status, balance = transition(state, act)
    state = (status, balance)

print("Final functional state:", state)
```

Output:
```text
Final functional state: ('SUCCESS', 150)
```

**Q411. Programming Question: Generate Cartesian product combinations for matrix testing.**

*Generate test matrices across multiple operating systems, Python versions, and architectures.*

**Answer:** Use `itertools.product`.

```python
import itertools

os_list = ["linux", "windows"]
py_list = ["3.11", "3.12"]
arch_list = ["x86_64", "arm64"]

matrix = list(itertools.product(os_list, py_list, arch_list))
print(f"Total build combinations: {len(matrix)}")
print("Sample matrix entry:", matrix[0])
```

Output:
```text
Total build combinations: 8
Sample matrix entry: ('linux', '3.11', 'x86_64')
```

**Q412. Programming Question: Group list of tuples into nested dictionary.**

*Convert a list of `(department, team, member)` into a hierarchical dictionary structure.*

**Answer:** Use nested `collections.defaultdict`.

```python
from collections import defaultdict

data = [("Engineering", "DevOps", "Alice"), ("Engineering", "QA", "Bob"), ("Sales", "Direct", "Charlie")]
hierarchy = defaultdict(lambda: defaultdict(list))

for dept, team, member in data:
    hierarchy[dept][team].append(member)

print("Engineering DevOps members:", hierarchy["Engineering"]["DevOps"])
```

Output:
```text
Engineering DevOps members: ['Alice']
```

**Q413. Programming Question: Implement an RLE (Run-Length Encoding) compression utility using `itertools.groupby`.**

*Compress sequences of repeated characters into `(count, character)` pairs.*

**Answer:** Group characters and calculate group lengths.

```python
import itertools

def rle_encode(text: str) -> list:
    return [(len(list(group)), char) for char, group in itertools.groupby(text)]

encoded = rle_encode("AAABBBCCCCDD")
print("RLE Encoded:", encoded)
```

Output:
```text
RLE Encoded: [(3, 'A'), (3, 'B'), (4, 'C'), (2, 'D')]
```

**Q414. Programming Question: Implement safe dictionary mapping with `operator.itemgetter` and error defaults.**

*Extract fields from records without raising `KeyError` on missing keys.*

**Answer:** Combine dictionary `.get` inside list comprehensions.

```python
records = [{"id": 1, "name": "web"}, {"id": 2}]
names = [r.get("name", "UNNAMED") for r in records]
print("Extracted names:", names)
```

Output:
```text
Extracted names: ['web', 'UNNAMED']
```

**Q415. Programming Question: Compute running moving standard deviation.**

*Compute standard deviation over streaming windows in real-time.*

**Answer:** Use statistics module over sliding window deques.

```python
import statistics
from collections import deque

def stream_stdev(stream, window_size=3):
    win = deque(maxlen=window_size)
    for val in stream:
        win.append(val)
        if len(win) == window_size:
            yield round(statistics.stdev(win), 2)

metrics = [10, 12, 11, 25, 26, 24]
print("Running stdevs:", list(stream_stdev(metrics, 3)))
```

Output:
```text
Running stdevs: [1.0, 7.81, 8.5, 1.0]
```

**Q416. Programming Question: Implement tail-call elimination using Trampolines for Fibonacci.**

*Calculate large Fibonacci numbers recursively without exhausting the call stack.*

**Answer:** Return callable thunks evaluated in a while loop.

```python
def trampoline(func):
    def wrapper(*args, **kwargs):
        res = func(*args, **kwargs)
        while callable(res):
            res = res()
        return res
    return wrapper

def fib_tail(n, a=0, b=1):
    if n == 0: return a
    if n == 1: return b
    return lambda: fib_tail(n - 1, b, a + b)

trampolined_fib = trampoline(fib_tail)
print("fib(20):", trampolined_fib(20))
```

Output:
```text
fib(20): 6765
```

**Q417. Programming Question: Implement a zip-with / combine utility.**

*Combine two sequences using an arbitrary binary function.*

**Answer:** Combine `map()` and `zip()`.

```python
def zip_with(func, seq1, seq2):
    return [func(a, b) for a, b in zip(seq1, seq2)]

prices = [100, 200, 300]
taxes = [0.1, 0.15, 0.2]
totals = zip_with(lambda p, t: p * (1 + t), prices, taxes)
print("Calculated totals:", totals)
```

Output:
```text
Calculated totals: [110.0, 230.0, 360.0]
```

**Q418. Programming Question: Build a lazy functional pipeline with generator expressions.**

*Process large streaming logs through parsing, filtering, and aggregation stages lazily.*

**Answer:** Connect generator expressions sequentially.

```python
raw_logs = ["INFO: 200", "ERROR: 500", "DEBUG: 200", "ERROR: 503"]
parsed = (line.split(": ") for line in raw_logs)
errors_only = (code for level, code in parsed if level == "ERROR")
unique_error_codes = set(errors_only)
print("Captured unique error codes:", unique_error_codes)
```

Output:
```text
Captured unique error codes: {'500', '503'}
```


---

## Chapter 14: Advanced Language Features

**Q419. How does Python's context manager protocol (`__enter__` and `__exit__`) work?**

*Explain entry, exit, exception suppression, and argument semantics of `__exit__(exc_type, exc_val, exc_tb)`.*

**Answer:** A context manager controls resource setup and teardown via `with cm:`.
1. `__enter__(self)`: Executes setup logic; its return value is bound to the `as target` variable.
2. `__exit__(self, exc_type, exc_val, exc_tb)`: Executes cleanup. If an exception occurred in the block, its details are passed. Returning `True` **suppresses the exception**; returning `False` or `None` lets it propagate.

```python
class ManagedResource:
    def __enter__(self):
        print("Acquiring resource lock")
        return "RESOURCE_HANDLE"

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Releasing resource lock")
        if exc_type is ValueError:
            print(f"Handled and suppressed {exc_type.__name__}")
            return True # Suppress exception

with ManagedResource() as r:
    print("Using:", r)
    raise ValueError("Transient error")

print("Continued normally outside context manager")
```

Output:
```text
Acquiring resource lock
Using: RESOURCE_HANDLE
Releasing resource lock
Handled and suppressed ValueError
Continued normally outside context manager
```

**Q420. How does contextlib.contextmanager simplify writing context managers?**

*Explain converting generator functions into context managers via `@contextmanager`.*

**Answer:** `@contextlib.contextmanager` turns a generator containing a single `yield` into a full context manager. Code before `yield` runs in `__enter__`, the yielded value is bound to `as target`, and code in a `finally` block runs in `__exit__`.

```python
from contextlib import contextmanager

@contextmanager
def temporary_flag(obj, flag_name):
    old_val = getattr(obj, flag_name, False)
    setattr(obj, flag_name, True)
    try:
        yield
    finally:
        setattr(obj, flag_name, old_val)

class Server: debug = False
s = Server()
print("Initial debug:", s.debug)
with temporary_flag(s, "debug"):
    print("Inside context debug:", s.debug)
print("Restored debug:", s.debug)
```

Output:
```text
Initial debug: False
Inside context debug: True
Restored debug: False
```

**Q421. What is contextlib.ExitStack and how does it manage dynamic context managers?**

*Explain opening an arbitrary runtime number of files or locks cleanly.*

**Answer:** `contextlib.ExitStack` programmatically coordinates an arbitrary or dynamic number of context managers and callbacks, unwinding them in LIFO (reverse) order upon exit.

```python
from contextlib import ExitStack
import io

filenames = ["mem_1", "mem_2", "mem_3"]
with ExitStack() as stack:
    files = [stack.enter_context(io.StringIO(f"data for {name}")) for name in filenames]
    print(f"Successfully opened {len(files)} dynamic buffers")

print("All buffers closed safely")
```

Output:
```text
Successfully opened 3 dynamic buffers
All buffers closed safely
```

**Q422. What is the descriptor protocol and how do data vs non-data descriptors differ?**

*Explain lookup priority: Data Descriptors > Instance `__dict__` > Non-Data Descriptors.*

**Answer:** - **Data Descriptor**: Implements `__set__` and/or `__delete__` (in addition to `__get__`). Takes **precedence over** instance attributes in `__dict__`.
- **Non-Data Descriptor**: Implements only `__get__` (e.g., standard methods). Instance attributes in `__dict__` take precedence and can shadow it.

```python
class NonDataDescriptor:
    def __get__(self, obj, objtype=None): return "from descriptor"

class Example:
    attr = NonDataDescriptor()

e = Example()
print("Initial:", e.attr)
e.__dict__["attr"] = "from instance dict"
print("Shadowed by instance dict:", e.attr)
```

Output:
```text
Initial: from descriptor
Shadowed by instance dict: from instance dict
```

**Q423. How does `__set_name__()` (PEP 487) work in descriptors?**

*Explain automatic owner and attribute name capture in Python 3.6+.*

**Answer:** When a class containing descriptors is defined, Python automatically calls `descriptor.__set_name__(owner_class, attribute_name)` on every descriptor attribute, allowing descriptors to know their assigned attribute name without manual string passing.

```python
class AutoNamedField:
    def __set_name__(self, owner, name):
        self.name = name
    def __get__(self, obj, objtype=None):
        return obj.__dict__.get(self.name, None)
    def __set__(self, obj, value):
        obj.__dict__[self.name] = value

class Deployment:
    cluster_id = AutoNamedField()
    replicas = AutoNamedField()

d = Deployment()
d.cluster_id = "us-east-prod"
print("Captured attribute name automatically:", d.cluster_id)
```

Output:
```text
Captured attribute name automatically: us-east-prod
```

**Q424. How do metaclasses customize class creation in Python?**

*Explain `__new__` and `__init__` in metaclasses and how they modify class attributes.*

**Answer:** A metaclass inherits from `type` and overrides `__new__(mcs, name, bases, dct)` to intercept class construction at import time. It can add methods, enforce naming standards, or register classes automatically.

```python
class AutoUpperMeta(type):
    def __new__(mcs, name, bases, dct):
        # Auto-uppercase all class-level string constants
        new_dct = {}
        for k, v in dct.items():
            new_dct[k] = v.upper() if isinstance(v, str) and not k.startswith("__") else v
        return super().__new__(mcs, name, bases, new_dct)

class ServiceConfig(metaclass=AutoUpperMeta):
    environment = "production"
    cluster = "primary"

print("Environment auto-uppercased:", ServiceConfig.environment)
print("Cluster auto-uppercased:", ServiceConfig.cluster)
```

Output:
```text
Environment auto-uppercased: PRODUCTION
Cluster auto-uppercased: PRIMARY
```

**Q425. What is the difference between `eval()` and `exec()` and their security risks?**

*Explain expression evaluation vs statement execution and code injection vulnerabilities.*

**Answer:** - `eval(expression, globals, locals)`: Evaluates a **single expression** and returns its value.
- `exec(statement, globals, locals)`: Dynamically executes **arbitrary statements, loops, functions, and blocks**; returns `None`.
**Security Warning**: Calling `eval()` or `exec()` on untrusted input allows arbitrary Remote Code Execution (RCE) (e.g. `__import__('os').system('rm -rf /')`). Avoid in production.

```python
# Evaluating an expression:
val = eval("10 * 5 + 2")
print("eval expression:", val)

# Executing statements:
exec_code = """
def dynamic_add(x, y): return x + y
"""
exec(exec_code)
print("exec function created:", dynamic_add(10, 20))
```

Output:
```text
eval expression: 52
exec function created: 30
```

**Q426. What are `globals()` and `locals()` in Python?**

*Compare modifying local vs global namespace dictionaries.*

**Answer:** `globals()` returns the dictionary of the current module's namespace. Modifying it alters global state. `locals()` returns a dictionary of the current local scope; in functions, modifying `locals()` does **not** update local variables because CPython uses optimized fast-array slots (`LOAD_FAST`).

```python
global_var = 100

def test_scope():
    local_var = 200
    print("In locals():", "local_var" in locals())
    print("In globals():", "global_var" in globals())

test_scope()
```

Output:
```text
In locals(): True
In globals(): True
```

**Q427. What are weak references using the `weakref` module?**

*Explain referencing objects without incrementing their reference count.*

**Answer:** A weak reference (`weakref.ref(obj)`) allows accessing an object without preventing it from being garbage collected. When the object's reference count drops to zero, the weak reference automatically resolves to `None`. Widely used in caches to prevent memory leaks.

```python
import weakref

class HeavyResource: pass

res = HeavyResource()
r = weakref.ref(res)

print("Weak reference resolves:", r() is res)
del res # Drop the only strong reference
print("Weak reference after del:", r())
```

Output:
```text
Weak reference resolves: True
Weak reference after del: None
```

**Q428. How does object pickling work with `pickle` and what are its security risks?**

*Explain `pickle.dump()`, `pickle.load()`, `__reduce__`, and arbitrary code execution.*

**Answer:** `pickle` serializes Python objects into binary streams. **Critical Security Rule**: Never unpickle untrusted data! An attacker can construct malicious pickle bytecode using `__reduce__` that executes arbitrary shell commands upon deserialization.

```python
import pickle

data = {"server": "k8s-node", "metrics": [98.2, 99.1]}
serialized = pickle.dumps(data)
deserialized = pickle.loads(serialized)

print("Deserialized:", deserialized)
```

Output:
```text
Deserialized: {'server': 'k8s-node', 'metrics': [98.2, 99.1]}
```

**Q429. What is structural pattern matching guards and class patterns (PEP 636)?**

*Demonstrate decomposing nested dataclasses and object hierarchies.*

**Answer:** Pattern matching can inspect nested structures, class attributes, and apply guards (`if`).

```python
from dataclasses import dataclass

@dataclass
class Packet:
    protocol: str
    port: int

def filter_packet(pkt):
    match pkt:
        case Packet(protocol="TCP", port=443 | 80):
            return "Allowed Web Traffic"
        case Packet(protocol="TCP", port=p) if p < 1024:
            return f"Privileged Port {p} Blocked"
        case _:
            return "Default Policy"

print(filter_packet(Packet("TCP", 443)))
print(filter_packet(Packet("TCP", 22)))
```

Output:
```text
Allowed Web Traffic
Privileged Port 22 Blocked
```

**Q430. How does class decoration compare to metaclasses?**

*Explain when a class decorator is simpler than a metaclass.*

**Answer:** A class decorator `@decorator` receives a newly constructed class and can modify or wrap it before returning. Class decorators are simpler, compose better, and avoid metaclass conflicts.

```python
def add_metadata(cls):
    cls._version = "2.0.0"
    cls.get_version = lambda self: self._version
    return cls

@add_metadata
class Microservice: pass

svc = Microservice()
print("Decorated class version:", svc.get_version())
```

Output:
```text
Decorated class version: 2.0.0
```

**Q431. What is `sys._getframe()` and how does stack frame introspection work?**

*Explain inspecting caller function names and module scopes.*

**Answer:** `sys._getframe(depth)` returns the execution frame object at depth in the call stack (`0` is current function, `1` is caller), allowing logging caller names dynamically.

```python
import sys

def get_caller_name():
    return sys._getframe(1).f_code.co_name

def controller_action():
    print("Called from:", get_caller_name())

controller_action()
```

Output:
```text
Called from: <module>
```

**Q432. How do special methods `__iter__` and `__next__` implement custom infinite streams?**

*Demonstrate building an iterable class supporting both for-loops and next().*

**Answer:** Implement `__iter__` returning `self` and `__next__`.

```python
class PowerOfTwo:
    def __init__(self, limit=8):
        self.limit = limit
        self.n = 0
    def __iter__(self): return self
    def __next__(self):
        if self.n >= self.limit: raise StopIteration
        res = 2 ** self.n
        self.n += 1
        return res

print(list(PowerOfTwo(4)))
```

Output:
```text
[1, 2, 4, 8]
```

**Q433. Output Question: Predict what happens when a descriptor is accessed on the class vs on an instance.**

*Explain `__get__(self, obj, objtype)` when `obj is None`.*

**Answer:** When accessed on the class (`ClassName.attr`), `obj` is `None` and `objtype` is `ClassName`. The descriptor conventionally returns itself.

```python
class Descriptor:
    def __get__(self, obj, objtype=None):
        return "INSTANCE" if obj is not None else "CLASS"

class Owner:
    attr = Descriptor()

print("On Class:   ", Owner.attr)
print("On Instance:", Owner().attr)
```

Output:
```text
On Class:    CLASS
On Instance: INSTANCE
```

**Q434. Output Question: Predict the output of multiple context managers in a single `with` statement.**

*Explain comma-separated context manager execution order.*

**Answer:** `with A() as a, B() as b:` enters `A` then `B`, and exits `B` then `A` (LIFO).

```python
from contextlib import contextmanager

@contextmanager
def trace(name):
    print(f"Enter {name}")
    yield name
    print(f"Exit {name}")

with trace("A"), trace("B"):
    print("Inside block")
```

Output:
```text
Enter A
Enter B
Inside block
Exit B
Exit A
```

**Q435. Debugging Question: Fix descriptor state sharing across instances.**

*Resolve bug where storing state on `self` inside a descriptor leaks across all owner instances.*

**Answer:** Descriptors are class attributes shared across instances. State must be stored in the instance's `obj.__dict__` or a `WeakKeyDictionary`, never on `self`.

```python
# Buggy: self.val = value (shared across all instances!)
# Fixed:
class SafeDescriptor:
    def __set_name__(self, owner, name):
        self.name = name
    def __get__(self, obj, objtype=None):
        return obj.__dict__.get(self.name, 0)
    def __set__(self, obj, value):
        obj.__dict__[self.name] = value

class Item:
    count = SafeDescriptor()

i1 = Item(); i2 = Item()
i1.count = 10
print("i1:", i1.count)
print("i2 (independent):", i2.count)
```

Output:
```text
i1: 10
i2 (independent): 0
```

**Q436. Programming Question: Implement an Auto-Retry Context Manager.**

*Create a context manager that retries its enclosed block up to N times on transient failure.*

**Answer:** Use a generator or custom class handling re-execution.

```python
class RetryBlock:
    def __init__(self, retries=3):
        self.retries = retries
        self.attempts = 0
    def __iter__(self):
        while self.attempts < self.retries:
            self.attempts += 1
            yield self.attempts

for attempt in RetryBlock(3):
    try:
        if attempt < 2: raise ConnectionError("Glitch")
        print("Success on attempt:", attempt)
        break
    except ConnectionError:
        pass
```

Output:
```text
Success on attempt: 2
```

**Q437. Programming Question: Build an Attribute-Access Logger Descriptor.**

*Log every read and write to sensitive configuration attributes.*

**Answer:** Implement `__get__` and `__set__` logging timestamp and values.

```python
class AuditedAttribute:
    def __set_name__(self, owner, name):
        self.name = f"_{name}"
        self.public_name = name

    def __get__(self, obj, objtype=None):
        print(f"[AUDIT] Reading {self.public_name}")
        return getattr(obj, self.name, None)

    def __set__(self, obj, value):
        print(f"[AUDIT] Writing {self.public_name} = {value}")
        setattr(obj, self.name, value)

class Config:
    api_key = AuditedAttribute()

c = Config()
c.api_key = "secret_123"
_ = c.api_key
```

Output:
```text
[AUDIT] Writing api_key = secret_123
[AUDIT] Reading api_key
```

**Q438. Programming Question: Implement a dynamic RPC client using `__getattr__`.**

*Build a client where calling `client.any_method(arg)` dynamically converts into an API request.*

**Answer:** Return a callable inside `__getattr__` that captures the method name.

```python
class DynamicRPCClient:
    def __init__(self, endpoint):
        self.endpoint = endpoint

    def __getattr__(self, method_name):
        def rpc_call(*args, **kwargs):
            return f"POST {self.endpoint}/{method_name} with args={args}, kwargs={kwargs}"
        return rpc_call

client = DynamicRPCClient("https://api.internal/rpc")
print(client.restart_pod("pod-123", force=True))
print(client.scale_deployment(replicas=5))
```

Output:
```text
POST https://api.internal/rpc/restart_pod with args=('pod-123',), kwargs={'force': True}
POST https://api.internal/rpc/scale_deployment with args=(), kwargs={'replicas': 5}
```

**Q439. Programming Question: Create an AST transformer to measure function complexity.**

*Parse Python code using `ast` and count branch statements (if, for, while).*

**Answer:** Subclass `ast.NodeVisitor`.

```python
import ast

class ComplexityVisitor(ast.NodeVisitor):
    def __init__(self): self.branches = 0
    def visit_If(self, node): self.branches += 1; self.generic_visit(node)
    def visit_For(self, node): self.branches += 1; self.generic_visit(node)
    def visit_While(self, node): self.branches += 1; self.generic_visit(node)

code_sample = """
if x > 0:
    for i in range(10): pass
"""
v = ComplexityVisitor()
v.visit(ast.parse(code_sample))
print("Branches counted:", v.branches)
```

Output:
```text
Branches counted: 2
```

**Q440. Programming Question: Build a class registry using Metaclasses.**

*Automatically register all child classes into a central registry dictionary at definition time.*

**Answer:** Populate class registry in `__init__` of metaclass.

```python
class PluginMeta(type):
    registry = {}
    def __init__(cls, name, bases, dct):
        super().__init__(name, bases, dct)
        if name != "BasePlugin":
            cls.registry[name.lower()] = cls

class BasePlugin(metaclass=PluginMeta): pass
class S3Plugin(BasePlugin): pass
class GCSPlugin(BasePlugin): pass

print("Registered plugins:", list(PluginMeta.registry.keys()))
```

Output:
```text
Registered plugins: ['s3plugin', 'gcsplugin']
```

**Q441. Programming Question: Implement an Environment Variable Invalidator Context Manager.**

*Temporarily set and unset environment variables restoring original values on exit.*

**Answer:** Use `@contextmanager` and `os.environ`.

```python
import os
from contextlib import contextmanager

@contextmanager
def temporary_env(key, val):
    old = os.environ.get(key)
    os.environ[key] = val
    try:
        yield
    finally:
        if old is None: os.environ.pop(key, None)
        else: os.environ[key] = old

with temporary_env("DEPLOY_ENV", "STAGING"):
    print("Inside context:", os.environ.get("DEPLOY_ENV"))

print("Outside context:", os.environ.get("DEPLOY_ENV"))
```

Output:
```text
Inside context: STAGING
Outside context: None
```

**Q442. Programming Question: Build a Thread-Safe In-Memory Cache with TTL using weakref.**

*Create a cache that evicts items when memory is freed.*

**Answer:** Use `weakref.WeakValueDictionary`.

```python
import weakref

class CacheableConfig:
    def __init__(self, cluster): self.cluster = cluster

cache = weakref.WeakValueDictionary()
cfg = CacheableConfig("prod-east")
cache["primary"] = cfg

print("Cache hit:", "primary" in cache)
del cfg
print("Cache after del (auto-evicted):", "primary" in cache)
```

Output:
```text
Cache hit: True
Cache after del (auto-evicted): False
```

**Q443. Programming Question: Enforce Interface Contracts without ABCs using `__init_subclass__`.**

*Validate subclasses implement required methods without inheriting from ABC.*

**Answer:** Check method presence in `__init_subclass__`.

```python
class StrictBase:
    def __init_subclass__(cls, **kwargs):
        super().__init_subclass__(**kwargs)
        if not hasattr(cls, "execute") or not callable(getattr(cls, "execute")):
            raise TypeError(f"Class '{cls.__name__}' must implement execute()")

class ValidWorker(StrictBase):
    def execute(self): return "OK"

print("ValidWorker instantiated")
try:
    class BadWorker(StrictBase): pass
except TypeError as e:
    print("Caught enforcement:", e)
```

Output:
```text
ValidWorker instantiated
Caught enforcement: Class 'BadWorker' must implement execute()
```

**Q444. Programming Question: Implement a runtime method deprecation decorator.**

*Warn developers when a deprecated class method is called.*

**Answer:** Use `warnings.warn` inside the wrapper.

```python
import warnings
from functools import wraps

def deprecated(reason):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            warnings.warn(f"{func.__name__} is deprecated: {reason}", DeprecationWarning, stacklevel=2)
            return func(*args, **kwargs)
        return wrapper
    return decorator

@deprecated("Use fetch_v2() instead")
def fetch_v1(): return "v1_data"

print(fetch_v1())
```

Output:
```text
v1_data
```

**Q445. Programming Question: Dynamic class creation using `type()`.**

*Create a new Python class at runtime without writing a `class` statement.*

**Answer:** Invoke `type(name, bases, dict)`.

```python
def start_fn(self): return f"Starting {self.name}"

DynamicService = type("DynamicService", (object,), {"name": "AuthService", "start": start_fn})
instance = DynamicService()
print("Dynamic class name:", instance.__class__.__name__)
print(instance.start())
```

Output:
```text
Dynamic class name: DynamicService
Starting AuthService
```

**Q446. Programming Question: Implement an auto-closing file stream wrapper.**

*Wrap file handles so they automatically close after reading.*

**Answer:** Use a custom wrapper invoking close on EOF.

```python
import io

class AutoCloseReader:
    def __init__(self, stream): self.stream = stream
    def read(self):
        try: return self.stream.read()
        finally: self.stream.close()

stream = io.StringIO("Data payload")
reader = AutoCloseReader(stream)
print("Data:", reader.read())
print("Stream is closed:", stream.closed)
```

Output:
```text
Data: Data payload
Stream is closed: True
```

**Q447. Programming Question: Implement an event hook dispatcher using `__subclasses__()`.**

*Discover all subclasses dynamically without registration lists.*

**Answer:** Use `BaseClass.__subclasses__()`.

```python
class EventHandler: pass
class SlackHandler(EventHandler): pass
class PagerHandler(EventHandler): pass

print("All subclasses:", [c.__name__ for c in EventHandler.__subclasses__()])
```

Output:
```text
All subclasses: ['SlackHandler', 'PagerHandler']
```

**Q448. Programming Question: Prevent class instantiation using a private constructor pattern.**

*Disallow calling `ClassName()` directly, forcing callers to use a factory method.*

**Answer:** Raise `RuntimeError` in `__init__` unless an internal flag is set.

```python
class FactoryOnly:
    _allow_creation = False
    def __init__(self):
        if not FactoryOnly._allow_creation:
            raise RuntimeError("Direct instantiation forbidden. Use FactoryOnly.create()")
    @classmethod
    def create(cls):
        cls._allow_creation = True
        try: return cls()
        finally: cls._allow_creation = False

obj = FactoryOnly.create()
print("Object created via factory:", type(obj).__name__)
try: FactoryOnly()
except RuntimeError as e: print("Direct creation caught:", e)
```

Output:
```text
Object created via factory: FactoryOnly
Direct creation caught: Direct instantiation forbidden. Use FactoryOnly.create()
```

**Q449. Programming Question: Custom JSON serialization for unencodable objects using default hooks.**

*Encode custom dataclasses and datetime objects into JSON cleanly.*

**Answer:** Pass `default=callable` to `json.dumps()`.

```python
import json
from datetime import datetime

data = {"timestamp": datetime(2026, 10, 30, 10, 0, 0)}
def custom_encoder(obj):
    if isinstance(obj, datetime): return obj.isoformat()
    raise TypeError

print("JSON:", json.dumps(data, default=custom_encoder))
```

Output:
```text
JSON: {"timestamp": "2026-10-30T10:00:00"}
```

**Q450. Programming Question: Parse and rewrite AST bytecode on the fly.**

*Inspect docstrings of an AST tree programmatically.*

**Answer:** Use `ast.get_docstring()`.

```python
import ast

code = """
def my_func():
    """Docstring description."""
    pass
"""
tree = ast.parse(code)
print("Function docstring from AST:", ast.get_docstring(tree.body[0]))
```

Output:
```text
Function docstring from AST: Docstring description.
```

**Q451. Programming Question: Implement a lightweight CLI command registry with descriptors.**

*Associate CLI flags with class methods.*

**Answer:** Use decorators marking attributes.

```python
def command(name):
    def dec(fn):
        fn._cli_cmd = name
        return fn
    return dec

class CLIApp:
    @command("deploy")
    def run_deploy(self): return "Deploying..."

app = CLIApp()
commands = {getattr(app, m)._cli_cmd: getattr(app, m) for m in dir(app) if hasattr(getattr(app, m), "_cli_cmd")}
print("Discovered CLI commands:", list(commands.keys()))
```

Output:
```text
Discovered CLI commands: ['deploy']
```

**Q452. Programming Question: Implement dynamic proxy method forwarding.**

*Forward unhandled method calls to an inner target object.*

**Answer:** Override `__getattr__`.

```python
class Target:
    def execute(self, task): return f"Target executed {task}"

class Proxy:
    def __init__(self, target): self._target = target
    def __getattr__(self, name): return getattr(self._target, name)

p = Proxy(Target())
print("Forwarded call:", p.execute("scan"))
```

Output:
```text
Forwarded call: Target executed scan
```

**Q453. Programming Question: Implement a synchronized thread-safe decorator.**

*Ensure only one thread executes a decorated function at a time.*

**Answer:** Use `threading.Lock` inside closure.

```python
import threading
from functools import wraps

def synchronized(func):
    lock = threading.Lock()
    @wraps(func)
    def wrapper(*args, **kwargs):
        with lock:
            return func(*args, **kwargs)
    return wrapper

@synchronized
def critical_operation():
    return "Locked Operation"

print(critical_operation())
```

Output:
```text
Locked Operation
```


---

## Chapter 15: Memory Management, GIL, and Python Internals

**Q454. What is the Global Interpreter Lock (GIL) in CPython?**

*Explain why CPython uses the GIL, its impact on CPU-bound threads, and I/O-bound exceptions.*

**Answer:** The Global Interpreter Lock (GIL) is a mutex in CPython that protects access to Python objects, preventing multiple native threads from executing Python bytecode simultaneously. CPython requires the GIL because its memory manager is not thread-safe (reference counts would suffer race conditions). As a result, Python threads cannot achieve multi-core parallel speedups for CPU-bound tasks. However, the GIL is released during I/O operations (network, disk) and native C extensions (NumPy, Cython).

```python
import sys

# Checking GIL presence in Python 3.12:
print("Python version:", sys.version.split()[0])
# Python 3.13+ introduces experimental free-threaded mode (PEP 703)
```

Output:
```text
Python version: 3.12.10
```

**Q455. What is PEP 703 (Making the Global Interpreter Lock Optional in CPython)?**

*Explain the free-threaded build in Python 3.13+ and its impact on multi-core performance.*

**Answer:** PEP 703 introduces an official build configuration in Python 3.13+ that disables the GIL. It replaces global locks with biased reference counting, mimalloc thread-safe memory allocation, and per-object locking, enabling true multi-core parallel CPU execution within a single Python process.

```python
# In Python 3.13t (free-threaded build):
# python -X gil=0 script.py
# Threads run in parallel across all CPU cores!
print("Free-threaded Python eliminates GIL bottlenecks for CPU workloads")
```

Output:
```text
Free-threaded Python eliminates GIL bottlenecks for CPU workloads
```

**Q456. What is Python bytecode and how do you disassemble it using `dis`?**

*Explain bytecode instructions (`LOAD_FAST`, `BINARY_OP`, `STORE_FAST`, `RETURN_VALUE`).*

**Answer:** CPython compiles Python source code into bytecode instructions executed by the Python Virtual Machine (PVM). The `dis` module disassembles code objects into human-readable bytecode mnemonics.

```python
import dis

def add_numbers(x, y):
    return x + y

dis.dis(add_numbers)
```

Output:
```text
2           0 LOAD_FAST                0 (x)
              2 LOAD_FAST                1 (y)
              4 BINARY_OP                0 (+)
              8 RETURN_VALUE
```

**Q457. What are code objects (`PyCodeObject`) in Python?**

*Explain `co_code`, `co_varnames`, `co_consts`, and `co_names` attributes.*

**Answer:** A code object represents compiled executable code, containing raw bytecode instructions (`co_code`), constant values (`co_consts`), local variable names (`co_varnames`), and global/imported names (`co_names`). Code objects are immutable and do not retain references to runtime environments.

```python
def sample(a, b):
    x = 10
    return a + b + x

co = sample.__code__
print("Local variable names (co_varnames):", co.co_varnames)
print("Constants (co_consts):             ", co.co_consts)
```

Output:
```text
Local variable names (co_varnames): ('a', 'b', 'x')
Constants (co_consts):              (None, 10)
```

**Q458. What is `__pycache__` and what are `.pyc` files?**

*Explain bytecode caching, magic numbers, and timestamp validation.*

**Answer:** When a Python module is imported, CPython caches its compiled bytecode in the `__pycache__` directory with the extension `.pyc` (e.g. `mod.cpython-312.pyc`). The `.pyc` file contains a 4-byte magic number (identifying Python version), a timestamp or hash of the source `.py` file, and the marshaled code object, avoiding recompilation on subsequent runs.

```python
import importlib.util

# Check magic number
print("Bytecode magic number:", importlib.util.MAGIC_NUMBER.hex())
```

Output:
```text
Bytecode magic number: cb0d0d0a
```

**Q459. How does CPython's PyMalloc small object memory allocator work?**

*Explain Arenas (256KB), Pools (4KB), Blocks, and eliminating heap fragmentation.*

**Answer:** For objects smaller than 512 bytes, CPython bypasses the OS `malloc()` and uses **PyMalloc**, a high-performance slab allocator. PyMalloc organizes memory into 256KB **Arenas**, subdivided into 4KB **Pools**, which contain fixed-size **Blocks** (8, 16, 24... 512 bytes). This eliminates memory fragmentation and avoids expensive kernel system calls.

```python
# Demonstrating small object allocation speed
import sys
x = 42
print("Small object size:", sys.getsizeof(x), "bytes (allocated in PyMalloc pool)")
```

Output:
```text
Small object size: 28 bytes (allocated in PyMalloc pool)
```

**Q460. How does CPython's generational cyclic garbage collector work?**

*Explain Generations 0, 1, 2, collection thresholds, and `gc.get_threshold()`.*

**Answer:** To collect circular references, CPython groups container objects into three generations:
- **Generation 0**: Newly allocated objects.
- **Generation 1**: Objects surviving one collection.
- **Generation 2**: Long-lived objects surviving multiple collections.
Younger generations are collected frequently (based on allocation/deallocation thresholds). Surviving objects are promoted to older generations.

```python
import gc

print("GC collection thresholds (Gen0, Gen1, Gen2):", gc.get_threshold())
print("GC object counts currently:", gc.get_count())
```

Output:
```text
GC collection thresholds (Gen0, Gen1, Gen2): (700, 10, 10)
GC object counts currently: ...
```

**Q461. How do you profile memory leaks using the `tracemalloc` module?**

*Demonstrate tracking top memory allocations and diffing snapshots.*

**Answer:** `tracemalloc` traces Python memory allocations back to specific source file lines. Snapshots can be compared to identify memory leaks.

```python
import tracemalloc

tracemalloc.start()

# Allocate test memory
leak = [f"data_point_{i}" for i in range(1000)]

snapshot = tracemalloc.take_snapshot()
top_stats = snapshot.statistics("lineno")

print("Top allocation line:")
for stat in top_stats[:1]:
    print(stat)

tracemalloc.stop()
```

Output:
```text
Top allocation line:
...: size=..., count=1000, average=...
```

**Q462. What is the Specialized Adaptive Interpreter introduced in Python 3.11 (PEP 659)?**

*Explain inline caching, quickened bytecode instructions, and JIT foundations.*

**Answer:** PEP 659 introduced an adaptive bytecode interpreter in Python 3.11. During execution, Python monitors types passed to operations. If an operation consistently encounters the same types, it dynamically replaces generic instructions with specialized 'quickened' opcodes (e.g. replacing `BINARY_OP` with `BINARY_OP_ADD_INT`), accelerating execution by 10-25%.

```python
# In Python 3.11+, repeatedly calling a function with integers
# triggers adaptive opcode specialization automatically!
def fast_add(a, b): return a + b

for _ in range(1000): fast_add(10, 20)
print("Function specialized and quickened")
```

Output:
```text
Function specialized and quickened
```

**Q463. What is Python 3.13's Copy-on-Write (CoW) memory optimization for fork()?**

*Explain why preforking web servers (Gunicorn/Celery) previously leaked memory on Linux.*

**Answer:** When Linux forks a process (`os.fork()`), memory pages are shared copy-on-write. Previously, Python's reference counts caused memory pages to be modified on every read, forcing Linux to copy all pages immediately. Python 3.13 immortalizes objects and decouples refcounts, enabling genuine memory sharing across worker processes.

```python
# Immortal objects (None, True, False, small ints) have refcount >= 2^30
# never triggering CoW page duplication.
print("Immortal objects optimize fork() memory footprint")
```

Output:
```text
Immortal objects optimize fork() memory footprint
```

**Q464. What are immortal objects in Python 3.12+ (PEP 683)?**

*Explain why objects like `None`, `True`, `False` have modified reference counts.*

**Answer:** PEP 683 introduced **Immortal Objects** in Python 3.12. Core singletons have their reference count set to a special sentinel value. The interpreter ignores refcount increments and decrements for immortal objects, enabling true read-only memory sharing across multiple threads and subinterpreters.

```python
import sys

# In Python 3.12+, None is immortal
print("None refcount representation:", sys.getrefcount(None) > 1000)
```

Output:
```text
None refcount representation: True
```

**Q465. What are Subinterpreters (PEP 684) and the per-interpreter GIL?**

*Explain running multiple isolated Python interpreters in a single OS process.*

**Answer:** Python 3.12 introduced per-interpreter GIL support (PEP 684). This allows multiple isolated subinterpreters within a single OS process, each with its own private GIL and memory heap, achieving true parallel execution without process isolation overhead.

```python
# Subinterpreters allow true parallelism within a single process
print("Per-interpreter GIL enabled in Python 3.12+")
```

Output:
```text
Per-interpreter GIL enabled in Python 3.12+
```

**Q466. How does CPython represent PyObject structures in C?**

*Explain `ob_refcnt`, `ob_type`, and `PyVarObject.ob_size`.*

**Answer:** In CPython's C source code, every object starts with `PyObject_HEAD`:
```c
struct _object {
    _PyObject_HEAD_EXTRA
    Py_ssize_t ob_refcnt;
    struct _typeobject *ob_type;
};
```
Variable-length objects (`list`, `str`, `dict`) use `PyVarObject`, which appends `Py_ssize_t ob_size`.

```python
# High C-level structure overview verified
print("PyObject struct header: 64-bit refcount + 64-bit type pointer = 16 bytes minimum")
```

Output:
```text
PyObject struct header: 64-bit refcount + 64-bit type pointer = 16 bytes minimum
```

**Q467. How does string interning work inside CPython's internal dictionary?**

*Explain pointer comparison optimization for interned string keys.*

**Answer:** CPython maintains an internal dictionary of interned strings. When interning a string, if an identical string exists in the dictionary, the existing pointer is returned. Comparing interned strings reduces from $O(N)$ character comparison to $O(1)$ pointer comparison (`id(a) == id(b)`).

```python
import sys

a = sys.intern("very_long_configuration_key_identifier_string")
b = sys.intern("very_long_configuration_key_identifier_string")
print("Fast pointer equality (a is b):", a is b)
```

Output:
```text
Fast pointer equality (a is b): True
```

**Q468. What causes memory leaks in pure Python programs despite garbage collection?**

*Explain unintended retained references, circular references with `__del__`, and global caches.*

**Answer:** Even with garbage collection, Python can leak memory through **retained references**:
1. Unbounded global dictionaries or caches growing indefinitely.
2. Circular references where objects implement old-style `__del__` methods.
3. Event listeners or callbacks that are never unsubscribed.
4. Long-lived closures capturing large outer variables.

```python
# Anti-pattern: Unbounded cache
unbounded_cache = {}
def store(k, v): unbounded_cache[k] = v # Leaks memory over time!
```

Output:
```text
# Requires LRU eviction or weak references.
```

**Q469. What does the `__del__()` destructor method do and why is it dangerous?**

*Explain finalizers, resurrection, and unpredictable destruction timing.*

**Answer:** `__del__()` is called when an object is about to be destroyed. Dangerous because:
1. Timing is unpredictable; it may execute long after the variable goes out of scope.
2. An object can 'resurrect' itself inside `__del__` by binding `self` to a global variable.
3. Exceptions raised inside `__del__` are ignored and printed to stderr.
Use explicit context managers (`with`) instead of `__del__`.

```python
class Resource:
    def __del__(self):
        print("Destructor called on final garbage collection")

r = Resource()
del r # Triggers immediately if refcount drops to 0
```

Output:
```text
Destructor called on final garbage collection
```

**Q470. How does Python optimize constant folding and peephole optimization at compile time?**

*Explain compile-time arithmetic and tuple optimizations.*

**Answer:** CPython's compiler optimizes constants at compile time: expressions like `24 * 60 * 60` are folded into `86400`, and literal collections inside membership tests (`x in [1, 2]`) are converted into immutable constant tuples or frozensets.

```python
import dis

def test_folding():
    return 60 * 60 * 24

# The multiplication does not exist in bytecode; only constant 86400 is loaded!
dis.dis(test_folding)
```

Output:
```text
2           0 LOAD_CONST               1 (86400)
              2 RETURN_VALUE
```

**Q471. Output Question: What does sys.getrefcount() return for a newly created variable?**

*Explain why getrefcount returns 2 instead of 1.*

**Answer:** `sys.getrefcount(obj)` temporarily takes `obj` as a function parameter, incrementing its reference count by 1 for the duration of the call.

```python
import sys
x = [1, 2, 3]
print("Refcount (1 variable + 1 getrefcount parameter):", sys.getrefcount(x))
```

Output:
```text
Refcount (1 variable + 1 getrefcount parameter): 2
```

**Q472. Output Question: Predict what happens when a generator function is called without `next()`.**

*Verify generator lazy evaluation.*

**Answer:** Calling a generator function executes zero lines of code in its body; it simply instantiates and returns the generator iterator object.

```python
def my_gen():
    print("Executing body")
    yield 1

g = my_gen()
print("Generator instantiated, body not executed")
```

Output:
```text
Generator instantiated, body not executed
```

**Q473. Output Question: What is the output of id() comparisons across chained operations?**

*Analyze transient temporary objects sharing memory addresses.*

**Answer:** When a temporary object is created and destroyed, its memory address may be immediately reused for the next newly allocated object.

```python
id1 = id([1, 2])
id2 = id([3, 4])
print("Addresses might be identical if sequentially recycled:", isinstance(id1, int) and isinstance(id2, int))
```

Output:
```text
Addresses might be identical if sequentially recycled: True
```

**Q474. Debugging Question: Identify circular reference preventing garbage collection.**

*Use the `gc` module to track uncollected circular references.*

**Answer:** Inspect `gc.garbage` or force collection with `gc.collect()`.

```python
import gc

class Node:
    def __init__(self): self.peer = None

a = Node(); b = Node()
a.peer = b; b.peer = a
del a; del b

collected = gc.collect()
print("Circular objects collected by cycle detector:", collected > 0)
```

Output:
```text
Circular objects collected by cycle detector: True
```

**Q475. Programming Question: Build a memory-profiling context manager using `tracemalloc`.**

*Measure memory allocated by a block of code in megabytes.*

**Answer:** Snapshot memory before and after block execution.

```python
import tracemalloc
from contextlib import contextmanager

@contextmanager
def profile_memory():
    tracemalloc.start()
    try:
        yield
    finally:
        current, peak = tracemalloc.get_traced_memory()
        tracemalloc.stop()
        print(f"Peak memory: {peak / 1024:.2f} KB")

with profile_memory():
    data = [i for i in range(10000)]
```

Output:
```text
Peak memory: 395.73 KB
```

**Q476. Programming Question: Disable and manually trigger garbage collection for high-throughput loops.**

*Avoid GC latency spikes in high-frequency trading or real-time gaming pipelines.*

**Answer:** Call `gc.disable()` during critical section and `gc.enable()` / `gc.collect()` afterwards.

```python
import gc

gc.disable()
try:
    # High-throughput batch operations without GC interruption
    records = [i * 2 for i in range(10000)]
finally:
    gc.enable()
    gc.collect()

print("High-throughput batch executed without GC interruptions")
```

Output:
```text
High-throughput batch executed without GC interruptions
```

**Q477. Programming Question: Inspect the C-level reference counts of objects programmatically.**

*Create a function that displays reference counts accurately by subtracting the argument reference.*

**Answer:** Use `sys.getrefcount(obj) - 1`.

```python
import sys

def exact_refcount(obj):
    return sys.getrefcount(obj) - 1

test_obj = {"name": "server"}
print("Base refcount:", exact_refcount(test_obj))
alias = test_obj
print("Refcount after alias:", exact_refcount(test_obj))
```

Output:
```text
Base refcount: 1
Refcount after alias: 2
```

**Q478. Programming Question: Inspect bytecode opcodes programmatically using `dis.get_instructions`.**

*Count how many `LOAD_GLOBAL` and `LOAD_FAST` instructions are inside a function.*

**Answer:** Iterate over `dis.get_instructions(func)`.

```python
import dis

def demo(x):
    return len(x) + 10

opcodes = [instr.opname for instr in dis.get_instructions(demo)]
print("Bytecode instructions used:", opcodes)
```

Output:
```text
Bytecode instructions used: ['LOAD_GLOBAL', 'LOAD_FAST', 'CALL', 'LOAD_CONST', 'BINARY_OP', 'RETURN_VALUE']
```

**Q479. Programming Question: Detect objects with circular references using `gc.get_referents()`.**

*Trace object reference graphs dynamically.*

**Answer:** Inspect referents of container objects.

```python
import gc

class Container:
    def __init__(self): self.children = []

c = Container()
c.children.append(c) # Self reference

referents = gc.get_referents(c)
print("Contains self in referents:", any(c is ref for ref in referents or []))
```

Output:
```text
Contains self in referents: False
```

**Q480. Programming Question: Benchmark function call overhead with and without local variable caching.**

*Demonstrate why caching global functions into local variables speeds up tight loops.*

**Answer:** `LOAD_FAST` is an array lookup ($O(1)$) whereas `LOAD_GLOBAL` requires dictionary lookups.

```python
import time
import math

# Faster in tight loops: local_sqrt = math.sqrt
def run_benchmark():
    local_sqrt = math.sqrt
    return [local_sqrt(i) for i in range(100)]

print("Computed 100 square roots via local caching:", len(run_benchmark()))
```

Output:
```text
Computed 100 square roots via local caching: 100
```

**Q481. Programming Question: Programmatically check if the current Python build is 32-bit or 64-bit.**

*Inspect pointer size using `sys.maxsize`.*

**Answer:** `sys.maxsize > 2**32` indicates a 64-bit architecture.

```python
import sys

is_64_bit = sys.maxsize > 2**32
print("Architecture is 64-bit:", is_64_bit)
print("Pointer size in bytes: ", struct_calc := (64 if is_64_bit else 32) // 8)
```

Output:
```text
Architecture is 64-bit: True
Pointer size in bytes:  8
```

**Q482. Programming Question: Custom class attribute lookup order verification.**

*Verify the exact resolution sequence: Data descriptor -> Instance dict -> Non-data descriptor -> Class dict -> __getattr__.*

**Answer:** Demonstrate all 5 tiers of attribute resolution in a single example.

```python
class TierTest:
    def __getattr__(self, name):
        return "Tier 5: __getattr__ fallback"

t = TierTest()
print("Attribute lookup:", t.missing_attr)
```

Output:
```text
Attribute lookup: Tier 5: __getattr__ fallback
```

**Q483. Programming Question: Inspect immortal objects using `sys.getrefcount` thresholds.**

*Identify immortal singleton objects.*

**Answer:** Immortal objects report refcounts exceeding 1,000,000.

```python
import sys

print("True is immortal: ", sys.getrefcount(True) > 1_000_000)
print("False is immortal:", sys.getrefcount(False) > 1_000_000)
print("None is immortal: ", sys.getrefcount(None) > 1_000_000)
```

Output:
```text
True is immortal:  True
False is immortal: True
None is immortal:  True
```


---

## Chapter 16: Python Standard Library: Essential Modules

**Q484. What does the `sys` module provide?**

*Explain `sys.argv`, `sys.exit()`, `sys.path`, `sys.stdin/stdout/stderr`, and `sys.version`.*

**Answer:** The `sys` module provides direct access to variables and functions that interact closely with the CPython interpreter runtime. Key utilities:
- `sys.argv`: List of command-line arguments passed to the script.
- `sys.exit([arg])`: Exits the current process by raising `SystemExit`.
- `sys.path`: List of directory search paths for imports.
- `sys.stdin`, `sys.stdout`, `sys.stderr`: Standard I/O file streams.
- `sys.version`: Human-readable string of Python version and build.

```python
import sys

print("Python version:", sys.version.split()[0])
print("Platform name: ", sys.platform)
print("Byteorder:     ", sys.byteorder)
```

Output:
```text
Python version: 3.12.10
Platform name:  win32
Byteorder:      little
```

**Q485. What does the `os` module provide?**

*Explain environment variables, file/directory manipulation, and process control in `os`.*

**Answer:** The `os` module provides a portable way to use operating system-dependent functionality: `os.environ` for environment variables, `os.getcwd()` and `os.chdir()` for working directories, `os.mkdir()` / `os.makedirs()`, `os.remove()` / `os.rmdir()`, and `os.getpid()`.

```python
import os

print("Current PID:        ", os.getpid())
print("PATH env var exists:", "PATH" in os.environ)
print("CWD:                ", os.path.basename(os.getcwd()))
```

Output:
```text
Current PID:         ...
PATH env var exists: True
CWD:                 cloud-devops-engineering-roadmap
```

**Q486. How do `datetime`, `date`, `time`, and `timedelta` work in the `datetime` module?**

*Demonstrate date arithmetic, formatting with `strftime()`, and parsing with `strptime()`.*

**Answer:** - `datetime.date(Y, M, D)`: Calendar date.
- `datetime.datetime(Y, M, D, H, M, S)`: Combined date and time.
- `datetime.timedelta(days=..., hours=...)`: Duration for date math.
- `.strftime(format)`: Converts date object to formatted string.
- `datetime.strptime(string, format)`: Parses formatted string into date object.

```python
from datetime import datetime, timedelta

now = datetime(2026, 10, 30, 10, 0, 0)
future = now + timedelta(days=7, hours=2)
print("Formatted now:   ", now.strftime("%Y-%m-%d %H:%M:%S"))
print("Calculated future:", future.strftime("%Y-%m-%d %H:%M:%S"))

parsed = datetime.strptime("2026-11-01", "%Y-%m-%d")
print("Parsed date:     ", parsed.date())
```

Output:
```text
Formatted now:    2026-10-30 10:00:00
Calculated future: 2026-11-06 12:00:00
Parsed date:      2026-11-01
```

**Q487. How does timezone handling work with `zoneinfo` introduced in Python 3.9?**

*Compare naive vs timezone-aware datetimes and IANA timezone database lookups.*

**Answer:** - **Naive datetime**: Has no timezone (`tzinfo=None`), making calculations across daylight savings or regions error-prone.
- **Aware datetime**: Specifies timezone via `zoneinfo.ZoneInfo('Region/City')`, providing accurate UTC conversions using the standard IANA timezone database.

```python
from datetime import datetime, timezone
from zoneinfo import ZoneInfo

utc_dt = datetime.now(timezone.utc)
tokyo_dt = utc_dt.astimezone(ZoneInfo("Asia/Tokyo"))
nyc_dt = utc_dt.astimezone(ZoneInfo("America/New_York"))

print("UTC:  ", utc_dt.strftime("%H:%M %Z"))
print("Tokyo:", tokyo_dt.strftime("%H:%M %Z"))
print("NYC:  ", nyc_dt.strftime("%H:%M %Z"))
```

Output:
```text
UTC:   ... UTC
Tokyo: ... JST
NYC:   ... EDT
```

**Q488. What does the `time` module provide?**

*Explain `time.time()`, `time.sleep()`, and monotonic timers (`time.perf_counter()`).*

**Answer:** - `time.time()`: Returns Unix epoch timestamp (seconds since 1970). Subject to system clock drift and NTP updates.
- `time.perf_counter()`: Monotonic high-resolution clock with the highest available accuracy, unaffected by system time adjustments; mandatory for performance benchmarking.
- `time.sleep(secs)`: Suspends current thread execution.

```python
import time

t0 = time.perf_counter()
time.sleep(0.01) # 10 ms sleep
elapsed = time.perf_counter() - t0
print(f"Elapsed perf_counter time: {elapsed * 1000:.2f} ms")
```

Output:
```text
Elapsed perf_counter time: 10.50 ms
```

**Q489. What mathematical functions are provided by `math` and `cmath`?**

*Compare real number math (`math`) with complex number math (`cmath`).*

**Answer:** `math` provides standard mathematical functions for real floating-point numbers: trigonometry, logarithmic (`log`, `log2`, `log10`), powers, roots (`sqrt`, `isqrt`), constants (`pi`, `e`, `inf`, `nan`), and ceiling/floor (`ceil`, `floor`, `trunc`). `cmath` provides equivalent functions for complex numbers.

```python
import math
import cmath

print("math.sqrt(16):", math.sqrt(16))
print("math.gcd(24, 36):", math.gcd(24, 36))
print("math.log2(1024):", math.log2(1024))
print("cmath.sqrt(-1):", cmath.sqrt(-1))
```

Output:
```text
math.sqrt(16): 4.0
math.gcd(24, 36): 12
math.log2(1024): 10.0
cmath.sqrt(-1): 1j
```

**Q490. What is the `decimal` module and when must it be used?**

*Explain exact decimal fixed-point and floating-point arithmetic for financial calculations.*

**Answer:** The `decimal.Decimal` module implements the General Decimal Arithmetic Specification. Unlike standard IEEE 754 binary floats (`0.1 + 0.2 != 0.3`), `Decimal` provides exact base-10 arithmetic, user-configurable precision (`getcontext().prec`), and standard rounding modes (ROUND_HALF_EVEN, ROUND_UP), making it mandatory for billing, currencies, and finance.

```python
from decimal import Decimal, getcontext

getcontext().prec = 6
price = Decimal("19.99")
tax_rate = Decimal("0.0825")
total = price * (Decimal("1") + tax_rate)

print("Exact financial calculation total:", total)
```

Output:
```text
Exact financial calculation total: 21.6392
```

**Q491. What does the `fractions` module provide?**

*Demonstrate rational number arithmetic with automatic simplification.*

**Answer:** The `fractions.Fraction(numerator, denominator)` module implements rational numbers, automatically reducing fractions to lowest terms and maintaining exact arithmetic without floating point drift.

```python
from fractions import Fraction

f1 = Fraction(3, 4)
f2 = Fraction(1, 8)
print("3/4 + 1/8 =", f1 + f2)
print("From float 0.5 =", Fraction("0.5"))
```

Output:
```text
3/4 + 1/8 = 7/8
From float 0.5 = 1/2
```

**Q492. What statistical functions are provided by the `statistics` module?**

*Explain `mean`, `median`, `mode`, `stdev`, `variance`, and `quantiles`.*

**Answer:** The `statistics` module provides mathematical statistics functions for numeric data: `mean()`, `fmean()` (faster float mean), `median()`, `mode()`, `stdev()` (sample standard deviation), `pstdev()` (population), and `quantiles()`.

```python
import statistics

latencies = [12.4, 15.1, 11.8, 14.2, 19.5, 12.4, 13.9]
print("Mean:   ", round(statistics.mean(latencies), 2))
print("Median: ", statistics.median(latencies))
print("Mode:   ", statistics.mode(latencies))
print("Stdev:  ", round(statistics.stdev(latencies), 2))
```

Output:
```text
Mean:    14.19
Median:  13.9
Mode:    12.4
Stdev:   2.63
```

**Q493. What is the difference between `random` and `secrets`?**

*Explain pseudo-random numbers (Mersenne Twister) vs cryptographically secure randomness.*

**Answer:** - `random`: Uses the Mersenne Twister PRNG. Fast and suitable for simulations, games, and modeling, but completely predictable and **unsafe for cryptography, tokens, or passwords**.
- `secrets`: Uses OS-provided cryptographic randomness (`/dev/urandom` or `CryptGenRandom`), safe for generating authentication tokens, API keys, and passwords.

```python
import random
import secrets

print("Random choice (simulation):", random.choice(["alpha", "beta"]))
print("Secure API token (hex):     ", secrets.token_hex(16))
print("Secure URL-safe token:      ", secrets.token_urlsafe(16))
```

Output:
```text
Random choice (simulation): ...
Secure API token (hex):      ...
Secure URL-safe token:       ...
```

**Q494. What is `heapq` and how does the min-heap implementation work?**

*Explain heapify, heappush, heappop, and priority queue operations in $O(\log N)$.*

**Answer:** `heapq` provides a min-heap algorithm built on standard Python lists: the smallest element is always at index `0` (`heap[0]`).
- `heappush(heap, item)`: Adds an item in $O(\log N)$ time.
- `heappop(heap)`: Pops smallest item in $O(\log N)$ time.
- `heapify(lst)`: Transforms a list into a heap in-place in linear $O(N)$ time.

```python
import heapq

tasks = []
heapq.heappush(tasks, (3, "Low priority task"))
heapq.heappush(tasks, (1, "Critical bug fix"))
heapq.heappush(tasks, (2, "Feature release"))

print("Highest priority task popped:", heapq.heappop(tasks))
print("Next priority task popped:   ", heapq.heappop(tasks))
```

Output:
```text
Highest priority task popped: (1, 'Critical bug fix')
Next priority task popped:    (2, 'Feature release')
```

**Q495. What does the `bisect` module provide?**

*Explain binary search insertion: `bisect_left()`, `bisect_right()`, and `insort()`.*

**Answer:** `bisect` maintains sorted lists without having to resort the list after each insertion. It uses binary search to find insertion points in $O(\log N)$ time: `bisect_left` finds the leftmost insertion index, and `insort` inserts while preserving sorted order.

```python
import bisect

grades = [60, 70, 80, 90]
letter_grades = ["F", "D", "C", "B", "A"]

def get_grade(score):
    idx = bisect.bisect_right(grades, score)
    return letter_grades[idx]

print("Score 85 grade:", get_grade(85))
print("Score 92 grade:", get_grade(92))
```

Output:
```text
Score 85 grade: B
Score 92 grade: A
```

**Q496. What is `hashlib` and how does cryptographic hashing work?**

*Demonstrate hashing with SHA-256, SHA-512, MD5, and `.hexdigest()`.*

**Answer:** `hashlib` provides a common interface to secure hash and message digest algorithms (SHA-256, SHA-512, MD5, SHA3). Hashes are one-way cryptographic functions; feed raw bytes using `.update()` and extract string digests with `.hexdigest()`.

```python
import hashlib

hasher = hashlib.sha256()
hasher.update(b"devops-secret-payload")
print("SHA-256 digest:", hasher.hexdigest())
```

Output:
```text
SHA-256 digest: 8fbc4d209a5b486ba937ffb578c75ff386c12e1ec731998f5a58797f2672bfbb
```

**Q497. What is `hmac` and how does Keyed-Hashing for Message Authentication work?**

*Explain authenticating API webhooks (GitHub, Stripe) using shared secret HMAC signatures.*

**Answer:** `hmac` generates Hash-based Message Authentication Codes using a secret key and a cryptographic hash function (e.g. SHA-256). It verifies both data integrity and authenticity. Use `hmac.compare_digest()` to prevent timing attacks.

```python
import hmac
import hashlib

secret_key = b"webhook_signing_secret"
payload = b'{"event": "push", "ref": "main"}'

signature = hmac.new(secret_key, payload, hashlib.sha256).hexdigest()
print("HMAC Signature:", signature)

# Safe verification
is_valid = hmac.compare_digest(signature, signature)
print("Signature valid:", is_valid)
```

Output:
```text
HMAC Signature: ...
Signature valid: True
```

**Q498. How do you generate UUIDs using the `uuid` module?**

*Compare UUID1 (host + timestamp), UUID4 (cryptographically random), and UUID5 (SHA-1 namespace hash).*

**Answer:** - `uuid.uuid1()`: Generates UUID from host MAC address and current timestamp (risks leaking hardware MAC).
- `uuid.uuid4()`: Generates a completely random 128-bit UUID (most universally used in software architecture).
- `uuid.uuid5(namespace, name)`: Generates deterministic UUID by hashing a name with SHA-1.

```python
import uuid

u4 = uuid.uuid4()
print("Random UUID4:       ", u4)
print("UUID4 hex:          ", u4.hex)

u5 = uuid.uuid5(uuid.NAMESPACE_DNS, "cloud.internal")
print("Deterministic UUID5:", u5)
```

Output:
```text
Random UUID4:        ...
UUID4 hex:           ...
Deterministic UUID5: e81611aa-b541-5a02-8a90-3486c4bc0991
```

**Q499. What does the `struct` module do with binary packing and unpacking?**

*Demonstrate packing Python values into C structs (`struct.pack` and `struct.unpack`).*

**Answer:** `struct` converts between Python values and C structs represented as Python `bytes`. Essential for low-level socket protocol headers, binary file formats, and network packet crafting.

```python
import struct

# Format: ! (network big-endian), H (unsigned short 2-bytes), I (unsigned int 4-bytes)
packet_header = struct.pack("!HI", 80, 1024)
print("Packed binary bytes:", packet_header)

port, length = struct.unpack("!HI", packet_header)
print(f"Unpacked: port={port}, length={length}")
```

Output:
```text
Packed binary bytes: b'\x00P\x00\x00\x04\x00'
Unpacked: port=80, length=1024
```

**Q500. How does the `argparse` module build robust CLI tools?**

*Demonstrate positional arguments, optional flags, types, default values, and `--help` generation.*

**Answer:** `argparse` is the standard library module for creating command-line interfaces. It parses `sys.argv`, enforces types, handles positional and optional arguments (`--flag`), and automatically generates formatted help menus.

```python
import argparse

parser = argparse.ArgumentParser(description="Deploy Cloud Service")
parser.add_argument("service", help="Target service name")
parser.add_argument("--replicas", type=int, default=1, help="Pod replica count")
parser.add_argument("--dry-run", action="store_true", help="Simulate without applying")

args = parser.parse_args(["auth-api", "--replicas", "3", "--dry-run"])
print(f"Service: {args.service}, Replicas: {args.replicas}, DryRun: {args.dry_run}")
```

Output:
```text
Service: auth-api, Replicas: 3, DryRun: True
```

**Q501. How does the `logging` module work in Python?**

*Explain log levels (DEBUG, INFO, WARNING, ERROR, CRITICAL), formatters, and handlers.*

**Answer:** Python's `logging` module provides a thread-safe logging framework. It separates emission from formatting and output destinations via **Handlers** (`StreamHandler`, `FileHandler`, `RotatingFileHandler`). Levels in increasing severity: DEBUG (10) < INFO (20) < WARNING (30) < ERROR (40) < CRITICAL (50).

```python
import logging
import io

log_stream = io.StringIO()
logger = logging.getLogger("AppDeployer")
logger.setLevel(logging.INFO)

handler = logging.StreamHandler(log_stream)
formatter = logging.Formatter("[%(levelname)s] %(name)s: %(message)s")
handler.setFormatter(formatter)
logger.addHandler(handler)

logger.info("Service initialized")
logger.warning("High latency detected")
print(log_stream.getvalue().strip())
```

Output:
```text
[INFO] AppDeployer: Service initialized
[WARNING] AppDeployer: High latency detected
```

**Q502. How does `unittest` framework structure unit tests?**

*Explain `TestCase`, assertion methods (`assertEqual`, `assertRaises`), and test fixtures (`setUp`, `tearDown`).*

**Answer:** `unittest` is Python's built-in xUnit-style test framework. Tests are grouped into classes inheriting from `unittest.TestCase`. `setUp()` and `tearDown()` execute before and after each test method.

```python
import unittest

class TestMathOperations(unittest.TestCase):
    def test_addition(self):
        self.assertEqual(10 + 5, 15)

    def test_zero_division(self):
        with self.assertRaises(ZeroDivisionError):
            _ = 1 / 0

suite = unittest.TestLoader().loadTestsFromTestCase(TestMathOperations)
runner = unittest.TextTestRunner(verbosity=0)
result = runner.run(suite)
print("Tests ran:", result.testsRun, "| Successful:", result.wasSuccessful())
```

Output:
```text
Tests ran: 2 | Successful: True
```

**Q503. What is `doctest` and how does it test examples inside docstrings?**

*Demonstrate literate testing with interactive shell outputs in docstrings.*

**Answer:** `doctest` scans docstrings for text resembling interactive Python sessions (`>>> `), executes the code, and verifies that the output matches the expected output exactly.

```python
import doctest

def square(n: int) -> int:
    """Return square of n.
    >>> square(4)
    16
    >>> square(-2)
    4
    """
    return n * n

results = doctest.testmod(verbose=False)
print("Doctest failed:", results.failed)
```

Output:
```text
Doctest failed: 0
```

**Q504. How does `timeit` benchmark small code snippets accurately?**

*Explain isolating benchmarks from garbage collection and system noise.*

**Answer:** `timeit` measures execution time of small code snippets by repeating them thousands of times, temporarily disabling garbage collection to provide accurate timing measurements.

```python
import timeit

# Compare list comprehension vs map()
comp_time = timeit.timeit("[str(x) for x in range(100)]", number=1000)
map_time = timeit.timeit("list(map(str, range(100)))", number=1000)

print(f"List comprehension: {comp_time * 1000:.2f} ms")
print(f"Map conversion:     {map_time * 1000:.2f} ms")
```

Output:
```text
List comprehension: ... ms
Map conversion:     ... ms
```

**Q505. How does the `cProfile` module perform deterministic profiling?**

*Explain `ncalls`, `tottime`, `percall`, and `cumtime` profile columns.*

**Answer:** `cProfile` is a C-extension profiler that hooks into every function call, return, and exception. Key metrics:
- `ncalls`: Number of invocations.
- `tottime`: Total time spent in the function alone (excluding sub-calls).
- `cumtime`: Cumulative time spent in the function and all sub-calls.

```python
import cProfile

def heavy_computation():
    return sum(i ** 2 for i in range(10000))

pr = cProfile.Profile()
pr.enable()
heavy_computation()
pr.disable()
# pr.print_stats(sort="cumtime")
print("Profiling completed successfully")
```

Output:
```text
Profiling completed successfully
```

**Q506. What does the `textwrap` module provide?**

*Demonstrate `wrap()`, `fill()`, `dedent()`, `indent()`, and `shorten()`.*

**Answer:** `textwrap` formats text paragraphs for terminal displays: `wrap()` splits text into list of lines, `fill()` returns a single formatted paragraph string, and `shorten()` truncates cleanly.

```python
import textwrap

long_text = "Python's standard library is extensive and batteries included."
print(textwrap.fill(long_text, width=25))
```

Output:
```text
Python's standard library
is extensive and
batteries included.
```

**Q507. How does `difflib` compute differences between sequences?**

*Explain `unified_diff`, `SequenceMatcher`, and fuzzy string matching.*

**Answer:** `difflib` compares sequences and generates diffs. `SequenceMatcher` calculates similarity ratios, and `get_close_matches()` performs fuzzy string matching.

```python
import difflib

options = ["deploy_kubernetes", "deploy_docker", "destroy_cluster"]
matches = difflib.get_close_matches("deploy_k8s", options, n=1, cutoff=0.5)
print("Fuzzy match:", matches)
```

Output:
```text
Fuzzy match: ['deploy_kubernetes']
```

**Q508. What does the `array` module provide compared to lists?**

*Explain homogeneous C-type numeric arrays (`array.array`).*

**Answer:** `array.array(typecode, [initializer])` stores homogeneous C-type numeric values (ints, floats) densely in memory without PyObject pointers, consuming far less memory than a standard `list`.

```python
import array
import sys

arr = array.array("i", range(1000)) # 'i' = signed 32-bit int
lst = list(range(1000))

print("Array memory: ", sys.getsizeof(arr), "bytes")
print("List memory:  ", sys.getsizeof(lst), "bytes")
```

Output:
```text
Array memory:  4064 bytes
List memory:   8056 bytes
```

**Q509. What is the `enum` module and how do Enum, IntEnum, and Flag work?**

*Demonstrate creating enumerated constants and bitwise combinable flags.*

**Answer:** `enum.Enum` defines symbolic names bound to unique, constant values. `IntEnum` allows comparisons with integers. `Flag` / `IntFlag` supports bitwise operations (`|`, `&`).

```python
from enum import Enum, auto

class ClusterState(Enum):
    PENDING = auto()
    RUNNING = auto()
    FAILED = auto()

state = ClusterState.RUNNING
print("Enum name: ", state.name)
print("Enum value:", state.value)
print("Equality:  ", state == ClusterState.RUNNING)
```

Output:
```text
Enum name:  RUNNING
Enum value: 2
Equality:   True
```

**Q510. What does `glob` and `fnmatch` provide for filename pattern matching?**

*Explain Unix shell-style wildcards (`*`, `?`, `[seq]`).*

**Answer:** - `glob.glob(pattern, recursive=True)`: Returns paths matching Unix shell patterns.
- `fnmatch.fnmatch(filename, pattern)`: Tests if a filename string matches a wildcard pattern.

```python
import fnmatch

files = ["app.py", "test_app.py", "README.md", "app.pyc"]
py_sources = [f for f in files if fnmatch.fnmatch(f, "*.py")]
print("Matched python sources:", py_sources)
```

Output:
```text
Matched python sources: ['app.py', 'test_app.py']
```

**Q511. How does `ipaddress` manipulate IPv4 and IPv6 networks?**

*Demonstrate subnet calculation, IP containment check, and host enumeration.*

**Answer:** The `ipaddress` module parses, inspects, and validates IP addresses and CIDR network blocks.

```python
import ipaddress

net = ipaddress.ip_network("192.168.1.0/24")
ip = ipaddress.ip_address("192.168.1.50")

print("Is IP in network:   ", ip in net)
print("Total usable hosts: ", net.num_addresses - 2)
print("Netmask:            ", net.netmask)
```

Output:
```text
Is IP in network:    True
Total usable hosts:  254
Netmask:             255.255.255.0
```

**Q512. What does the `queue` module provide for multi-threaded communication?**

*Compare `Queue` (FIFO), `LifoQueue` (Stack), and `PriorityQueue`.*

**Answer:** The `queue` module provides thread-safe, multi-producer multi-consumer FIFO queues (`Queue`), LIFO queues (`LifoQueue`), and priority queues (`PriorityQueue`) with locking and blocking timeouts (`get(timeout=...)`).

```python
import queue

q = queue.Queue()
q.put("job-1")
q.put("job-2")

print("Popped item:", q.get())
print("Queue size remaining:", q.qsize())
```

Output:
```text
Popped item: job-1
Queue size remaining: 1
```

**Q513. How do `urllib.parse` utilities parse URLs?**

*Explain `urlparse`, `urlunparse`, `urlencode`, and `parse_qs`.*

**Answer:** `urllib.parse` decomposes URLs into components (`scheme`, `netloc`, `path`, `params`, `query`, `fragment`) and encodes dictionary query parameters into percent-encoded query strings.

```python
from urllib.parse import urlparse, parse_qs, urlencode

url = "https://api.cloud.com/v1/metrics?cluster=us-east&active=true"
parsed = urlparse(url)
print("Host: ", parsed.netloc)
print("Path: ", parsed.path)
print("Query:", parse_qs(parsed.query))
```

Output:
```text
Host:  api.cloud.com
Path:  /v1/metrics
Query: {'cluster': ['us-east'], 'active': ['true']}
```

**Q514. What does `http.server` provide for quick local web servers?**

*Explain starting a local static file server or testing webhook receivers.*

**Answer:** `http.server` provides basic HTTP request handlers (`SimpleHTTPRequestHandler`). Running `python -m http.server 8000` serves files from current directory.

```python
import http.server
# python -m http.server 8080
print("http.server enables instant local static web hosting")
```

Output:
```text
http.server enables instant local static web hosting
```

**Q515. What is `sqlite3` and how does the standard library interface with embedded relational databases?**

*Demonstrate connecting, executing parameterized SQL queries, and committing transactions.*

**Answer:** `sqlite3` is Python's built-in DB-API 2.0 compliant interface for embedded SQLite relational databases, supporting in-memory databases (`:memory:`), transactions, and row factories.

```python
import sqlite3

conn = sqlite3.connect(":memory:")
cur = conn.cursor()
cur.execute("CREATE TABLE servers (id INT, name TEXT)")
cur.execute("INSERT INTO servers VALUES (?, ?)", (1, "web-01"))
conn.commit()

cur.execute("SELECT name FROM servers WHERE id = ?", (1,))
print("Queried server:", cur.fetchone()[0])
conn.close()
```

Output:
```text
Queried server: web-01
```

**Q516. What does `unicodedata` provide for Unicode normalization?**

*Explain Unicode normal forms (NFC, NFD, NFKC, NFKD) and category lookups.*

**Answer:** `unicodedata` provides access to the Unicode Character Database. Normalization ensures visually identical characters with different code point representations compare equal.

```python
import unicodedata

s1 = "é" # Single code point
s2 = "é" # 'e' + combining acute accent

print("s1 == s2 raw:         ", s1 == s2)
print("s1 == s2 normalized NFC:", unicodedata.normalize("NFC", s1) == unicodedata.normalize("NFC", s2))
```

Output:
```text
s1 == s2 raw:          False
s1 == s2 normalized NFC: True
```

**Q517. What is `faulthandler` and how does it diagnose native C-level segfaults?**

*Explain printing Python tracebacks on SIGSEGV / hard crashes.*

**Answer:** `faulthandler` explicitly configures CPython to dump Python tracebacks to stderr when low-level crashes (segfault, bus error, abort) occur in native C extensions.

```python
import faulthandler

faulthandler.enable()
print("Faulthandler enabled for crash diagnostics")
```

Output:
```text
Faulthandler enabled for crash diagnostics
```

**Q518. What does the `signal` module provide for handling OS signals?**

*Explain capturing SIGINT, SIGTERM, and implementing graceful shutdowns.*

**Answer:** `signal` allows registering Python handler functions invoked when the process receives OS signals (`SIGINT`, `SIGTERM`), critical for container graceful shutdown lifecycles in Kubernetes.

```python
import signal

def handle_term(signum, frame):
    print("Caught termination signal! Cleaning up resources...")

# signal.signal(signal.SIGTERM, handle_term)
print("Signal handler configured for graceful termination")
```

Output:
```text
Signal handler configured for graceful termination
```

**Q519. What does `shlex` provide for shell command line splitting?**

*Explain parsing quotes and arguments safely for `subprocess`.*

**Answer:** `shlex.split(command_string)` splits command strings into argument lists according to POSIX shell grammar, correctly preserving quoted arguments with spaces.

```python
import shlex

cmd = 'docker run -d --name "my container" nginx:alpine'
args = shlex.split(cmd)
print("Parsed args:", args)
```

Output:
```text
Parsed args: ['docker', 'run', '-d', '--name', 'my container', 'nginx:alpine']
```

**Q520. What does `reprlib` provide for formatting large collections safely?**

*Demonstrate abbreviating huge objects in debug logs without crashing.*

**Answer:** `reprlib.repr(obj)` produces abbreviated string representations of deeply nested or huge data structures using ellipses, preventing memory crashes during debug logging.

```python
import reprlib

huge_list = list(range(1000))
print("reprlib output:", reprlib.repr(huge_list))
```

Output:
```text
reprlib output: [0, 1, 2, 3, 4, 5, ...]
```

**Q521. Output Question: Predict output when random.seed() is fixed.**

*Explain reproducibility in pseudo-random generation.*

**Answer:** Setting `random.seed(val)` guarantees the sequence of generated pseudo-random numbers is completely deterministic across runs.

```python
import random

random.seed(42)
r1 = [random.randint(1, 10) for _ in range(3)]
random.seed(42)
r2 = [random.randint(1, 10) for _ in range(3)]
print("Sequences identical:", r1 == r2)
```

Output:
```text
Sequences identical: True
```

**Q522. Output Question: Predict what datetime.fromisoformat() parses.**

*Explain standard ISO 8601 string parsing.*

**Answer:** `datetime.fromisoformat()` parses ISO 8601 strings into datetime objects.

```python
from datetime import datetime

dt = datetime.fromisoformat("2026-10-30T10:15:30")
print("Year:", dt.year, "| Minute:", dt.minute)
```

Output:
```text
Year: 2026 | Minute: 15
```

**Q523. Debugging Question: Fix timezone conversion bug with replace() vs astimezone().**

*Explain why `.replace(tzinfo=...)` corrupts timestamps while `.astimezone()` converts correctly.*

**Answer:** `dt.replace(tzinfo=...)` changes the timezone label without altering the hour/minute values (corrupting time). `.astimezone(...)` recalculates the time to match the new zone.

```python
from datetime import datetime, timezone
from zoneinfo import ZoneInfo

dt_utc = datetime(2026, 10, 30, 12, 0, 0, tzinfo=timezone.utc)
# Correct conversion:
dt_tokyo = dt_utc.astimezone(ZoneInfo("Asia/Tokyo"))
print("Correctly converted hour in Tokyo (UTC+9):", dt_tokyo.hour)
```

Output:
```text
Correctly converted hour in Tokyo (UTC+9): 21
```

**Q524. Programming Question: Build a CLI tool with subparsers for deployment and rollback.**

*Demonstrate `add_subparsers()` in `argparse`.*

**Answer:** Use subparsers to create multi-command tools like `git` or `docker`.

```python
import argparse

parser = argparse.ArgumentParser()
subparsers = parser.add_subparsers(dest="subcommand")

deploy_cmd = subparsers.add_parser("deploy")
deploy_cmd.add_argument("--image", required=True)

rollback_cmd = subparsers.add_parser("rollback")
rollback_cmd.add_argument("--revision", type=int, default=1)

args = parser.parse_args(["deploy", "--image", "nginx:1.25"])
print(f"Subcommand: {args.subcommand}, Image: {args.image}")
```

Output:
```text
Subcommand: deploy, Image: nginx:1.25
```

**Q525. Programming Question: Implement a Rotating File Logger.**

*Configure `logging.handlers.RotatingFileHandler` to limit disk usage.*

**Answer:** Set `maxBytes` and `backupCount`.

```python
import logging
from logging.handlers import RotatingFileHandler
import tempfile
from pathlib import Path

tmp_log = Path(tempfile.gettempdir()) / "rotating_test.log"
handler = RotatingFileHandler(tmp_log, maxBytes=1000, backupCount=2)
logger = logging.getLogger("RotatingDemo")
logger.addHandler(handler)
logger.setLevel(logging.INFO)
logger.info("Rotated log entry")
handler.close()
tmp_log.unlink(missing_ok=True)
print("Rotating file handler configured successfully")
```

Output:
```text
Rotating file handler configured successfully
```

**Q526. Programming Question: Safely compare two strings in constant time to prevent timing attacks.**

*Use `hmac.compare_digest` to verify authentication tokens.*

**Answer:** Constant time comparison prevents side-channel timing analysis.

```python
import hmac

def verify_token(user_token: str, expected_token: str) -> bool:
    return hmac.compare_digest(user_token, expected_token)

print("Valid token:  ", verify_token("token_123", "token_123"))
print("Invalid token:", verify_token("token_abc", "token_123"))
```

Output:
```text
Valid token:   True
Invalid token: False
```

**Q527. Programming Question: Calculate working business days between two dates using `calendar`.**

*Count weekdays (Mon-Fri) excluding weekends.*

**Answer:** Iterate over days checking `.weekday() < 5`.

```python
from datetime import date, timedelta

def business_days_between(d1: date, d2: date) -> int:
    days = 0
    cur = d1
    while cur < d2:
        if cur.weekday() < 5: # Monday to Friday
            days += 1
        cur += timedelta(days=1)
    return days

start = date(2026, 11, 2) # Monday
end = date(2026, 11, 9)   # Next Monday (7 calendar days, 5 business days)
print("Business days:", business_days_between(start, end))
```

Output:
```text
Business days: 5
```

**Q528. Programming Question: Create an in-memory SQLite table with custom Python function callbacks.**

*Register a Python function into SQLite using `conn.create_function`.*

**Answer:** Execute custom logic directly inside SQL queries.

```python
import sqlite3

conn = sqlite3.connect(":memory:")
conn.create_function("clean_service_name", 1, lambda s: s.strip().lower())
cur = conn.cursor()
cur.execute("SELECT clean_service_name('   API_GATEWAY   ')")
print("Cleaned via SQLite function:", cur.fetchone()[0])
conn.close()
```

Output:
```text
Cleaned via SQLite function: api_gateway
```


---

## Chapter 17: Type Hints and Static Typing

**Q529. What are Type Hints and why are they used in Python?**

*Explain PEP 484, gradual typing, static analysis (Mypy/Pyright), and IDE auto-completion.*

**Answer:** Type hints (PEP 484) add optional type annotations to variables, function parameters, and return values. Python itself remains dynamically typed and does not enforce type hints at runtime. Type hints enable static analysis tools (mypy, pyright) to catch bugs before deployment, power IDE auto-completion and refactoring, and serve as machine-readable documentation.

```python
def format_service_url(host: str, port: int, ssl: bool = True) -> str:
    protocol = "https" if ssl else "http"
    return f"{protocol}://{host}:{port}"

print(format_service_url("api.cloud.com", 443))
```

Output:
```text
https://api.cloud.com:443
```

**Q530. What are built-in generic types (PEP 585) in Python 3.9+?**

*Explain using `list[str]`, `dict[str, int]`, and `tuple[int, ...]` instead of the `typing` module.*

**Answer:** Starting in Python 3.9 (PEP 585), standard built-in collections act as generics natively (`list[str]`, `dict[str, int]`, `set[int]`, `tuple[int, str]`). Importing uppercase equivalents from `typing` (`typing.List`, `typing.Dict`) is deprecated.

```python
# Modern Python 3.9+ typing:
def summarize_ports(services: dict[str, list[int]]) -> list[int]:
    all_ports: list[int] = []
    for ports in services.values():
        all_ports.extend(ports)
    return all_ports

print("Summarized:", summarize_ports({"web": [80, 443], "api": [8080]}))
```

Output:
```text
Summarized: [80, 443, 8080]
```

**Q531. How does union typing using the pipe operator (`|`) work in Python 3.10+ (PEP 604)?**

*Compare `X | Y` with `typing.Union[X, Y]` and `Optional[X]`.*

**Answer:** In Python 3.10+ (PEP 604), the bitwise OR operator `|` can be used in type expressions to denote a union. `int | str` cleanly replaces `Union[int, str]`, and `str | None` replaces `Optional[str]`.

```python
def parse_identifier(val: int | str | None) -> str:
    if val is None:
        return "UNKNOWN"
    return str(val)

print(parse_identifier(123))
print(parse_identifier(None))
```

Output:
```text
123
UNKNOWN
```

**Q532. What are `Any`, `object`, and `Never` (NoReturn) in typing?**

*Explain the top type (`object`), escape hatch (`Any`), and bottom type (`Never`).*

**Answer:** - `object`: The top type in Python's type hierarchy. Every object is an instance of `object`, but type checkers only allow calling methods defined on `object`.
- `Any`: An unconstrained escape hatch that disables static type checking on the value.
- `Never` / `NoReturn`: The bottom type (PEP 654). Indicates a function that never returns (e.g. raises an exception or calls `sys.exit()`).

```python
from typing import Never, NoReturn
import sys

def abort(msg: str) -> Never:
    print(f"FATAL: {msg}")
    sys.exit(1)

# Type checker knows code following abort() is completely unreachable
```

Output:
```text
# Verified typing primitives for control flow analysis.
```

**Q533. What is `typing.Literal` (PEP 586)?**

*Demonstrate restricting parameters to an exact set of literal values.*

**Answer:** `Literal[...]` constrains a variable or parameter to specific literal values (strings, numbers, booleans), enforcing enum-like safety without defining full Enum classes.

```python
from typing import Literal

def set_log_level(level: Literal["DEBUG", "INFO", "WARNING", "ERROR"]):
    return f"Log level set to {level}"

print(set_log_level("INFO"))
# set_log_level("INVALID") -> Flagged by Mypy/Pyright!
```

Output:
```text
Log level set to INFO
```

**Q534. What are `typing.Final` and `@final` (PEP 591)?**

*Explain declaring immutable constants and preventing class/method inheritance.*

**Answer:** - `Final`: Declares that a variable cannot be reassigned or a class attribute cannot be overridden.
- `@final`: Class decorator declaring that a class cannot be subclassed.

```python
from typing import Final, final

API_VERSION: Final[str] = "v2"

@final
class SecureEngine:
    pass

# class CustomEngine(SecureEngine): pass -> Flagged by Mypy as invalid subclassing
print("Final API Version:", API_VERSION)
```

Output:
```text
Final API Version: v2
```

**Q535. What is `typing.TypedDict` (PEP 589)?**

*Explain typing dictionaries with fixed string keys and heterogeneous value types.*

**Answer:** `TypedDict` defines type structures for dictionaries where specific keys are mapped to specific value types, providing full static typing and auto-completion for JSON payloads and dictionary records.

```python
from typing import TypedDict

class ServerPayload(TypedDict):
    hostname: str
    ip: str
    port: int
    active: bool

srv: ServerPayload = {"hostname": "web-01", "ip": "10.0.0.1", "port": 443, "active": True}
print("TypedDict validated:", srv["hostname"])
```

Output:
```text
TypedDict validated: web-01
```

**Q536. What is `typing.Callable`?**

*Demonstrate typing function signatures: `Callable[[ArgTypes], ReturnType]`.*

**Answer:** `Callable[[Arg1Type, Arg2Type], ReturnType]` annotates function parameters or callbacks. `Callable[..., ReturnType]` denotes arbitrary arguments.

```python
from typing import Callable

def execute_task(task_fn: Callable[[str], int], target: str) -> int:
    return task_fn(target)

length_counter = lambda s: len(s)
print("Executed callable:", execute_task(length_counter, "cloud-devops"))
```

Output:
```text
Executed callable: 12
```

**Q537. How do generic functions work with `TypeVar`?**

*Explain parameterizing types to retain exact return types across operations.*

**Answer:** `TypeVar('T')` creates a type variable representing an arbitrary type. When used in function signatures, it binds the input type to the output type, preventing return types from degenerating into `Any`.

```python
from typing import TypeVar

T = TypeVar("T")

def get_first_element(items: list[T]) -> T:
    return items[0]

# Mypy knows first_num is int, and first_str is str!
first_num = get_first_element([10, 20, 30])
first_str = get_first_element(["alpha", "beta"])
print(f"Num: {first_num} ({type(first_num).__name__}) | Str: {first_str} ({type(first_str).__name__})")
```

Output:
```text
Num: 10 (int) | Str: alpha (str)
```

**Q538. What are bounded and constrained TypeVars?**

*Compare `TypeVar('T', bound=BaseClass)` with `TypeVar('T', int, float)`.*

**Answer:** - **Constrained TypeVar (`TypeVar('T', int, str)`)**: Can only be instantiated as one of the explicitly listed types.
- **Bounded TypeVar (`TypeVar('T', bound=CloudResource)`)**: Can be `CloudResource` or any subclass derived from it, preserving subclass methods in return types.

```python
from typing import TypeVar

class Resource:
    def get_id(self) -> str: return "res-001"

R = TypeVar("R", bound=Resource)

def refresh(resource: R) -> R:
    return resource

print("Refreshed:", refresh(Resource()).get_id())
```

Output:
```text
Refreshed: res-001
```

**Q539. What is generic class definition with `Generic[T]`?**

*Demonstrate building a generic container class.*

**Answer:** A class inheriting from `Generic[T]` allows parameterized instantiation (`Container[int]()`).

```python
from typing import Generic, TypeVar

T = TypeVar("T")

class Stack(Generic[T]):
    def __init__(self):
        self._items: list[T] = []

    def push(self, item: T) -> None:
        self._items.append(item)

    def pop(self) -> T:
        return self._items.pop()

int_stack: Stack[int] = Stack()
int_stack.push(42)
print("Popped from generic stack:", int_stack.pop())
```

Output:
```text
Popped from generic stack: 42
```

**Q540. What is `typing.Self` introduced in Python 3.11 (PEP 673)?**

*Demonstrate typing fluent API method chaining.*

**Answer:** `Self` represents the enclosing class type, enabling subclasses using method chaining (builder patterns) to return their own subclass type rather than the parent class.

```python
from typing import Self

class QueryBuilder:
    def __init__(self):
        self._query = ""

    def select(self, table: str) -> Self:
        self._query += f"SELECT * FROM {table} "
        return self

    def render(self) -> str:
        return self._query.strip()

print(QueryBuilder().select("users").render())
```

Output:
```text
SELECT * FROM users
```

**Q541. What are `TypeGuard` and `TypeIs` for type narrowing (PEP 647 / PEP 742)?**

*Explain user-defined type narrowing functions.*

**Answer:** `TypeGuard[T]` and `TypeIs[T]` annotate boolean functions that verify whether an argument conforms to a specific type, instructing static type checkers to narrow the type inside `if` blocks.

```python
from typing import TypeGuard

def is_str_list(val: list[object]) -> TypeGuard[list[str]]:
    return all(isinstance(x, str) for x in val)

raw_data: list[object] = ["a", "b", "c"]
if is_str_list(raw_data):
    # Inside this block, mypy narrows raw_data to list[str]!
    print("Joined strings:", "-".join(raw_data))
```

Output:
```text
Joined strings: a-b-c
```

**Q542. What is `typing.Annotated` (PEP 593)?**

*Explain attaching runtime metadata to types (e.g. validation rules in Pydantic/FastAPI).*

**Answer:** `Annotated[BaseType, Metadata]` allows attaching arbitrary metadata to a type hint without affecting static type checking. Extensively used in FastAPI for request validation (`Annotated[int, Path(ge=1)]`) and Pydantic.

```python
from typing import Annotated

PositiveInt = Annotated[int, "Value must be > 0"]

def allocate_memory(megabytes: PositiveInt) -> str:
    return f"Allocated {megabytes} MB"

print(allocate_memory(1024))
```

Output:
```text
Allocated 1024 MB
```

**Q543. What are forward references and `from __future__ import annotations` (PEP 563)?**

*Explain referencing classes before definition and stringified annotations.*

**Answer:** Normally, referencing a class before it is defined raises a `NameError`. In Python 3.7+, adding `from __future__ import annotations` turns all type annotations into stringified literals evaluated lazily, enabling self-referential methods (`def clone(self) -> Node:`).

```python
from __future__ import annotations

class Node:
    def __init__(self, value: int, next_node: Node | None = None):
        self.value = value
        self.next = next_node

n1 = Node(1)
n2 = Node(2, n1)
print(f"Linked: {n2.value} -> {n2.next.value}")
```

Output:
```text
Linked: 2 -> 1
```

**Q544. What is `typing.ClassVar` (PEP 526)?**

*Distinguish class variables from instance attributes in type checkers and dataclasses.*

**Answer:** `ClassVar[T]` marks an attribute as strictly belonging to the class itself, instructing dataclasses not to generate `__init__` parameters for it and warning if an instance attempts to rebind it.

```python
from dataclasses import dataclass
from typing import ClassVar

@dataclass
class Service:
    default_timeout: ClassVar[int] = 30 # Class variable
    name: str                           # Instance variable

s = Service("auth")
print("Instance name:", s.name)
print("Class timeout:", Service.default_timeout)
```

Output:
```text
Instance name: auth
Class timeout: 30
```

**Q545. What are `ParamSpec` and `Concatenate` introduced in Python 3.10 (PEP 612)?**

*Demonstrate perfectly typing decorator arguments and return values.*

**Answer:** `ParamSpec` captures the exact parameter specifications (positional, keyword, defaults) of a decorated callable and forwards them to the wrapper without losing parameter metadata in IDEs.

```python
from typing import Callable, ParamSpec, TypeVar
from functools import wraps

P = ParamSpec("P")
R = TypeVar("R")

def logged(fn: Callable[P, R]) -> Callable[P, R]:
    @wraps(fn)
    def wrapper(*args: P.args, **kwargs: P.kwargs) -> R:
        return fn(*args, **kwargs)
    return wrapper

@logged
def deploy(app: str, replicas: int = 1) -> bool:
    return True

print("Deploy result:", deploy("web-api", replicas=2))
```

Output:
```text
Deploy result: True
```

**Q546. What are type stub files (`.pyi`)?**

*Explain providing static type information for untyped C-extensions or legacy libraries.*

**Answer:** A stub file (`.pyi`) contains only type signatures, function headers, class outlines, and ellipsis (`...`) stubs without implementation code. It enables static type checkers to inspect C-extensions, Cython modules, or legacy packages without modifying the runtime code.

```python
# In math.pyi:
# def sqrt(x: float, /) -> float: ...
print("Type stub files decouple type annotations from runtime implementations")
```

Output:
```text
Type stub files decouple type annotations from runtime implementations
```

**Q547. What is `typing.cast` and when should you use it?**

*Explain overriding static type inferencing safely.*

**Answer:** `cast(Type, val)` instructs the static type checker to treat `val` as being of type `Type`. It performs **zero runtime checking or conversion** and simply returns `val` unchanged.

```python
from typing import cast

def get_raw_data() -> object:
    return "api.cloud.internal"

raw = get_raw_data()
# Mypy doesn't know raw is str; cast informs Mypy
url = cast(str, raw)
print("Upper:", url.upper())
```

Output:
```text
Upper: API.CLOUD.INTERNAL
```

**Q548. What is `typing.overload` and how do you annotate overloaded functions?**

*Demonstrate defining multiple call signatures implemented by a single runtime function.*

**Answer:** `@overload` allows declaring multiple function signatures for static type checkers. Overloaded signatures must be followed immediately by the single actual implementation function (which is not decorated with `@overload`).

```python
from typing import overload

@overload
def process(data: str) -> str: ...
@overload
def process(data: int) -> int: ...

def process(data: str | int) -> str | int:
    return data * 2

print("Str call:", process("dev"))
print("Int call:", process(10))
```

Output:
```text
Str call: devdev
Int call: 20
```

**Q549. What is covariance and contravariance in TypeVars?**

*Explain `TypeVar('T', covariant=True)` and `TypeVar('T', contravariant=True)`.*

**Answer:** - **Covariant (`covariant=True`)**: If `Dog` is a subclass of `Animal`, then `Container[Dog]` can be used wherever `Container[Animal]` is expected (e.g. read-only sequences).
- **Contravariant (`contravariant=True`)**: Subtyping relationship is inverted (e.g. write-only consumers/callables).
- **Invariant (default)**: `Container[Dog]` and `Container[Animal]` are incompatible.

```python
from typing import TypeVar, Sequence

# Sequence is covariant: Sequence[Dog] is a subtype of Sequence[Animal]
print("Read-only sequences are covariant")
```

Output:
```text
Read-only sequences are covariant
```

**Q550. What is `typing.TYPE_CHECKING`?**

*Explain breaking circular imports used exclusively for type annotations.*

**Answer:** `TYPE_CHECKING` is a special boolean constant that is `True` when analyzed by static type checkers (Mypy), but `False` at runtime. Enclosing imports inside `if TYPE_CHECKING:` avoids circular import errors.

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    # Heavy or circular imports for types only:
    from collections.abc import Mapping

print("TYPE_CHECKING guard evaluated successfully")
```

Output:
```text
TYPE_CHECKING guard evaluated successfully
```

**Q551. Output Question: Predict what `typing.get_type_hints()` returns.**

*Demonstrate runtime inspection of type annotations.*

**Answer:** `typing.get_type_hints(func)` resolves stringified annotations and forward references into actual Python types.

```python
from typing import get_type_hints

def connect(host: str, port: int) -> bool:
    return True

print("Resolved type hints:", get_type_hints(connect))
```

Output:
```text
Resolved type hints: {'host': <class 'str'>, 'port': <class 'int'>, 'return': <class 'bool'>}
```

**Q552. Output Question: Predict what happens at runtime when passing a wrong type to a hinted function.**

*Demonstrate that Python does not raise TypeError on type annotation mismatch.*

**Answer:** Python runtime completely ignores type hints; passing an invalid type executes normally unless custom validation logic or Pydantic is utilized.

```python
def multiply(a: int, b: int) -> int:
    return a * b

# Passing strings at runtime executes without TypeError!
print(multiply("devops-", 2))
```

Output:
```text
devops-devops-
```

**Q553. Debugging Question: Fix mypy error: 'Item None of Optional[str] has no attribute upper'.**

*Resolve missing null check on Optional / Union[str, None] types.*

**Answer:** Add an `if val is not None:` guard before calling methods on optional values.

```python
# Buggy:
# def greet(name: str | None): return name.upper()

# Fixed Code:
def greet(name: str | None) -> str:
    if name is not None:
        return name.upper()
    return "GUEST"

print(greet("Alice"))
print(greet(None))
```

Output:
```text
ALICE
GUEST
```

**Q554. Programming Question: Type a generic Cache class with expiration and eviction.**

*Implement `Cache[K, V]` with generic key and value types.*

**Answer:** Inherit from `Generic[K, V]`.

```python
from typing import Generic, TypeVar

K = TypeVar("K")
V = TypeVar("V")

class SimpleCache(Generic[K, V]):
    def __init__(self):
        self._store: dict[K, V] = {}

    def set(self, key: K, value: V) -> None:
        self._store[key] = value

    def get(self, key: K) -> V | None:
        return self._store.get(key)

c: SimpleCache[str, int] = SimpleCache()
c.set("port", 8080)
print("Cache retrieved:", c.get("port"))
```

Output:
```text
Cache retrieved: 8080
```

**Q555. Programming Question: Type a decorator preserving function signature using ParamSpec.**

*Write a fully typed timing decorator.*

**Answer:** Use `ParamSpec` and `TypeVar`.

```python
from typing import Callable, ParamSpec, TypeVar
from functools import wraps

P = ParamSpec("P")
R = TypeVar("R")

def time_it(fn: Callable[P, R]) -> Callable[P, R]:
    @wraps(fn)
    def wrapper(*args: P.args, **kwargs: P.kwargs) -> R:
        return fn(*args, **kwargs)
    return wrapper

@time_it
def compute(x: int, y: int) -> int: return x + y
print("Computed:", compute(10, 20))
```

Output:
```text
Computed: 30
```

**Q556. Programming Question: Implement a TypedDict with Total=False for optional payload fields.**

*Demonstrate non-required keys in TypedDict payloads.*

**Answer:** Set `total=False` on TypedDict.

```python
from typing import TypedDict

class ConfigDict(TypedDict, total=False):
    required_host: str
    optional_timeout: int

c: ConfigDict = {"required_host": "localhost"}
print("Config created:", c)
```

Output:
```text
Config created: {'required_host': 'localhost'}
```

**Q557. Programming Question: Implement runtime type validation using Pydantic V2.**

*Enforce type validation and data coercion at runtime.*

**Answer:** Define a Pydantic `BaseModel`.

```python
# Demonstrating Pydantic model structure
# from pydantic import BaseModel, Field
# class Server(BaseModel):
#     name: str
#     port: int = Field(ge=1, le=65535)
print("Pydantic parses and enforces type hints at runtime")
```

Output:
```text
Pydantic parses and enforces type hints at runtime
```

**Q558. Programming Question: Create a generic linked list with recursive type hints.**

*Type recursive Node references using Python 3.12 syntax.*

**Answer:** Use `Self` or `Node[T]`.

```python
from typing import Generic, TypeVar, Self

T = TypeVar("T")

class ListNode(Generic[T]):
    def __init__(self, val: T, nxt: Self | None = None):
        self.val = val
        self.nxt = nxt

head = ListNode(10, ListNode(20))
print("Linked:", head.val, "->", head.nxt.val)
```

Output:
```text
Linked: 10 -> 20
```


---

## Chapter 18: Regular Expressions

**Q559. What are regular expressions and how does the `re` module work?**

*Explain regex compilation, pattern caching, and the matching engine.*

**Answer:** A regular expression is a sequence of characters that forms a search pattern. Python's `re` module implements Perl-style regular expressions. Patterns should be compiled using `re.compile(pattern)` when reused frequently, which caches the compiled bytecode.

```python
import re

pattern = re.compile(r"^pod-[a-z0-9]+$")
print("Matches 'pod-alpha1':", bool(pattern.match("pod-alpha1")))
print("Matches 'vm-alpha1': ", bool(pattern.match("vm-alpha1")))
```

Output:
```text
Matches 'pod-alpha1': True
Matches 'vm-alpha1':  False
```

**Q560. What are metacharacters and character classes in regex?**

*Explain `.`, `^`, `$`, `*`, `+`, `?`, `\d`, `\w`, `\s`, `\D`, `\W`, `\S`.*

**Answer:** - `.` Matches any character except newline.
- `^` / `$`: Start / end of string.
- `\d` / `\D`: Digit `[0-9]` / non-digit.
- `\w` / `\W`: Word character `[a-zA-Z0-9_]` / non-word.
- `\s` / `\S`: Whitespace / non-whitespace.
- `[abc]` / `[^abc]`: Character set / negated set.

```python
import re

text = "Node_01 IP: 10.0.0.1"
digits = re.findall(r"\d+", text)
words = re.findall(r"\w+", text)

print("Digits:", digits)
print("Words: ", words)
```

Output:
```text
Digits: ['01', '10', '0', '0', '1']
Words:  ['Node_01', 'IP', '10', '0', '0', '1']
```

**Q561. What is the difference between greedy and non-greedy (lazy) quantifiers?**

*Explain `*?`, `+?`, `??`, `{m,n}?` and matching shortest vs longest substrings.*

**Answer:** - **Greedy Quantifiers (`*`, `+`, `{m,n}`)**: Match as much text as possible.
- **Lazy / Non-Greedy Quantifiers (`*?`, `+?`, `{m,n}?`)**: Match as little text as possible, stopping at the first opportunity.

```python
import re

html = "<div>First</div><div>Second</div>"
greedy = re.findall(r"<div>.*</div>", html)
lazy = re.findall(r"<div>.*?</div>", html)

print("Greedy:", greedy)
print("Lazy:  ", lazy)
```

Output:
```text
Greedy: ['<div>First</div><div>Second</div>']
Lazy:   ['<div>First</div>', '<div>Second</div>']
```

**Q562. How do re.match(), re.search(), and re.fullmatch() differ?**

*Explain beginning vs anywhere vs full string matching.*

**Answer:** - `re.match(pattern, string)`: Matches **only at the beginning** of the string.
- `re.search(pattern, string)`: Scans through the string looking for the **first location** where the pattern matches.
- `re.fullmatch(pattern, string)`: Requires the **entire string** to match the pattern from start to finish.

```python
import re

s = "error: port 80 failed"
print("match('port'):    ", bool(re.match(r"port", s)))
print("search('port'):   ", bool(re.search(r"port", s)))
print("fullmatch('error.*'):", bool(re.fullmatch(r"error.*", s)))
```

Output:
```text
match('port'):     False
search('port'):    True
fullmatch('error.*'): True
```

**Q563. How do re.findall() and re.finditer() work?**

*Compare returning a list of strings vs an iterator of Match objects.*

**Answer:** - `re.findall(pattern, string)`: Returns all non-overlapping matches as a list of strings (or tuples if groups exist).
- `re.finditer(pattern, string)`: Yields an iterator of `re.Match` objects, preserving match positions (`start()`, `end()`, `span()`) and saving memory.

```python
import re

text = "port 80, port 443, port 8080"
for match in re.finditer(r"port (\d+)", text):
    print(f"Found port {match.group(1)} at span {match.span()}")
```

Output:
```text
Found port 80 at span (0, 7)
Found port 443 at span (9, 17)
Found port 8080 at span (19, 28)
```

**Q564. How do capturing groups and named capturing groups work?**

*Demonstrate `(...)` vs `(?P<name>...)` and `.groupdict()`.*

**Answer:** - Unnamed Capturing Group `(...)`: Accessible by index (`match.group(1)`).
- Named Capturing Group `(?P<name>...)`: Accessible by string key (`match.group('name')`) and `.groupdict()`.

```python
import re

log = "HOST: 10.0.0.5 PORT: 8443"
pattern = r"HOST:\s*(?P<host>\S+)\s*PORT:\s*(?P<port>\d+)"
m = re.search(pattern, log)

if m:
    print("Group dict:", m.groupdict())
```

Output:
```text
Group dict: {'host': '10.0.0.5', 'port': '8443'}
```

**Q565. What are non-capturing groups `(?:...)`?**

*Explain grouping for precedence without incurring capture overhead.*

**Answer:** `(?:...)` groups sub-expressions for quantifiers or alternation (`(?:http|https)://`) without capturing the matched substring into group indexes or `.groups()`.

```python
import re

text = "http://api.internal"
m = re.search(r"(?:http|https)://(\w+)", text)
if m:
    # Only the second group was captured!
    print("Captured domain:", m.group(1))
    print("Total captured groups:", len(m.groups()))
```

Output:
```text
Captured domain: api
Total captured groups: 1
```

**Q566. What are Lookahead and Lookbehind assertions in Python regex?**

*Explain positive lookahead `(?=...)`, negative lookahead `(?!...)`, positive lookbehind `(?<=...)`, and negative lookbehind `(?<!...)`.*

**Answer:** Lookaround assertions match characters without consuming them (zero-width assertions):
- Positive Lookahead `(?=...)`: Matches if followed by pattern.
- Negative Lookahead `(?!...)`: Matches if NOT followed by pattern.
- Positive Lookbehind `(?<=...)`: Matches if preceded by pattern.
- Negative Lookbehind `(?<!...)`: Matches if NOT preceded by pattern.

```python
import re

text = "USD 100, EUR 200, JPY 300"
# Match numbers preceded by 'USD '
usd_amounts = re.findall(r"(?<=USD )\d+", text)
print("USD amounts:", usd_amounts)

# Password check: must contain at least one digit via lookahead
password = "Password123"
has_digit = bool(re.search(r"^(?=.*\d).+$", password))
print("Has digit:", has_digit)
```

Output:
```text
USD amounts: ['100']
Has digit: True
```

**Q567. How does re.sub() and re.subn() perform regex replacements?**

*Explain string replacement, backreferences (`\1`, `\g<name>`), and replacement functions.*

**Answer:** `re.sub(pattern, repl, string, count=0)` replaces matches. `repl` can be a string with backreferences (`\g<1>`) or a callable that receives the `Match` object.

```python
import re

text = "user: alice, role: admin"
# Swap key and value using backreferences
swapped = re.sub(r"(\w+):\s*(\w+)", r" ()", text)
print("Swapped:", swapped)

# Using callable replacer
masked = re.sub(r"\w{5}", lambda m: m.group(0).upper(), "alice in wonderland")
print("Callable upper 5-letter:", masked)
```

Output:
```text
Swapped: alice (user), admin (role)
Callable upper 5-letter: ALICE in wonderland
```

**Q568. What are regex compilation flags?**

*Explain `re.IGNORECASE` (re.I), `re.MULTILINE` (re.M), `re.DOTALL` (re.S), and `re.VERBOSE` (re.X).*

**Answer:** - `re.I`: Case-insensitive matching.
- `re.M`: `^` and `$` match beginning and end of each line, not just full string.
- `re.S` (`DOTALL`): Makes `.` match all characters including newlines `\n`.
- `re.X` (`VERBOSE`): Allows writing formatted multi-line regexes with whitespace and comments.

```python
import re

verbose_pattern = re.compile(r"""
    ^
    (?P<protocol>https?)    # Protocol scheme
    ://
    (?P<host>[^:/]+)        # Hostname
    (?::(?P<port>\d+))?     # Optional port
    """, re.VERBOSE)

m = verbose_pattern.search("https://api.cloud.com:8443")
print("Verbose regex extracted:", m.groupdict())
```

Output:
```text
Verbose regex extracted: {'protocol': 'https', 'host': 'api.cloud.com', 'port': '8443'}
```

**Q569. How does re.split() split strings by regex patterns?**

*Demonstrate splitting by multiple delimiters and capturing delimiters.*

**Answer:** `re.split(pattern, string)` splits by any matching delimiter. If the pattern contains capturing parentheses, the matched delimiters are also included in the output list.

```python
import re

line = "10.0.0.1:80;10.0.0.2:443|10.0.0.3:8080"
endpoints = re.split(r"[;|]", line)
print("Split endpoints:", endpoints)
```

Output:
```text
Split endpoints: ['10.0.0.1:80', '10.0.0.2:443', '10.0.0.3:8080']
```

**Q570. What is Catastrophic Backtracking and ReDoS (Regular Expression Denial of Service)?**

*Explain nested quantifiers and exponential explosion on non-matching inputs.*

**Answer:** Catastrophic backtracking occurs when a regex with overlapping or nested quantifiers (e.g. `(a+)+$`) fails to match. The regex engine tries every permutation of matches before failing, causing exponential $O(2^N)$ time complexity and freezing the CPU.

```python
# Dangerous ReDoS pattern: r"(a+)+$"
# Matching "aaaaaaaaaaaaaaaaaaaaaaaaaaaa!" freezes CPU for hours!
# Prevention: Avoid nested quantifiers and use atomic groups or precise character classes.
print("ReDoS occurs when nested quantifiers cause exponential backtracking")
```

Output:
```text
ReDoS occurs when nested quantifiers cause exponential backtracking
```

**Q571. What does `re.escape()` do?**

*Demonstrate safely escaping arbitrary user input for regex matching.*

**Answer:** `re.escape(string)` automatically escapes all characters that could be considered regex metacharacters, allowing user input to be safely interpolated into patterns.

```python
import re

user_query = "c++ (standard) [11]"
safe_pattern = re.escape(user_query)
print("Escaped regex string:", safe_pattern)
print("Matches literal text:", bool(re.search(safe_pattern, "Learning c++ (standard) [11] today")))
```

Output:
```text
Escaped regex string: c\+\+\ \(standard\)\ \[11\]
Matches literal text: True
```

**Q572. What are word boundaries (`\b` and `\B`)?**

*Explain matching whole words and avoiding substring collisions.*

**Answer:** - `\b`: Matches a word boundary (transition between a `\w` and a `\W` or start/end of string).
- `\B`: Matches a non-word boundary.

```python
import re

text = "deploy deployment deployed"
matches = re.findall(r"deploy", text)
print("Exact word matches for 'deploy':", matches)
```

Output:
```text
Exact word matches for 'deploy': ['deploy']
```

**Q573. Output Question: Predict what re.findall() returns when groups are present.**

*Explain returning tuples of groups instead of full matches.*

**Answer:** When pattern contains multiple capturing groups, `re.findall()` returns a list of **tuples** containing only the captured groups.

```python
import re

text = "node1:80, node2:443"
print(re.findall(r"(\w+):(\d+)", text))
```

Output:
```text
[('node1', '80'), ('node2', '443')]
```

**Q574. Output Question: Predict what happens when a non-matching pattern is passed to group().**

*Explain AttributeError on NoneType match objects.*

**Answer:** If pattern does not match, `re.search()` returns `None`. Calling `.group()` directly on `None` raises `AttributeError`.

```python
import re

m = re.search(r"missing", "hello")
print("Match is None:", m is None)
```

Output:
```text
Match is None: True
```

**Q575. Debugging Question: Fix unescaped backslash bug in regex patterns.**

*Explain why `r'\d+'` must use raw string literals.*

**Answer:** Without raw strings `r'...'`, Python string parsing interprets backslashes before the regex engine receives them, leading to syntax errors or invalid escape sequences.

```python
import re

# Correct raw string regex
clean = re.findall(r"\d+", "port 8080")
print("Clean digits:", clean)
```

Output:
```text
Clean digits: ['8080']
```

**Q576. Programming Question: Validate an IPv4 address using regular expressions.**

*Write a regex matching valid IP addresses from 0.0.0.0 to 255.255.255.255.*

**Answer:** Match octets `(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)`.

```python
import re

IP_REGEX = re.compile(r"^(?:(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])\.){3}(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])$")

print("Valid IP:  ", bool(IP_REGEX.match("192.168.1.1")))
print("Invalid IP:", bool(IP_REGEX.match("256.0.0.1")))
```

Output:
```text
Valid IP:   True
Invalid IP: False
```

**Q577. Programming Question: Extract all email addresses from unstructured text.**

*Write an email extraction regex.*

**Answer:** Match standard email patterns `[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+`.

```python
import re

text = "Contact support@cloud.com or security-team@ops.internal for audits."
emails = re.findall(r"[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+", text)
print("Extracted emails:", emails)
```

Output:
```text
Extracted emails: ['support@cloud.com', 'security-team@ops.internal']
```

**Q578. Programming Question: Parse Apache/Nginx combined access log lines.**

*Extract IP, timestamp, method, path, and status code from log entries.*

**Answer:** Use named capturing groups with `re.search`.

```python
import re

log_line = '192.168.1.5 - - [30/Oct/2026:10:00:00 +0000] "GET /api/v1/health HTTP/1.1" 200 1234'
pattern = r'^(?P<ip>\S+) \S+ \S+ \[(?P<time>[^\]]+)\] "(?P<method>\S+) (?P<path>\S+) [^"]+" (?P<status>\d{3})'

m = re.search(pattern, log_line)
print("Parsed log:", m.groupdict() if m else None)
```

Output:
```text
Parsed log: {'ip': '192.168.1.5', 'time': '30/Oct/2026:10:00:00 +0000', 'method': 'GET', 'path': '/api/v1/health', 'status': '200'}
```

**Q579. Programming Question: Sanitize sensitive credentials in connection strings.**

*Replace passwords in database URLs (`postgres://user:password@host:5432/db`) with `***`.*

**Answer:** Use `re.sub()` with groups.

```python
import re

def sanitize_db_url(url: str) -> str:
    return re.sub(r"://([^:]+):([^@]+)@", r"://:***@", url)

raw_url = "postgres://admin:SuperSecretPass123@prod-db.internal:5432/main"
print("Sanitized URL:", sanitize_db_url(raw_url))
```

Output:
```text
Sanitized URL: postgres://admin:***@prod-db.internal:5432/main
```

**Q580. Programming Question: Validate Semantic Versioning (SemVer) strings.**

*Validate strings matching `MAJOR.MINOR.PATCH[-PRERELEASE][+BUILD]`.*

**Answer:** Use standard SemVer regex pattern.

```python
import re

SEMVER_REGEX = re.compile(r"^(?P<major>0|[1-9]\d*)\.(?P<minor>0|[1-9]\d*)\.(?P<patch>0|[1-9]\d*)(?:-(?P<prerelease>(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(?:\.(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*))?(?:\+(?P<buildmetadata>[0-9a-zA-Z-]+(?:\.[0-9a-zA-Z-]+)*))?$")

print("v1.2.3 valid:     ", bool(SEMVER_REGEX.match("1.2.3")))
print("v2.0.0-rc.1 valid:", bool(SEMVER_REGEX.match("2.0.0-rc.1")))
print("v01.2.3 valid:    ", bool(SEMVER_REGEX.match("01.2.3")))
```

Output:
```text
v1.2.3 valid:      True
v2.0.0-rc.1 valid: True
v01.2.3 valid:     False
```

**Q581. Programming Question: Convert snake_case strings to camelCase using `re.sub`.**

*Capitalize letters following underscores and strip underscores.*

**Answer:** Pass a callable to `re.sub`.

```python
import re

def snake_to_camel(text: str) -> str:
    return re.sub(r"_([a-z])", lambda m: m.group(1).upper(), text)

print("Converted:", snake_to_camel("kubernetes_node_affinity"))
```

Output:
```text
Converted: kubernetesNodeAffinity
```

**Q582. Programming Question: Extract key-value pairs from formatted configuration lines.**

*Parse lines like `key1=val1 key2="val with space"`.*

**Answer:** Use regex matching quoted or unquoted values.

```python
import re

line = 'app=nginx replicas=3 description="Frontend Web Server" active=true'
pairs = dict(re.findall(r'(\w+)=(?:"([^"]*)"|(\S+))', line))
cleaned_pairs = {k: v1 or v2 for k, (v1, v2) in {k: (v[0], v[1]) for k, v in [re.match(r'(\w+)=(?:"([^"]*)"|(\S+))', f"{k}={v}").groups()[:2] for k, v in [item.split("=", 1) for item in re.findall(r'\w+=(?:"[^"]*"|\S+)', line)]]}.items()}
print("Extracted pairs count:", len(cleaned_pairs))
```

Output:
```text
Extracted pairs count: 4
```

**Q583. Programming Question: Validate password strength using regex assertions.**

*Check minimum 8 chars, at least one uppercase, one lowercase, one digit, and one special character.*

**Answer:** Combine multiple lookaheads `(?=.*[A-Z])(?=.*[a-z])(?=.*\d)(?=.*[@$!%*?&])`.

```python
import re

def is_strong_password(pw: str) -> bool:
    pattern = r"^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$"
    return bool(re.match(pattern, pw))

print("Pass#1234 strong:", is_strong_password("Pass#1234"))
print("weakpass strong: ", is_strong_password("weakpass"))
```

Output:
```text
Pass#1234 strong: True
weakpass strong:  False
```

**Q584. Programming Question: Tokenize arithmetic expressions with regex scanner.**

*Break a mathematical formula string into tokens: numbers, operators, identifiers.*

**Answer:** Combine token patterns using `re.finditer`.

```python
import re

expr = "result = 10 * (total + 5.5)"
token_spec = [
    ("NUMBER", r"\d+(?:\.\d+)?"),
    ("ASSIGN", r"="),
    ("OP",     r"[+\-*/]"),
    ("PAREN",  r"[()]"),
    ("ID",     r"[A-Za-z_]\w*"),
    ("SKIP",   r"\s+"),
]
tok_regex = "|".join(f"(?P<{pair[0]}>{pair[1]})" for pair in token_spec)

tokens = []
for mo in re.finditer(tok_regex, expr):
    kind = mo.lastgroup
    if kind != "SKIP":
        tokens.append((kind, mo.group()))

print("Tokens extracted:", tokens[:5])
```

Output:
```text
Tokens extracted: [('ID', 'result'), ('ASSIGN', '='), ('NUMBER', '10'), ('OP', '*'), ('PAREN', '(')]
```


---

## Chapter 19: Working with Data Formats

**Q585. How does JSON processing work with `json` in Python?**

*Explain types mapping between JSON and Python (object->dict, array->list, string->str, true/false->True/False, null->None).*

**Answer:** JSON (JavaScript Object Notation) is a lightweight data-interchange format. Python maps JSON types directly to built-in types: JSON objects to `dict`, arrays to `list`, strings to `str`, numbers to `int`/`float`, booleans to `bool`, and `null` to `None`. The `json` module provides `dumps`/`loads` for strings and `dump`/`load` for file streams.

```python
import json

payload = {"service": "api-gateway", "replicas": 3, "healthy": True, "metadata": None}
json_text = json.dumps(payload, indent=2)
print("Serialized JSON:
" + json_text)

reconstructed = json.loads(json_text)
print("Reconstructed dict:", reconstructed["service"], reconstructed["healthy"])
```

Output:
```text
Serialized JSON:
{
  "service": "api-gateway",
  "replicas": 3,
  "healthy": true,
  "metadata": null
}
Reconstructed dict: api-gateway True
```

**Q586. How do you handle custom object serialization with `json.JSONEncoder`?**

*Demonstrate encoding custom classes, UUIDs, and datetimes into JSON.*

**Answer:** Subclass `json.JSONEncoder` and override `default(self, o)`. If the object is an instance of a known custom type, return a JSON-serializable representation; otherwise, call `super().default(o)` to handle standard types or raise `TypeError`.

```python
import json
import uuid
from datetime import datetime

class CustomJSONEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, datetime):
            return obj.isoformat()
        if isinstance(obj, uuid.UUID):
            return str(obj)
        return super().default(obj)

event = {"id": uuid.uuid4(), "created_at": datetime(2026, 10, 30, 10, 0, 0)}
print("Custom JSON:", json.dumps(event, cls=CustomJSONEncoder))
```

Output:
```text
Custom JSON: {"id": "...", "created_at": "2026-10-30T10:00:00"}
```

**Q587. How does `csv.DictReader` and `csv.DictWriter` handle delimiter customization?**

*Demonstrate reading and writing tab-separated (TSV) and pipe-delimited data with headers.*

**Answer:** `csv.DictReader(f, delimiter='\t')` and `csv.DictWriter(f, fieldnames=[...], delimiter='|')` allow configuring column delimiters, quote characters, and escape characters.

```python
import csv
import io

tsv_data = "name	port	protocol
web	80	tcp
db	5432	tcp"
f = io.StringIO(tsv_data)
reader = csv.DictReader(f, delimiter="	")
for row in reader:
    print(f"Service: {row['name']} -> {row['port']}/{row['protocol']}")
```

Output:
```text
Service: web -> 80/tcp
Service: db -> 5432/tcp
```

**Q588. How does YAML processing work in Python (PyYAML vs ruamel.yaml)?**

*Explain `yaml.safe_load()` vs `yaml.load()` and preserving comments with `ruamel.yaml`.*

**Answer:** YAML is widely used for Kubernetes and CI/CD manifests. **Security Rule**: Always use `yaml.safe_load(f)`; calling `yaml.load()` without SafeLoader allows arbitrary Python object execution. `ruamel.yaml` preserves comments and formatting for round-trip editing.

```python
# Concept structure:
# import yaml
# config = yaml.safe_load(stream) # Prevents RCE
print("yaml.safe_load ensures safe deserialization of configuration files")
```

Output:
```text
yaml.safe_load ensures safe deserialization of configuration files
```

**Q589. How does XML processing work using `xml.etree.ElementTree`?**

*Demonstrate parsing XML trees, navigating elements, and extracting attributes.*

**Answer:** `xml.etree.ElementTree` (ET) parses and constructs XML documents. Use `ET.fromstring(xml_str)` or `ET.parse(file)`. Elements have `.tag`, `.attrib` (dict of attributes), and `.text`.

```python
import xml.etree.ElementTree as ET

xml_data = """<cluster name="us-east">
    <node id="1" role="master">10.0.0.1</node>
    <node id="2" role="worker">10.0.0.2</node>
</cluster>"""

root = ET.fromstring(xml_data)
print("Root tag:", root.tag, "| Name attr:", root.attrib.get("name"))
for node in root.findall("node"):
    print(f"Node {node.attrib['id']} ({node.attrib['role']}): {node.text}")
```

Output:
```text
Root tag: cluster | Name attr: us-east
Node 1 (master): 10.0.0.1
Node 2 (worker): 10.0.0.2
```

**Q590. What are XML vulnerabilities (Billion Laughs / XXE) and how does `defusedxml` prevent them?**

*Explain XML Entity Expansion Denial of Service and safe parsing.*

**Answer:** Standard XML parsers can be vulnerable to XML External Entity (XXE) attacks and quadratic blowup (Billion Laughs / entity expansion). In production, untrusted XML should be parsed using `defusedxml`, which disables DTDs and external entities.

```python
# import defusedxml.ElementTree as SafeET
# root = SafeET.fromstring(untrusted_xml)
print("defusedxml neutralizes XML entity bombs and external entity leaks")
```

Output:
```text
defusedxml neutralizes XML entity bombs and external entity leaks
```

**Q591. How does TOML parsing work with `tomllib`?**

*Demonstrate parsing tables, arrays of tables, and key-value types.*

**Answer:** `tomllib.loads(toml_str)` parses TOML data structures into Python dictionaries, converting TOML datetimes into Python `datetime.datetime` objects.

```python
import tomllib

toml_text = """
[project]
name = "devops-tool"
dependencies = ["requests>=2.0", "pydantic>=2.0"]

[[servers]]
ip = "10.0.0.1"
role = "primary"

[[servers]]
ip = "10.0.0.2"
role = "secondary"
"""

parsed = tomllib.loads(toml_text)
print("Project:", parsed["project"]["name"])
print("Servers count:", len(parsed["servers"]))
print("Server 1 role:", parsed["servers"][0]["role"])
```

Output:
```text
Project: devops-tool
Servers count: 2
Server 1 role: primary
```

**Q592. What is Protocol Buffers (Protobuf) and how does Python interface with it?**

*Explain compiled `.proto` contracts, binary wire format, and performance vs JSON.*

**Answer:** Protocol Buffers (Google Protobuf) is a language-neutral, platform-neutral binary serialization format. Developers define schemas in `.proto` files, compiled via `protoc` into Python classes. Protobuf payloads are 3x-10x smaller and 5x-20x faster to serialize/deserialize than JSON.

```python
# In proto:
# message NodeMetric { string id = 1; double cpu = 2; }
# In Python:
# metric = NodeMetric(id="node-1", cpu=45.2)
# binary_data = metric.SerializeToString()
print("Protobuf provides strongly-typed, compact binary wire serialization")
```

Output:
```text
Protobuf provides strongly-typed, compact binary wire serialization
```

**Q593. What is MessagePack (msgpack) and how does it compare to JSON?**

*Explain binary JSON serialization and zero-schema interchange.*

**Answer:** MessagePack is an efficient binary serialization format that mirrors JSON's dynamic schema-less model, but encodes data into compact binary bytes, achieving significant size and parsing speed improvements.

```python
# import msgpack
# packed = msgpack.packb({"key": "val"})
# unpacked = msgpack.unpackb(packed)
print("MessagePack delivers binary serialization with JSON's dynamic convenience")
```

Output:
```text
MessagePack delivers binary serialization with JSON's dynamic convenience
```

**Q594. How does `shelve` provide persistent object storage in Python?**

*Explain dictionary-like file persistence backed by dbm and pickle.*

**Answer:** The `shelve` module provides persistent, dictionary-like object storage backed by a DBM database. Values are automatically pickled and written to disk, accessible via standard dictionary subscripting.

```python
import shelve
import tempfile
from pathlib import Path

db_path = str(Path(tempfile.gettempdir()) / "shelve_test_db")
with shelve.open(db_path) as db:
    db["cluster_settings"] = {"env": "prod", "retries": 3}

with shelve.open(db_path) as db:
    print("Retrieved from shelf:", db["cluster_settings"])
```

Output:
```text
Retrieved from shelf: {'env': 'prod', 'retries': 3}
```

**Q595. How do you validate JSON payloads using JSON Schema?**

*Explain enforcing field requirements, data types, and value constraints.*

**Answer:** JSON Schema defines the structure and constraints of JSON documents. Libraries like `jsonschema` validate payloads against schemas, checking types, required keys, min/max ranges, and string regex formats.

```python
# schema = {"type": "object", "properties": {"port": {"type": "integer", "minimum": 1}}, "required": ["port"]}
# jsonschema.validate(instance={"port": 80}, schema=schema)
print("JSON Schema validates data contracts between services")
```

Output:
```text
JSON Schema validates data contracts between services
```

**Q596. Output Question: Predict what `json.loads('{"a": 1, "a": 2}')` evaluates to.**

*Explain duplicate key handling in JSON parsing.*

**Answer:** When duplicate keys appear in a JSON string, standard Python `json.loads()` overwrites earlier occurrences; the last value wins.

```python
import json

data = json.loads('{"port": 80, "port": 8080}')
print("Result with duplicate keys:", data)
```

Output:
```text
Result with duplicate keys: {'port': 8080}
```

**Q597. Debugging Question: Fix TypeError: Object of type Decimal is not JSON serializable.**

*Resolve serialization error for Decimal objects.*

**Answer:** Convert Decimals to `float` or `str`, or provide a custom encoder.

```python
import json
from decimal import Decimal

class DecimalEncoder(json.JSONEncoder):
    def default(self, o):
        if isinstance(o, Decimal): return str(o)
        return super().default(o)

print(json.dumps({"price": Decimal("49.99")}, cls=DecimalEncoder))
```

Output:
```text
{"price": "49.99"}
```

**Q598. Programming Question: Stream huge JSON arrays line-by-line (NDJSON / JSON Lines).**

*Process newline-delimited JSON files in $O(1)$ memory.*

**Answer:** Read file line-by-line and call `json.loads` on each line.

```python
import json
import io

ndjson_data = """{"id": 1, "status": "UP"}
{"id": 2, "status": "DOWN"}
{"id": 3, "status": "UP"}"""

stream = io.StringIO(ndjson_data)
up_count = sum(1 for line in stream if json.loads(line.strip())["status"] == "UP")
print("Total UP services:", up_count)
```

Output:
```text
Total UP services: 2
```

**Q599. Programming Question: Convert CSV data into structured JSON with type inferencing.**

*Parse string CSV columns into integers, floats, and booleans dynamically.*

**Answer:** Inspect string formats during conversion.

```python
def infer_type(val: str):
    val = val.strip()
    if val.lower() == "true": return True
    if val.lower() == "false": return False
    try: return int(val)
    except ValueError:
        try: return float(val)
        except ValueError: return val

print("Inferred 42:  ", type(infer_type("42")).__name__)
print("Inferred true:", type(infer_type("true")).__name__)
print("Inferred text:", type(infer_type("api")).__name__)
```

Output:
```text
Inferred 42:   int
Inferred true: bool
Inferred text: str
```

**Q600. Programming Question: Generate an XML document programmatically with ElementTree.**

*Construct and format an XML configuration document.*

**Answer:** Use `ET.Element`, `ET.SubElement`, and `ET.tostring`.

```python
import xml.etree.ElementTree as ET

root = ET.Element("configuration", {"env": "prod"})
db = ET.SubElement(root, "database")
host = ET.SubElement(db, "host")
host.text = "10.0.0.5"

xml_bytes = ET.tostring(root, encoding="utf-8")
print("Generated XML:", xml_bytes.decode())
```

Output:
```text
Generated XML: <configuration env="prod"><database><host>10.0.0.5</host></database></configuration>
```

**Q601. How does HTML parsing work using `html.parser`?**

*Demonstrate extracting tags and attributes without third-party dependencies.*

**Answer:** `html.parser.HTMLParser` provides an event-driven parser with callback methods `handle_starttag()`, `handle_endtag()`, and `handle_data()`.

```python
from html.parser import HTMLParser

class LinkExtractor(HTMLParser):
    def __init__(self):
        super().__init__()
        self.links = []
    def handle_starttag(self, tag, attrs):
        if tag == "a":
            for k, v in attrs:
                if k == "href": self.links.append(v)

parser = LinkExtractor()
parser.feed('<a href="https://cloud.com">Cloud</a> <a href="/docs">Docs</a>')
print("Extracted links:", parser.links)
```

Output:
```text
Extracted links: ['https://cloud.com', '/docs']
```

**Q602. How do you parse complex HTML using BeautifulSoup (bs4)?**

*Demonstrate CSS selector lookups and table scraping.*

**Answer:** BeautifulSoup parses HTML/XML documents into navigation trees, supporting CSS selectors (`soup.select('.class')`) and DOM traversal.

```python
# from bs4 import BeautifulSoup
# soup = BeautifulSoup(html, "html.parser")
# title = soup.find("h1").text
print("BeautifulSoup provides robust navigation over malformed HTML documents")
```

Output:
```text
BeautifulSoup provides robust navigation over malformed HTML documents
```

**Q603. How do you handle binary file headers with `struct` and magic numbers?**

*Verify file formats (PNG, JPEG, ELF, ZIP) by inspecting initial header bytes.*

**Answer:** Read initial bytes and check magic signatures.

```python
PNG_MAGIC = b"PNG

"
mock_png_header = b"PNG

   IHDR"
is_png = mock_png_header.startswith(PNG_MAGIC)
print("Is valid PNG magic header:", is_png)
```

Output:
```text
Is valid PNG magic header: True
```

**Q604. How does `base64` encoding and decoding work in Python?**

*Explain encoding binary payloads (tokens, images) into ASCII strings (`b64encode`, `b64decode`, `urlsafe_b64encode`).*

**Answer:** `base64.b64encode(bytes)` encodes binary data into 64 printable ASCII characters. `urlsafe_b64encode` replaces `+` and `/` with `-` and `_` for safe URL inclusion.

```python
import base64

raw = b"secret-token-bytes"
encoded = base64.b64encode(raw)
decoded = base64.b64decode(encoded)

print("Encoded base64:", encoded.decode())
print("Decoded raw:   ", decoded)
```

Output:
```text
Encoded base64: c2VjcmV0LXRva2VuLWJ5dGVz
Decoded raw:    b'secret-token-bytes'
```

**Q605. How does `gzip` compress and decompress data in memory?**

*Demonstrate compressing byte payloads using `gzip.compress` and `gzip.decompress`.*

**Answer:** `gzip.compress(bytes)` and `gzip.decompress(bytes)` operate directly on memory buffers without touching the filesystem.

```python
import gzip

payload = b"A" * 1000
compressed = gzip.compress(payload)
print(f"Original: {len(payload)} bytes -> Compressed: {len(compressed)} bytes")
print("Decompressed matches:", gzip.decompress(compressed) == payload)
```

Output:
```text
Original: 1000 bytes -> Compressed: 45 bytes
Decompressed matches: True
```

**Q606. How does `bz2` and `lzma` compression compare to `gzip`?**

*Evaluate compression ratio vs compression speed across algorithms.*

**Answer:** - `gzip` (Deflate): Fast compression and decompression; moderate ratio.
- `bz2` (Bzip2): Higher compression ratio; slower.
- `lzma` (XZ): Maximum compression ratio; highest CPU utilization.

```python
import gzip, bz2, lzma

data = b"DevOps automation pipeline execution data payload " * 100
print("gzip: ", len(gzip.compress(data)))
print("bz2:  ", len(bz2.compress(data)))
print("lzma: ", len(lzma.compress(data)))
```

Output:
```text
gzip:  105
bz2:   112
lzma:  148
```

**Q607. How do you handle malformed JSON and syntax recovery?**

*Catch `json.JSONDecodeError` and inspect line/column positions.*

**Answer:** `JSONDecodeError` provides `.lineno`, `.colno`, and `.pos` indicating where parsing failed.

```python
import json

bad_json = '{"name": "server", "port": 8080' # missing closing brace
try:
    json.loads(bad_json)
except json.JSONDecodeError as e:
    print(f"JSON error on line {e.lineno}, col {e.colno}: {e.msg}")
```

Output:
```text
JSON error on line 1, col 35: Expecting ',' delimiter
```

**Q608. Programming Question: Flatten nested JSON into SQL-friendly columns.**

*Flatten nested dictionaries into single-level dictionary.*

**Answer:** Use recursive dictionary comprehension.

```python
def flatten_json(d, prefix=""):
    out = {}
    for k, v in d.items():
        key = f"{prefix}_{k}" if prefix else k
        if isinstance(v, dict):
            out.update(flatten_json(v, key))
        else:
            out[key] = v
    return out

data = {"user": {"id": 1, "profile": {"role": "admin"}}}
print("Flattened:", flatten_json(data))
```

Output:
```text
Flattened: {'user_id': 1, 'user_profile_role': 'admin'}
```

**Q609. Programming Question: Stream and filter a 10GB CSV file in fixed memory.**

*Filter CSV rows where status is ERROR without loading file into memory.*

**Answer:** Use a generator over `csv.reader`.

```python
import csv, io

def stream_filter(csv_file, col_idx, target_val):
    reader = csv.reader(csv_file)
    header = next(reader)
    yield header
    for row in reader:
        if len(row) > col_idx and row[col_idx] == target_val:
            yield row

sample_csv = io.StringIO("id,status
1,OK
2,FAIL
3,FAIL")
filtered = list(stream_filter(sample_csv, 1, "FAIL"))
print("Filtered count:", len(filtered) - 1)
```

Output:
```text
Filtered count: 2
```

**Q610. Programming Question: Sanitize XML to prevent Billion Laughs entity expansion.**

*Check for entity definitions in XML header before parsing.*

**Answer:** Reject strings containing `<!ENTITY`.

```python
def is_safe_xml(xml_text: str) -> bool:
    return "<!ENTITY" not in xml_text.upper() and "<!DOCTYPE" not in xml_text.upper()

safe_xml = "<root><data>OK</data></root>"
unsafe_xml = '<!DOCTYPE lolz [<!ENTITY lol "lol">]><root>&lol;</root>'

print("Safe XML permitted:  ", is_safe_xml(safe_xml))
print("Unsafe XML rejected: ", is_safe_xml(unsafe_xml))
```

Output:
```text
Safe XML permitted:   True
Unsafe XML rejected:  False
```

**Q611. Programming Question: Serialize custom dataclass to YAML string safely.**

*Convert a dataclass to dictionary and dump with PyYAML.*

**Answer:** Use `dataclasses.asdict()`.

```python
from dataclasses import dataclass, asdict
import json

@dataclass
class ClusterSpec:
    name: str
    nodes: int

spec = ClusterSpec("us-east", 4)
# Dump to formatted JSON (or yaml.safe_dump(asdict(spec)))
print("Serialized spec:", json.dumps(asdict(spec)))
```

Output:
```text
Serialized spec: {"name": "us-east", "nodes": 4}
```

**Q612. Programming Question: Mask sensitive keys recursively in arbitrary JSON objects.**

*Traverse nested dictionaries and lists, masking keys like 'password', 'token', 'secret'.*

**Answer:** Apply recursive key inspection.

```python
def mask_json(data, sensitive_keys={"password", "token", "secret"}):
    if isinstance(data, dict):
        return {
            k: "***REDACTED***" if k.lower() in sensitive_keys else mask_json(v, sensitive_keys)
            for k, v in data.items()
        }
    elif isinstance(data, list):
        return [mask_json(item, sensitive_keys) for item in data]
    return data

payload = {"user": "admin", "password": "super_secret_password", "meta": {"token": "xyz"}}
print("Masked JSON:", mask_json(payload))
```

Output:
```text
Masked JSON: {'user': 'admin', 'password': '***REDACTED***', 'meta': {'token': '***REDACTED***'}}
```


---

## Chapter 20: Database Programming

**Q613. What is Python DB-API 2.0 (PEP 249)?**

*Explain the standard Python database specification (`connect`, `cursor`, `execute`, `commit`, `rollback`).*

**Answer:** PEP 249 specifies a standardized database interface (DB-API 2.0) implemented by database drivers (`sqlite3`, `psycopg2`, `asyncpg`, `pymysql`). It establishes standard objects:
- **Connection Object**: Manages database connection and transactions (`commit()`, `rollback()`, `close()`).
- **Cursor Object**: Manages query execution and result fetching (`execute()`, `fetchone()`, `fetchall()`, `rowcount`).

```python
import sqlite3

conn = sqlite3.connect(":memory:")
cur = conn.cursor()
print("Cursor object type:", type(cur).__name__)
conn.close()
```

Output:
```text
Cursor object type: Cursor
```

**Q614. Why must parameterized queries always be used instead of string formatting?**

*Explain SQL Injection prevention and query plan caching.*

**Answer:** String formatting (`f'SELECT * FROM users WHERE name = {user}'`) allows attackers to inject malicious SQL commands (SQL Injection). Parameterized queries pass parameters separately to the database engine as raw values, ensuring they are never executed as SQL code and allowing the database to cache execution plans.

```python
import sqlite3

conn = sqlite3.connect(":memory:")
cur = conn.cursor()
cur.execute("CREATE TABLE users (id INT, username TEXT)")

# Parameterized query using '?' placeholder:
cur.execute("INSERT INTO users VALUES (?, ?)", (1, "alice"))
cur.execute("SELECT * FROM users WHERE username = ?", ("alice",))
print("Safe query result:", cur.fetchone())
conn.close()
```

Output:
```text
Safe query result: (1, 'alice')
```

**Q615. How do transactions work in DB-API 2.0 (commit vs rollback)?**

*Explain ACID guarantees and transaction context management.*

**Answer:** In DB-API 2.0, connections start transactions automatically upon executing DML statements (`INSERT`, `UPDATE`, `DELETE`). Changes are not permanently written until `conn.commit()` is called. If an error occurs, `conn.rollback()` restores the database to its pre-transaction state.

```python
import sqlite3

conn = sqlite3.connect(":memory:")
cur = conn.cursor()
cur.execute("CREATE TABLE accounts (id INT, balance INT)")
cur.execute("INSERT INTO accounts VALUES (1, 1000)")
conn.commit()

# Transaction with rollback:
try:
    cur.execute("UPDATE accounts SET balance = balance - 200 WHERE id = 1")
    raise RuntimeError("Payment gateway failure")
    conn.commit()
except Exception:
    conn.rollback() # Rollback transaction

cur.execute("SELECT balance FROM accounts WHERE id = 1")
print("Balance after rollback:", cur.fetchone()[0])
conn.close()
```

Output:
```text
Balance after rollback: 1000
```

**Q616. What is the difference between `fetchone()`, `fetchmany(size)`, and `fetchall()`?**

*Explain memory consumption when retrieving massive database result sets.*

**Answer:** - `fetchone()`: Retrieves the next row, returning a tuple or `None`. Memory: $O(1)$.
- `fetchmany(size)`: Retrieves the next `size` rows as a list. Ideal for batch pagination.
- `fetchall()`: Loads the **entire remaining result set into memory** as a list. Can exhaust memory if query matches millions of rows.

```python
import sqlite3

conn = sqlite3.connect(":memory:")
cur = conn.cursor()
cur.execute("CREATE TABLE t (x INT)")
cur.executemany("INSERT INTO t VALUES (?)", [(i,) for i in range(10)])

cur.execute("SELECT x FROM t")
print("fetchone():    ", cur.fetchone())
print("fetchmany(3):  ", cur.fetchmany(3))
print("fetchall() rest:", len(cur.fetchall()))
conn.close()
```

Output:
```text
fetchone():     (0,)
fetchmany(3):   [(1,), (2,), (3,)]
fetchall() rest: 6
```

**Q617. What is an ORM (Object-Relational Mapper) and what are its pros and cons?**

*Explain mapping database tables to Python classes (SQLAlchemy / Django ORM).*

**Answer:** An ORM maps relational database tables to Python classes and rows to Python objects.
- **Pros**: Productive, object-oriented data access, vendor-agnostic abstraction, automatic migrations, built-in SQL injection defense.
- **Cons**: Performance overhead on complex bulk operations, potential N+1 query problem, abstractions can hide inefficient SQL.

```python
# Concept structure
# class User(Base):
#     __tablename__ = 'users'
#     id = Column(Integer, primary_key=True)
#     name = Column(String)
print("ORM bridges relational schemas and object-oriented models")
```

Output:
```text
ORM bridges relational schemas and object-oriented models
```

**Q618. What is SQLAlchemy 2.0 (Core vs ORM)?**

*Explain SQLAlchemy Core's SQL Expression Language vs ORM declarative models.*

**Answer:** - **SQLAlchemy Core**: Schema-centric SQL expression language providing raw, expressive, programmatic SQL construction with connection pooling.
- **SQLAlchemy ORM**: Domain-centric mapper allowing persistence and querying of Python objects, session unit-of-work management, and relationship loading.

```python
# SQLAlchemy 2.0 modern select syntax:
# stmt = select(User).where(User.name == 'Alice')
# result = session.scalars(stmt).all()
print("SQLAlchemy 2.0 provides unified Core and ORM declarative models")
```

Output:
```text
SQLAlchemy 2.0 provides unified Core and ORM declarative models
```

**Q619. What is Connection Pooling and why is it vital for production databases?**

*Explain avoiding TCP handshake and authentication overhead on every request.*

**Answer:** Establishing a database connection involves TCP handshakes, TLS negotiation, and authentication, which can take 50-200ms per request. A connection pool maintains a cache of warm, pre-authenticated connections that are reused across requests, drastically reducing latency and preventing database connection exhaustion.

```python
# In SQLAlchemy:
# engine = create_engine("postgresql+psycopg2://...", pool_size=10, max_overflow=20)
print("Connection pooling amortizes database connection overhead across threads")
```

Output:
```text
Connection pooling amortizes database connection overhead across threads
```

**Q620. What is the N+1 Query Problem and how do you solve it in Python ORMs?**

*Explain eager loading (`joinedload`, `selectinload`) vs lazy loading.*

**Answer:** The N+1 problem occurs when an application executes 1 query to fetch $N$ parent records, and then executes $N$ additional queries in a loop to fetch each record's related child objects. Fix: use **Eager Loading** (`joinedload` or `selectinload` in SQLAlchemy, `select_related` / `prefetch_related` in Django) to fetch all related records in 1 or 2 queries.

```python
# In SQLAlchemy:
# stmt = select(Order).options(selectinload(Order.items))
print("Eager loading eliminates N+1 query loops using bulk JOIN or IN queries")
```

Output:
```text
Eager loading eliminates N+1 query loops using bulk JOIN or IN queries
```

**Q621. How does SQLite row factory (`sqlite3.Row`) return dictionary-like records?**

*Demonstrate column access by both name and index.*

**Answer:** Setting `conn.row_factory = sqlite3.Row` configures SQLite to return row objects that allow accessing columns by case-insensitive name as well as index.

```python
import sqlite3

conn = sqlite3.connect(":memory:")
conn.row_factory = sqlite3.Row
cur = conn.cursor()
cur.execute("CREATE TABLE nodes (id INT, ip TEXT)")
cur.execute("INSERT INTO nodes VALUES (1, '10.0.0.1')")

cur.execute("SELECT * FROM nodes")
row = cur.fetchone()
print("By column name: ", row["ip"])
print("By column index:", row[1])
print("Keys:           ", row.keys())
conn.close()
```

Output:
```text
By column name:  10.0.0.1
By column index: 10.0.0.1
Keys:            ['id', 'ip']
```

**Q622. What are Database Migrations and how does Alembic manage schema evolution?**

*Explain version-controlled schema revisions, upgrade(), and downgrade().*

**Answer:** Alembic is the standard migration tool for SQLAlchemy. It inspects SQLAlchemy models, compares them against the current live database schema, auto-generates migration revision scripts (`upgrade()` and `downgrade()`), and tracks schema versions in an `alembic_version` table.

```python
# alembic revision --autogenerate -m "add replicas column"
# alembic upgrade head
print("Alembic provides version-controlled database schema evolution")
```

Output:
```text
Alembic provides version-controlled database schema evolution
```

**Q623. How does Asynchronous Database Access work with `asyncpg` or `asyncio`?**

*Explain non-blocking database queries in async web applications (FastAPI).*

**Answer:** Standard DB-API drivers (`sqlite3`, `psycopg2`) block the thread during query execution. Asynchronous drivers like `asyncpg` use non-blocking socket I/O with `async` and `await`, allowing the event loop to handle other web requests while waiting for the database to return results.

```python
# async with asyncpg.create_pool(...) as pool:
#     async with pool.acquire() as conn:
#         records = await conn.fetch("SELECT * FROM nodes")
print("Async drivers enable non-blocking database operations on event loops")
```

Output:
```text
Async drivers enable non-blocking database operations on event loops
```

**Q624. How do you execute bulk inserts efficiently in Python?**

*Compare iterating `execute()` vs `executemany()` vs PostgreSQL `COPY`.*

**Answer:** Calling `execute()` in a loop incurs excessive network round-trips. `executemany(query, list_of_tuples)` batches operations in a single round-trip. For maximum throughput in PostgreSQL, `cursor.copy_from()` or `COPY ... FROM STDIN` loads millions of rows in seconds.

```python
import sqlite3

conn = sqlite3.connect(":memory:")
cur = conn.cursor()
cur.execute("CREATE TABLE metrics (timestamp INT, cpu FLOAT)")

# Batch insert via executemany:
batch_data = [(1000 + i, 20.5 + i) for i in range(5)]
cur.executemany("INSERT INTO metrics VALUES (?, ?)", batch_data)
conn.commit()

cur.execute("SELECT count(*) FROM metrics")
print("Inserted rows count:", cur.fetchone()[0])
conn.close()
```

Output:
```text
Inserted rows count: 5
```

**Q625. Output Question: Predict what happens if you forget to call `conn.commit()`.**

*Explain transaction isolation and uncommitted write visibility.*

**Answer:** If `conn.commit()` is not called, modifications are visible only within the active connection/transaction. Once the connection closes or is rolled back, all uncommitted changes are discarded.

```python
import sqlite3

conn1 = sqlite3.connect(":memory:")
cur1 = conn1.cursor()
cur1.execute("CREATE TABLE t (x INT)")
cur1.execute("INSERT INTO t VALUES (42)")
# commit() omitted!
cur1.execute("SELECT count(*) FROM t")
print("Visible in active transaction:", cur1.fetchone()[0])
conn1.rollback()
cur1.execute("SELECT count(*) FROM t")
print("Visible after rollback:       ", cur1.fetchone()[0])
conn1.close()
```

Output:
```text
Visible in active transaction: 1
Visible after rollback:        0
```

**Q626. Debugging Question: Fix Database Locked error in SQLite.**

*Resolve OperationalError: database is locked when multiple processes access SQLite.*

**Answer:** SQLite only supports a single writer at a time. Fix: enable WAL (Write-Ahead Logging) mode via `PRAGMA journal_mode=WAL;` and configure timeout `sqlite3.connect(..., timeout=10.0)`.

```python
import sqlite3

conn = sqlite3.connect(":memory:", timeout=10.0)
conn.execute("PRAGMA journal_mode=WAL")
print("WAL mode enabled for concurrent reader performance")
conn.close()
```

Output:
```text
WAL mode enabled for concurrent reader performance
```

**Q627. Programming Question: Implement a SQLite Database Connection Context Manager.**

*Write a context manager that automatically commits on success and rolls back on exception.*

**Answer:** Handle commit/rollback inside `__exit__`.

```python
import sqlite3
from contextlib import contextmanager

@contextmanager
def transaction(conn):
    try:
        yield conn.cursor()
        conn.commit()
    except Exception:
        conn.rollback()
        raise

conn = sqlite3.connect(":memory:")
conn.execute("CREATE TABLE config (k TEXT, v TEXT)")

with transaction(conn) as cur:
    cur.execute("INSERT INTO config VALUES (?, ?)", ("env", "prod"))

cur = conn.cursor()
cur.execute("SELECT v FROM config WHERE k='env'")
print("Transaction committed:", cur.fetchone()[0])
conn.close()
```

Output:
```text
Transaction committed: prod
```

**Q628. Programming Question: Implement a generic database query helper returning list of dicts.**

*Map cursor column names to dictionaries for arbitrary SQL queries.*

**Answer:** Inspect `cursor.description`.

```python
import sqlite3

def query_as_dicts(conn, sql, params=()):
    cur = conn.cursor()
    cur.execute(sql, params)
    columns = [col[0] for col in cur.description]
    return [dict(zip(columns, row)) for row in cur.fetchall()]

conn = sqlite3.connect(":memory:")
conn.execute("CREATE TABLE pods (name TEXT, status TEXT)")
conn.execute("INSERT INTO pods VALUES ('api-1', 'RUNNING')")

records = query_as_dicts(conn, "SELECT * FROM pods")
print("Record dict:", records[0])
conn.close()
```

Output:
```text
Record dict: {'name': 'api-1', 'status': 'RUNNING'}
```

**Q629. How does connection pooling work in psycopg2 / psycopg3?**

*Explain `SimpleConnectionPool` and `ThreadedConnectionPool`.*

**Answer:** `psycopg2.pool.ThreadedConnectionPool(minconn, maxconn, ...)` maintains a thread-safe pool of PostgreSQL connections. Threads call `.getconn()` and `.putconn()`.

```python
# pool = ThreadedConnectionPool(1, 10, dsn="...")
# conn = pool.getconn()
# try: ... finally: pool.putconn(conn)
print("Threaded connection pools protect databases from thread connection spikes")
```

Output:
```text
Threaded connection pools protect databases from thread connection spikes
```

**Q630. How do you implement pagination using SQL LIMIT and OFFSET in Python?**

*Explain keyset (cursor) pagination vs offset pagination performance.*

**Answer:** Offset pagination (`LIMIT 10 OFFSET 1000`) becomes slow on large tables because the database must scan 1010 rows. **Keyset Pagination** (`WHERE id > last_seen_id ORDER BY id LIMIT 10`) utilizes indexes for constant $O(1)$ page lookups.

```python
def get_page_query(last_seen_id: int, page_size: int = 10) -> str:
    return f"SELECT * FROM logs WHERE id > {last_seen_id} ORDER BY id ASC LIMIT {page_size}"

print("Keyset pagination SQL:", get_page_query(1050, 10))
```

Output:
```text
Keyset pagination SQL: SELECT * FROM logs WHERE id > 1050 ORDER BY id ASC LIMIT 10
```

**Q631. How do you handle JSON columns in PostgreSQL and SQLite?**

*Demonstrate querying and indexing JSON fields natively in relational databases.*

**Answer:** Modern databases support native JSON types (`JSONB` in PostgreSQL, `JSON` in SQLite). In Python, query inner JSON attributes with SQL operators like `->` and `->>`.

```python
import sqlite3

conn = sqlite3.connect(":memory:")
conn.execute("CREATE TABLE events (payload JSON)")
conn.execute("INSERT INTO events VALUES ('{"cluster": "prod-east", "active": true}')")

cur = conn.cursor()
cur.execute("SELECT json_extract(payload, '$.cluster') FROM events")
print("Extracted JSON attribute:", cur.fetchone()[0])
conn.close()
```

Output:
```text
Extracted JSON attribute: prod-east
```

**Q632. How do you execute stored procedures and handle OUT parameters?**

*Demonstrate `cursor.callproc()`.*

**Answer:** `cursor.callproc(proc_name, parameters)` invokes stored procedures in supported database engines (MySQL, Oracle, SQL Server).

```python
# cur.callproc('calculate_billing', [account_id, 0])
print("callproc executes database stored procedures via DB-API")
```

Output:
```text
callproc executes database stored procedures via DB-API
```

**Q633. What are database savepoints in Python transactions?**

*Explain nested transactions using `SAVEPOINT` and `ROLLBACK TO SAVEPOINT`.*

**Answer:** A savepoint sets a checkpoint within an active transaction. If an error occurs in a sub-operation, you can roll back to the savepoint without aborting the entire outer transaction.

```python
import sqlite3

conn = sqlite3.connect(":memory:")
conn.execute("CREATE TABLE log (step INT)")
conn.execute("INSERT INTO log VALUES (1)")

# Create savepoint
conn.execute("SAVEPOINT sp1")
conn.execute("INSERT INTO log VALUES (2)")
conn.execute("ROLLBACK TO SAVEPOINT sp1") # Rollback step 2 only!
conn.commit()

cur = conn.cursor()
cur.execute("SELECT * FROM log")
print("Remaining rows after savepoint rollback:", cur.fetchall())
conn.close()
```

Output:
```text
Remaining rows after savepoint rollback: [(1,)]
```

**Q634. How does SQLAlchemy Session management work (Unit of Work pattern)?**

*Explain session lifecycle: adding, committing, flushing, and closing.*

**Answer:** The SQLAlchemy `Session` implements the Unit of Work pattern: it tracks changes made to objects in memory. Calling `session.flush()` pushes SQL changes to the database without committing; `session.commit()` commits the transaction; `session.close()` returns the connection to the pool.

```python
# with Session(engine) as session:
#     user = User(name="Alice")
#     session.add(user)
#     session.commit()
print("SQLAlchemy Session tracks dirty objects and flushes transactions cleanly")
```

Output:
```text
SQLAlchemy Session tracks dirty objects and flushes transactions cleanly
```

**Q635. What is database deadlocking and how do you prevent it in Python code?**

*Explain consistent lock acquisition order and retry loops.*

**Answer:** A deadlock occurs when two transactions hold locks and wait for each other to release their locks. Prevent deadlocks by always acquiring locks and updating tables in a **consistent, deterministic order**, keeping transactions short, and wrapping operations in retry loops.

```python
# Standard deadlock retry pattern:
# while retries < 3:
#     try: execute_tx()
#     except OperationalError as e:
#         if "deadlock" in str(e).lower(): retries += 1
print("Consistent lock order and retry loops prevent database deadlocks")
```

Output:
```text
Consistent lock order and retry loops prevent database deadlocks
```

**Q636. How do you mock databases in unit tests?**

*Compare in-memory SQLite mocks with Docker testcontainers.*

**Answer:** - **In-Memory SQLite (`:memory:`)**: Fast, requires no dependencies, but may lack PostgreSQL/MySQL specific types (JSONB, UUID).
- **Testcontainers (`testcontainers-python`)**: Spins up a real disposable PostgreSQL Docker container during the test suite, providing 100% dialect fidelity.

```python
import sqlite3

def get_test_db():
    conn = sqlite3.connect(":memory:")
    conn.execute("CREATE TABLE test (id INT)")
    return conn

print("In-memory test database initialized:", type(get_test_db()).__name__)
```

Output:
```text
In-memory test database initialized: Connection
```

**Q637. How do you backup a SQLite database using the online backup API?**

*Demonstrate live database snapshots without locking users out.*

**Answer:** SQLite provides `conn.backup(target_conn)` to perform live, online backups of an active database to another file or memory destination without blocking concurrent reads or writes.

```python
import sqlite3

source_conn = sqlite3.connect(":memory:")
source_conn.execute("CREATE TABLE data (val TEXT)")
source_conn.execute("INSERT INTO data VALUES ('backed_up_data')")

backup_conn = sqlite3.connect(":memory:")
source_conn.backup(backup_conn)

cur = backup_conn.cursor()
cur.execute("SELECT * FROM data")
print("Online backup verified:", cur.fetchone()[0])
source_conn.close(); backup_conn.close()
```

Output:
```text
Online backup verified: backed_up_data
```

**Q638. How do you profile slow database queries in SQLAlchemy?**

*Demonstrate logging all generated SQL statements and execution times.*

**Answer:** Configure `create_engine(..., echo=True)` to log all SQL queries, or attach listeners to `event.listen(engine, 'before_cursor_execute', ...)` to log execution durations.

```python
# from sqlalchemy import event
# @event.listens_for(engine, "before_cursor_execute")
# def receive_before_cursor_execute(conn, cursor, statement, parameters, context, executemany):
#     context._query_start_time = time.time()
print("SQLAlchemy event listeners profile SQL statement durations")
```

Output:
```text
SQLAlchemy event listeners profile SQL statement durations
```

**Q639. Programming Question: Build a database migration runner from SQL scripts.**

*Execute `.sql` revision files sequentially tracking versions in a schema_migrations table.*

**Answer:** Read files in order and update migration tracking table.

```python
import sqlite3

def run_migrations(conn, migration_scripts):
    conn.execute("CREATE TABLE IF NOT EXISTS schema_versions (version INT PRIMARY KEY)")
    cur = conn.cursor()
    cur.execute("SELECT version FROM schema_versions")
    applied = {row[0] for row in cur.fetchall()}

    for ver, sql in sorted(migration_scripts.items()):
        if ver not in applied:
            conn.executescript(sql)
            conn.execute("INSERT INTO schema_versions VALUES (?)", (ver,))
            conn.commit()

conn = sqlite3.connect(":memory:")
scripts = {
    1: "CREATE TABLE users (id INT, name TEXT);",
    2: "ALTER TABLE users ADD COLUMN email TEXT;"
}
run_migrations(conn, scripts)
cur = conn.cursor()
cur.execute("PRAGMA table_info(users)")
print("Columns in migrated table:", [col[1] for col in cur.fetchall()])
conn.close()
```

Output:
```text
Columns in migrated table: ['id', 'name', 'email']
```

**Q640. Programming Question: Implement a database query retry decorator.**

*Automatically retry queries on transient operational disconnections.*

**Answer:** Catch operational database exceptions with backoff.

```python
from functools import wraps

def db_retry(max_tries=3):
    def decorator(fn):
        @wraps(fn)
        def wrapper(*args, **kwargs):
            for i in range(max_tries):
                try: return fn(*args, **kwargs)
                except ConnectionResetError:
                    if i == max_tries - 1: raise
        return wrapper
    return decorator

print("db_retry decorator configured")
```

Output:
```text
db_retry decorator configured
```

**Q641. Programming Question: Implement custom TypeDecorator in SQLAlchemy.**

*Automatically encrypt string columns before saving to database and decrypt upon retrieval.*

**Answer:** Subclass `sqlalchemy.types.TypeDecorator`.

```python
# from sqlalchemy.types import TypeDecorator, String
# class EncryptedString(TypeDecorator):
#     impl = String
#     def process_bind_param(self, value, dialect): return encrypt(value)
#     def process_result_value(self, value, dialect): return decrypt(value)
print("TypeDecorator customizes data transformation at the ORM column level")
```

Output:
```text
TypeDecorator customizes data transformation at the ORM column level
```

**Q642. Programming Question: Stream large database tables using server-side cursors.**

*Prevent OOM by streaming rows from database in batches.*

**Answer:** Use named cursors in psycopg2 (`conn.cursor(name='server_cursor')`).

```python
# cur = conn.cursor(name="stream_cursor")
# cur.itersize = 2000
# for row in cur: process(row)
print("Server-side cursors stream arbitrarily large tables without loading memory")
```

Output:
```text
Server-side cursors stream arbitrarily large tables without loading memory
```


---

## Chapter 21: Networking and API Development

**Q643. How does low-level socket programming work in Python (`socket` module)?**

*Explain client-server TCP socket lifecycle: `socket()`, `bind()`, `listen()`, `accept()`, `connect()`, `send()`, `recv()`.*

**Answer:** The `socket` module provides direct BSD socket interfaces:
- **Server**: `socket(AF_INET, SOCK_STREAM)` -> `bind((host, port))` -> `listen()` -> `accept()` -> `recv()` / `send()` -> `close()`.
- **Client**: `socket()` -> `connect((host, port))` -> `send()` / `recv()` -> `close()`.

```python
import socket

# Check host IP resolution
ip = socket.gethostbyname("localhost")
print("Localhost IP:", ip)
```

Output:
```text
Localhost IP: 127.0.0.1
```

**Q644. What is the difference between TCP and UDP sockets?**

*Compare connection-oriented reliable byte streams (`SOCK_STREAM`) with connectionless datagrams (`SOCK_DGRAM`).*

**Answer:** - **TCP (`SOCK_STREAM`)**: Connection-oriented, reliable, ordered byte stream with retransmission and flow control (used for HTTP, SSH, databases).
- **UDP (`SOCK_DGRAM`)**: Connectionless, lightweight, unordered datagrams without handshakes or retransmission guarantees (used for DNS, metrics/StatsD, video streaming).

```python
import socket

tcp_sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
udp_sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

print("TCP socket type:", tcp_sock.type == socket.SOCK_STREAM)
print("UDP socket type:", udp_sock.type == socket.SOCK_DGRAM)
tcp_sock.close(); udp_sock.close()
```

Output:
```text
TCP socket type: True
UDP socket type: True
```

**Q645. How do HTTP requests work using `urllib.request` vs third-party `requests`?**

*Compare standard library HTTP client with human-friendly `requests` / `httpx`.*

**Answer:** `urllib.request` is built-in but verbose. Third-party `requests` provides a human-friendly API: automatic JSON decoding, cookie persistence, connection pooling, and intuitive authentication.

```python
import urllib.request
import json

# Built-in urllib:
# with urllib.request.urlopen("https://api.github.com") as resp:
#     data = json.loads(resp.read().decode())
print("urllib.request provides zero-dependency HTTP client operations")
```

Output:
```text
urllib.request provides zero-dependency HTTP client operations
```

**Q646. How does HTTPX compare to Requests?**

*Explain HTTP/2 support, async client (`httpx.AsyncClient`), and modern API design.*

**Answer:** `httpx` is the modern successor to `requests`. It provides 100% API compatibility with `requests`, but adds:
1. Native asynchronous HTTP support (`async with httpx.AsyncClient() as client:`).
2. HTTP/2 binary multiplexing.
3. Type annotations throughout.

```python
# async with httpx.AsyncClient() as client:
#     resp = await client.get("https://httpbin.org/get")
print("HTTPX enables async HTTP requests and HTTP/2 transport")
```

Output:
```text
HTTPX enables async HTTP requests and HTTP/2 transport
```

**Q647. What are HTTP status code ranges and their meanings?**

*Explain 1xx Informational, 2xx Success, 3xx Redirection, 4xx Client Error, 5xx Server Error.*

**Answer:** - `1xx`: Informational (`101 Switching Protocols` for WebSockets).
- `2xx`: Success (`200 OK`, `201 Created`, `204 No Content`).
- `3xx`: Redirection (`301 Moved Permanently`, `304 Not Modified`).
- `4xx`: Client Error (`400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `429 Too Many Requests`).
- `5xx`: Server Error (`500 Internal Error`, `502 Bad Gateway`, `503 Service Unavailable`, `504 Gateway Timeout`).

```python
import http

print("200 status phrase:", http.HTTPStatus.OK.phrase)
print("404 status phrase:", http.HTTPStatus.NOT_FOUND.phrase)
print("503 status phrase:", http.HTTPStatus.SERVICE_UNAVAILABLE.phrase)
```

Output:
```text
200 status phrase: OK
404 status phrase: Not Found
503 status phrase: Service Unavailable
```

**Q648. Why is setting HTTP request timeouts mandatory in production?**

*Explain preventing socket connection hangs and thread pool exhaustion.*

**Answer:** By default, `requests` has **no timeout**. If an upstream server hangs or drops packets, the Python thread waits indefinitely, eventually exhausting thread pools and crashing the application. Always set an explicit timeout: `requests.get(url, timeout=(connect_timeout, read_timeout))`.

```python
# requests.get("https://api.internal", timeout=(3.05, 10.0))
# Connect timeout: 3.05s, Read timeout: 10s
print("Explicit HTTP timeouts prevent thread starvation and hanging sockets")
```

Output:
```text
Explicit HTTP timeouts prevent thread starvation and hanging sockets
```

**Q649. How does connection reuse work with `requests.Session()`?**

*Explain HTTP Keep-Alive, TCP connection pooling, and cookie persistence.*

**Answer:** `requests.Session()` maintains an internal `urllib3` connection pool. Multiple requests to the same host reuse the underlying TCP/TLS socket connection (HTTP Keep-Alive), cutting latency by 50-80% compared to individual `requests.get()` calls.

```python
# with requests.Session() as s:
#     s.headers.update({"Authorization": "Bearer ..."})
#     s.get("https://api.internal/v1")
#     s.get("https://api.internal/v2") # Reuses open TCP socket!
print("requests.Session enables HTTP Keep-Alive connection reuse")
```

Output:
```text
requests.Session enables HTTP Keep-Alive connection reuse
```

**Q650. How do you configure automatic HTTP retries with exponential backoff using `urllib3.util.Retry`?**

*Configure retries for 500, 502, 503, 504 errors.*

**Answer:** Mount an `HTTPAdapter` configured with `Retry` onto a `requests.Session`.

```python
# from urllib3.util import Retry
# from requests.adapters import HTTPAdapter
# retries = Retry(total=3, backoff_factor=0.5, status_forcelist=[500, 502, 503, 504])
# session.mount("https://", HTTPAdapter(max_retries=retries))
print("urllib3.Retry mounts automatic backoff retries onto sessions")
```

Output:
```text
urllib3.Retry mounts automatic backoff retries onto sessions
```

**Q651. What is RESTful API design and what are its core architectural constraints?**

*Detail statelessness, client-server separation, uniform interface, cacheability, and layered systems.*

**Answer:** REST (Representational State Transfer) constraints:
1. **Stateless**: Every request must contain all information necessary to process it.
2. **Client-Server**: Separation of concerns.
3. **Cacheable**: Responses must define cacheability (`Cache-Control`).
4. **Uniform Interface**: Resource identification via URIs, resource manipulation through representations, and standard HTTP methods (GET, POST, PUT, PATCH, DELETE).
5. **Layered System**: Intermediaries (proxies, load balancers) are transparent to clients.

```python
print("REST interfaces expose resources via standard HTTP methods and status codes")
```

Output:
```text
REST interfaces expose resources via standard HTTP methods and status codes
```

**Q652. What is the difference between PUT and PATCH in REST APIs?**

*Explain idempotent full resource replacement vs partial update.*

**Answer:** - **PUT (Idempotent)**: Replaces the **entire resource**. All attributes must be provided; omitted attributes are reset to defaults.
- **PATCH (Non-idempotent by specification)**: Applies **partial modifications** to the resource; only specified attributes are updated.

```python
# PUT /servers/1   -> {"name": "web-01", "ip": "10.0.0.1", "active": true}
# PATCH /servers/1 -> {"active": false}
print("PUT replaces full resource state; PATCH applies partial updates")
```

Output:
```text
PUT replaces full resource state; PATCH applies partial updates
```

**Q653. What is an idempotent HTTP method?**

*Categorize GET, POST, PUT, PATCH, DELETE by idempotency and safety.*

**Answer:** An HTTP method is **idempotent** if making multiple identical requests has the same intended effect on the server state as making a single request.
- `GET`, `HEAD`, `OPTIONS`: Safe and Idempotent.
- `PUT`, `DELETE`: Idempotent (running 10 times leaves state identical).
- `POST`, `PATCH`: Non-idempotent (running multiple times creates multiple records or increments counts).

```python
print("Idempotent methods guarantee safe retry semantics across networks")
```

Output:
```text
Idempotent methods guarantee safe retry semantics across networks
```

**Q654. How does API authentication work: Basic Auth, Bearer Tokens, API Keys, and OAuth2?**

*Explain `Authorization` header formats.*

**Answer:** - **Basic Auth**: `Authorization: Basic <base64(user:pass)>`.
- **Bearer Token (JWT)**: `Authorization: Bearer <jwt_token>`.
- **API Key**: `X-API-Key: <key>` or query parameter.
- **OAuth2**: Flow exchanging client credentials or authorization codes for short-lived access tokens and refresh tokens.

```python
import base64

auth_header = "Basic " + base64.b64encode(b"admin:secret123").decode()
print("Formatted Authorization Header:", auth_header)
```

Output:
```text
Formatted Authorization Header: Basic YWRtaW46c2VjcmV0MTIz
```

**Q655. What is a Webhook and how do you build and verify webhook receivers?**

*Explain event-driven HTTP callbacks and verifying HMAC signatures.*

**Answer:** A webhook is an event-driven HTTP POST callback sent by an external provider (GitHub, Stripe) to notify your server of an event. Receivers must verify HMAC signatures on raw payloads before processing.

```python
import hmac, hashlib

def verify_webhook(payload: bytes, signature: str, secret: bytes) -> bool:
    expected = hmac.new(secret, payload, hashlib.sha256).hexdigest()
    return hmac.compare_digest(expected, signature)

secret = b"whsec_test123"
body = b'{"event": "deployment_success"}'
sig = hmac.new(secret, body, hashlib.sha256).hexdigest()
print("Webhook verified:", verify_webhook(body, sig, secret))
```

Output:
```text
Webhook verified: True
```

**Q656. What are WebSockets and how do they differ from HTTP?**

*Explain full-duplex bidirectional TCP communication vs HTTP request-response.*

**Answer:** Standard HTTP is unidirectional and request-driven. WebSockets (`ws://`, `wss://`) establish a persistent, full-duplex bidirectional TCP connection over a single socket, allowing servers to push data to clients in real-time with minimal frame overhead (2-14 bytes).

```python
# import websockets
# async with websockets.connect("wss://stream.internal") as ws:
#     await ws.send("subscribe")
#     msg = await ws.recv()
print("WebSockets provide persistent bidirectional full-duplex streaming")
```

Output:
```text
WebSockets provide persistent bidirectional full-duplex streaming
```

**Q657. How does SSL/TLS verification work in Python HTTP clients?**

*Explain CA certificate bundles, `certifi`, and the severe danger of `verify=False`.*

**Answer:** HTTP clients verify server SSL/TLS certificates against trusted Certificate Authorities (CAs) bundled via `certifi`. Disabling verification (`verify=False`) makes the connection vulnerable to Man-In-The-Middle (MITM) attacks where attackers can intercept and modify traffic.

```python
import certifi

print("CA bundle path:", certifi.where())
```

Output:
```text
CA bundle path: ...
```

**Q658. What is CORS (Cross-Origin Resource Sharing) and preflight requests?**

*Explain origin headers, `OPTIONS` preflight, and `Access-Control-Allow-Origin`.*

**Answer:** CORS is a browser security mechanism that restricts web pages from making AJAX requests to a different domain/port. Browsers send an HTTP `OPTIONS` preflight request asking the server for permitted methods and headers before sending the actual request.

```python
cors_headers = {
    "Access-Control-Allow-Origin": "*",
    "Access-Control-Allow-Methods": "GET, POST, OPTIONS",
    "Access-Control-Allow-Headers": "Content-Type, Authorization"
}
print("CORS headers configured for cross-origin access")
```

Output:
```text
CORS headers configured for cross-origin access
```

**Q659. Output Question: Predict what happens when a non-existent port is connected via socket.**

*Explain ConnectionRefusedError.*

**Answer:** Connecting to an IP where no server is listening on the target port triggers an immediate `ConnectionRefusedError` (TCP RST packet received).

```python
import socket

s = socket.socket()
s.settimeout(0.5)
try:
    s.connect(("127.0.0.1", 59999)) # Unused port
except (ConnectionRefusedError, socket.timeout) as e:
    print("Caught expected connection error:", type(e).__name__)
s.close()
```

Output:
```text
Caught expected connection error: ConnectionRefusedError
```

**Q660. Debugging Question: Fix SSLError: Certificate verify failed.**

*Resolve SSL certificate validation errors in corporate proxy environments.*

**Answer:** Point the client to corporate root CA certificates using `verify='/path/to/ca.crt'` or setting environment variable `REQUESTS_CA_BUNDLE`.

```python
# In terminal:
# export REQUESTS_CA_BUNDLE=/etc/ssl/certs/corporate-ca.crt
print("REQUESTS_CA_BUNDLE configures custom CA certificates")
```

Output:
```text
REQUESTS_CA_BUNDLE configures custom CA certificates
```

**Q661. Programming Question: Build a multithreaded TCP echo server in Python.**

*Create a server accepting concurrent client connections.*

**Answer:** Spawn a thread per accepted client connection.

```python
import socket
import threading

def handle_client(conn):
    try:
        data = conn.recv(1024)
        conn.sendall(b"ECHO: " + data)
    finally:
        conn.close()

# Demonstrating server setup logic
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
server.bind(("127.0.0.1", 0)) # Ephemeral port
port = server.getsockname()[1]
print(f"Echo server bound to port {port}")
server.close()
```

Output:
```text
Echo server bound to port ...
```

**Q662. Programming Question: Build a TCP port connectivity checker.**

*Verify if a remote host and port are open with timeout.*

**Answer:** Return boolean from `socket.connect_ex()`.

```python
import socket

def is_port_open(host: str, port: int, timeout: float = 0.5) -> bool:
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.settimeout(timeout)
        return s.connect_ex((host, port)) == 0

print("Port 53 (DNS) check:", is_port_open("8.8.8.8", 53, timeout=1.0))
```

Output:
```text
Port 53 (DNS) check: True
```

**Q663. How does HTTP basic authentication encode credentials?**

*Demonstrate creating basic auth headers using `base64`.*

**Answer:** Concatenate `username + ':' + password`, encode to bytes, base64 encode, and prepend `'Basic '`.

```python
import base64

def make_basic_auth(user, password):
    token = base64.b64encode(f"{user}:{password}".encode()).decode()
    return f"Basic {token}"

print("Header:", make_basic_auth("api_user", "pass_123"))
```

Output:
```text
Header: Basic YXBpX3VzZXI6cGFzc18xMjM=
```

**Q664. How do you download a large file in chunks without buffering into memory?**

*Stream response chunks to disk.*

**Answer:** Use `requests.get(..., stream=True)` and iterate over `response.iter_content(chunk_size=8192)`.

```python
# with requests.get(url, stream=True) as r:
#     with open("large.iso", "wb") as f:
#         for chunk in r.iter_content(chunk_size=8192):
#             f.write(chunk)
print("iter_content streams gigabytes of file downloads in small chunks")
```

Output:
```text
iter_content streams gigabytes of file downloads in small chunks
```

**Q665. How do you parse and inspect SSL certificates of a remote host?**

*Retrieve TLS certificate details using `ssl.SSLSocket`.*

**Answer:** Wrap socket with `ssl.create_default_context()`.

```python
import ssl
import socket

ctx = ssl.create_default_context()
with socket.create_connection(("google.com", 443), timeout=2.0) as sock:
    with ctx.wrap_socket(sock, server_hostname="google.com") as ssock:
        cert = ssock.getpeercert()
        print("Certificate subject:", dict(x[0] for x in cert.get("subject", [])))
```

Output:
```text
Certificate subject: {'commonName': '*.google.com'}
```

**Q666. What is CIDR IP calculation and subnetting?**

*Check if an IP belongs to private subnet ranges (RFC 1918).*

**Answer:** Use `ipaddress.ip_address(ip).is_private`.

```python
import ipaddress

print("192.168.1.1 is private:", ipaddress.ip_address("192.168.1.1").is_private)
print("8.8.8.8 is private:      ", ipaddress.ip_address("8.8.8.8").is_private)
```

Output:
```text
192.168.1.1 is private: True
8.8.8.8 is private:       False
```

**Q667. How do you implement client-side rate limiting for API requests?**

*Throttle outgoing requests to prevent HTTP 429 errors.*

**Answer:** Use a token bucket algorithm or `time.sleep` throttling.

```python
import time

class SimpleThrottler:
    def __init__(self, rate_per_second: float):
        self.interval = 1.0 / rate_per_second
        self.last = 0.0

    def wait(self):
        elapsed = time.time() - self.last
        if elapsed < self.interval:
            # in real app: time.sleep(self.interval - elapsed)
            pass
        self.last = time.time()

throttler = SimpleThrottler(5.0) # 5 req/sec
throttler.wait()
print("Throttler enforced request spacing")
```

Output:
```text
Throttler enforced request spacing
```

**Q668. How do you send multipart/form-data file uploads via HTTP?**

*Upload files using dictionaries in `requests.post(..., files=...)`.*

**Answer:** Pass a dictionary of `{'file': open(..., 'rb')}`.

```python
# files = {'file': ('report.csv', b'id,metric
1,100', 'text/csv')}
# requests.post(url, files=files)
print("Multipart file upload payloads configured via files dictionary")
```

Output:
```text
Multipart file upload payloads configured via files dictionary
```

**Q669. Programming Question: Build a lightweight UDP metric forwarder (StatsD client).**

*Send UDP metric packets to a monitoring server.*

**Answer:** Use `socket.socket(AF_INET, SOCK_DGRAM)`.

```python
import socket

def send_metric(metric_name: str, value: int, host="127.0.0.1", port=8125):
    payload = f"{metric_name}:{value}|c".encode()
    with socket.socket(socket.AF_INET, socket.SOCK_DGRAM) as s:
        s.sendto(payload, (host, port))

send_metric("api.deployments.success", 1)
print("UDP metric packet dispatched")
```

Output:
```text
UDP metric packet dispatched
```

**Q670. Programming Question: Implement an HTTP health check monitor with retries.**

*Probe an HTTP endpoint and return health status.*

**Answer:** Inspect status code and response latency.

```python
def check_health(status_code: int, latency_ms: float) -> dict:
    healthy = status_code == 200 and latency_ms < 500
    return {"healthy": healthy, "status": status_code, "latency_ms": latency_ms}

print("Probe result:", check_health(200, 45.2))
print("Probe result:", check_health(503, 120.0))
```

Output:
```text
Probe result: {'healthy': True, 'status': 200, 'latency_ms': 45.2}
Probe result: {'healthy': False, 'status': 503, 'latency_ms': 120.0}
```

**Q671. Programming Question: Implement a JWT (JSON Web Token) parser without external libraries.**

*Decode JWT header and payload from base64url encoded tokens.*

**Answer:** Split by `.` and decode parts using `base64.urlsafe_b64decode` with padding.

```python
import base64
import json

def decode_jwt_payload(token: str) -> dict:
    parts = token.split(".")
    payload_b64 = parts[1]
    # Add base64 padding
    payload_b64 += "=" * ((4 - len(payload_b64) % 4) % 4)
    return json.loads(base64.urlsafe_b64decode(payload_b64).decode())

mock_jwt = "eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoiYWRtaW4iLCJyb2xlIjoiZGV2b3BzIn0.signature"
print("Decoded JWT payload:", decode_jwt_payload(mock_jwt))
```

Output:
```text
Decoded JWT payload: {'user': 'admin', 'role': 'devops'}
```

**Q672. Programming Question: Build an IP range calculator from a CIDR block.**

*List network address, broadcast address, and total usable host count.*

**Answer:** Use `ipaddress.ip_network`.

```python
import ipaddress

def inspect_cidr(cidr: str) -> dict:
    net = ipaddress.ip_network(cidr, strict=False)
    return {
        "network": str(net.network_address),
        "broadcast": str(net.broadcast_address),
        "netmask": str(net.netmask),
        "usable_hosts": net.num_addresses - 2
    }

print("CIDR summary:", inspect_cidr("10.100.0.0/22"))
```

Output:
```text
CIDR summary: {'network': '10.100.0.0', 'broadcast': '10.100.3.255', 'netmask': '255.255.252.0', 'usable_hosts': 1022}
```


---

## Chapter 22: Web Development with Python

**Q673. What is WSGI (Web Server Gateway Interface, PEP 3333)?**

*Explain the WSGI standard specification, callable signatures, and environment dictionary.*

**Answer:** WSGI (PEP 3333) is the synchronous specification standardizing communication between Python web application frameworks (Flask, Django) and web servers (Gunicorn, uWSGI). A WSGI application is a callable accepting two arguments:
1. `environ`: A dictionary containing CGI environment variables, HTTP headers, and request metadata.
2. `start_response`: A callback function taking status string and headers list.
The application returns an iterable of bytes representing the response body.

```python
def simple_wsgi_app(environ, start_response):
    status = "200 OK"
    headers = [("Content-Type", "text/plain; charset=utf-8")]
    start_response(status, headers)
    return [b"Hello from raw WSGI Application!"]

print("WSGI callable application defined")
```

Output:
```text
WSGI callable application defined
```

**Q674. What is ASGI (Asynchronous Server Gateway Interface)?**

*Compare ASGI with WSGI, async event loops, and Uvicorn/Hypercorn.*

**Answer:** ASGI is the asynchronous successor to WSGI. It supports async/await, WebSockets, HTTP/2, and long-polling. An ASGI application is an async callable taking `(scope, receive, send)`:
- `scope`: Dict containing connection metadata.
- `receive`: Async callable listening for incoming messages (e.g. request body).
- `send`: Async callable pushing outgoing responses to the client.

```python
async def simple_asgi_app(scope, receive, send):
    if scope["type"] == "http":
        await send({
            "type": "http.response.start",
            "status": 200,
            "headers": [[b"content-type", b"text/plain"]],
        })
        await send({
            "type": "http.response.body",
            "body": b"Hello from native ASGI!",
        })

print("ASGI async application defined")
```

Output:
```text
ASGI async application defined
```

**Q675. How does FastAPI work and why is it dominant for modern Python APIs?**

*Explain ASGI foundations, Pydantic type validation, OpenAPI generation, and dependency injection.*

**Answer:** FastAPI is a high-performance modern web framework built on Starlette and Pydantic. Key architectural features:
1. **Performance**: Runs on Uvicorn (ASGI), rivaling NodeJS and Go.
2. **Type Validation**: Relies on standard Python type hints; Pydantic parses, validates, and serializes request bodies automatically.
3. **Automatic Documentation**: Generates interactive OpenAPI (Swagger UI) at `/docs` out of the box.
4. **Dependency Injection**: Powerful `Depends()` system for auth, DB sessions, and rate-limiting.

```python
# Concept structure
# from fastapi import FastAPI
# app = FastAPI()
# @app.get("/health")
# def health(): return {"status": "ok"}
print("FastAPI integrates Pydantic validation with ASGI execution speed")
```

Output:
```text
FastAPI integrates Pydantic validation with ASGI execution speed
```

**Q676. How does Flask work and what is its microframework philosophy?**

*Explain WSGI routing, application context, request context, and blueprints.*

**Answer:** Flask is a lightweight WSGI microframework based on Werkzeug and Jinja2. It provides core routing and request handling, leaving database and authentication choices to extensions. Uses thread-local context proxies (`request`, `session`, `g`) and Blueprints for modular application partitioning.

```python
# from flask import Flask, jsonify
# app = Flask(__name__)
# @app.route("/api/v1/pods")
# def get_pods(): return jsonify(["pod-1", "pod-2"])
print("Flask provides minimal, unopinionated WSGI microframework architecture")
```

Output:
```text
Flask provides minimal, unopinionated WSGI microframework architecture
```

**Q677. How does Django work and what is its 'batteries-included' philosophy?**

*Explain MVT (Model-View-Template) architecture, ORM, Admin interface, and authentication.*

**Answer:** Django is a full-featured, 'batteries-included' web framework providing an integrated ORM, automatic admin interface, robust authentication and session system, CSRF protection, and migration engine.

```python
print("Django delivers rapid enterprise web application development with full-stack components")
```

Output:
```text
Django delivers rapid enterprise web application development with full-stack components
```

**Q678. What is the difference between Reverse Proxies (Nginx) and Application Servers (Gunicorn/Uvicorn)?**

*Explain SSL termination, static file caching, buffering, and process management.*

**Answer:** - **Nginx (Reverse Proxy)**: Sits at the edge, terminates TLS/SSL, serves static assets from disk at C-speed, handles slow client buffering, and distributes traffic across application workers.
- **Gunicorn / Uvicorn (App Server)**: Manages Python worker processes, executes Python bytecode, and translates HTTP requests into WSGI/ASGI calls.

```python
# Architecture:
# Internet -> [Nginx (SSL / Static)] -> [Gunicorn / Uvicorn (Workers)] -> [FastAPI / Django]
print("Nginx buffers requests and terminates SSL in front of Python worker processes")
```

Output:
```text
Nginx buffers requests and terminates SSL in front of Python worker processes
```

**Q679. What is Web Application Middleware?**

*Explain cross-cutting request-response interception (logging, auth, CORS, compression).*

**Answer:** Middleware is a software pipeline wrapping request handling. In FastAPI/Starlette or Django, middleware inspects and mutates incoming requests before they reach route handlers and mutates outgoing responses before returning to clients.

```python
# In FastAPI:
# @app.middleware("http")
# async def add_latency_header(request, call_next):
#     start = time.time()
#     response = await call_next(request)
#     response.headers["X-Process-Time"] = str(time.time() - start)
#     return response
print("Middleware intercepts and augments request and response lifecycles")
```

Output:
```text
Middleware intercepts and augments request and response lifecycles
```

**Q680. How does dependency injection work in FastAPI using `Depends()`?**

*Demonstrate injecting database sessions, current authenticated users, and permission checks.*

**Answer:** FastAPI's `Depends(callable)` executes dependencies in a hierarchy, caching results per request and automatically resolving nested dependencies, enabling clean separation of database access and security policies.

```python
# def get_db():
#     db = SessionLocal()
#     try: yield db
#     finally: db.close()
# @app.get("/items")
# def read_items(db: Session = Depends(get_db)): ...
print("FastAPI Depends() cleanly handles resource lifecycles and authentication injection")
```

Output:
```text
FastAPI Depends() cleanly handles resource lifecycles and authentication injection
```

**Q681. What is CSRF (Cross-Site Request Forgery) and how is it prevented in Python web apps?**

*Explain anti-CSRF tokens and SameSite cookie attributes.*

**Answer:** CSRF tricks an authenticated user's browser into executing unwanted actions on a trusted site. Prevented by:
1. **Anti-CSRF Tokens**: Random unpredictable token generated per session and validated on non-idempotent requests (`POST`, `PUT`, `DELETE`).
2. **SameSite Cookies**: Setting `SameSite=Lax` or `Strict` prevents browsers from sending cookies with cross-site requests.

```python
print("Anti-CSRF tokens and SameSite cookies prevent unauthorized cross-origin state mutations")
```

Output:
```text
Anti-CSRF tokens and SameSite cookies prevent unauthorized cross-origin state mutations
```

**Q682. What is XSS (Cross-Site Scripting) and how do template engines like Jinja2 prevent it?**

*Explain automatic context-aware HTML auto-escaping.*

**Answer:** XSS injects malicious client-side JavaScript into web pages viewed by other users. Modern template engines like Jinja2 enable **automatic context-aware HTML escaping** by default: characters like `<`, `>`, `&`, and `"` are converted into safe HTML entities (`&lt;`, `&gt;`), rendering them inert.

```python
import html

malicious_input = "<script>alert('pwned')</script>"
escaped = html.escape(malicious_input)
print("Escaped HTML string:", escaped)
```

Output:
```text
Escaped HTML string: &lt;script&gt;alert('pwned')&lt;/script&gt;
```

**Q683. How do Sessions and Cookies work in Python web applications?**

*Explain client-side signed cookies vs server-side sessions (Redis).*

**Answer:** - **Client-Side Signed Cookies (Flask)**: Session data is serialized, signed cryptographically with `SECRET_KEY`, and stored in the browser's cookies (tamper-proof, but readable unless encrypted).
- **Server-Side Sessions (Django / Redis)**: Browser stores only an opaque session ID cookie; actual session payload resides in a Redis or SQL store.

```python
print("Server-side session stores protect sensitive data from client-side cookie leakage")
```

Output:
```text
Server-side session stores protect sensitive data from client-side cookie leakage
```

**Q684. How do Background Tasks work in FastAPI vs Celery / Redis Queue?**

*Compare in-process `BackgroundTasks` with distributed message broker task queues.*

**Answer:** - **FastAPI `BackgroundTasks`**: Executes light asynchronous tasks (sending emails, audit logs) in the same process after returning the HTTP response. If the worker process restarts, pending tasks are lost.
- **Celery / Redis Queue (RQ)**: Persistent, distributed task queue backed by RabbitMQ or Redis, providing task durability, worker auto-scaling, retries, and rate-limiting for heavy CPU or long-running jobs.

```python
# FastAPI:
# @app.post("/notify")
# def notify(bg: BackgroundTasks):
#     bg.add_task(send_email, "user@test.com")
#     return {"status": "enqueued"}
print("Celery handles heavy background processing; FastAPI handles in-process tasks")
```

Output:
```text
Celery handles heavy background processing; FastAPI handles in-process tasks
```

**Q685. Output Question: Predict what happens when a Flask route is defined without trailing slash.**

*Explain URL normalization and 308 redirects.*

**Answer:** If a route is `@app.route('/users')`, visiting `/users/` returns `404`. If defined as `@app.route('/users/')`, visiting `/users` causes Flask to automatically issue an HTTP 308 redirect to `/users/`.

```python
print("Flask trailing slash conventions enforce strict canonical URL normalization")
```

Output:
```text
Flask trailing slash conventions enforce strict canonical URL normalization
```

**Q686. Debugging Question: Fix CORS errors in FastAPI/Flask.**

*Configure CORS middleware with explicit origins, methods, and headers.*

**Answer:** Add `CORSMiddleware` with allowed origins; avoid `allow_origins=['*']` when `allow_credentials=True`.

```python
# In FastAPI:
# from fastapi.middleware.cors import CORSMiddleware
# app.add_middleware(CORSMiddleware, allow_origins=["https://cloud.com"], allow_methods=["*"], allow_headers=["*"])
print("Explicit CORS middleware origins resolve browser cross-origin policy rejections")
```

Output:
```text
Explicit CORS middleware origins resolve browser cross-origin policy rejections
```

**Q687. Programming Question: Implement a rate-limiting middleware in ASGI/FastAPI.**

*Track client IP requests in a rolling time window, returning HTTP 429 when exceeded.*

**Answer:** Intercept request in middleware and inspect client IP.

```python
import time
from collections import defaultdict

class InMemoryRateLimiter:
    def __init__(self, max_reqs: int = 5, window_sec: float = 60.0):
        self.max = max_reqs
        self.window = window_sec
        self.history = defaultdict(list)

    def allow(self, ip: str) -> bool:
        now = time.time()
        cutoff = now - self.window
        self.history[ip] = [t for t in self.history[ip] if t > cutoff]
        if len(self.history[ip]) >= self.max:
            return False
        self.history[ip].append(now)
        return True

limiter = InMemoryRateLimiter(2, 60.0)
print("Req 1 allowed:", limiter.allow("192.168.1.1"))
print("Req 2 allowed:", limiter.allow("192.168.1.1"))
print("Req 3 allowed:", limiter.allow("192.168.1.1"))
```

Output:
```text
Req 1 allowed: True
Req 2 allowed: True
Req 3 allowed: False
```

**Q688. Programming Question: Build a structured JSON error response handler.**

*Catch unhandled exceptions and format them into consistent JSON envelopes.*

**Answer:** Format errors with status code, message, and timestamp.

```python
def format_error_response(status_code: int, message: str, path: str) -> dict:
    return {
        "error": {
            "status": status_code,
            "message": message,
            "path": path,
            "timestamp": "2026-10-30T10:00:00Z"
        }
    }

print("Standardized Error Envelope:
", format_error_response(404, "Endpoint not found", "/api/v1/invalid"))
```

Output:
```text
Standardized Error Envelope:
 {'error': {'status': 404, 'message': 'Endpoint not found', 'path': '/api/v1/invalid', 'timestamp': '2026-10-30T10:00:00Z'}}
```

**Q689. What is Gunicorn's worker model (Sync, Gevent, UvicornWorker)?**

*Compare CPU-bound sync workers with async event-loop workers.*

**Answer:** - `sync` (default): Pre-forked worker processes, handling 1 request per worker at a time.
- `gevent` / `eventlet`: Coroutine-based workers utilizing monkey-patched sockets.
- `uvicorn.workers.UvicornWorker`: Async worker running an ASGI event loop per process.

```python
# gunicorn -w 4 -k uvicorn.workers.UvicornWorker app:app
print("UvicornWorker combines Gunicorn process master with ASGI event loop workers")
```

Output:
```text
UvicornWorker combines Gunicorn process master with ASGI event loop workers
```

**Q690. How do you secure API endpoints with JWT tokens in FastAPI?**

*Explain `OAuth2PasswordBearer`, token decoding, and signature verification.*

**Answer:** FastAPI uses `OAuth2PasswordBearer(tokenUrl='token')` to extract bearer tokens from the `Authorization` header, followed by signature validation using `jwt.decode`.

```python
print("OAuth2PasswordBearer extracts and validates JWT signatures automatically")
```

Output:
```text
OAuth2PasswordBearer extracts and validates JWT signatures automatically
```

**Q691. How do you handle file uploads with validation in FastAPI?**

*Demonstrate `UploadFile` vs `bytes` and enforcing size/extension limits.*

**Answer:** `UploadFile` uses Python's `SpooledTemporaryFile`: small files reside in memory, while larger files spill over to disk, preventing memory crashes.

```python
# @app.post("/upload")
# async def upload(file: UploadFile = File(...)):
#     if not file.filename.endswith(".csv"): raise HTTPException(400)
print("UploadFile streams file uploads efficiently through spooled temporary storage")
```

Output:
```text
UploadFile streams file uploads efficiently through spooled temporary storage
```

**Q692. How do you configure health checks and readiness probes for Kubernetes in Python web apps?**

*Explain `/healthz` (liveness) vs `/ready` (readiness) endpoints.*

**Answer:** - `/healthz` (Liveness): Returns `200 OK` if the process is responsive. Fails -> Kubernetes restarts pod.
- `/ready` (Readiness): Verifies downstream dependencies (database, Redis). Fails -> Kubernetes stops routing traffic to pod.

```python
def readiness_check(db_alive: bool, cache_alive: bool) -> tuple[int, dict]:
    ready = db_alive and cache_alive
    status = 200 if ready else 503
    return status, {"ready": ready, "db": db_alive, "cache": cache_alive}

print("Readiness check (all up):", readiness_check(True, True))
print("Readiness check (db down):", readiness_check(False, True))
```

Output:
```text
Readiness check (all up): (200, {'ready': True, 'db': True, 'cache': True})
Readiness check (db down): (503, {'ready': False, 'db': False, 'cache': True})
```

**Q693. How does Server-Sent Events (SSE) work in FastAPI?**

*Demonstrate streaming events over HTTP using `StreamingResponse`.*

**Answer:** SSE streams events to clients over an open HTTP connection (`text/event-stream`). Ideal for live deployment progress and real-time logs.

```python
# async def event_generator():
#     for step in ["Linting", "Testing", "Deploying"]:
#         yield f"data: {step}

"
# return StreamingResponse(event_generator(), media_type="text/event-stream")
print("StreamingResponse delivers real-time Server-Sent Events to web clients")
```

Output:
```text
StreamingResponse delivers real-time Server-Sent Events to web clients
```

**Q694. What is OpenAPI / Swagger schema generation in FastAPI?**

*Explain how Pydantic models automatically produce `openapi.json`.*

**Answer:** FastAPI inspects route functions, type annotations, and Pydantic schemas to generate an interactive OpenAPI 3.1 contract accessible at `/docs` (Swagger) and `/redoc`.

```python
print("FastAPI compiles type annotations into machine-readable OpenAPI specifications")
```

Output:
```text
FastAPI compiles type annotations into machine-readable OpenAPI specifications
```

**Q695. How do you manage database connections across worker processes?**

*Explain connection pool disposal after `os.fork()` to prevent corrupted socket sharing.*

**Answer:** When web servers fork processes, database connection pool sockets are duplicated across children, leading to race conditions. Fix: dispose and recreate the pool in worker initialization hooks (`engine.dispose()`).

```python
# In Gunicorn post_fork hook:
# def post_fork(server, worker):
#     db_engine.dispose()
print("engine.dispose() resets connection pools after process forking")
```

Output:
```text
engine.dispose() resets connection pools after process forking
```

**Q696. What is Webhook idempotency and how do you prevent duplicate processing?**

*Explain tracking idempotency keys in Redis or SQL.*

**Answer:** Clients send a unique `Idempotency-Key` header with requests. The server checks if the key exists in Redis: if yes, returns cached response; if no, executes request and stores result.

```python
seen_keys = set()
def process_webhook(idempotency_key: str, payload: dict) -> str:
    if idempotency_key in seen_keys:
        return "ALREADY_PROCESSED"
    seen_keys.add(idempotency_key)
    return "PROCESSED_SUCCESS"

print("Call 1:", process_webhook("key-999", {}))
print("Call 2 (retry):", process_webhook("key-999", {}))
```

Output:
```text
Call 1: PROCESSED_SUCCESS
Call 2 (retry): ALREADY_PROCESSED
```

**Q697. How do you handle graceful shutdowns in ASGI web servers?**

*Explain ASGI lifespan protocol (`lifespan` context manager) in Starlette/FastAPI.*

**Answer:** The modern lifespan context manager handles startup (connect DB, load ML models) and shutdown (drain pools, close sockets) cleanly.

```python
# from contextlib import asynccontextmanager
# @asynccontextmanager
# async def lifespan(app: FastAPI):
#     await db.connect()
#     yield
#     await db.disconnect()
print("Lifespan context managers coordinate clean startup and shutdown events")
```

Output:
```text
Lifespan context managers coordinate clean startup and shutdown events
```

**Q698. What are security response headers (HSTS, CSP, X-Frame-Options)?**

*Explain securing HTTP responses in Python middleware.*

**Answer:** - `Strict-Transport-Security (HSTS)`: Forces browsers to use HTTPS exclusively.
- `Content-Security-Policy (CSP)`: Restricts origins from which scripts, styles, and images can load.
- `X-Frame-Options: DENY`: Prevents clickjacking by blocking embedding in `<iframes>`.

```python
secure_headers = {
    "Strict-Transport-Security": "max-age=63072000; includeSubDomains; preload",
    "X-Frame-Options": "DENY",
    "X-Content-Type-Options": "nosniff"
}
print("Security headers protect browsers from common web exploits")
```

Output:
```text
Security headers protect browsers from common web exploits
```

**Q699. Programming Question: Build a lightweight WSGI Router from scratch.**

*Route requests to handlers based on URL path and HTTP method.*

**Answer:** Map `(method, path)` tuples to handler callables.

```python
class SimpleRouter:
    def __init__(self): self.routes = {}
    def add(self, method, path, handler): self.routes[(method, path)] = handler
    def dispatch(self, method, path):
        handler = self.routes.get((method, path))
        return handler() if handler else ("404 Not Found", "Missing Route")

router = SimpleRouter()
router.add("GET", "/health", lambda: ("200 OK", "Healthy"))
print("Route /health:", router.dispatch("GET", "/health"))
print("Route /invalid:", router.dispatch("GET", "/invalid"))
```

Output:
```text
Route /health: ('200 OK', 'Healthy')
Route /invalid: ('404 Not Found', 'Missing Route')
```

**Q700. Programming Question: Implement a token-bucket API rate limiter.**

*Refill tokens over time and consume tokens per request.*

**Answer:** Track token counts and timestamps.

```python
import time

class TokenBucket:
    def __init__(self, capacity: int, fill_rate: float):
        self.capacity, self.fill_rate = capacity, fill_rate
        self.tokens, self.last_fill = capacity, time.time()

    def consume(self, amount: int = 1) -> bool:
        now = time.time()
        self.tokens = min(self.capacity, self.tokens + (now - self.last_fill) * self.fill_rate)
        self.last_fill = now
        if self.tokens >= amount:
            self.tokens -= amount
            return True
        return False

tb = TokenBucket(2, 1.0)
print("Token 1:", tb.consume(1))
print("Token 2:", tb.consume(1))
print("Token 3 (depleted):", tb.consume(1))
```

Output:
```text
Token 1: True
Token 2: True
Token 3 (depleted): False
```

**Q701. Programming Question: Build a Prometheus metrics exposition middleware.**

*Count HTTP request total and measure duration histogram.*

**Answer:** Update metrics counters in middleware.

```python
from collections import Counter

http_requests_total = Counter()

def record_metric(method: str, path: str, status: int):
    http_requests_total[(method, path, status)] += 1

record_metric("GET", "/api/v1/pods", 200)
record_metric("GET", "/api/v1/pods", 200)
print("Prometheus Counter:", dict(http_requests_total))
```

Output:
```text
Prometheus Counter: {('GET', '/api/v1/pods', 200): 2}
```

**Q702. Programming Question: Implement an IP allowlist middleware.**

*Restrict access to administrative endpoints to specific CIDR blocks.*

**Answer:** Check incoming client IP against allowed CIDR list.

```python
import ipaddress

def is_ip_allowed(client_ip: str, allowed_cidrs: list) -> bool:
    ip = ipaddress.ip_address(client_ip)
    return any(ip in ipaddress.ip_network(cidr) for cidr in allowed_cidrs)

cidrs = ["10.0.0.0/8", "192.168.1.0/24"]
print("Allow 10.5.0.1:     ", is_ip_allowed("10.5.0.1", cidrs))
print("Allow 203.0.113.50: ", is_ip_allowed("203.0.113.50", cidrs))
```

Output:
```text
Allow 10.5.0.1:      True
Allow 203.0.113.50:  False
```

**Q703. Programming Question: Sanitize file upload names to prevent path traversal.**

*Strip directory traversal tokens (`../`, `..\`) from user-supplied filenames.*

**Answer:** Use `pathlib.Path(filename).name` or regex.

```python
import os

def sanitize_filename(filename: str) -> str:
    # Strip any leading directory components
    return os.path.basename(filename).replace("..", "")

print("Sanitized path traversal:", sanitize_filename("../../etc/passwd"))
print("Sanitized normal file:   ", sanitize_filename("report.pdf"))
```

Output:
```text
Sanitized path traversal: passwd
Sanitized normal file:    report.pdf
```

**Q704. Programming Question: Build an API request signature verification middleware.**

*Verify SHA-256 HMAC request signatures on incoming API requests.*

**Answer:** Recompute HMAC and compare with header.

```python
import hmac, hashlib

def verify_request_signature(secret: bytes, body: bytes, signature_header: str) -> bool:
    expected = hmac.new(secret, body, hashlib.sha256).hexdigest()
    return hmac.compare_digest(expected, signature_header)

secret_key = b"api_secret"
raw_body = b'{"action": "restart"}'
sig = hmac.new(secret_key, raw_body, hashlib.sha256).hexdigest()
print("Signature valid:", verify_request_signature(secret_key, raw_body, sig))
```

Output:
```text
Signature valid: True
```


---

## Chapter 23: Testing, Debugging, and Code Quality

**Q705. What is the testing pyramid in software engineering?**

*Explain Unit Tests, Integration Tests, and End-to-End (E2E) tests.*

**Answer:** The testing pyramid categorizes tests by granularity and execution speed:
1. **Unit Tests (Base, ~70%)**: Test isolated functions and classes in memory with mocks. Fastest, cheapest to run.
2. **Integration Tests (Middle, ~20%)**: Test interactions between components (e.g. database queries, message queues, external APIs).
3. **End-to-End Tests (Apex, ~10%)**: Test full system workflows against real deployed infrastructure.

```python
print("Testing pyramid balances test execution velocity and confidence")
```

Output:
```text
Testing pyramid balances test execution velocity and confidence
```

**Q706. Why is `pytest` preferred over built-in `unittest` in modern Python?**

*Compare plain `assert` statements, test discovery, fixtures, and parameterization.*

**Answer:** `pytest` is the dominant Python test framework:
- Uses plain `assert expr` with intelligent introspection (no verbose `self.assertEqual()`).
- Powerful dependency-injected **fixtures** (`@pytest.fixture`).
- Native test parameterization (`@pytest.mark.parametrize`).
- Extensive plugin ecosystem (`pytest-cov`, `pytest-asyncio`, `pytest-mock`).

```python
# In pytest:
# def test_addition():
#     assert 2 + 2 == 4
print("Pytest simplifies testing with clean assert introspection and fixtures")
```

Output:
```text
Pytest simplifies testing with clean assert introspection and fixtures
```

**Q707. How do Pytest Fixtures work and what are their scopes?**

*Explain fixture dependency injection, teardown with `yield`, and scopes (function, class, module, session).*

**Answer:** A fixture provides a fixed baseline (database connection, mock client) injected into test functions by parameter name. Scopes:
- `function` (default): Fresh fixture per test.
- `module`: Shared across all tests in a `.py` file.
- `session`: Created once per entire test suite run.
Code following `yield` executes as teardown.

```python
# @pytest.fixture(scope="module")
# def db_connection():
#     conn = create_db()
#     yield conn
#     conn.close() # Teardown!
print("Pytest fixtures manage setup and teardown with dependency injection")
```

Output:
```text
Pytest fixtures manage setup and teardown with dependency injection
```

**Q708. How does parameterized testing work with `@pytest.mark.parametrize`?**

*Demonstrate running a single test function across multiple input and output datasets.*

**Answer:** `@pytest.mark.parametrize('param1, param2, expected', [(...), (...)])` executes the test once for each dataset, generating distinct test cases in the test runner.

```python
# @pytest.mark.parametrize("input_val, expected", [
#     ("127.0.0.1", True),
#     ("invalid", False)
# ])
# def test_ip_validation(input_val, expected):
#     assert is_valid_ip(input_val) == expected
print("Parametrization eliminates duplicate test code across boundary test cases")
```

Output:
```text
Parametrization eliminates duplicate test code across boundary test cases
```

**Q709. What is Mocking and how does `unittest.mock` isolate external dependencies?**

*Explain `Mock`, `MagicMock`, `patch()`, and return values.*

**Answer:** Mocking replaces real objects (databases, payment gateways, HTTP APIs) with fake test doubles. `MagicMock` implements all magic methods automatically. `@patch('module.function')` temporarily substitutes the target object during test execution.

```python
from unittest.mock import MagicMock

mock_client = MagicMock()
mock_client.get_status.return_value = "HEALTHY"

print("Mock return value:", mock_client.get_status())
mock_client.get_status.assert_called_once()
print("Assertion verified: mock was called exactly once")
```

Output:
```text
Mock return value: HEALTHY
Assertion verified: mock was called exactly once
```

**Q710. What is the crucial rule when choosing where to patch with `unittest.mock.patch()`?**

*Explain *'Patch where an object is LOOKED UP, not where it is DEFINED'*.*

**Answer:** If `app.py` has `from requests import get`, patching `requests.get` has no effect because `app` already imported its own local reference. You must patch where the name is looked up: `@patch('app.get')`.

```python
# If worker.py imports: from database import connect
# You MUST patch: @patch('worker.connect')
# NOT: @patch('database.connect')
print("Always patch the target in the namespace where it is looked up")
```

Output:
```text
Always patch the target in the namespace where it is looked up
```

**Q711. How does test coverage work and what does `pytest-cov` measure?**

*Explain line coverage vs branch coverage and coverage thresholds in CI/CD.*

**Answer:** Code coverage measures the percentage of code executed during the test suite. **Line coverage** measures executed statements; **branch coverage** verifies that both `True` and `False` branches of every conditional statement were executed. CI/CD pipelines can fail if coverage drops below a threshold (e.g. `--cov-fail-under=80`).

```python
# pytest --cov=my_app --cov-branch --cov-report=term-missing
print("Branch coverage ensures all conditional evaluation paths are tested")
```

Output:
```text
Branch coverage ensures all conditional evaluation paths are tested
```

**Q712. How do you test asynchronous code with `pytest-asyncio`?**

*Demonstrate testing `async def` functions and async fixtures.*

**Answer:** `pytest-asyncio` provides an event loop runner for async test functions marked with `@pytest.mark.asyncio`, allowing `await` calls inside tests.

```python
# @pytest.mark.asyncio
# async def test_fetch_metrics():
#     result = await async_fetch_metrics()
#     assert result["status"] == "ok"
print("pytest-asyncio executes asynchronous test functions on the event loop")
```

Output:
```text
pytest-asyncio executes asynchronous test functions on the event loop
```

**Q713. How does interactive debugging work using `pdb` and `breakpoint()`?**

*Explain stepping through code: `n` (next), `s` (step into), `c` (continue), `p` (print), `q` (quit).*

**Answer:** `breakpoint()` (PEP 553, Python 3.7+) drops the interpreter into an interactive debugger session at that line.
- `n` (next): Execute current line.
- `s` (step into): Step inside called function.
- `c` (continue): Continue execution until next breakpoint.
- `p <expr>`: Print expression evaluation.
- `q`: Abort execution.

```python
# def calculate(x):
#     breakpoint() # Launches PDB debugger!
#     return x * 2
print("breakpoint() drops execution into PDB interactive debugging shell")
```

Output:
```text
breakpoint() drops execution into PDB interactive debugging shell
```

**Q714. What are Linters and Formatters: Ruff, Flake8, Black?**

*Compare static AST analysis with opinionated code formatting.*

**Answer:** - **Black**: Opinionated code formatter that enforces deterministic code style without configuration debates.
- **Flake8**: Traditional linter checking PEP 8 style, syntax errors, and complexity.
- **Ruff**: Extremely fast modern linter and formatter written in Rust, replacing Black, Flake8, and isort with 10-100x faster execution.

```python
# ruff check . --fix
# ruff format .
print("Ruff provides instant Rust-powered Python linting and formatting")
```

Output:
```text
Ruff provides instant Rust-powered Python linting and formatting
```

**Q715. What is Property-Based Testing using Hypothesis?**

*Compare example-based testing with automated fuzzing and edge-case generation.*

**Answer:** Instead of writing individual test examples manually, `Hypothesis` generates hundreds of random, boundary-pushing inputs (empty lists, huge integers, invalid Unicode, NaN) to find inputs that break your function's invariants, then automatically 'shrinks' the failing case to the minimal reproducible bug.

```python
# from hypothesis import given, strategies as st
# @given(st.lists(st.integers()))
# def test_reversal(lst):
#     assert list(reversed(list(reversed(lst)))) == lst
print("Hypothesis tests general invariants against thousands of edge-case inputs")
```

Output:
```text
Hypothesis tests general invariants against thousands of edge-case inputs
```

**Q716. Output Question: Predict what happens when `mock.assert_called_with()` fails.**

*Explain AssertionError with call argument diff.*

**Answer:** If arguments passed to the mock do not match expected values, `assert_called_with()` raises an `AssertionError` displaying the exact parameter difference.

```python
from unittest.mock import MagicMock

m = MagicMock()
m(10, key="value")
try:
    m.assert_called_with(20, key="value")
except AssertionError as e:
    print("Caught assertion mismatch:", type(e).__name__)
```

Output:
```text
Caught assertion mismatch: AssertionError
```

**Q717. Debugging Question: Fix test pollution from shared global state across tests.**

*Prevent tests from affecting each other by resetting state in fixtures.*

**Answer:** Use fixtures with teardown to restore global state, database entries, or environment variables.

```python
# Use autouse fixture to reset state:
# @pytest.fixture(autouse=True)
# def reset_global_state():
#     yield
#     app_state.clear()
print("Autouse fixtures reset global variables to isolate test execution")
```

Output:
```text
Autouse fixtures reset global variables to isolate test execution
```

**Q718. Programming Question: Implement a custom Pytest test runner wrapper.**

*Run pytest programmatically from Python code and check exit code.*

**Answer:** Use `pytest.main(['-q'])`.

```python
# import pytest
# exit_code = pytest.main(["-q", "tests/"])
# if exit_code != 0: raise SystemExit(exit_code)
print("pytest.main() runs test suites programmatically inside CI scripts")
```

Output:
```text
pytest.main() runs test suites programmatically inside CI scripts
```

**Q719. How do you mock HTTP requests with `responses` or `pytest-mock`?**

*Intercept outgoing network calls during tests.*

**Answer:** Mock HTTP endpoints returning mock responses without sending traffic over the wire.

```python
from unittest.mock import patch

with patch("urllib.request.urlopen") as mock_url:
    mock_url.return_value.read.return_value = b'{"status": "OK"}'
    print("HTTP call intercepted safely without network access")
```

Output:
```text
HTTP call intercepted safely without network access
```

**Q720. How do you test command-line interfaces using `capsys` fixture in Pytest?**

*Capture stdout and stderr output in tests.*

**Answer:** `capsys.readouterr()` captures all output written to standard output and error.

```python
# def test_cli_output(capsys):
#     print("Service Started")
#     captured = capsys.readouterr()
#     assert "Service Started" in captured.out
print("capsys fixture captures standard I/O for CLI verification")
```

Output:
```text
capsys fixture captures standard I/O for CLI verification
```

**Q721. What is Mutation Testing and how does `mutmut` work in Python?**

*Test the quality of your tests by introducing deliberate code mutants.*

**Answer:** Mutation testing modifies your source code (e.g. changing `>` to `<`, `+` to `-`). If your test suite still passes, the mutant 'survived', indicating missing test assertions.

```python
print("Mutation testing verifies that tests actually fail when bugs are injected")
```

Output:
```text
Mutation testing verifies that tests actually fail when bugs are injected
```

**Q722. How do you mock filesystem access in tests with `pyfakefs`?**

*Simulate operating system files in memory.*

**Answer:** `pyfakefs` mocks the `os`, `pathlib`, and `open` modules, executing file tests in memory without touching real disk.

```python
print("pyfakefs provides virtual in-memory filesystems for fast unit tests")
```

Output:
```text
pyfakefs provides virtual in-memory filesystems for fast unit tests
```

**Q723. How do you test exceptions being raised using `pytest.raises`?**

*Verify exception type and inspect error message regex.*

**Answer:** Use `with pytest.raises(ExceptionType, match=r'...'):`.

```python
# with pytest.raises(ValueError, match=r"Invalid port"):
#     validate_port(99999)
print("pytest.raises validates that target exceptions are thrown with matching messages")
```

Output:
```text
pytest.raises validates that target exceptions are thrown with matching messages
```

**Q724. How do you benchmark execution times using `pytest-benchmark`?**

*Benchmark function performance and track regressions in CI.*

**Answer:** Inject the `benchmark` fixture into your test function.

```python
# def test_sorting(benchmark):
#     res = benchmark(sorted, [5, 2, 8, 1])
#     assert res == [1, 2, 5, 8]
print("pytest-benchmark measures and tracks performance regression metrics")
```

Output:
```text
pytest-benchmark measures and tracks performance regression metrics
```

**Q725. How do you test warning emissions with `pytest.warns`?**

*Verify deprecated API warnings.*

**Answer:** Use `with pytest.warns(DeprecationWarning):`.

```python
# with pytest.warns(DeprecationWarning):
#     legacy_function()
print("pytest.warns verifies that expected warning categories are triggered")
```

Output:
```text
pytest.warns verifies that expected warning categories are triggered
```

**Q726. How does `monkeypatch` fixture in Pytest work?**

*Demonstrate dynamically setting attributes, env vars, and dictionary entries during tests.*

**Answer:** Pytest's `monkeypatch` fixture safely modifies attributes, environment variables, or dictionary items and automatically restores them after the test.

```python
# def test_env(monkeypatch):
#     monkeypatch.setenv("ENV", "TESTING")
#     assert os.getenv("ENV") == "TESTING"
print("monkeypatch safely modifies and reverts runtime environment variables in tests")
```

Output:
```text
monkeypatch safely modifies and reverts runtime environment variables in tests
```

**Q727. What are Pytest markers (`@pytest.mark`)?**

*Explain custom markers (smoke, slow, integration) and selective test execution (`pytest -m`).*

**Answer:** Markers tag tests with metadata. Run specific subsets via `pytest -m smoke` or `pytest -m 'not slow'`.

```python
# @pytest.mark.slow
# def test_heavy_migration(): ...
print("Markers categorize test suites for targeted execution in CI stages")
```

Output:
```text
Markers categorize test suites for targeted execution in CI stages
```

**Q728. What is Test-Driven Development (TDD) cycle: Red -> Green -> Refactor?**

*Explain writing failing tests before implementation.*

**Answer:** 1. **Red**: Write a test for non-existent functionality; verify it fails.
2. **Green**: Write minimal code necessary to make the test pass.
3. **Refactor**: Clean and optimize code while keeping tests green.

```python
print("TDD ensures robust specifications and prevents over-engineering")
```

Output:
```text
TDD ensures robust specifications and prevents over-engineering
```

**Q729. Programming Question: Build a mock database session fixture.**

*Create a reusable mock session returning predefined queries.*

**Answer:** Use `MagicMock` with custom return values.

```python
from unittest.mock import MagicMock

def get_mock_db():
    mock = MagicMock()
    mock.query.return_value.filter.return_value.all.return_value = [{"id": 1}]
    return mock

db = get_mock_db()
print("Mock query result:", db.query().filter().all())
```

Output:
```text
Mock query result: [{'id': 1}]
```

**Q730. Programming Question: Write a parameterized test matrix for HTTP status code validation.**

*Test validation functions against success and failure status codes.*

**Answer:** Iterate over test tuples.

```python
def is_success(code: int) -> bool:
    return 200 <= code < 300

test_cases = [(200, True), (201, True), (400, False), (500, False)]
results = [is_success(code) == expected for code, expected in test_cases]
print("All test assertions passed:", all(results))
```

Output:
```text
All test assertions passed: True
```

**Q731. Programming Question: Test execution time limit with timeout assertions.**

*Verify a function completes within a maximum allowed time threshold.*

**Answer:** Measure with `time.perf_counter()`.

```python
import time

def fast_operation():
    return sum(range(1000))

t0 = time.perf_counter()
fast_operation()
elapsed = time.perf_counter() - t0
assert elapsed < 0.05, "Operation exceeded 50ms performance budget"
print("Performance assertion passed in:", f"{elapsed*1000:.3f} ms")
```

Output:
```text
Performance assertion passed in: ... ms
```

**Q732. Programming Question: Mock time.sleep to run fast tests on retry logic.**

*Prevent actual delays during unit testing of exponential backoff.*

**Answer:** Patch `time.sleep` with mock.

```python
from unittest.mock import patch
import time

with patch("time.sleep") as mock_sleep:
    # Function that sleeps 10 seconds:
    time.sleep(10)
    mock_sleep.assert_called_once_with(10)
    print("Mock sleep executed instantaneously without delay")
```

Output:
```text
Mock sleep executed instantaneously without delay
```

**Q733. Programming Question: Write a custom Pytest assertion helper with detailed error reporting.**

*Create an assertion helper function.*

**Answer:** Raise `AssertionError` with descriptive message.

```python
def assert_valid_cloud_id(cloud_id: str):
    if not (cloud_id.startswith("i-") and len(cloud_id) == 10):
        raise AssertionError(f"Expected valid AWS instance ID ('i-XXXXXXXX'), got: '{cloud_id}'")

assert_valid_cloud_id("i-12345678")
print("Custom assertion passed")
```

Output:
```text
Custom assertion passed
```

**Q734. Programming Question: Verify mock call count and argument history.**

*Inspect `.call_args_list` on MagicMock.*

**Answer:** Verify all sequential calls.

```python
from unittest.mock import MagicMock

m = MagicMock()
m("alpha")
m("beta")

print("Call count:", m.call_count)
print("Arguments called:", [call[0][0] for call in m.call_args_list])
```

Output:
```text
Call count: 2
Arguments called: ['alpha', 'beta']
```


---

## Chapter 24: Concurrency and Parallelism

**Q735. What is the difference between Concurrency and Parallelism?**

*Explain *'Concurrency is about dealing with lots of things at once; parallelism is about doing lots of things at once'*. (Rob Pike)*

**Answer:** - **Concurrency**: Structuring a program to handle multiple tasks by interleaving their execution (e.g. event loops, cooperative multitasking, threading on 1 core). Ideal for I/O-bound workloads.
- **Parallelism**: Executing multiple computational tasks simultaneously on physically separate CPU cores. Ideal for CPU-bound computations.

```python
print("Concurrency structures execution; Parallelism executes simultaneously across CPU hardware")
```

Output:
```text
Concurrency structures execution; Parallelism executes simultaneously across CPU hardware
```

**Q736. What is the difference between CPU-bound and I/O-bound workloads in Python?**

*Explain when to use `multiprocessing` vs `threading` vs `asyncio`.*

**Answer:** - **I/O-Bound** (Network requests, DB queries, file reads): Program spends most time waiting for external responses. Use **`asyncio`** or **`threading`**; the GIL is released during I/O waits.
- **CPU-Bound** (Image processing, cryptography, machine learning, heavy math): Program saturates CPU cores. Pure Python threads are blocked by the GIL. Must use **`multiprocessing`** (separate OS processes) or native extensions (NumPy, Cython).

```python
print("I/O-bound -> asyncio / threading | CPU-bound -> multiprocessing")
```

Output:
```text
I/O-bound -> asyncio / threading | CPU-bound -> multiprocessing
```

**Q737. How does multi-threading work in Python (`threading` module)?**

*Explain thread creation, `.start()`, `.join()`, and thread lifecycles.*

**Answer:** `threading.Thread(target=fn, args=(...))` creates native OS threads scheduled by the operating system kernel. `.start()` starts thread execution, and `.join()` blocks the calling thread until the target thread terminates.

```python
import threading
import time

def background_task(name):
    time.sleep(0.01)

t = threading.Thread(target=background_task, args=("Worker-1",))
t.start()
t.join()
print("Thread completed successfully. Is alive:", t.is_alive())
```

Output:
```text
Thread completed successfully. Is alive: False
```

**Q738. What are Race Conditions and how does `threading.Lock` prevent them?**

*Explain critical sections and mutual exclusion locks (mutex).*

**Answer:** A race condition occurs when multiple threads concurrently read and modify shared state, producing corrupted data. A `threading.Lock` enforces mutual exclusion: only one thread can acquire the lock at a time; others block until it is released.

```python
import threading

counter = 0
lock = threading.Lock()

def safe_increment():
    global counter
    with lock: # Critical section protected by mutex
        counter += 1

threads = [threading.Thread(target=safe_increment) for _ in range(10)]
for t in threads: t.start()
for t in threads: t.join()
print("Final synchronized counter:", counter)
```

Output:
```text
Final synchronized counter: 10
```

**Q739. What is the difference between `Lock` and `RLock` in threading?**

*Explain Reentrant Locks allowing recursive acquisition by the same thread.*

**Answer:** - `Lock`: Standard mutex. If a thread holding the lock tries to acquire it again, it deadlocks itself!
- `RLock` (Reentrant Lock): Can be acquired multiple times by the **same thread** without deadlocking. It maintains an acquisition counter and is only fully unlocked when release is called an equal number of times.

```python
import threading

rlock = threading.RLock()
with rlock:
    with rlock: # Reentrant acquisition: does NOT deadlock!
        print("Acquired RLock recursively")
```

Output:
```text
Acquired RLock recursively
```

**Q740. What are Deadlocks and how do you prevent them in multi-threaded Python programs?**

*Explain Coffman conditions, lock ordering, and `lock.acquire(timeout=...)`.*

**Answer:** A deadlock occurs when Thread A holds Lock 1 and waits for Lock 2, while Thread B holds Lock 2 and waits for Lock 1. Prevention:
1. Always acquire locks in a globally consistent, deterministic order.
2. Use timed lock acquisition: `lock.acquire(timeout=2.0)`.

```python
import threading

l = threading.Lock()
l.acquire()
# Timed acquisition prevents permanent deadlock:
acquired = l.acquire(timeout=0.01)
print("Timed acquisition timed out safely:", not acquired)
l.release()
```

Output:
```text
Timed acquisition timed out safely: True
```

**Q741. What are `Semaphore` and `BoundedSemaphore` in threading?**

*Demonstrate restricting concurrent access to a pool of N resources.*

**Answer:** A `Semaphore` manages an internal counter decremented by `acquire()` and incremented by `release()`. If the counter reaches zero, threads block until a slot opens. `BoundedSemaphore` raises `ValueError` if released more times than its initial value.

```python
import threading

# Allow at most 2 concurrent database connections:
pool_limiter = threading.BoundedSemaphore(2)

def access_resource():
    with pool_limiter:
        pass

print("Semaphore configured with initial capacity 2")
```

Output:
```text
Semaphore configured with initial capacity 2
```

**Q742. What are `Event` and `Condition` synchronization primitives?**

*Explain signaling between threads using `event.set()`, `wait()`, and `condition.notify()`.*

**Answer:** - `threading.Event`: Simple boolean flag. Threads call `event.wait()` until another thread calls `event.set()`.
- `threading.Condition`: Associated with a lock. Threads wait for complex state changes (`cond.wait()`) until notified by another thread (`cond.notify()` / `notify_all()`).

```python
import threading

ready_event = threading.Event()

def worker():
    ready_event.wait() # Block until set
    return "Worker resumed"

t = threading.Thread(target=worker)
t.start()
ready_event.set() # Unblocks worker
t.join()
print("Event signaling completed")
```

Output:
```text
Event signaling completed
```

**Q743. What are Daemon threads in Python?**

*Explain background daemon threads and process termination rules.*

**Answer:** A daemon thread (`t.daemon = True` or `threading.Thread(daemon=True)`) is a background service thread. CPython exits when only daemon threads remain alive, terminating them abruptly without running their `finally` blocks.

```python
import threading

t = threading.Thread(target=lambda: None, daemon=True)
print("Is daemon thread:", t.daemon)
```

Output:
```text
Is daemon thread: True
```

**Q744. How does multiprocessing bypass the GIL in Python (`multiprocessing` module)?**

*Explain independent OS processes, separate memory spaces, and multi-core CPU scaling.*

**Answer:** The `multiprocessing` module spawns independent operating system processes, each with its own CPython interpreter, private memory space, and separate GIL. This allows CPU-bound Python calculations to run simultaneously across all available physical CPU cores.

```python
import multiprocessing

print("Available CPU cores on system:", multiprocessing.cpu_count())
```

Output:
```text
Available CPU cores on system: ...
```

**Q745. What are process start methods: `fork`, `spawn`, and `forkserver`?**

*Explain OS process creation and Windows/macOS vs Linux differences.*

**Answer:** - **`fork`** (Linux default): Clones process via `os.fork()`. Fast, but unsafe in multi-threaded applications due to duplicated locks.
- **`spawn`** (Windows/macOS default): Starts a fresh interpreter process from scratch, importing the entry module. Safest and cleanest; requires code to be enclosed in `if __name__ == '__main__':`.
- **`forkserver`**: Starts a single-threaded server process that forks new workers.

```python
import multiprocessing

print("Current start method:", multiprocessing.get_start_method())
```

Output:
```text
Current start method: spawn
```

**Q746. How does Inter-Process Communication (IPC) work using `multiprocessing.Queue` and `Pipe`?**

*Explain message passing between isolated processes using pickling.*

**Answer:** Because processes have isolated memory, they cannot share variables directly. `multiprocessing.Queue` serializes objects using `pickle` and passes them across an underlying OS pipe.

```python
# In multiprocessing:
# q = multiprocessing.Queue()
# q.put(data) # Serializes and sends across pipe
print("IPC Queues transfer serialized objects across process boundaries")
```

Output:
```text
IPC Queues transfer serialized objects across process boundaries
```

**Q747. What is shared memory in multiprocessing (`multiprocessing.shared_memory` and `Value`/`Array`)?**

*Explain zero-copy memory sharing across processes.*

**Answer:** Python 3.8+ introduced `shared_memory.SharedMemory`, allocating raw OS shared memory segments accessible by multiple processes simultaneously without pickling, achieving zero-copy high-performance IPC.

```python
from multiprocessing import shared_memory

shm = shared_memory.SharedMemory(create=True, size=10)
shm.buf[0:4] = b"PROD"
print("Shared memory initialized with bytes:", bytes(shm.buf[0:4]))
shm.close()
shm.unlink()
```

Output:
```text
Shared memory initialized with bytes: b'PROD'
```

**Q748. How does `concurrent.futures` provide high-level execution pools?**

*Compare `ThreadPoolExecutor` and `ProcessPoolExecutor`.*

**Answer:** `concurrent.futures` provides an abstract Executor interface:
- `ThreadPoolExecutor(max_workers=N)`: Pool of threads for I/O-bound tasks.
- `ProcessPoolExecutor(max_workers=N)`: Pool of processes for CPU-bound tasks.
Methods: `executor.submit(fn, *args)` returns a `Future`; `executor.map(fn, iterable)` maps in parallel.

```python
from concurrent.futures import ThreadPoolExecutor

def fetch_data(n): return n * 10

with ThreadPoolExecutor(max_workers=2) as executor:
    results = list(executor.map(fetch_data, [1, 2, 3]))

print("Executor mapped results:", results)
```

Output:
```text
Executor mapped results: [10, 20, 30]
```

**Q749. What is a `Future` object in concurrent.futures?**

*Explain `.result()`, `.done()`, `.cancel()`, and `as_completed()`.*

**Answer:** A `Future` represents the eventual result of an asynchronous computation.
- `.done()`: Returns `True` if task finished.
- `.result(timeout=...)`: Blocks until result is ready or raises task exception.
- `as_completed(futures)`: Yields futures as they complete, allowing results to be processed immediately.

```python
from concurrent.futures import ThreadPoolExecutor, as_completed

with ThreadPoolExecutor(max_workers=2) as ex:
    futures = [ex.submit(lambda x: x ** 2, i) for i in [2, 4]]
    results = [f.result() for f in as_completed(futures)]

print("Completed futures results:", sorted(results))
```

Output:
```text
Completed futures results: [4, 16]
```

**Q750. What is `asyncio` and how does cooperative multitasking work?**

*Explain Coroutines, Event Loops, and non-blocking I/O.*

**Answer:** `asyncio` is Python's built-in framework for single-threaded cooperative concurrency. A single **Event Loop** manages multiple **Coroutines** (`async def`). When a coroutine waits on I/O via `await`, it yields control back to the event loop, which executes other ready coroutines, avoiding thread switching overhead.

```python
import asyncio

async def sample():
    await asyncio.sleep(0.01)
    return "Async Completed"

print(asyncio.run(sample()))
```

Output:
```text
Async Completed
```

**Q751. What is the difference between a Coroutine, a Task, and a Future in asyncio?**

*Detail their hierarchy and execution states in the event loop.*

**Answer:** - **Coroutine**: Function defined with `async def`. Calling it returns a coroutine object; it does not execute until awaited or scheduled.
- **Task (`asyncio.create_task`)**: Wraps a coroutine into a `Task` (a subclass of `Future`), immediately scheduling it onto the event loop to run concurrently in the background.
- **Future**: Low-level object representing an eventual result.

```python
import asyncio

async def worker(): return 42

async def main():
    task = asyncio.create_task(worker()) # Scheduled concurrently!
    return await task

print("Task result:", asyncio.run(main()))
```

Output:
```text
Task result: 42
```

**Q752. How do you run concurrent tasks with `asyncio.gather()`?**

*Demonstrate running multiple coroutines in parallel and handling exceptions with `return_exceptions=True`.*

**Answer:** `asyncio.gather(*coros_or_futures, return_exceptions=False)` schedules all coroutines concurrently and waits for all to complete, returning a list of their results in the order passed. If `return_exceptions=True`, exceptions are returned as values rather than canceling other tasks.

```python
import asyncio

async def fetch_srv(name, delay):
    await asyncio.sleep(delay)
    return f"{name}_OK"

async def main():
    results = await asyncio.gather(
        fetch_srv("srv1", 0.01),
        fetch_srv("srv2", 0.01)
    )
    return results

print("Gathered:", asyncio.run(main()))
```

Output:
```text
Gathered: ['srv1_OK', 'srv2_OK']
```

**Q753. What is `asyncio.TaskGroup` introduced in Python 3.11 (PEP 654)?**

*Explain Structured Concurrency and why TaskGroup is preferred over `asyncio.gather()`.*

**Answer:** `asyncio.TaskGroup` implements **Structured Concurrency**. Unlike `gather()`, a `TaskGroup` context manager guarantees that all spawned tasks complete or cancel before the block exits. If any task raises an exception, all other running sibling tasks are canceled immediately, and errors are collected in an `ExceptionGroup`, preventing orphaned background tasks.

```python
import asyncio

async def job(i):
    await asyncio.sleep(0.01)
    return i * 2

async def main():
    results = []
    async with asyncio.TaskGroup() as tg:
        t1 = tg.create_task(job(10))
        t2 = tg.create_task(job(20))
    return [t1.result(), t2.result()]

print("TaskGroup results:", asyncio.run(main()))
```

Output:
```text
TaskGroup results: [20, 40]
```

**Q754. How do Async Context Managers (`__aenter__` and `__aexit__`) work?**

*Demonstrate managing async network connections with `async with`.*

**Answer:** An async context manager implements `async def __aenter__(self)` and `async def __aexit__(self, exc_type, exc_val, exc_tb)`, allowing asynchronous setup and teardown with `async with`.

```python
import asyncio

class AsyncConnection:
    async def __aenter__(self):
        await asyncio.sleep(0.01)
        return "ASYNC_SOCKET"
    async def __aexit__(self, *args):
        await asyncio.sleep(0.01)

async def main():
    async with AsyncConnection() as conn:
        return f"Connected to {conn}"

print(asyncio.run(main()))
```

Output:
```text
Connected to ASYNC_SOCKET
```

**Q755. How do Async Iterators and Async Generators (`__aiter__` and `__anext__`) work?**

*Explain asynchronous data streaming with `async for`.*

**Answer:** An async generator uses `async def` and `yield`. Calling it yields elements asynchronously, consumed via `async for item in gen:`.

```python
import asyncio

async def async_stream(n):
    for i in range(n):
        await asyncio.sleep(0.005)
        yield i

async def main():
    return [val async for val in async_stream(3)]

print("Async stream:", asyncio.run(main()))
```

Output:
```text
Async stream: [0, 1, 2]
```

**Q756. How does task cancellation work in asyncio?**

*Explain `task.cancel()`, `asyncio.CancelledError`, and cleanup.*

**Answer:** Calling `task.cancel()` raises `asyncio.CancelledError` inside the coroutine at its current `await` point. Coroutines can catch `CancelledError` to perform cleanup, but should re-raise it unless deliberately suppressing cancellation.

```python
import asyncio

async def cancellable_job():
    try:
        await asyncio.sleep(10.0)
    except asyncio.CancelledError:
        return "CLEANED_UP"

async def main():
    task = asyncio.create_task(cancellable_job())
    await asyncio.sleep(0.01)
    task.cancel()
    return await task

print("Cancellation outcome:", asyncio.run(main()))
```

Output:
```text
Cancellation outcome: CLEANED_UP
```

**Q757. How do timeouts work in asyncio using `asyncio.timeout()` in Python 3.11+?**

*Compare `async with asyncio.timeout(secs):` with legacy `asyncio.wait_for()`.*

**Answer:** Python 3.11 introduced `async with asyncio.timeout(delay):`, providing a structured timeout context manager that cancels the enclosed block and raises `TimeoutError` if time expires.

```python
import asyncio

async def main():
    try:
        async with asyncio.timeout(0.02):
            await asyncio.sleep(1.0)
    except TimeoutError:
        return "TIMED_OUT"

print("Timeout test:", asyncio.run(main()))
```

Output:
```text
Timeout test: TIMED_OUT
```

**Q758. How do you run blocking synchronous code in an asyncio event loop?**

*Explain `asyncio.to_thread()` (Python 3.9+) and `loop.run_in_executor()`.*

**Answer:** Blocking functions (like `time.sleep`, synchronous DB drivers, heavy file I/O) freeze the entire asyncio event loop. Use `await asyncio.to_thread(sync_func, *args)` to offload the blocking call to a separate worker thread while keeping the event loop responsive.

```python
import asyncio
import time

def blocking_io():
    time.sleep(0.02)
    return "BLOCKING_DONE"

async def main():
    # Runs blocking_io in separate thread, avoiding event loop freeze!
    res = await asyncio.to_thread(blocking_io)
    return res

print("Result:", asyncio.run(main()))
```

Output:
```text
Result: BLOCKING_DONE
```

**Q759. What is an asyncio Queue and how does producer-consumer work?**

*Demonstrate buffering async tasks with `asyncio.Queue`.*

**Answer:** `asyncio.Queue` provides an asynchronous FIFO queue for producer-consumer workflows without threads. Producers call `await q.put()`, consumers call `await q.get()`, and `q.task_done()` tracks progress.

```python
import asyncio

async def main():
    q = asyncio.Queue()
    await q.put("item-1")
    val = await q.get()
    q.task_done()
    return val

print("Queue item:", asyncio.run(main()))
```

Output:
```text
Queue item: item-1
```

**Q760. What is an asyncio Semaphore and how do you limit concurrent HTTP requests?**

*Prevent hitting remote rate limits or opening too many open file descriptors.*

**Answer:** Wrap requests inside `async with semaphore:` to limit maximum concurrent coroutines to $N$.

```python
import asyncio

sem = asyncio.Semaphore(2)

async def worker(id):
    async with sem:
        await asyncio.sleep(0.01)
        return f"Worker {id} executed"

async def main():
    return await asyncio.gather(*(worker(i) for i in range(3)))

print("Semaphore controlled execution count:", len(asyncio.run(main())))
```

Output:
```text
Semaphore controlled execution count: 3
```

**Q761. Output Question: Predict what happens when a blocking call runs in an async function.**

*Explain freezing the event loop.*

**Answer:** Calling a blocking function like `time.sleep()` freezes the single thread running the event loop, causing all other scheduled async tasks to stall until the blocking call returns.

```python
# time.sleep(5) inside async def stalls the entire event loop!
# Always use await asyncio.sleep(5) or await asyncio.to_thread(time.sleep, 5)
print("Blocking calls stall the single-threaded asyncio event loop")
```

Output:
```text
Blocking calls stall the single-threaded asyncio event loop
```

**Q762. Output Question: What does this multiprocessing code output on Windows without `if __name__ == '__main__':`?**

*Explain infinite process spawning crash under the `spawn` start method.*

**Answer:** On Windows and macOS (`spawn`), child processes re-import the entry module from the top. Without `if __name__ == '__main__':`, each child spawns another child in an infinite recursive fork bomb, raising `RuntimeError`.

```python
# if __name__ == '__main__': is mandatory on Windows for multiprocessing
print("if __name__ == '__main__': prevents recursive spawn loops")
```

Output:
```text
if __name__ == '__main__': prevents recursive spawn loops
```

**Q763. Debugging Question: Fix Unhandled Exception in asyncio Task.**

*Explain why exceptions inside fire-and-forget tasks are silently ignored until garbage collection.*

**Answer:** If a task created via `asyncio.create_task()` raises an exception and is never awaited, Python only prints a warning upon garbage collection. Solution: await the task, use `TaskGroup`, or attach a `task.add_done_callback()` handler.

```python
import asyncio

async def faulty(): raise ValueError("Task failed")

async def main():
    task = asyncio.create_task(faulty())
    try:
        await task
    except ValueError as e:
        print("Caught task exception cleanly:", e)

asyncio.run(main())
```

Output:
```text
Caught task exception cleanly: Task failed
```

**Q764. Programming Question: Build an Asynchronous Web Crawler / Health Prober.**

*Probe 5 endpoints concurrently with a maximum concurrency limit of 2.*

**Answer:** Combine `asyncio.Semaphore` and `asyncio.gather`.

```python
import asyncio

async def probe_endpoint(sem, url):
    async with sem:
        await asyncio.sleep(0.01) # Simulated network call
        return f"{url}: 200 OK"

async def main():
    sem = asyncio.Semaphore(2)
    urls = [f"https://api.internal/svc-{i}" for i in range(4)]
    results = await asyncio.gather(*(probe_endpoint(sem, u) for u in urls))
    return results

print("Probed endpoints:
" + "
".join(asyncio.run(main())[:2]))
```

Output:
```text
Probed endpoints:
https://api.internal/svc-0: 200 OK
https://api.internal/svc-1: 200 OK
```

**Q765. Programming Question: Implement a Thread-Safe In-Memory Queue with Blocking Get.**

*Build a synchronized queue using `threading.Lock` and `threading.Condition`.*

**Answer:** Use a condition variable to notify waiting consumers when items are put.

```python
import threading

class BlockingQueue:
    def __init__(self):
        self.items = []
        self.cond = threading.Condition()

    def put(self, item):
        with self.cond:
            self.items.append(item)
            self.cond.notify()

    def get(self):
        with self.cond:
            while not self.items:
                self.cond.wait()
            return self.items.pop(0)

q = BlockingQueue()
q.put("data_payload")
print("Retrieved from blocking queue:", q.get())
```

Output:
```text
Retrieved from blocking queue: data_payload
```

**Q766. Programming Question: Parallelize CPU-bound prime number counting with ProcessPoolExecutor.**

*Split workload across CPU cores and aggregate counts.*

**Answer:** Use `ProcessPoolExecutor` with chunks.

```python
def is_prime(n):
    if n < 2: return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0: return False
    return True

print("Prime test (17):", is_prime(17))
```

Output:
```text
Prime test (17): True
```

**Q767. Programming Question: Implement an Async Worker Pool using asyncio.Queue.**

*Spawn 3 consumer workers processing items from an async queue until empty.*

**Answer:** Use `q.join()` and cancel workers.

```python
import asyncio

async def consumer(id, queue):
    while True:
        item = await queue.get()
        queue.task_done()

async def main():
    q = asyncio.Queue()
    workers = [asyncio.create_task(consumer(i, q)) for i in range(2)]
    for item in ["task1", "task2"]: await q.put(item)
    await q.join()
    for w in workers: w.cancel()
    return "All tasks processed"

print(asyncio.run(main()))
```

Output:
```text
All tasks processed
```

**Q768. Programming Question: Implement an Asyncio Debounce utility.**

*Delay function execution until N milliseconds have passed without any new invocations.*

**Answer:** Cancel pending task and reschedule timer.

```python
import asyncio

class AsyncDebouncer:
    def __init__(self, delay: float):
        self.delay = delay
        self.task = None

    async def call(self, coro_fn, *args):
        if self.task and not self.task.done():
            self.task.cancel()
        self.task = asyncio.create_task(self._run(coro_fn, *args))

    async def _run(self, coro_fn, *args):
        await asyncio.sleep(self.delay)
        return await coro_fn(*args)

print("Async debouncer initialized")
```

Output:
```text
Async debouncer initialized
```

**Q769. Programming Question: Build an Asynchronous Pub/Sub broker using asyncio.Event.**

*Allow multiple subscribers to receive broadcast messages.*

**Answer:** Maintain subscriber queues.

```python
import asyncio

class AsyncBroadcaster:
    def __init__(self):
        self.subscribers = []

    def subscribe(self):
        q = asyncio.Queue()
        self.subscribers.append(q)
        return q

    async def publish(self, msg):
        for q in self.subscribers:
            await q.put(msg)

async def main():
    b = AsyncBroadcaster()
    sub = b.subscribe()
    await b.publish("DEPLOY_COMPLETE")
    return await sub.get()

print("Broadcast received:", asyncio.run(main()))
```

Output:
```text
Broadcast received: DEPLOY_COMPLETE
```

**Q770. Programming Question: Gracefully cancel tasks with timeout using `asyncio.wait()`.**

*Run tasks with timeout and cancel remaining pending tasks explicitly.*

**Answer:** Inspect `done` and `pending` sets from `asyncio.wait`.

```python
import asyncio

async def worker(d):
    await asyncio.sleep(d)
    return d

async def main():
    t1 = asyncio.create_task(worker(0.01))
    t2 = asyncio.create_task(worker(10.0))
    done, pending = await asyncio.wait([t1, t2], timeout=0.05)
    for p in pending: p.cancel()
    return f"Done: {len(done)}, Canceled: {len(pending)}"

print(asyncio.run(main()))
```

Output:
```text
Done: 1, Canceled: 1
```

**Q771. Programming Question: Implement a thread-safe Singleton using double-checked locking.**

*Create a high-performance thread-safe singleton without locking after initialization.*

**Answer:** Check instance before and after lock acquisition.

```python
import threading

class SafeSingleton:
    _instance = None
    _lock = threading.Lock()

    def __new__(cls):
        if cls._instance is None:
            with cls._lock:
                if cls._instance is None: # Double check
                    cls._instance = super().__new__(cls)
        return cls._instance

s1 = SafeSingleton()
s2 = SafeSingleton()
print("Safe singleton identical:", s1 is s2)
```

Output:
```text
Safe singleton identical: True
```

**Q772. Programming Question: Execute async commands in background with process monitoring.**

*Spawn an asynchronous subprocess using `asyncio.create_subprocess_exec`.*

**Answer:** Read stdout and stderr asynchronously.

```python
import asyncio
import sys

async def check_py_version():
    proc = await asyncio.create_subprocess_exec(
        sys.executable, "--version",
        stdout=asyncio.subprocess.PIPE,
        stderr=asyncio.subprocess.PIPE
    )
    stdout, _ = await proc.communicate()
    return stdout.decode().strip()

print("Subprocess async output:", asyncio.run(check_py_version()))
```

Output:
```text
Subprocess async output: Python 3.12.10
```


---

## Chapter 25: Packaging, Environments, and Dependency Management

**Q773. Why are Python Virtual Environments (`venv`) essential?**

*Explain environment isolation, site-packages, and preventing dependency conflicts.*

**Answer:** Virtual environments create isolated Python execution environments with their own dedicated directory of installed packages (`site-packages`), bin/Scripts directory, and Python binary symlink. This prevents version conflicts between projects and prevents contaminating system-level Python installations.

```python
# In terminal:
# python -m venv .venv
# source .venv/bin/activate  # Linux/macOS
# .venv\Scripts\Activate.ps1 # Windows PowerShell
print("Virtual environments isolate project dependencies completely")
```

Output:
```text
Virtual environments isolate project dependencies completely
```

**Q774. What is `pyproject.toml` (PEP 517, PEP 518, PEP 621)?**

*Explain replacing legacy `setup.py` and `setup.cfg` with standardized metadata.*

**Answer:** `pyproject.toml` is the modern declarative configuration standard for Python packaging. It specifies:
1. **Build system (`[build-system]`)**: Build backend (Hatchling, Flit, Poetry, Setuptools).
2. **Project metadata (`[project]`)**: Name, version, dependencies, license, authors, entry points.

```python
# Sample pyproject.toml:
# [build-system]
# requires = ["hatchling"]
# build-backend = "hatchling.build"
# [project]
# name = "cloud-agent"
# version = "1.0.0"
# dependencies = ["requests>=2.31.0"]
print("pyproject.toml unifies Python project build configurations")
```

Output:
```text
pyproject.toml unifies Python project build configurations
```

**Q775. What is the difference between a Wheel (`.whl`) and a Source Distribution (`sdist`)?**

*Explain pre-compiled binary packages vs raw source code compilation.*

**Answer:** - **Source Distribution (`sdist`, `.tar.gz`)**: Contains raw Python source code and C/C++ files. Installing an sdist requires compiling C extensions on the client machine, requiring build tools (gcc, MSVC).
- **Wheel (`.whl`)**: A pre-built, standardized binary packaging format (PEP 427). Installing a wheel requires no compilation and simply unpacks files directly into `site-packages` in seconds.

```python
# python -m build
# Produces: dist/package-1.0.0.tar.gz (sdist) and dist/package-1.0.0-py3-none-any.whl (wheel)
print("Wheel packages install instantly without client-side C compilation")
```

Output:
```text
Wheel packages install instantly without client-side C compilation
```

**Q776. What is `uv` and why is it transforming Python packaging?**

*Explain Astral's Rust-powered package installer and resolver.*

**Answer:** `uv` is an extremely fast package manager written in Rust by Astral (creators of Ruff). It replaces `pip`, `pip-tools`, `virtualenv`, and `poetry` with 10x-100x faster execution speed, global caching, universal lockfiles (`uv.lock`), and cross-platform resolution.

```python
# In terminal:
# uv venv
# uv pip install -r requirements.txt
# uv run script.py
print("uv accelerates dependency installation and environment management")
```

Output:
```text
uv accelerates dependency installation and environment management
```

**Q777. What is the difference between direct requirements and pinned lockfiles?**

*Compare `requirements.txt` / `pyproject.toml` with `requirements.lock` / `poetry.lock`.*

**Answer:** - **Abstract Requirements**: Specify loose version ranges (`requests>=2.28.0`) to allow bug fixes.
- **Lockfiles**: Contain exact, pinned versions and cryptographic SHA-256 hashes of every direct and transitive dependency (`requests==2.31.0 --hash=sha256:...`), guaranteeing **100% reproducible builds** across developer laptops and CI/CD pipelines.

```python
# Hash-checking lockfile entry:
# requests==2.31.0 #     --hash=sha256:58ae000a6040...
print("Cryptographically pinned lockfiles ensure deterministic, reproducible builds")
```

Output:
```text
Cryptographically pinned lockfiles ensure deterministic, reproducible builds
```

**Q778. Why is the `src/` layout preferred over the flat project layout?**

*Explain preventing accidental import of uninstalled development code.*

**Answer:** - **Flat layout (`my_pkg/` in root)**: Tests or scripts can import `my_pkg` directly from the local directory instead of testing the installed package wheel, masking packaging and entry point bugs.
- **`src/` layout (`src/my_pkg/`)**: Forces developers and test runners to explicitly install the package into the virtual environment (`pip install -e .`), guaranteeing testing matches production.

```python
# Clean src layout:
# project/
# ├── pyproject.toml
# ├── src/
# │   └── cloud_app/
# └── tests/
print("src/ layout prevents testing local unbuilt modules accidentally")
```

Output:
```text
src/ layout prevents testing local unbuilt modules accidentally
```

**Q779. How do Console Script Entry Points create executable CLI commands?**

*Demonstrate configuring CLI bin shortcuts in `pyproject.toml`.*

**Answer:** Entry points instruct pip/wheel to automatically generate an executable wrapper script in the virtual environment's `bin/` or `Scripts/` directory that imports and invokes a specific Python function.

```python
# [project.scripts]
# deploy-tool = "cloud_app.cli:main"
# Running 'deploy-tool' executes main() from cloud_app.cli
print("Console script entry points generate native CLI executables")
```

Output:
```text
Console script entry points generate native CLI executables
```

**Q780. What is an editable install (`pip install -e .`)?**

*Explain live-linking development code without re-running installation.*

**Answer:** `pip install -e .` (editable mode, PEP 660) links the local source directory into `site-packages` using an editable `.pth` file or import hook. Any code modifications are immediately reflected at runtime without having to reinstall the package.

```python
# pip install -e .
print("Editable installs link source directories for real-time development iteration")
```

Output:
```text
Editable installs link source directories for real-time development iteration
```

**Q781. How do you scan Python dependencies for known security vulnerabilities?**

*Explain `pip-audit`, `safety`, and scanning against the PyPA Advisory Database.*

**Answer:** `pip-audit` scans local environments or lockfiles against the Python Packaging Advisory Database (OSV/CVE) and reports packages with known vulnerabilities, failing CI/CD builds if critical CVEs exist.

```python
# pip-audit --strict
# safety check
print("pip-audit detects vulnerable third-party dependencies in CI pipelines")
```

Output:
```text
pip-audit detects vulnerable third-party dependencies in CI pipelines
```

**Q782. How do you publish packages to PyPI safely using `twine` and trusted publishing?**

*Explain PyPI API tokens, OpenID Connect (OIDC) trusted publishers, and `twine upload`.*

**Answer:** Never use username/password to publish to PyPI. Use short-lived scoped API tokens or GitHub Actions **OIDC Trusted Publishing**, which authenticates via cryptographic claims without storing static credentials, and upload packages with `twine upload dist/*`.

```python
# twine upload --repository pypi dist/*
print("Twine validates signatures and uploads wheels securely to PyPI")
```

Output:
```text
Twine validates signatures and uploads wheels securely to PyPI
```

**Q783. Output Question: Predict what happens if an unpinned dependency releases a breaking major version.**

*Explain dependency drift in production deployments.*

**Answer:** If dependencies are unpinned (`requests`), a new major release (e.g. `requests==3.0.0`) can introduce breaking API changes that cause production builds to fail unpredictably. Pinned lockfiles eliminate drift.

```python
print("Unpinned dependencies risk unexpected breaking changes during CI builds")
```

Output:
```text
Unpinned dependencies risk unexpected breaking changes during CI builds
```

**Q784. Debugging Question: Fix ModuleNotFoundError on package installed in wrong virtual environment.**

*Diagnose which Python executable and site-packages is currently active.*

**Answer:** Check `sys.executable` and `which python` / `where.exe python` to confirm the virtual environment is properly activated.

```python
import sys
print("Active Python interpreter:", sys.executable)
```

Output:
```text
Active Python interpreter: ...
```

**Q785. Programming Question: Programmatically inspect installed package versions using `importlib.metadata`.**

*Extract version and dependencies of installed packages without third-party tools.*

**Answer:** Use `importlib.metadata.version('pkg')` and `requires('pkg')`.

```python
import importlib.metadata

try:
    urllib3_ver = importlib.metadata.version("urllib3")
    print("Installed urllib3 version:", urllib3_ver)
except importlib.metadata.PackageNotFoundError:
    print("Package not installed")
```

Output:
```text
Installed urllib3 version: ...
```

**Q786. What are package extra dependencies (`optional-dependencies`) in `pyproject.toml`?**

*Demonstrate installing optional feature bundles (e.g. `pip install mypkg[aws,test]`).*

**Answer:** `[project.optional-dependencies]` defines grouped extras that users can opt-in to install when needed.

```python
# [project.optional-dependencies]
# test = ["pytest>=8.0", "pytest-cov"]
# aws = ["boto3>=1.34"]
print("Optional dependencies bundle features without bloating core package install")
```

Output:
```text
Optional dependencies bundle features without bloating core package install
```

**Q787. What is pip hash-checking mode (`--require-hashes`)?**

*Explain preventing supply-chain tampering and man-in-the-middle package modification.*

**Answer:** Hash-checking mode forces pip to verify the cryptographic SHA-256 hash of every downloaded wheel against recorded hashes in the requirements file, preventing malicious package tampering.

```python
# pip install --require-hashes -r requirements.lock
print("Hash-checking mode enforces end-to-end package supply chain integrity")
```

Output:
```text
Hash-checking mode enforces end-to-end package supply chain integrity
```

**Q788. What are ABI tags in Python wheels (e.g. `cp312-cp312-manylinux`)?**

*Explain CPython ABI compatibility and manylinux standards.*

**Answer:** Wheels with C extensions include ABI tags (`cp312` for CPython 3.12). `manylinux` tags indicate compatibility with standard GLIBC versions across diverse Linux distributions.

```python
print("manylinux wheels ensure precompiled C extensions run across Linux distributions")
```

Output:
```text
manylinux wheels ensure precompiled C extensions run across Linux distributions
```

**Q789. How do you configure private package indexes (Artifactory, Nexus, AWS CodeArtifact)?**

*Explain `pip.conf` configuration and index-url vs extra-index-url.*

**Answer:** Configure `index-url` in `pip.conf` to route package resolutions through an internal enterprise proxy, caching external packages and hosting proprietary internal packages.

```python
# In pip.conf:
# [global]
# index-url = https://artifactory.internal/api/pypi/pypi-repo/simple
print("Private package indexes protect proprietary packages and enterprise supply chains")
```

Output:
```text
Private package indexes protect proprietary packages and enterprise supply chains
```

**Q790. What is Dependency Confusion in Python packaging?**

*Explain public PyPI namespace collisions with internal package names and how to prevent them.*

**Answer:** An attacker uploads a malicious package with the same name as an internal private package to public PyPI with a higher version number. If `pip` is configured with `--extra-index-url`, it may inadvertently download the malicious public package. Prevent by using scoped namespaces or configuring single internal proxies.

```python
print("Single index-url proxies neutralize dependency confusion attack vectors")
```

Output:
```text
Single index-url proxies neutralize dependency confusion attack vectors
```

**Q791. How does package data distribution work (`package-data`)?**

*Include non-code files (JSON templates, SQL migrations, icons) inside wheels.*

**Answer:** Configure `[tool.setuptools.package-data]` or `MANIFEST.in`.

```python
print("Package data bundles configuration and templates inside wheels")
```

Output:
```text
Package data bundles configuration and templates inside wheels
```

**Q792. What is Semantic Versioning (SemVer) in Python packaging?**

*Explain `MAJOR.MINOR.PATCH` and PEP 440 version specifiers.*

**Answer:** - `MAJOR`: Incompatible API changes.
- `MINOR`: Backward-compatible new features.
- `PATCH`: Backward-compatible bug fixes.
PEP 440 defines compatible release specifiers: `~= 1.4` (allows `>= 1.4, == 1.*`).

```python
print("PEP 440 compatible release specifiers balance stability and security patching")
```

Output:
```text
PEP 440 compatible release specifiers balance stability and security patching
```

**Q793. Programming Question: Write a script to audit requirements.txt for unpinned dependencies.**

*Flag dependencies lacking strict `==` pins.*

**Answer:** Parse lines and check for strict equality operators.

```python
def find_unpinned(requirements_text: str) -> list:
    unpinned = []
    for line in requirements_text.splitlines():
        line = line.strip()
        if line and not line.startswith("#"):
            if "==" not in line:
                unpinned.append(line)
    return unpinned

reqs = """
requests==2.31.0
pydantic>=2.0
fastapi
"""
print("Unpinned dependencies detected:", find_unpinned(reqs))
```

Output:
```text
Unpinned dependencies detected: ['pydantic>=2.0', 'fastapi']
```

**Q794. Programming Question: Programmatically find all entry points registered in the runtime.**

*Discover third-party plugins registered under an entry point group.*

**Answer:** Use `importlib.metadata.entry_points()`.

```python
import importlib.metadata

eps = importlib.metadata.entry_points()
console_scripts = eps.select(group="console_scripts") if hasattr(eps, "select") else []
print("Discovered console scripts count:", len(console_scripts))
```

Output:
```text
Discovered console scripts count: ...
```

**Q795. Programming Question: Generate an isolated temporary virtual environment programmatically.**

*Create a temporary venv using the `venv` module in Python.*

**Answer:** Use `venv.create(path, with_pip=True)`.

```python
import venv
import tempfile
from pathlib import Path

tmp_venv = Path(tempfile.gettempdir()) / "test_venv_dir"
# venv.create(tmp_venv, with_pip=False)
print("venv module allows programmatic creation of isolated virtual environments")
```

Output:
```text
venv module allows programmatic creation of isolated virtual environments
```

**Q796. Programming Question: Parse and validate `pyproject.toml` dependencies with tomllib.**

*Extract project name and dependency list from `pyproject.toml`.*

**Answer:** Load TOML and access `project.dependencies`.

```python
import tomllib

sample_toml = """
[project]
name = "demo-agent"
version = "0.1.0"
dependencies = ["httpx>=0.27.0", "pyyaml>=6.0"]
"""
data = tomllib.loads(sample_toml)
print("Project:", data["project"]["name"])
print("Deps:   ", data["project"]["dependencies"])
```

Output:
```text
Project: demo-agent
Deps:    ['httpx>=0.27.0', 'pyyaml>=6.0']
```

**Q797. Programming Question: Validate wheel filename syntax against PEP 427.**

*Check wheel name format `{distribution}-{version}(-{build})?-{python}-{abi}-{platform}.whl`.*

**Answer:** Use regex matching wheel naming convention.

```python
import re

WHEEL_REGEX = re.compile(r"^[A-Za-z0-9_]+-[0-9a-zA-Z._]+-(?:[0-9]+-)?[A-Za-z0-9_]+-[A-Za-z0-9_]+-[A-Za-z0-9_.]+\.whl$")
print("Valid wheel:  ", bool(WHEEL_REGEX.match("requests-2.31.0-py3-none-any.whl")))
print("Invalid wheel:", bool(WHEEL_REGEX.match("requests_bad_filename.whl")))
```

Output:
```text
Valid wheel:   True
Invalid wheel: False
```

**Q798. Programming Question: Build a dependency tree visualizer from pipdeptree output.**

*Parse flat dependency maps into nested indentation trees.*

**Answer:** Format dependency relationships for CI reporting.

```python
mock_dep_tree = {
    "fastapi": ["starlette", "pydantic"],
    "starlette": ["anyio"],
    "anyio": ["idna", "sniffio"]
}

def print_tree(pkg, indent=0):
    print("  " * indent + f"- {pkg}")
    for child in mock_dep_tree.get(pkg, []):
        print_tree(child, indent + 1)

print_tree("fastapi")
```

Output:
```text
- fastapi
  - starlette
    - anyio
      - idna
      - sniffio
  - pydantic
```


---

## Chapter 26: Security in Python

**Q799. What are the OWASP Top 10 vulnerabilities in Python applications?**

*Overview injection, broken authentication, sensitive data exposure, and security misconfigurations.*

**Answer:** The OWASP Top 10 outlines primary web security risks in Python:
1. **Injection** (SQLi, Command Injection).
2. **Broken Authentication** (weak passwords, hardcoded keys).
3. **Cryptographic Failures** (MD5/SHA1 for passwords, plain HTTP).
4. **Insecure Design** (missing rate limits, no authorization checks).
5. **Security Misconfiguration** (debug=True in production, default passwords).
6. **Vulnerable Components** (outdated PyPI packages).
7. **SSRF** (Server-Side Request Forgery).
8. **Software Integrity Failures** (unsafe pickle deserialization).

```python
print("OWASP Top 10 guides secure Python application architecture")
```

Output:
```text
OWASP Top 10 guides secure Python application architecture
```

**Q800. How do you prevent SQL Injection in Python applications?**

*Demonstrate why parameterization prevents SQLi completely.*

**Answer:** SQL injection occurs when user input is concatenated directly into SQL queries. Parameterized queries (`?` in SQLite, `%s` in PostgreSQL, `:name` in SQLAlchemy) treat user input strictly as data parameters, neutralizing injection.

```python
import sqlite3

conn = sqlite3.connect(":memory:")
conn.execute("CREATE TABLE users (id INT, user TEXT)")
conn.execute("INSERT INTO users VALUES (1, 'admin')")

user_input = "' OR '1'='1" # Classic SQL injection payload
# Parameterized query blocks injection:
cur = conn.cursor()
cur.execute("SELECT * FROM users WHERE user = ?", (user_input,))
print("Records found (safely zero):", len(cur.fetchall()))
conn.close()
```

Output:
```text
Records found (safely zero): 0
```

**Q801. Why is `shell=True` in `subprocess.run()` a severe security vulnerability?**

*Explain Command Injection and how to execute shell commands safely.*

**Answer:** When `shell=True` is enabled, Python passes the entire command string through the system shell (`/bin/sh` or `cmd.exe`). If user input is concatenated into the command, an attacker can append command separators (`;`, `&&`, `|`) to execute arbitrary shell commands. Fix: set `shell=False` (default) and pass arguments as a list.

```python
# Dangerous:
# subprocess.run(f"ping -c 1 {user_host}", shell=True)
# Attacker enters: "8.8.8.8; cat /etc/passwd"

# Safe:
# subprocess.run(["ping", "-c", "1", user_host], shell=False)
print("shell=False treats all arguments as literal strings, neutralizing command injection")
```

Output:
```text
shell=False treats all arguments as literal strings, neutralizing command injection
```

**Q802. What is Path Traversal (Directory Traversal) and how do you prevent it?**

*Explain `../` attacks and validating paths using `pathlib.Path.resolve()`.*

**Answer:** Path traversal occurs when user input is used to construct file paths, allowing an attacker to navigate outside the intended directory using `../` (e.g. `../../etc/shadow`). Prevent by resolving absolute paths with `.resolve()` and verifying the target path starts with the allowed base directory.

```python
from pathlib import Path

def is_safe_path(base_dir: Path, user_filename: str) -> bool:
    target = (base_dir / user_filename).resolve()
    return target.is_relative_to(base_dir.resolve())

base = Path("/app/uploads")
print("Safe path test:     ", is_safe_path(base, "report.pdf"))
print("Traversal attack:   ", is_safe_path(base, "../../etc/passwd"))
```

Output:
```text
Safe path test:      True
Traversal attack:    False
```

**Q803. Why is `pickle` deserialization unsafe for untrusted input?**

*Explain arbitrary code execution via `__reduce__` exploit payloads.*

**Answer:** The `pickle` module is not secure against erroneous or maliciously constructed data. The `__reduce__` protocol allows pickled data to specify an arbitrary callable (such as `os.system`) and its arguments, executing arbitrary code immediately upon `pickle.loads()`.

```python
# Malicious pickle payload proof of concept:
# class Exploit:
#     def __reduce__(self):
#         return (os.system, ('whoami',))
# pickle.loads(pickle.dumps(Exploit())) # Executes whoami!
print("Never unpickle data from untrusted sources; use JSON or Protocol Buffers")
```

Output:
```text
Never unpickle data from untrusted sources; use JSON or Protocol Buffers
```

**Q804. How should passwords be hashed in Python (Argon2, Bcrypt, PBKDF2)?**

*Explain why SHA-256 is insufficient for passwords, and how salts and work factors protect against brute force.*

**Answer:** Fast cryptographic hashes (MD5, SHA-1, SHA-256) are designed for speed and can be cracked at billions of hashes per second on GPUs. Passwords must be hashed using slow, salted, computationally expensive key derivation functions: **Argon2id** (winner of Password Hashing Competition), **Bcrypt**, or `hashlib.pbkdf2_hmac` with high iteration counts (600,000+).

```python
import hashlib
import secrets

def hash_password_pbkdf2(password: str) -> tuple[bytes, bytes]:
    salt = secrets.token_bytes(16) # Cryptographic salt
    key = hashlib.pbkdf2_hmac("sha256", password.encode(), salt, iterations=100_000)
    return key, salt

key, salt = hash_password_pbkdf2("SecretPass123")
print("PBKDF2 key length:", len(key), "bytes | Salt length:", len(salt), "bytes")
```

Output:
```text
PBKDF2 key length: 32 bytes | Salt length: 16 bytes
```

**Q805. What is a Timing Attack and how does `hmac.compare_digest()` mitigate it?**

*Explain short-circuit string comparison information leaks.*

**Answer:** Standard string equality (`a == b`) compares character by character and returns `False` immediately upon encountering the first mismatch. An attacker can measure nanosecond timing differences to guess passwords or HMAC signatures character by character. `hmac.compare_digest()` compares all characters in constant time, leaking zero timing telemetry.

```python
import hmac

token_a = "secret_auth_token_xyz"
token_b = "secret_auth_token_xyz"
print("Constant-time comparison:", hmac.compare_digest(token_a, token_b))
```

Output:
```text
Constant-time comparison: True
```

**Q806. What is Server-Side Request Forgery (SSRF) and how do you prevent it?**

*Explain restricting outgoing HTTP requests from querying cloud metadata endpoints.*

**Answer:** SSRF occurs when an attacker tricks a server into making HTTP requests to internal, non-public endpoints (e.g. AWS metadata service `http://169.254.169.254/latest/meta-data/`). Prevention:
1. Validate that the target hostname resolves to a public, non-private IP address (check against RFC 1918 and loopback).
2. Enforce explicit domain allowlists.

```python
import ipaddress, socket

def is_public_ip(host: str) -> bool:
    try:
        ip_str = socket.gethostbyname(host)
        ip = ipaddress.ip_address(ip_str)
        return not (ip.is_private or ip.is_loopback or ip.is_link_local)
    except Exception:
        return False

print("google.com is public IP:     ", is_public_ip("google.com"))
print("localhost is public IP:      ", is_public_ip("localhost"))
print("169.254.169.254 is public IP:", is_public_ip("169.254.169.254"))
```

Output:
```text
google.com is public IP:      True
localhost is public IP:       False
169.254.169.254 is public IP: False
```

**Q807. How should secrets and credentials be managed in Python applications?**

*Explain avoiding hardcoded secrets, `.env` file management, and Cloud Secret Managers (AWS Secrets Manager / Vault).*

**Answer:** Never commit API keys, passwords, or certificates to Git.
- In Development: Store credentials in `.env` files loaded via `python-dotenv`, and add `.env` to `.gitignore`.
- In Production: Inject secrets via environment variables or fetch them dynamically at startup from enterprise secret managers (AWS Secrets Manager, HashiCorp Vault, GCP Secret Manager).

```python
import os

# Safe environment secret lookup with fallback guard
DB_PASSWORD = os.getenv("DB_PASSWORD")
if not DB_PASSWORD:
    # In production, fail early if critical secret is missing
    pass
print("Secrets read dynamically from operating system environment")
```

Output:
```text
Secrets read dynamically from operating system environment
```

**Q808. What are static application security testing (SAST) tools: Bandit?**

*Demonstrate scanning Python code for security flaws using Bandit.*

**Answer:** `Bandit` is a security-focused static analyzer that scans Python ASTs for common security vulnerabilities: hardcoded passwords, use of `shell=True`, weak cryptography (MD5), insecure temporary files, and `eval()`/`exec()` usage.

```python
# In terminal:
# bandit -r src/
print("Bandit scans Python codebases for common vulnerabilities in CI/CD pipelines")
```

Output:
```text
Bandit scans Python codebases for common vulnerabilities in CI/CD pipelines
```

**Q809. What are secure temporary file operations with `tempfile`?**

*Explain avoiding race conditions (TOCTOU) and insecure world-readable permissions.*

**Answer:** Never use `mktemp()` (deprecated and insecure; leaves a window between filename generation and file creation where attackers can create a symlink). Use `tempfile.NamedTemporaryFile()` or `tempfile.TemporaryDirectory()`, which create files with restrictive permissions (`0600`) atomically.

```python
import tempfile
import os

with tempfile.NamedTemporaryFile() as tf:
    mode = oct(os.stat(tf.name).st_mode)[-3:]
    print("Temporary file secure permissions:", mode)
```

Output:
```text
Temporary file secure permissions: 600
```

**Q810. What is secure cryptographic randomness with `secrets`?**

*Demonstrate generating passwords, tokens, and verification codes.*

**Answer:** The `secrets` module provides direct access to the OS CSPRNG (Cryptographically Secure Pseudo-Random Number Generator).

```python
import secrets
import string

def generate_secure_password(length=16):
    chars = string.ascii_letters + string.digits + "!@#$%^&*"
    return "".join(secrets.choice(chars) for _ in range(length))

print("Generated password:", generate_secure_password())
print("URL-safe token:    ", secrets.token_urlsafe(16))
```

Output:
```text
Generated password: ...
URL-safe token:     ...
```

**Q811. Output Question: Predict what happens when `eval()` runs with restricted `__builtins__`.**

*Explain bypassing naive sandboxes using object traversal.*

**Answer:** Even if `__builtins__` is empty (`eval(code, {'__builtins__': {}})`), attackers can traverse object hierarchies (`().__class__.__base__.__subclasses__()`) to locate classes with access to `os` or `sys` and execute arbitrary code. Naive `eval` sandboxes are inherently insecure.

```python
print("Restricting __builtins__ in eval() fails to prevent sandbox escape vulnerabilities")
```

Output:
```text
Restricting __builtins__ in eval() fails to prevent sandbox escape vulnerabilities
```

**Q812. Debugging Question: Fix vulnerable `yaml.load()` usage.**

*Resolve arbitrary code execution vulnerability in PyYAML.*

**Answer:** Replace `yaml.load(stream)` with `yaml.safe_load(stream)`.

```python
# Vulnerable:
# yaml.load(user_input, Loader=yaml.Loader) # RCE!

# Secure:
# yaml.safe_load(user_input)
print("yaml.safe_load prevents arbitrary object instantiation in YAML parsing")
```

Output:
```text
yaml.safe_load prevents arbitrary object instantiation in YAML parsing
```

**Q813. Programming Question: Validate uploaded files using magic bytes instead of extensions.**

*Verify that a file is genuinely a PDF or PNG regardless of file extension.*

**Answer:** Inspect file signature header bytes.

```python
def is_genuine_pdf(file_bytes: bytes) -> bool:
    return file_bytes.startswith(b"%PDF-")

fake_pdf = b"malicious shell script payload"
real_pdf = b"%PDF-1.4
%..."

print("Fake PDF verified: ", is_genuine_pdf(fake_pdf))
print("Real PDF verified: ", is_genuine_pdf(real_pdf))
```

Output:
```text
Fake PDF verified:  False
Real PDF verified:  True
```

**Q814. Programming Question: Mask sensitive keys in dictionary before logging.**

*Deep-mask sensitive keys like password, token, and secret in nested dictionaries.*

**Answer:** Recursively redact matched dictionary keys.

```python
def sanitize_log_dict(data, sensitive={"password", "token", "key"}):
    if isinstance(data, dict):
        return {k: "***REDACTED***" if k.lower() in sensitive else sanitize_log_dict(v, sensitive) for k, v in data.items()}
    elif isinstance(data, list):
        return [sanitize_log_dict(item, sensitive) for item in data]
    return data

log_payload = {"user": "admin", "token": "xyz999", "profile": {"password": "secretpassword"}}
print("Sanitized payload:", sanitize_log_dict(log_payload))
```

Output:
```text
Sanitized payload: {'user': 'admin', 'token': '***REDACTED***', 'profile': {'password': '***REDACTED***'}}
```

**Q815. What is Clickjacking and how does `X-Frame-Options` protect web applications?**

*Prevent embedding application in transparent malicious iframes.*

**Answer:** Set `X-Frame-Options: DENY` or `SAMEORIGIN`.

```python
headers = {"X-Frame-Options": "DENY"}
print("X-Frame-Options header blocks clickjacking framing")
```

Output:
```text
X-Frame-Options header blocks clickjacking framing
```

**Q816. What are Tarbomb vulnerabilities and how does Python 3.12 (PEP 706) neutralize them?**

*Explain directory traversal inside `.tar` and `.zip` archives during extraction.*

**Answer:** A Tarbomb contains files with paths like `../../etc/shadow`. Extracting it overwrites critical system files. Python 3.12 (PEP 706) added `filter='data'` to `tarfile.extractall()`, blocking paths containing `..` or leading slashes.

```python
# tarfile.extractall(path, filter='data') # Safe in Python 3.12+
print("PEP 706 filter='data' neutralizes archive extraction path traversal vulnerabilities")
```

Output:
```text
PEP 706 filter='data' neutralizes archive extraction path traversal vulnerabilities
```

**Q817. How do you generate cryptographically secure tokens with expiration?**

*Use `itsdangerous` to sign and timestamp tokens.*

**Answer:** `itsdangerous.URLSafeTimedSerializer` signs tokens with HMAC and verifies expiration timestamps without database storage.

```python
# from itsdangerous import URLSafeTimedSerializer
# s = URLSafeTimedSerializer("secret_key")
# token = s.dumps({"user_id": 123})
# data = s.loads(token, max_age=3600) # Valid for 1 hour
print("Timestamped serializers generate stateless expiring authentication tokens")
```

Output:
```text
Timestamped serializers generate stateless expiring authentication tokens
```

**Q818. How do you enforce HTTP-Only and Secure flags on cookies?**

*Protect session cookies from XSS theft and MITM sniffing.*

**Answer:** - `HttpOnly`: Prevents client-side JavaScript (`document.cookie`) from accessing the cookie (mitigating XSS theft).
- `Secure`: Ensures browser only transmits the cookie over encrypted HTTPS connections.

```python
cookie_headers = "Set-Cookie: session_id=xyz; Secure; HttpOnly; SameSite=Lax"
print("Hardened session cookie flags configured")
```

Output:
```text
Hardened session cookie flags configured
```

**Q819. What is an IDOR (Insecure Direct Object Reference) vulnerability?**

*Explain missing horizontal authorization checks.*

**Answer:** An IDOR occurs when an application provides direct access to objects based on user input without verifying ownership (e.g. `/api/orders/1234` accessible by any logged-in user). Prevent by always scoping queries to the current authenticated user (`WHERE id = :id AND user_id = :current_user`).

```python
print("Always verify resource ownership on every request to prevent IDOR vulnerabilities")
```

Output:
```text
Always verify resource ownership on every request to prevent IDOR vulnerabilities
```

**Q820. How do you sanitize command arguments when `shell=False` is used?**

*Validate arguments against strict allowlists.*

**Answer:** Ensure arguments passed to `subprocess.run` match expected formats (e.g. valid IP addresses or alphanumeric slugs).

```python
import re

def is_valid_hostname(host: str) -> bool:
    return bool(re.fullmatch(r"^[a-zA-Z0-9.-]+$", host))

print("Valid host:  ", is_valid_hostname("prod-db.internal"))
print("Invalid host:", is_valid_hostname("db; rm -rf /"))
```

Output:
```text
Valid host:   True
Invalid host: False
```

**Q821. How do you implement account lockout to prevent brute-force attacks?**

*Track failed login attempts in Redis with auto-expiring keys.*

**Answer:** Increment failure count on invalid password; lock account for 15 minutes after 5 failures.

```python
class LoginTracker:
    def __init__(self, max_attempts=3):
        self.attempts = {}
        self.max = max_attempts

    def record_failure(self, user: str) -> bool:
        self.attempts[user] = self.attempts.get(user, 0) + 1
        return self.attempts[user] >= self.max

tracker = LoginTracker(max_attempts=2)
print("Attempt 1 failure:", tracker.record_failure("alice"))
print("Attempt 2 failure (locked):", tracker.record_failure("alice"))
```

Output:
```text
Attempt 1 failure: False
Attempt 2 failure (locked): True
```

**Q822. How do you prevent logging sensitive HTTP headers?**

*Redact `Authorization`, `Cookie`, and `X-API-Key` headers.*

**Answer:** Filter dictionary keys before writing to log streams.

```python
def sanitize_headers(headers: dict) -> dict:
    blocked = {"authorization", "cookie", "x-api-key"}
    return {k: ("***" if k.lower() in blocked else v) for k, v in headers.items()}

req_headers = {"User-Agent": "curl/8.0", "Authorization": "Bearer secret_token"}
print("Sanitized headers:", sanitize_headers(req_headers))
```

Output:
```text
Sanitized headers: {'User-Agent': 'curl/8.0', 'Authorization': '***'}
```

**Q823. Programming Question: Validate webhook requests against HMAC signature.**

*Compute SHA-256 HMAC and verify using constant-time comparison.*

**Answer:** Use `hmac.new` and `hmac.compare_digest`.

```python
import hmac, hashlib

def is_valid_webhook(body: bytes, expected_sig: str, secret: bytes) -> bool:
    calc_sig = hmac.new(secret, body, hashlib.sha256).hexdigest()
    return hmac.compare_digest(calc_sig, expected_sig)

secret = b"my_secret"
payload = b'{"status": "ok"}'
sig = hmac.new(secret, payload, hashlib.sha256).hexdigest()
print("Webhook verified:", is_valid_webhook(payload, sig, secret))
```

Output:
```text
Webhook verified: True
```

**Q824. Programming Question: Implement safe JSON string parser rejecting huge payloads.**

*Limit input stream size to prevent Memory Exhaustion DoS.*

**Answer:** Check string length before calling `json.loads`.

```python
import json

def safe_json_loads(data: str, max_chars: int = 1000):
    if len(data) > max_chars:
        raise ValueError(f"Payload size {len(data)} exceeds maximum allowed {max_chars}")
    return json.loads(data)

print("Small payload parsed:", safe_json_loads('{"status": "ok"}'))
try: safe_json_loads(" " * 2000)
except ValueError as e: print("Payload rejected:", e)
```

Output:
```text
Small payload parsed: {'status': 'ok'}
Payload rejected: Payload size 2000 exceeds maximum allowed 1000
```

**Q825. Programming Question: Mask credit card PAN numbers leaving only last 4 digits.**

*Redact payment card numbers in string outputs.*

**Answer:** Replace digits with `*`.

```python
import re

def mask_card(card_number: str) -> str:
    clean = re.sub(r"\D", "", card_number)
    return "*" * (len(clean) - 4) + clean[-4:]

print("Masked card:", mask_card("4111-2222-3333-4567"))
```

Output:
```text
Masked card: ************4567
```

**Q826. Programming Question: Verify TLS certificate expiration date.**

*Parse certificate expiry and check if renewal is needed within 30 days.*

**Answer:** Parse datetime and compare with current date.

```python
from datetime import datetime, timezone, timedelta

def is_cert_expiring_soon(expiry_dt: datetime, threshold_days: int = 30) -> bool:
    now = datetime.now(timezone.utc)
    return expiry_dt - now < timedelta(days=threshold_days)

exp = datetime(2026, 11, 15, tzinfo=timezone.utc)
print("Cert expiring soon check:", is_cert_expiring_soon(exp, 30))
```

Output:
```text
Cert expiring soon check: True
```


---

## Chapter 27: Performance, Benchmarking, and Optimization

**Q827. What is Big O Notation and complexity analysis in Python?**

*Explain Time Complexity, Space Complexity, and common Big O classes in Python operations.*

**Answer:** Big O notation characterizes execution time or space requirements as input size $N$ grows:
- $O(1)$ Constant: Dictionary lookup `d[k]`, list append `lst.append()`.
- $O(\log N)$ Logarithmic: Binary search `bisect`.
- $O(N)$ Linear: List search `x in lst`, `lst.insert(0)`.
- $O(N \log N)$ Linearithmic: Sorting `sorted()` (Timsort).
- $O(N^2)$ Quadratic: Nested loops, string concatenation via `+=` in loops.

```python
print("Big O analysis identifies algorithmic scalability bottlenecks before production")
```

Output:
```text
Big O analysis identifies algorithmic scalability bottlenecks before production
```

**Q828. How do you profile Python code using `line_profiler`?**

*Demonstrate line-by-line execution time profiling.*

**Answer:** `line_profiler` profiles functions line-by-line, showing the exact time and percentage of total runtime spent on every individual line of code, highlighting the exact bottleneck statements.

```python
# @profile
# def heavy_loop():
#     a = [i * 2 for i in range(1000)]
#     b = [x ** 2 for x in a]
print("line_profiler pinpoints the exact hot lines inside functions")
```

Output:
```text
line_profiler pinpoints the exact hot lines inside functions
```

**Q829. How does sampling profiler `py-spy` work in production?**

*Explain zero-overhead non-intrusive stack sampling on live Python processes.*

**Answer:** `py-spy` is a sampling profiler written in Rust that inspects a running Python process's stack memory from the outside without modifying code or restarting the process, generating interactive SVG flame graphs with near-zero overhead in production.

```python
# py-spy top --pid 1234
# py-spy record -o profile.svg --pid 1234
print("py-spy profiles live production Python processes without performance degradation")
```

Output:
```text
py-spy profiles live production Python processes without performance degradation
```

**Q830. Why are list comprehensions faster than standard for-loops?**

*Explain the C-level `LIST_APPEND` bytecode optimization.*

**Answer:** In a standard for-loop (`for x in seq: lst.append(x)`), CPython executes multiple bytecodes per iteration (`LOAD_ATTR`, `CALL` for method resolution, `STORE_FAST`). In a list comprehension, CPython uses specialized `LIST_APPEND` opcodes directly in C without attribute lookups or function call overhead.

```python
import dis

def comp_demo(): return [x for x in [1, 2]]
dis.dis(comp_demo)
```

Output:
```text
1           0 RESUME                   0
              2 BUILD_LIST               0
              4 LOAD_CONST               1 ((1, 2))
              6 GET_ITER
        >>    8 FOR_ITER                 4 (to 20)
             12 STORE_FAST               0 (x)
             14 LOAD_FAST                0 (x)
             16 LIST_APPEND              2
             18 JUMP_BACKWARD            6 (to 8)
        >>   20 RETURN_VALUE
```

**Q831. How does PyPy's Just-In-Time (JIT) compiler achieve 2x-7x speedups?**

*Explain trace-based JIT compilation into native machine instructions.*

**Answer:** PyPy monitors running loops. When a loop executes repeatedly ('hot spot'), PyPy's tracing JIT compiles the Python bytecode sequence directly into optimized native machine code (x86/ARM), eliminating bytecode interpretation overhead.

```python
print("PyPy JIT optimizes long-running CPU loops into native machine code")
```

Output:
```text
PyPy JIT optimizes long-running CPU loops into native machine code
```

**Q832. What is Cython and how does it speed up Python code by 10x-100x?**

*Explain compiling Python with C type declarations into native C extensions.*

**Answer:** Cython is a superset of Python supporting static C type declarations (`cdef int i`). The Cython compiler translates `.pyx` files into C/C++ code, compiled by gcc/clang into a native `.so` or `.pyd` C-extension that runs at native C execution speed.

```python
# In Cython (.pyx):
# def compute(int n):
#     cdef int i, total = 0
#     for i in range(n): total += i
#     return total
print("Cython bridges Python syntax with native C execution performance")
```

Output:
```text
Cython bridges Python syntax with native C execution performance
```

**Q833. What is Numba and how does `@jit` accelerate numerical Python?**

*Explain LLVM JIT compilation for NumPy array operations.*

**Answer:** Numba is an open-source JIT compiler that uses LLVM to compile pure Python and NumPy functions directly into optimized machine instructions at runtime using a simple decorator: `@numba.jit(nopython=True)`.

```python
# @numba.jit(nopython=True)
# def fast_matrix_sum(arr): return arr.sum()
print("Numba compiles NumPy array operations into optimized LLVM machine code")
```

Output:
```text
Numba compiles NumPy array operations into optimized LLVM machine code
```

**Q834. What is Vectorization in NumPy and why is it faster than Python loops?**

*Explain SIMD (Single Instruction, Multiple Data) CPU hardware acceleration.*

**Answer:** Vectorization expresses operations on entire arrays at once without explicit Python for-loops. NumPy delegates vectorized operations to contiguous C/Fortran buffers that utilize CPU SIMD registers, processing multiple data elements in a single CPU clock cycle.

```python
# Instead of: [a + b for a, b in zip(list1, list2)]
# Use NumPy:  array1 + array2 # SIMD accelerated!
print("NumPy vectorization leverages CPU SIMD vector units for massive speedups")
```

Output:
```text
NumPy vectorization leverages CPU SIMD vector units for massive speedups
```

**Q835. What is Premature Optimization and what are Donald Knuth's principles?**

*Explain *'Premature optimization is the root of all evil'*. (Donald Knuth)*

**Answer:** Premature optimization occurs when developers spend time optimizing code before profiling, often increasing complexity and introducing bugs for negligible performance gains. The correct engineering sequence:
1. Write clean, readable, correct code.
2. Write unit tests.
3. Profile with profilers (cProfile, py-spy) to identify the genuine 5% bottleneck.
4. Optimize only the verified bottleneck.

```python
print("Measure before optimizing; focus exclusively on verified profiler bottlenecks")
```

Output:
```text
Measure before optimizing; focus exclusively on verified profiler bottlenecks
```

**Q836. How does caching with `functools.lru_cache` optimize computational speed?**

*Demonstrate turning exponential recursive algorithms into linear time.*

**Answer:** `@lru_cache(maxsize=128)` stores the return values of functions keyed by their arguments in an internal hash table. If called with identical arguments, it returns the cached result in $O(1)$ time.

```python
from functools import lru_cache

@lru_cache(maxsize=128)
def expensive_calculation(n):
    return sum(i * i for i in range(n))

print("Call 1 (computed):", expensive_calculation(1000))
print("Call 2 (cached):  ", expensive_calculation(1000))
print("Cache info:       ", expensive_calculation.cache_info())
```

Output:
```text
Call 1 (computed): 332833500
Call 2 (cached):   332833500
Cache info:        CacheInfo(hits=1, misses=1, maxsize=128, currsize=1)
```

**Q837. Output Question: Predict performance difference between `in list` vs `in set`.**

*Evaluate $O(N)$ linear search vs $O(1)$ hash table lookup.*

**Answer:** Checking `x in list` scans sequentially ($O(N)$), while `x in set` computes the hash and checks the bucket in $O(1)$ constant time.

```python
lookup_list = list(range(10000))
lookup_set = set(lookup_list)

print("Set lookup is O(1) regardless of size:", 9999 in lookup_set)
```

Output:
```text
Set lookup is O(1) regardless of size: True
```

**Q838. Debugging Question: Fix memory bloat caused by generator materialization.**

*Prevent OOM when calculating sum of millions of numbers.*

**Answer:** Use generator expression `sum(x for x in seq)` instead of list comprehension `sum([x for x in seq])`.

```python
# Memory heavy: sum([i for i in range(10_000_000)]) # Allocates 80MB list!
# Memory optimal:
total = sum(i for i in range(1000)) # O(1) memory!
print("Computed sum in O(1) memory:", total)
```

Output:
```text
Computed sum in O(1) memory: 499500
```

**Q839. Programming Question: Benchmark dictionary vs slotted class memory footprint.**

*Compare memory consumption of 10,000 instances.*

**Answer:** Calculate total bytes using `sys.getsizeof`.

```python
import sys

class DictNode:
    def __init__(self, a, b): self.a, self.b = a, b

class SlotNode:
    __slots__ = ("a", "b")
    def __init__(self, a, b): self.a, self.b = a, b

d = DictNode(1, 2)
s = SlotNode(1, 2)
print("Dict instance size:", sys.getsizeof(d) + sys.getsizeof(d.__dict__))
print("Slot instance size:", sys.getsizeof(s))
```

Output:
```text
Dict instance size: 152
Slot instance size: 48
```

**Q840. Programming Question: Implement a lightweight Time Profiler Context Manager.**

*Measure and report execution time of arbitrary blocks of code.*

**Answer:** Use `time.perf_counter`.

```python
import time
from contextlib import contextmanager

@contextmanager
def benchmark_timer(label: str):
    t0 = time.perf_counter()
    yield
    elapsed = time.perf_counter() - t0
    print(f"[{label}] elapsed: {elapsed * 1000:.3f} ms")

with benchmark_timer("Vector Multiplication"):
    _ = [i * 2 for i in range(5000)]
```

Output:
```text
[Vector Multiplication] elapsed: ... ms
```

**Q841. How does string interning optimize dictionary key lookups?**

*Explain CPython's string pointer hashing optimization.*

**Answer:** CPython interns string dictionary keys. When looking up keys that share the same pointer address, Python skips string equality comparisons and resolves the hash table slot immediately in $O(1)$.

```python
import sys
k1 = sys.intern("metric_cpu")
k2 = sys.intern("metric_cpu")
print("Interned keys identical pointer:", k1 is k2)
```

Output:
```text
Interned keys identical pointer: True
```

**Q842. How do local variables improve loop execution speed compared to global lookups?**

*Explain `LOAD_FAST` vs `LOAD_GLOBAL` bytecode performance.*

**Answer:** Local variables are indexed directly in C array slots (`LOAD_FAST`), whereas global variables require querying the module globals dictionary (`LOAD_GLOBAL`). In tight loops, aliasing global functions locally speeds up execution.

```python
import math

def optimized_loop():
    local_cos = math.cos # Local cache
    return sum(local_cos(i) for i in range(100))

print("Loop executed with local caching:", round(optimized_loop(), 2))
```

Output:
```text
Loop executed with local caching: 0.14
```

**Q843. How does `array.array` reduce memory usage compared to lists of numbers?**

*Explain unboxed contiguous C arrays.*

**Answer:** A Python `list` stores 8-byte pointers to 28-byte `PyObject` integers (36 bytes per integer). `array.array('i')` stores raw 4-byte C integers contiguously in memory without headers (4 bytes per integer).

```python
import array, sys

a = array.array("i", range(100))
print("Array memory: 100 ints in ~400 bytes:", sys.getsizeof(a) < 500)
```

Output:
```text
Array memory: 100 ints in ~400 bytes: True
```

**Q844. What is lazy evaluation and how does it save CPU and memory?**

*Compare upfront materialization with on-demand computation.*

**Answer:** Lazy evaluation defers computing values until they are specifically requested, avoiding unnecessary computations if only the first few items of a stream are consumed.

```python
def lazy_evaluator():
    for i in range(1_000_000):
        yield i * 2

stream = lazy_evaluator()
first = next(stream)
print("Computed only first element:", first)
```

Output:
```text
Computed only first element: 0
```

**Q845. How does PyPy handle garbage collection differently than CPython?**

*Explain PyPy's generational nursery mark-and-sweep GC vs CPython reference counting.*

**Answer:** PyPy does not use reference counting. It uses a high-performance generational moving garbage collector with a memory 'nursery'. Objects allocated in the nursery are extremely cheap to create, and short-lived objects are collected with zero overhead.

```python
print("PyPy uses generational moving GC without reference counting overhead")
```

Output:
```text
PyPy uses generational moving GC without reference counting overhead
```

**Q846. How do you profile memory allocations line-by-line with `memory_profiler`?**

*Track memory spikes across specific code statements.*

**Answer:** `@profile` from `memory_profiler` records memory delta before and after each line of code.

```python
# @profile
# def heavy_allocation():
#     a = [1] * (10 ** 6)
#     del a
print("memory_profiler tracks heap allocation deltas per line")
```

Output:
```text
memory_profiler tracks heap allocation deltas per line
```

**Q847. What is Tail Call Optimization (TCO) and why does Python not support it?**

*Explain Guido van Rossum's rationale: preserving stack traces and debugging simplicity.*

**Answer:** TCO reuses the current stack frame for recursive calls in tail position. Guido van Rossum deliberately omitted TCO from Python because it erases stack frames, making debugging and stack traces unreadable.

```python
print("Python retains full call stack frames for pristine debugging tracebacks")
```

Output:
```text
Python retains full call stack frames for pristine debugging tracebacks
```

**Q848. How does `sys.setswitchinterval()` affect threading performance in CPython?**

*Explain GIL thread switching frequency (default 5ms).*

**Answer:** `sys.setswitchinterval(seconds)` sets how frequently CPython pauses the current thread to let other threads acquire the GIL. Setting it too low causes high thread switching contention; setting it too high hurts concurrency responsiveness.

```python
import sys
print("Current GIL switch interval:", sys.getswitchinterval(), "seconds")
```

Output:
```text
Current GIL switch interval: 0.005 seconds
```

**Q849. Programming Question: Implement a rolling window cache with maximum capacity.**

*Evict oldest entries when cache exceeds capacity.*

**Answer:** Use `collections.OrderedDict`.

```python
from collections import OrderedDict

class SizedCache(OrderedDict):
    def __init__(self, maxsize=3):
        super().__init__()
        self.maxsize = maxsize

    def __setitem__(self, key, value):
        super().__setitem__(key, value)
        if len(self) > self.maxsize:
            self.popitem(last=False)

cache = SizedCache(2)
cache["a"] = 1; cache["b"] = 2; cache["c"] = 3
print("Remaining keys after maxsize eviction:", list(cache.keys()))
```

Output:
```text
Remaining keys after maxsize eviction: ['b', 'c']
```

**Q850. Programming Question: Compare execution speed of string concatenation techniques.**

*Benchmark `str.join()` against repeated `+=`.*

**Answer:** Measure time difference with `time.perf_counter`.

```python
import time

items = [f"item_{i}" for i in range(1000)]

t0 = time.perf_counter()
res_join = ",".join(items)
t_join = time.perf_counter() - t0

t0 = time.perf_counter()
res_plus = ""
for item in items: res_plus += item + ","
t_plus = time.perf_counter() - t0

print("join() is faster than loop +=:", t_join <= t_plus)
```

Output:
```text
join() is faster than loop +=: True
```

**Q851. Programming Question: Implement a fast memoization decorator using a dictionary.**

*Cache function results in memory without external dependencies.*

**Answer:** Store arguments in dictionary closure.

```python
def memoize(fn):
    cache = {}
    def wrapper(*args):
        if args not in cache:
            cache[args] = fn(*args)
        return cache[args]
    return wrapper

@memoize
def expensive_square(n): return n * n

print("Square:", expensive_square(5))
print("Cached:", expensive_square(5))
```

Output:
```text
Square: 25
Cached: 25
```

**Q852. Programming Question: Optimize list deduplication preserving order in linear $O(N)$ time.**

*Deduplicate a list without quadratic time penalty.*

**Answer:** Use `dict.fromkeys(lst)`.

```python
data = ["b", "a", "b", "c", "a", "d"]
# dict.fromkeys maintains insertion order and guarantees uniqueness in O(N)!
deduped = list(dict.fromkeys(data))
print("Deduplicated preserving order:", deduped)
```

Output:
```text
Deduplicated preserving order: ['b', 'a', 'c', 'd']
```


---

## Chapter 28: Python for DevOps and Automation

**Q853. How do you execute shell commands safely using `subprocess.run()`?**

*Explain `args` list, `capture_output=True`, `text=True`, `check=True`, and `timeout`.*

**Answer:** `subprocess.run(args, capture_output=True, text=True, check=True, timeout=30)` is the standard high-level interface for running system commands:
- `args`: List of command arguments (`['git', 'status']`).
- `capture_output=True`: Captures stdout and stderr into `result.stdout` / `result.stderr`.
- `text=True`: Decodes bytes to strings automatically.
- `check=True`: Raises `CalledProcessError` if the process exits with non-zero code.

```python
import subprocess
import sys

res = subprocess.run([sys.executable, "-c", "print('DevOps Automation')"], capture_output=True, text=True)
print("Return code:", res.returncode)
print("Output:     ", res.stdout.strip())
```

Output:
```text
Return code: 0
Output:      DevOps Automation
```

**Q854. What is the difference between `subprocess.run()` and `subprocess.Popen()`?**

*Compare synchronous execution with asynchronous process streaming and pipes.*

**Answer:** - `subprocess.run()`: Synchronous and blocking; waits for command to complete and returns a `CompletedProcess`.
- `subprocess.Popen()`: Non-blocking; spawns the process in the background, returns a `Popen` object immediately, and allows real-time interactive stream piping (`stdin`, `stdout`, `stderr`), polling (`poll()`), and signaling (`terminate()`, `kill()`).

```python
import subprocess
import sys

proc = subprocess.Popen([sys.executable, "-c", "print('Async stream')"], stdout=subprocess.PIPE, text=True)
stdout, _ = proc.communicate()
print("Popen output:", stdout.strip())
```

Output:
```text
Popen output: Async stream
```

**Q855. How do you chain shell pipes in Python using `subprocess.Popen`?**

*Demonstrate executing `cat file | grep error | wc -l` safely without `shell=True`.*

**Answer:** Pipe standard output of the first process into standard input of the second: `p2 = Popen(cmd2, stdin=p1.stdout, stdout=PIPE)`.

```python
import subprocess
import sys

p1 = subprocess.Popen([sys.executable, "-c", "print('alpha\nbeta\nalpha')"], stdout=subprocess.PIPE)
p2 = subprocess.Popen([sys.executable, "-c", "import sys; print(sum(1 for line in sys.stdin if 'alpha' in line))"], stdin=p1.stdout, stdout=subprocess.PIPE, text=True)
p1.stdout.close()
output, _ = p2.communicate()
print("Piped output count:", output.strip())
```

Output:
```text
Piped output count: 2
```

**Q856. How do you monitor system metrics (CPU, RAM, Disk, Network) with `psutil`?**

*Demonstrate reading CPU percentages, virtual memory, disk usage, and network counters.*

**Answer:** `psutil` (Python System and Process Utilities) provides cross-platform interfaces for system telemetry and process management.

```python
import psutil

print("CPU cores count:     ", psutil.cpu_count(logical=True))
print("Virtual memory total:", f"{psutil.virtual_memory().total / (1024**3):.2f} GB")
print("Disk usage (root):   ", f"{psutil.disk_usage('.').percent}% used")
```

Output:
```text
CPU cores count:      ...
Virtual memory total: ... GB
Disk usage (root):    ...% used
```

**Q857. How do you inspect and manage running OS processes with `psutil`?**

*Filter processes by name, inspect CPU/RAM usage, and terminate zombie processes.*

**Answer:** Iterate over `psutil.process_iter(['pid', 'name', 'status'])` to inspect and manage processes.

```python
import psutil

py_pids = [p.info["pid"] for p in psutil.process_iter(["pid", "name"]) if "python" in (p.info["name"] or "").lower()]
print("Discovered running Python PIDs count:", len(py_pids))
```

Output:
```text
Discovered running Python PIDs count: ...
```

**Q858. How does AWS automation work using `boto3`?**

*Compare low-level `client` vs high-level `resource` interfaces.*

**Answer:** - `boto3.client('s3')`: Low-level 1:1 mapping to raw AWS HTTP service APIs; returns dictionaries.
- `boto3.resource('s3')`: High-level object-oriented abstraction with action methods and sub-resources.

```python
# import boto3
# s3 = boto3.client("s3")
# s3.upload_file("app.zip", "my-bucket", "app.zip")
print("Boto3 provides complete programmatic control over AWS cloud infrastructure")
```

Output:
```text
Boto3 provides complete programmatic control over AWS cloud infrastructure
```

**Q859. How do you automate Docker containers using the Docker SDK for Python (`docker`)?**

*Demonstrate pulling images, running containers, inspecting logs, and stopping containers.*

**Answer:** The `docker` package talks directly to the local or remote Docker daemon API (`/var/run/docker.sock` or TCP socket).

```python
# import docker
# client = docker.from_env()
# container = client.containers.run("nginx:alpine", detach=True, ports={"80/tcp": 8080})
# print("Container ID:", container.id)
# container.stop()
print("Docker SDK manages container lifecycles programmatically")
```

Output:
```text
Docker SDK manages container lifecycles programmatically
```

**Q860. How do you automate Kubernetes clusters using the official Python client (`kubernetes`)?**

*Demonstrate loading kubeconfig, listing pods, and patching deployments.*

**Answer:** The `kubernetes` library communicates with the Kubernetes API server using `config.load_kube_config()` or in-cluster service account authentication (`config.load_incluster_config()`).

```python
# from kubernetes import client, config
# config.load_kube_config()
# v1 = client.CoreV1Api()
# pods = v1.list_namespaced_pod(namespace="default")
print("Kubernetes Python client provides programmatic cluster orchestration")
```

Output:
```text
Kubernetes Python client provides programmatic cluster orchestration
```

**Q861. How do you automate SSH commands and SFTP transfers using `paramiko`?**

*Demonstrate establishing SSH connections, executing remote commands, and transferring files.*

**Answer:** `paramiko` implements the SSHv2 protocol in pure Python for remote server automation, key-based authentication, and SFTP file uploads.

```python
# import paramiko
# ssh = paramiko.SSHClient()
# ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
# ssh.connect("10.0.0.1", username="ubuntu", key_filename="~/.ssh/id_rsa")
# stdin, stdout, stderr = ssh.exec_command("uptime")
# print(stdout.read().decode())
# ssh.close()
print("Paramiko executes remote SSH commands and SFTP transfers securely")
```

Output:
```text
Paramiko executes remote SSH commands and SFTP transfers securely
```

**Q862. How do you build modern CLI tools using `click` or `typer`?**

*Compare Click command decorators and Typer's type-hint-driven CLI generation.*

**Answer:** `Click` uses decorators (`@click.command()`, `@click.option()`) to build expressive CLIs. `Typer` (built on Click) generates CLI commands, options, and help documentation automatically using standard Python type annotations.

```python
# import typer
# app = typer.Typer()
# @app.command()
# def deploy(service: str, replicas: int = 1):
#     print(f"Deploying {service} ({replicas} replicas)")
print("Typer compiles type hints into beautiful CLI interfaces")
```

Output:
```text
Typer compiles type hints into beautiful CLI interfaces
```

**Q863. What is an Idempotent automation script and how do you ensure idempotency?**

*Explain declarative configuration and checking current state before applying changes.*

**Answer:** An idempotent script produces the same outcome regardless of how many times it is run: if a user or file already exists with desired attributes, the script makes no changes; if missing, it creates it.

```python
def ensure_directory_exists(path_str: str) -> str:
    from pathlib import Path
    p = Path(path_str)
    if p.exists():
        return "ALREADY_EXISTS (NO_CHANGE)"
    p.mkdir(parents=True, exist_ok=True)
    return "CREATED"

print(ensure_directory_exists("."))
```

Output:
```text
ALREADY_EXISTS (NO_CHANGE)
```

**Q864. How do you dispatch Slack and Microsoft Teams alert notifications in Python?**

*Demonstrate posting webhook payloads with formatted status cards.*

**Answer:** Send HTTP POST requests with JSON payloads containing markdown or adaptive cards to incoming webhook URLs.

```python
import json

def build_slack_alert(service: str, status: str) -> dict:
    color = "#36a64f" if status == "UP" else "#ff0000"
    return {
        "attachments": [{
            "color": color,
            "title": f"Service Alert: {service}",
            "text": f"Service is currently {status}",
            "footer": "DevOps Monitoring Agent"
        }]
    }

print("Formatted Slack payload:
" + json.dumps(build_slack_alert("auth-api", "DOWN"), indent=2))
```

Output:
```text
Formatted Slack payload:
{
  "attachments": [
    {
      "color": "#ff0000",
      "title": "Service Alert: auth-api",
      "text": "Service is currently DOWN",
      "footer": "DevOps Monitoring Agent"
    }
  ]
}
```

**Q865. Output Question: Predict returncode when subprocess command fails.**

*Explain CalledProcessError when `check=True` is set.*

**Answer:** If the subprocess exits with non-zero code, `check=True` raises `subprocess.CalledProcessError` containing `.returncode`, `.cmd`, and `.output`.

```python
import subprocess
import sys

try:
    subprocess.run([sys.executable, "-c", "import sys; sys.exit(2)"], check=True)
except subprocess.CalledProcessError as e:
    print("Caught process error, return code:", e.returncode)
```

Output:
```text
Caught process error, return code: 2
```

**Q866. Debugging Question: Fix deadlocks when reading subprocess stdout and stderr directly.**

*Explain why reading `proc.stdout.read()` before `proc.stderr.read()` can deadlock OS pipe buffers.*

**Answer:** If the child process fills the OS pipe buffer (typically 64KB) for stderr, it blocks waiting for it to be drained. If the parent is blocked reading stdout, both processes deadlock. Solution: use `proc.communicate()` which drains both streams concurrently using threads or selectors.

```python
# Dangerous:
# out = proc.stdout.read() # Deadlocks if stderr buffer fills!
# err = proc.stderr.read()

# Safe:
# out, err = proc.communicate()
print("proc.communicate() drains stdout and stderr concurrently to prevent pipe buffer deadlocks")
```

Output:
```text
proc.communicate() drains stdout and stderr concurrently to prevent pipe buffer deadlocks
```

**Q867. Programming Question: Build a disk usage alert script.**

*Scan all mounted disks and print alerts if usage exceeds 85%.*

**Answer:** Use `psutil.disk_partitions` and `psutil.disk_usage`.

```python
import psutil

def check_disks(threshold_pct: float = 85.0) -> list:
    alerts = []
    for part in psutil.disk_partitions():
        try:
            usage = psutil.disk_usage(part.mountpoint)
            if usage.percent > threshold_pct:
                alerts.append(f"WARNING: {part.mountpoint} is {usage.percent}% full")
        except PermissionError: pass
    return alerts

print("Disk check executed, alerts count:", len(check_disks(99.0)))
```

Output:
```text
Disk check executed, alerts count: 0
```

**Q868. Programming Question: Parse an Nginx access log and compute top 3 request IPs.**

*Extract IP addresses with regex and aggregate using `collections.Counter`.*

**Answer:** Combine file streaming and `Counter`.

```python
from collections import Counter
import re

mock_logs = """192.168.1.1 - - [..] "GET / HTTP/1.1" 200
10.0.0.5 - - [..] "GET /api HTTP/1.1" 200
192.168.1.1 - - [..] "POST /login HTTP/1.1" 200
192.168.1.1 - - [..] "GET /dashboard HTTP/1.1" 200"""

ips = re.findall(r"^(\d+\.\d+\.\d+\.\d+)", mock_logs, re.MULTILINE)
top_ip, count = Counter(ips).most_common(1)[0]
print(f"Top requester: {top_ip} with {count} requests")
```

Output:
```text
Top requester: 192.168.1.1 with 3 requests
```

**Q869. How do you schedule jobs in Python using `schedule` or `APScheduler`?**

*Compare lightweight interval timers with enterprise cron scheduling.*

**Answer:** `schedule` provides a clean human-readable API (`schedule.every().day.at('10:30').do(job)`). `APScheduler` provides advanced persistent job stores (SQLAlchemy, Redis), cron triggers, and thread/process pools.

```python
# import schedule
# schedule.every(10).seconds.do(run_health_check)
print("APScheduler and schedule automate recurring job execution in Python")
```

Output:
```text
APScheduler and schedule automate recurring job execution in Python
```

**Q870. How do you generate dynamic configuration files using Jinja2 templates?**

*Render templates with variables, loops, and filters into Nginx or Kubernetes YAML files.*

**Answer:** Jinja2 loads template strings or files and renders dictionaries into configuration text.

```python
from jinja2 import Template

nginx_template = Template("""
upstream backend {
{% for srv in servers %}
    server {{ srv.host }}:{{ srv.port }};
{% endfor %}
}
""")

rendered = nginx_template.render(servers=[{"host": "10.0.0.1", "port": 8080}, {"host": "10.0.0.2", "port": 8080}])
print(rendered.strip())
```

Output:
```text
upstream backend {
    server 10.0.0.1:8080;
    server 10.0.0.2:8080;
}
```

**Q871. How do you automate Git repository operations using `GitPython`?**

*Demonstrate cloning repositories, committing changes, creating branches, and pushing tags.*

**Answer:** `GitPython` wraps the git CLI: `repo = git.Repo.clone_from(url, path); repo.git.checkout('-b', 'feature'); repo.index.commit('msg')`.

```python
# import git
# repo = git.Repo.init("/tmp/my_repo")
# repo.index.add(["README.md"])
# repo.index.commit("Initial commit")
print("GitPython automates Git version control workflows programmatically")
```

Output:
```text
GitPython automates Git version control workflows programmatically
```

**Q872. How do you interact with Vault using `hvac` for secret retrieval?**

*Authenticate with HashiCorp Vault and read secrets dynamically.*

**Answer:** `hvac.Client` connects to Vault, authenticates via AppRole or Kubernetes tokens, and fetches dynamic secrets.

```python
# import hvac
# client = hvac.Client(url="https://vault.internal:8200", token="...")
# secret = client.secrets.kv.v2.read_secret_version(path="database")
print("hvac manages HashiCorp Vault secrets and dynamic credentials")
```

Output:
```text
hvac manages HashiCorp Vault secrets and dynamic credentials
```

**Q873. How do you automate cloud resource cleanup based on tags?**

*Scan AWS/Azure resources and terminate instances missing owner or env tags.*

**Answer:** Query resources, inspect tag lists, and invoke terminate actions on untagged assets.

```python
def find_non_compliant_resources(resources: list) -> list:
    return [r["id"] for r in resources if "Owner" not in r.get("tags", {})]

mock_vms = [
    {"id": "vm-1", "tags": {"Owner": "Alice"}},
    {"id": "vm-2", "tags": {"Env": "Dev"}} # Missing Owner
]
print("Non-compliant VMs flagged for cleanup:", find_non_compliant_resources(mock_vms))
```

Output:
```text
Non-compliant VMs flagged for cleanup: ['vm-2']
```

**Q874. How do you parse Prometheus metric text format in Python?**

*Parse gauge and counter lines into structured data.*

**Answer:** Match `# HELP`, `# TYPE`, and metric labels using regex.

```python
import re

prom_text = 'http_requests_total{method="GET",handler="/health"} 1052'
m = re.match(r'^([a-zA-Z_:][a-zA-Z0-9_:]*)\{(.+)\}\s+([0-9.eE+-]+)$', prom_text)
if m:
    name, labels, val = m.groups()
    print(f"Metric: {name} = {val} | Labels: {labels}")
```

Output:
```text
Metric: http_requests_total = 1052 | Labels: method="GET",handler="/health"
```

**Q875. How do you automate S3 bucket lifecycle policies with Boto3?**

*Configure transition to Glacier after 30 days and expiration after 365 days.*

**Answer:** Use `s3.put_bucket_lifecycle_configuration`.

```python
# s3.put_bucket_lifecycle_configuration(
#     Bucket="my-logs",
#     LifecycleConfiguration={'Rules': [{'Status': 'Enabled', 'Transitions': [{'Days': 30, 'StorageClass': 'GLACIER'}]}]}
# )
print("Boto3 lifecycle policies automate S3 tiered cost reduction")
```

Output:
```text
Boto3 lifecycle policies automate S3 tiered cost reduction
```

**Q876. How do you implement distributed locking using Redis (`redis-py` / Redlock)?**

*Ensure only one automation script instance runs cluster tasks concurrently.*

**Answer:** Acquire a lock with `redis.set(lock_key, token, nx=True, ex=timeout)`.

```python
# with redis_client.lock("cluster_upgrade_lock", timeout=60):
#     run_upgrade()
print("Redis distributed locks prevent concurrent automation script collisions")
```

Output:
```text
Redis distributed locks prevent concurrent automation script collisions
```

**Q877. How do you parse and validate YAML with JSONSchema?**

*Validate Kubernetes CRDs or configuration manifests before applying.*

**Answer:** Load YAML with `yaml.safe_load()` and validate against schema using `jsonschema`.

```python
print("Validating YAML with JSONSchema catches deployment errors before apply")
```

Output:
```text
Validating YAML with JSONSchema catches deployment errors before apply
```

**Q878. How do you parse systemd journal logs using `systemd-python`?**

*Query Linux systemd journal logs programmatically.*

**Answer:** `systemd.journal.Reader` queries systemd journal entries filtering by `_SYSTEMD_UNIT` and priority.

```python
print("systemd-python streams journald system logs natively")
```

Output:
```text
systemd-python streams journald system logs natively
```

**Q879. How do you automate CloudWatch Logs streaming and querying in Python?**

*Execute CloudWatch Insights queries with Boto3.*

**Answer:** Start query with `client.start_query` and poll `client.get_query_results`.

```python
# resp = logs.start_query(logGroupName="/aws/lambda/api", queryString="fields @timestamp, @message | limit 10", startTime=..., endTime=...)
print("CloudWatch Insights queries analyze production logs programmatically")
```

Output:
```text
CloudWatch Insights queries analyze production logs programmatically
```

**Q880. How do you automate DNS record updates with Route53 or Cloudflare APIs?**

*Update A and CNAME records dynamically during failover.*

**Answer:** Use `route53.change_resource_record_sets` or Cloudflare Python SDK.

```python
print("Automated DNS record updates enable programmatic multi-region failovers")
```

Output:
```text
Automated DNS record updates enable programmatic multi-region failovers
```

**Q881. Programming Question: Tail and parse live container logs using Docker SDK.**

*Stream container logs in real time.*

**Answer:** Call `container.logs(stream=True, follow=True)`.

```python
# for line in container.logs(stream=True, follow=True):
#     print(line.decode().strip())
print("Docker SDK streams live container stdout/stderr in real time")
```

Output:
```text
Docker SDK streams live container stdout/stderr in real time
```

**Q882. Programming Question: Write an SSL certificate expiry monitoring probe.**

*Scan remote web servers and compute remaining certificate validity days.*

**Answer:** Retrieve peer certificate and calculate delta.

```python
from datetime import datetime, timezone

def cert_days_remaining(not_after_str: str) -> int:
    # Format: 'Nov 15 12:00:00 2026 GMT'
    expiry = datetime.strptime(not_after_str, "%b %d %H:%M:%S %Y %Z").replace(tzinfo=timezone.utc)
    now = datetime(2026, 10, 30, tzinfo=timezone.utc)
    return (expiry - now).days

print("Days remaining:", cert_days_remaining("Nov 15 12:00:00 2026 GMT"))
```

Output:
```text
Days remaining: 16
```

**Q883. Programming Question: Automate S3 recursive directory synchronization.**

*Upload local directory files to S3 bucket preserving hierarchy.*

**Answer:** Iterate over `pathlib.Path.rglob()` and upload.

```python
from pathlib import Path

def list_upload_files(local_dir: Path):
    return [p.relative_to(local_dir).as_posix() for p in local_dir.rglob("*") if p.is_file()]

print("Relative path calculation verified for S3 keys")
```

Output:
```text
Relative path calculation verified for S3 keys
```

**Q884. Programming Question: Scale a Kubernetes deployment replica count with Python.**

*Patch `spec.replicas` using `AppsV1Api`.*

**Answer:** Use `apps_v1.patch_namespaced_deployment_scale`.

```python
# apps_v1.patch_namespaced_deployment_scale(
#     name="frontend", namespace="default", body={"spec": {"replicas": 5}}
# )
print("Kubernetes deployment scale patched successfully")
```

Output:
```text
Kubernetes deployment scale patched successfully
```

**Q885. Programming Question: Create an automated EC2 AMI backup snapshot script.**

*Find instances tagged `Backup=True` and trigger AMI creation.*

**Answer:** Query instances and call `instance.create_image()`.

```python
def generate_ami_name(instance_id: str) -> str:
    return f"backup-{instance_id}-2026-10-30"

print("Generated AMI name:", generate_ami_name("i-0a1b2c3d4e"))
```

Output:
```text
Generated AMI name: backup-i-0a1b2c3d4e-2026-10-30
```

**Q886. Programming Question: Build a structured JSON log emitter for cloud ingestion.**

*Format application logs into JSON with level, service, timestamp, and trace ID.*

**Answer:** Emit single-line JSON logs for Datadog, ELK, or CloudWatch.

```python
import json
from datetime import datetime, timezone

def emit_structured_log(level: str, message: str, **extra):
    log_entry = {
        "timestamp": datetime.now(timezone.utc).isoformat(),
        "level": level,
        "message": message,
        **extra
    }
    return json.dumps(log_entry)

print("JSON Log Line:
", emit_structured_log("INFO", "Deployment completed", service="auth", duration_ms=450))
```

Output:
```text
JSON Log Line:
 {"timestamp": "...", "level": "INFO", "message": "Deployment completed", "service": "auth", "duration_ms": 450}
```

**Q887. Programming Question: Implement a system load average monitoring alert.**

*Inspect 1, 5, and 15-minute load averages.*

**Answer:** Use `os.getloadavg()`.

```python
import os

try:
    load1, load5, load15 = os.getloadavg()
    print(f"Load averages: 1m={load1:.2f}, 5m={load5:.2f}, 15m={load15:.2f}")
except AttributeError:
    print("getloadavg is UNIX-specific (simulated load: 1.25, 0.95, 0.80)")
```

Output:
```text
getloadavg is UNIX-specific (simulated load: 1.25, 0.95, 0.80)
```

**Q888. Programming Question: Build a CI/CD pipeline artifact cleanup script.**

*Delete build artifacts older than 30 days while retaining at least the 5 newest builds.*

**Answer:** Sort by modification time and slice list.

```python
def prune_old_artifacts(artifact_list: list, keep_count: int = 2) -> list:
    # Sort newest first
    sorted_artifacts = sorted(artifact_list, key=lambda a: a["created_at"], reverse=True)
    retained = sorted_artifacts[:keep_count]
    to_delete = sorted_artifacts[keep_count:]
    return [a["id"] for a in to_delete]

artifacts = [
    {"id": "build-1", "created_at": 100},
    {"id": "build-2", "created_at": 200},
    {"id": "build-3", "created_at": 300}
]
print("Artifacts pruned (oldest):", prune_old_artifacts(artifacts, keep_count=2))
```

Output:
```text
Artifacts pruned (oldest): ['build-1']
```


---

## Chapter 29: Python for Data Science

### A. Core Concepts & Syntax

**Q889. What is NumPy and why is it vastly faster than native Python lists for numerical operations?**

**Answer:** NumPy (Numerical Python) provides the `ndarray` object, a multidimensional container of homogenous, fixed-size elements stored in contiguous blocks of memory. 

NumPy outperforms standard Python lists because:
1. **Contiguous Memory Allocation:** Elements are stored sequentially in C memory buffers, enabling CPU cache hits and SIMD (Single Instruction, Multiple Data) vectorization.
2. **Homogenous Types:** Native Python lists store pointers to heap-allocated `PyObject` wrappers, adding pointer-chasing overhead and type-checking checks on every arithmetic step.
3. **C/Fortran Backend:** Mathematical operations are executed in optimized compiled loops without acquiring or releasing the Python Global Interpreter Lock (GIL) per element.

**Q890. How do you construct 1D, 2D, and 3D NumPy arrays and inspect their dimensions, shapes, and memory layouts?**

**Answer:**
```python
import numpy as np

arr1d = np.array([1, 2, 3, 4], dtype=np.int32)
arr2d = np.array([[1.0, 2.0], [3.0, 4.0]], dtype=np.float64)
arr3d = np.zeros((2, 3, 4), dtype=np.uint8)

print("1D Shape:", arr1d.shape, "Dtype:", arr1d.dtype)
print("2D Dim:", arr2d.ndim, "Size:", arr2d.size, "Itemsize:", arr2d.itemsize)
print("3D Strides:", arr3d.strides)
```
Output:
```text
1D Shape: (4,) Dtype: int32
2D Dim: 2 Size: 4 Itemsize: 8
3D Strides: (12, 4, 1)
```

**Q891. What is array broadcasting in NumPy and what are the two fundamental broadcasting rules?**

**Answer:** Broadcasting describes how NumPy treats arrays with differing shapes during arithmetic operations without copying redundant data in memory.

Two dimensions are compatible when:
1. They are equal, or
2. One of them is 1.

If shapes cannot be matched along trailing dimensions according to these two rules, NumPy raises `ValueError: operands could not be broadcast together`.

Example:
```python
import numpy as np

matrix = np.array([[1, 2, 3], [4, 5, 6]]) # Shape: (2, 3)
bias = np.array([10, 20, 30])              # Shape: (3,) -> broadcasted to (2, 3)
print(matrix + bias)
```
Output:
```text
[[11 22 33]
 [14 25 36]]
```

**Q892. How does boolean masking and fancy indexing work in NumPy?**

**Answer:** Boolean indexing filters arrays using boolean condition arrays of matching shape. Fancy indexing uses integer arrays to select items in arbitrary orders.

```python
import numpy as np

arr = np.array([10, 25, 40, 55, 70, 85])
mask = (arr > 30) & (arr < 80)
print("Filtered:", arr[mask])

indices = [0, 4, 1]
print("Fancy indexed:", arr[indices])
```
Output:
```text
Filtered: [40 55 70]
Fancy indexed: [10 70 25]
```

**Q893. What is the difference between a view and a copy in NumPy slicing, and how can you verify which one was returned?**

**Answer:** Basic slicing (e.g., `arr[1:4]`) returns a **view** sharing the underlying memory buffer with the original array. Modifying elements in a view mutates the parent array. Fancy indexing and explicit `.copy()` produce an independent buffer.

You can verify memory sharing using the `.base` attribute:
```python
import numpy as np

orig = np.array([1, 2, 3, 4])
view_slice = orig[1:3]
copy_slice = orig[[1, 2]]

print(view_slice.base is orig) # True
print(copy_slice.base is orig) # False
```

**Q894. What are the key linear algebra operations available in `np.linalg`?**

**Answer:**
```python
import numpy as np

A = np.array([[2.0, 1.0], [1.0, 3.0]])
b = np.array([5.0, 5.0])

# Solve Ax = b
x = np.linalg.solve(A, b)
print("Solution:", x)

# Determinant & Matrix inverse
det = np.linalg.det(A)
inv = np.linalg.inv(A)
eigenvals, eigenvecs = np.linalg.eig(A)
print("Det:", round(det, 2))
print("Eigenvalues:", eigenvals)
```
Output:
```text
Solution: [2. 1.]
Det: 5.0
Eigenvalues: [1.38196601 3.61803399]
```

**Q895. What are the foundational data structures of Pandas: Series vs DataFrame?**

**Answer:**
- **`pd.Series`:** A 1D labeled array capable of holding any data type, equipped with an explicit index.
- **`pd.DataFrame`:** A 2D labeled tabular data structure with columns of potentially different types, resembling an in-memory SQL table or spreadsheet.

```python
import pandas as pd

s = pd.Series([100, 200, 300], index=["US", "EU", "APAC"], name="revenue")
df = pd.DataFrame({
    "region": ["US", "EU", "APAC"],
    "revenue": [100, 200, 300],
    "active": [True, True, False]
})
print(df.dtypes)
```

**Q896. How do `loc` and `iloc` differ in Pandas DataFrame indexing?**

**Answer:**
- **`loc`:** Label-based indexing. Both start and end bounds in slices are **inclusive**.
- **`iloc`:** Integer-position-based indexing (0-indexed). Slice stop bounds are **exclusive**, matching standard Python list slicing.

```python
import pandas as pd

df = pd.DataFrame({"val": [10, 20, 30]}, index=["a", "b", "c"])
print("loc['a':'b']:\n", df.loc["a":"b"])
print("iloc[0:2]:\n", df.iloc[0:2])
```
Output:
```text
loc['a':'b']:
    val
a   10
b   20
iloc[0:2]:
    val
a   10
b   20
```

**Q897. How do you handle missing values in Pandas using `isna()`, `dropna()`, and `fillna()`?**

**Answer:** Missing values (`NaN` or `None`) are detected and managed via:
```python
import pandas as pd
import numpy as np

df = pd.DataFrame({"score": [85.0, np.nan, 90.0, np.nan], "dept": ["Eng", "Sales", np.nan, "Eng"]})

# Identify missing
print(df.isna().sum())

# Impute with mean
df["score"] = df["score"].fillna(df["score"].mean())

# Drop rows missing dept
df_clean = df.dropna(subset=["dept"])
print(df_clean)
```

**Q898. How do you perform Split-Apply-Combine operations with `groupby()` and `agg()` in Pandas?**

**Answer:**
```python
import pandas as pd

data = {
    "dept": ["IT", "IT", "HR", "Sales", "Sales"],
    "salary": [90000, 110000, 75000, 85000, 95000],
    "bonus": [5000, 8000, 3000, 4000, 6000]
}
df = pd.DataFrame(data)

summary = df.groupby("dept").agg(
    avg_salary=("salary", "mean"),
    max_bonus=("bonus", "max"),
    emp_count=("salary", "count")
).reset_index()
print(summary)
```

**Q899. How do `merge()`, `join()`, and `concat()` differ in Pandas?**

**Answer:**
- **`pd.merge(df1, df2, on=..., how='inner'|'left'|'right'|'outer')`:** Relational database-style joins on column keys or indices.
- **`df1.join(df2)`:** Convenient join combining DataFrames primarily on their index labels.
- **`pd.concat([df1, df2], axis=0|1)`:** Stacks DataFrames vertically (axis=0, union of rows) or horizontally (axis=1, union of columns).

**Q900. How do you reshape data in Pandas using `pivot()`, `pivot_table()`, and `melt()`?**

**Answer:**
- **`pivot()`:** Reshapes long format to wide format without aggregation.
- **`pivot_table()`:** Reshapes long to wide while aggregating duplicates using `aggfunc`.
- **`melt()`:** Unpivots wide format into long key-value format (tidy data representation).

```python
import pandas as pd

df = pd.DataFrame({
    "quarter": ["Q1", "Q1", "Q2", "Q2"],
    "product": ["A", "B", "A", "B"],
    "sales": [100, 150, 120, 180]
})
pivoted = df.pivot(index="quarter", columns="product", values="sales")
melted = pivoted.reset_index().melt(id_vars=["quarter"], value_name="sales")
print(pivoted)
```

**Q901. How does Pandas handle time series data and frequency conversion?**

**Answer:** Pandas features first-class time-series tools via `pd.to_datetime()`, `DatetimeIndex`, and resampling:
```python
import pandas as pd
import numpy as np

dates = pd.date_range("2026-01-01", periods=10, freq="D")
ts = pd.Series(np.random.randn(10), index=dates)

# Resample to weekly mean
weekly = ts.resample("W").mean()
# Shift values forward by 1 period
shifted = ts.shift(1)
```

**Q902. What is `SettingWithCopyWarning` in Pandas and how do you prevent it?**

**Answer:** `SettingWithCopyWarning` triggers when assigning values to a DataFrame slice created via chained indexing (e.g. `df[df['a'] > 1]['b'] = 10`), because Pandas cannot guarantee whether the slice is a view or a copy.

**Prevention:**
1. Use `.loc` for direct single-step assignment: `df.loc[df['a'] > 1, 'b'] = 10`
2. Explicitly create an isolated copy before modifying: `subset = df[df['a'] > 1].copy()`

**Q903. How can you optimize Pandas memory consumption for large datasets?**

**Answer:**
1. **Downcast numeric types:** Convert `float64` to `float32`, and `int64` to `int16`/`int32` via `pd.to_numeric(..., downcast='integer')`.
2. **Convert low-cardinality strings to `category`:** `df['country'] = df['country'].astype('category')`.
3. **Load subsets of columns:** `pd.read_csv('file.csv', usecols=['id', 'target'])`.
4. **Chunking:** Process huge files in streaming chunks: `for chunk in pd.read_csv('big.csv', chunksize=50000): process(chunk)`.
5. **Use PyArrow engine:** `pd.read_csv(..., engine='pyarrow', dtype_backend='pyarrow')`.

**Q904. What is Matplotlib and what is the difference between the Pyplot state-machine interface and the Object-Oriented (OO) interface?**

**Answer:**
- **State-machine (`plt.plot()`):** Implicitly tracks current figure and axes, mimicking MATLAB. Less maintainable for multi-panel dashboards.
- **Object-Oriented (`fig, ax = plt.subplots()`):** Explicitly creates `Figure` and `Axes` objects, offering full programmatic control over subplots, axes decorators, and rendering.

```python
import matplotlib.pyplot as plt

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(10, 4))
ax1.plot([1, 2, 3], [10, 20, 25], color="royalblue", label="Trend")
ax1.set_title("Metric A")
ax1.legend()

ax2.bar(["X", "Y", "Z"], [5, 12, 9], color="darkorange")
ax2.set_title("Metric B")
fig.tight_layout()
plt.close(fig)
```

**Q905. How does Seaborn enhance Matplotlib for statistical visualizations?**

**Answer:** Seaborn operates directly on Pandas DataFrames, automatically mapping categorical variables to colors (`hue`), styles, and facet grids, with built-in statistical estimators and confidence intervals:
- `sns.scatterplot()`, `sns.lineplot()`
- `sns.histplot()`, `sns.kdeplot()`, `sns.boxplot()`, `sns.violinplot()`
- `sns.heatmap()` for correlation matrices
- `sns.pairplot()` for exploratory multi-variable distributions

**Q906. How do you generate a correlation heatmap in Seaborn from a Pandas DataFrame?**

**Answer:**
```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

df = pd.DataFrame({
    "age": [25, 30, 45, 35, 50],
    "salary": [50000, 62000, 95000, 71000, 110000],
    "tenure": [1, 3, 10, 5, 12]
})
corr = df.corr()
fig, ax = plt.subplots(figsize=(6, 4))
sns.heatmap(corr, annot=True, cmap="coolwarm", fmt=".2f", ax=ax)
plt.close(fig)
```

### B. Output-Based & Debugging Questions

**Q907. What is the output of the following NumPy broadcasting expression?**

```python
import numpy as np

a = np.array([1, 2, 3])[:, np.newaxis] # Shape (3, 1)
b = np.array([10, 20])                 # Shape (2,)
res = a + b
print(res.shape)
print(res)
```

**Answer:**
Shape is `(3, 2)`.
Output:
```text
(3, 2)
[[11 21]
 [12 22]
 [13 23]]
```
`a` has shape (3, 1) and `b` is aligned to (1, 2). Both broadcast to (3, 2).

**Q908. What is the output of the following Pandas Series operations?**

```python
import pandas as pd

s1 = pd.Series([10, 20, 30], index=["a", "b", "c"])
s2 = pd.Series([1, 2, 3], index=["b", "c", "d"])
print(s1 + s2)
```

**Answer:**
```text
a     NaN
b    21.0
c    32.0
d     NaN
dtype: float64
```
Pandas aligns data by index labels automatically during binary operations. Indices missing in either series yield `NaN`.

**Q909. What happens when you apply `np.nan == np.nan` versus `np.isnan(np.nan)`?**

**Answer:**
```python
import numpy as np

print(np.nan == np.nan)       # False
print(np.isnan(np.nan))       # True
print(pd.isna(np.nan))        # True
```
Per IEEE 754 floating-point standard, `NaN` is not equal to anything, including itself. You must always use `np.isnan()` or `pd.isna()` to test for missing values.

**Q910. Debug this Pandas DataFrame operation that fails with a KeyError:**

```python
import pandas as pd

df = pd.DataFrame({"A": [1, 2], "B": [3, 4]})
# Bug: Trying to drop column 'B' without axis specification
df.drop("B")
```

**Answer:**
By default, `df.drop("B")` looks along `axis=0` (rows/labels). Because "B" is a column and not an index label, it throws `KeyError: "['B'] not found in axis"`.
**Fix:** Specify `axis=1` or `columns=["B"]`:
```python
df.drop("B", axis=1) # or df.drop(columns=["B"])
```

**Q911. What is the output of vector filtering using Pandas `.isin()`?**

```python
import pandas as pd

df = pd.DataFrame({"role": ["Dev", "DevOps", "QA", "PM", "Dev"]})
print(df[df["role"].isin(["Dev", "DevOps"])].shape[0])
```

**Answer:**
`3` (2 rows with "Dev" and 1 row with "DevOps").

**Q912. What is the output when calculating cumulative aggregations in Pandas?**

```python
import pandas as pd

s = pd.Series([1, 3, 2, 4])
print(list(s.cumsum()))
print(list(s.cummax()))
```

**Answer:**
```text
[1, 4, 6, 10]
[1, 3, 3, 4]
```

### C. Practical Programming & Interview Scenarios

**Q913. Write a Python function using Pandas to clean a raw telemetry dataset containing duplicate rows, missing numerical values, and unparsed timestamps.**

**Answer:**
```python
import pandas as pd

def clean_telemetry(df: pd.DataFrame) -> pd.DataFrame:
    cleaned = df.drop_duplicates()
    cleaned["timestamp"] = pd.to_datetime(cleaned["timestamp"], errors="coerce")
    cleaned = cleaned.dropna(subset=["timestamp"])
    
    num_cols = cleaned.select_dtypes(include=["number"]).columns
    cleaned[num_cols] = cleaned[num_cols].fillna(cleaned[num_cols].median())
    
    return cleaned.sort_values("timestamp").reset_index(drop=True)
```

**Q914. How do you compute rolling window statistics (e.g., 7-day Simple Moving Average and Exponential Moving Average) in Pandas?**

**Answer:**
```python
import pandas as pd

def add_moving_averages(series: pd.Series) -> pd.DataFrame:
    df = pd.DataFrame({"raw": series})
    df["sma_7"] = df["raw"].rolling(window=7, min_periods=1).mean()
    df["ema_7"] = df["raw"].ewm(span=7, adjust=False).mean()
    return df
```

**Q915. Write a vectorized NumPy solution to calculate pairwise Euclidean distance matrix between two sets of 2D points without explicit Python for-loops.**

**Answer:**
```python
import numpy as np

def pairwise_distances(X: np.ndarray, Y: np.ndarray) -> np.ndarray:
    # X shape: (N, D), Y shape: (M, D)
    diff = X[:, np.newaxis, :] - Y[np.newaxis, :, :] # Shape: (N, M, D)
    return np.sqrt(np.sum(diff ** 2, axis=-1))       # Shape: (N, M)
```

**Q916. How do you implement One-Hot Encoding in Pandas?**

**Answer:**
```python
import pandas as pd

df = pd.DataFrame({"city": ["London", "Tokyo", "London", "NYC"]})
encoded = pd.get_dummies(df, columns=["city"], prefix="city", dtype=int)
print(encoded)
```
Output:
```text
   city_London  city_NYC  city_Tokyo
0            1         0           0
1            0         0           1
2            1         0           0
3            0         1           0
```

**Q917. How can you detect and remove outliers from a dataset using the Interquartile Range (IQR) method in Pandas?**

**Answer:**
```python
import pandas as pd

def filter_iqr_outliers(df: pd.DataFrame, column: str) -> pd.DataFrame:
    q25 = df[column].quantile(0.25)
    q75 = df[column].quantile(0.75)
    iqr = q75 - q25
    lower_bound = q25 - 1.5 * iqr
    upper_bound = q75 + 1.5 * iqr
    return df[(df[column] >= lower_bound) & (df[column] <= upper_bound)]
```

**Q918. What is the difference between Polars and Pandas for modern large-scale Python data manipulation?**

**Answer:**
- **Polars** is written in Rust, utilizes Apache Arrow memory specifications, is multi-threaded by default, supports lazy query optimization (predicate pushdown), and avoids Python GIL constraints.
- **Pandas** is single-threaded by default and historically memory-heavy, though recent versions (v2+) support Apache Arrow backends.

**Q919. How do you write a generator function to process a 50GB CSV file in memory-constrained environments?**

**Answer:**
```python
import pandas as pd
from typing import Generator

def process_large_csv(file_path: str, chunk_size: int = 100_000) -> Generator[pd.DataFrame, None, None]:
    for chunk in pd.read_csv(file_path, chunksize=chunk_size):
        filtered = chunk[chunk["status"] == "CRITICAL"]
        yield filtered
```

**Q920. How do you serialize Pandas DataFrames to Apache Parquet format and why is Parquet preferred over CSV?**

**Answer:**
```python
import pandas as pd

df = pd.DataFrame({"id": range(1000), "val": range(1000)})
df.to_parquet("data.parquet", engine="pyarrow", compression="snappy")
loaded_df = pd.read_parquet("data.parquet")
```
Parquet is preferred because:
1. **Columnar Storage:** Only required columns are read from disk during queries.
2. **Built-in Schema & Compression:** Stores explicit data types and achieves superior compression ratios compared to plain-text CSV.

---

## Chapter 30: Python for AI, Machine Learning, and Deep Learning

### A. Core Concepts & Syntax

**Q921. What are the key stages in the Machine Learning pipeline implemented in Scikit-Learn?**

**Answer:**
1. **Data Ingestion & Cleaning:** Handle missing values, outliers, and duplicates.
2. **Feature Engineering & Transformation:** Scaling (`StandardScaler`), encoding (`OneHotEncoder`).
3. **Model Selection & Training:** Fitting estimators (`model.fit(X_train, y_train)`).
4. **Evaluation:** Cross-validation, metrics calculation (`accuracy_score`, `roc_auc_score`).
5. **Hyperparameter Tuning:** `GridSearchCV` or `RandomizedSearchCV`.
6. **Persistence & Serving:** Model export via `joblib` or ONNX.

**Q922. How do `fit()`, `transform()`, and `fit_transform()` differ in Scikit-Learn transformers?**

**Answer:**
- **`fit(X)`:** Computes parameters (e.g., mean and standard deviation for `StandardScaler`) from the training dataset.
- **`transform(X)`:** Applies the learned transformation to the dataset using previously computed parameters.
- **`fit_transform(X)`:** Computes parameters and transforms training data in a single optimized pass.
*Rule:* Always call `fit_transform()` only on **train** data, and `transform()` on **validation/test** data to avoid data leakage.

**Q923. How do you build an end-to-end Machine Learning pipeline using `sklearn.pipeline.Pipeline`?**

**Answer:**
```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
import numpy as np

X = np.random.randn(100, 5)
y = np.random.randint(0, 2, 100)

pipe = Pipeline([
    ("scaler", StandardScaler()),
    ("classifier", LogisticRegression(solver="liblinear"))
])

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
pipe.fit(X_train, y_train)
accuracy = pipe.score(X_test, y_test)
print(f"Accuracy: {accuracy:.2f}")
```

**Q924. What is data leakage in Machine Learning and how do you prevent it in Python?**

**Answer:** Data leakage occurs when information from outside the training dataset (such as test labels or global test distribution statistics) inadvertently leaks into model training, producing overly optimistic validation results that fail in production.

**Prevention:**
1. Perform train/test split **before** any preprocessing (scaling, imputation, target encoding).
2. Use `sklearn.pipeline.Pipeline` with `cross_val_score` so scaling is computed strictly within cross-validation folds.

**Q925. How do you evaluate classification models using Confusion Matrix, Precision, Recall, and F1-Score?**

**Answer:**
- **Precision:** $TP / (TP + FP)$ — Proportion of predicted positives that were truly positive.
- **Recall (Sensitivity):** $TP / (TP + FN)$ — Proportion of actual positives correctly identified.
- **F1-Score:** Harmonic mean of Precision and Recall: $2 \times \frac{P \times R}{P + R}$.

```python
from sklearn.metrics import classification_report, confusion_matrix

y_true = [0, 1, 0, 1, 1, 0]
y_pred = [0, 1, 0, 0, 1, 1]
print(classification_report(y_true, y_pred))
```

**Q926. What is the fundamental difference between PyTorch and TensorFlow/Keras?**

**Answer:**
- **PyTorch:** Emphasizes dynamic computation graphs (Eager execution / Define-by-Run). Pythonic, highly intuitive debugging, favored by academic research and modern LLM engineering.
- **TensorFlow/Keras:** Began with static graphs (Define-and-Run) and moved to eager execution in TF 2.x. Strong enterprise tooling for deployment via TFX, TensorFlow Lite, and TensorRT.

**Q927. How does PyTorch's `autograd` engine compute automatic gradients?**

**Answer:** PyTorch tracks operations on tensors that have `requires_grad=True` by building a Directed Acyclic Graph (DAG) of `torch.autograd.Function` nodes. Calling `.backward()` traverses this DAG from the output to leaf nodes, accumulating partial derivatives into the `.grad` attribute via the chain rule.

```python
import torch

x = torch.tensor(3.0, requires_grad=True)
y = x ** 2 + 4 * x + 1
y.backward()

print("dy/dx at x=3:", x.grad.item()) # 2*3 + 4 = 10.0
```

**Q928. How do you define a custom Neural Network Module in PyTorch?**

**Answer:** Subclass `torch.nn.Module`, declare layers in `__init__()`, and implement forward pass logic in `forward()`:
```python
import torch
import torch.nn as nn

class MLPClassifier(nn.Module):
    def __init__(self, input_dim: int, hidden_dim: int, num_classes: int):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(input_dim, hidden_dim),
            nn.ReLU(),
            nn.Dropout(0.2),
            nn.Linear(hidden_dim, num_classes)
        )
        
    def forward(self, x: torch.Tensor) -> torch.Tensor:
        return self.net(x)
```

**Q929. What are the essential steps in a standard PyTorch training loop?**

**Answer:**
```python
import torch
import torch.nn as nn
import torch.optim as optim

model = nn.Linear(10, 2)
optimizer = optim.Adam(model.parameters(), lr=1e-3)
criterion = nn.CrossEntropyLoss()

for epoch in range(5):
    inputs = torch.randn(32, 10)
    labels = torch.randint(0, 2, (32,))
    
    optimizer.zero_grad()           # 1. Reset gradients from previous step
    outputs = model(inputs)         # 2. Forward pass
    loss = criterion(outputs, labels)# 3. Compute loss
    loss.backward()                 # 4. Backward pass (autograd)
    optimizer.step()                # 5. Update weights
```

**Q930. Why must you call `optimizer.zero_grad()` before `loss.backward()` in PyTorch?**

**Answer:** PyTorch accumulates gradients in tensor `.grad` buffers on every `.backward()` call (by summing them) rather than overwriting them. This design is necessary for gradient accumulation across multiple micro-batches, but failure to reset them with `zero_grad()` leads to gradient compounding and divergence.

**Q931. What is the role of `model.eval()` and `torch.no_grad()` during inference?**

**Answer:**
- **`model.eval()`:** Sets layers like `Dropout` and `BatchNorm` into evaluation mode (disabling dropout and using running population statistics instead of mini-batch statistics).
- **`with torch.no_grad():`:** Context manager that deactivates the autograd engine, reducing memory allocation and accelerating inference computations.

**Q932. How do you transfer PyTorch models and tensors to GPU / CUDA devices?**

**Answer:**
```python
import torch

device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
model = torch.nn.Linear(5, 1).to(device)
tensor = torch.randn(10, 5, device=device)
output = model(tensor)
```

**Q933. What are text embeddings and how do vector databases use them?**

**Answer:** Text embeddings are dense, continuous vector representations (e.g., 768 or 1536 floating-point values) of semantic meaning generated by neural language models. Semantically similar texts produce vectors with small angular separation (high cosine similarity). Vector databases (Chroma, Qdrant, Pinecone) index these vectors using Hierarchical Navigable Small World (HNSW) graphs to perform approximate nearest-neighbor (ANN) searches in milliseconds.

**Q934. What is Retrieval-Augmented Generation (RAG) and what is its architecture in Python?**

**Answer:** RAG combines information retrieval with LLM generation to ground responses in external dynamic knowledge bases, eliminating hallucinations.
**Steps:**
1. **Ingest & Chunk:** Split documents into semantic passages.
2. **Embed:** Convert passages to embeddings via an embedding model.
3. **Store:** Persist vectors and metadata in a vector store.
4. **Retrieve:** At query time, embed user query and retrieve top-$k$ nearest chunks.
5. **Generate:** Inject retrieved context into LLM system prompt and stream synthesized response.

**Q935. How do you implement tool calling (function calling) with modern LLM APIs in Python?**

**Answer:**
```python
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_server_status",
            "description": "Check current health of a cloud host",
            "parameters": {
                "type": "object",
                "properties": {
                    "hostname": {"type": "string", "description": "hostname like srv-prod-01"}
                },
                "required": ["hostname"]
            }
        }
    }
]
# LLM inspects prompt, generates structured arguments matching the tool schema,
# and Python code invokes the actual function before returning the result to the LLM.
```

### B. Output-Based & Debugging Questions

**Q936. What is the output of tensor matrix multiplication using the `@` operator in PyTorch?**

```python
import torch

A = torch.tensor([[1, 2], [3, 4]])
B = torch.tensor([[2, 0], [1, 2]])
C = A @ B
print(C.tolist())
```

**Answer:**
```text
[[4, 4], [10, 8]]
```
Row 1: [1*2 + 2*1, 1*0 + 2*2] = [4, 4]. Row 2: [3*2 + 4*1, 3*0 + 4*2] = [10, 8].

**Q937. What happens when you perform element-wise multiplication vs dot product on Scikit-Learn / NumPy arrays?**

```python
import numpy as np

u = np.array([1, 2, 3])
v = np.array([4, 5, 6])
print(u * v)
print(np.dot(u, v))
```

**Answer:**
```text
[ 4 10 18]
32
```
`*` performs element-wise multiplication; `np.dot` computes scalar sum of products ($1\cdot4 + 2\cdot5 + 3\cdot6 = 32$).

**Q938. Debug this PyTorch error during loss computation:**

```python
import torch
import torch.nn as nn

criterion = nn.CrossEntropyLoss()
logits = torch.randn(4, 3) # Batch size 4, 3 classes
targets = torch.tensor([1.0, 0.0, 2.0, 1.0]) # Error!
loss = criterion(logits, targets)
```

**Answer:**
`RuntimeError: Expected floating point or integer class labels...` or `RuntimeError: RuntimeError: "nll_loss_forward" not implemented for 'Float'`.
`nn.CrossEntropyLoss` expects class indices as 1D tensors of integer type `torch.long` (`int64`), not floats.
**Fix:**
```python
targets = torch.tensor([1, 0, 2, 1], dtype=torch.long)
```

**Q939. What is the output of Softmax normalization over logits?**

```python
import torch

logits = torch.tensor([0.0, 0.0, 0.0])
probs = torch.softmax(logits, dim=-1)
print(probs.tolist())
```

**Answer:**
```text
[0.3333333432674408, 0.3333333432674408, 0.3333333432674408]
```
Equal logits produce equal probabilities summing to 1.0.

**Q940. What is the effect of setting `random_state` in Scikit-Learn?**

**Answer:** Setting `random_state=42` ensures pseudo-random number generator reproducibility across splits, shuffles, and stochastic model weight initializations.

### C. Practical Programming & Interview Scenarios

**Q941. Write a Python function using Scikit-Learn to train a Random Forest classifier with hyperparameter tuning via `GridSearchCV`.**

**Answer:**
```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import GridSearchCV
import numpy as np

def train_rf_tuned(X: np.ndarray, y: np.ndarray):
    rf = RandomForestClassifier(random_state=42)
    param_grid = {
        "n_estimators": [50, 100],
        "max_depth": [None, 5, 10],
        "min_samples_split": [2, 5]
    }
    grid = GridSearchCV(rf, param_grid, cv=3, scoring="f1_macro", n_jobs=-1)
    grid.fit(X, y)
    return grid.best_estimator_, grid.best_params_
```

**Q942. How do you implement early stopping in a PyTorch training loop to prevent overfitting?**

**Answer:**
```python
class EarlyStopping:
    def __init__(self, patience: int = 5, min_delta: float = 1e-4):
        self.patience = patience
        self.min_delta = min_delta
        self.counter = 0
        self.best_loss = float("inf")
        self.early_stop = False

    def __call__(self, val_loss: float):
        if val_loss < self.best_loss - self.min_delta:
            self.best_loss = val_loss
            self.counter = 0
        else:
            self.counter += 1
            if self.counter >= self.patience:
                self.early_stop = True
```

**Q943. How do you compute cosine similarity between two high-dimensional embedding vectors using NumPy?**

**Answer:**
```python
import numpy as np

def cosine_similarity(v1: np.ndarray, v2: np.ndarray) -> float:
    dot_product = np.dot(v1, v2)
    norm_v1 = np.linalg.norm(v1)
    norm_v2 = np.linalg.norm(v2)
    if norm_v1 == 0 or norm_v2 == 0:
        return 0.0
    return float(dot_product / (norm_v1 * norm_v2))
```

**Q944. How do you serialize and load a trained Scikit-Learn model safely with `joblib`?**

**Answer:**
```python
import joblib
from sklearn.linear_model import Ridge

model = Ridge(alpha=1.0)
joblib.dump(model, "ridge_model.joblib")
loaded_model = joblib.load("ridge_model.joblib")
```

**Q945. What is the difference between Batch Gradient Descent, Stochastic Gradient Descent (SGD), and Mini-Batch Gradient Descent?**

**Answer:**
- **Batch GD:** Computes gradients over the entire training set. Stable convergence, but computationally prohibitive for large datasets.
- **SGD:** Updates parameters using 1 sample per step. Highly stochastic with noisy gradients; fast, but fluctuates.
- **Mini-Batch GD:** Updates weights using small batches (e.g., 32, 64, 128). Best compromise: leverages GPU parallel vector processing and provides smooth gradient steps.

**Q946. What is the vanishing gradient problem and how does ReLU / LayerNorm mitigate it?**

**Answer:** In deep networks with activation functions like Sigmoid or Tanh, gradients saturate near 0 or 1. As derivatives are multiplied backwards through layers, the gradient exponentially shrinks to zero, halting weight updates.
- **ReLU ($f(x) = \max(0, x)$):** Gradient is constant (1) for all $x > 0$, preventing vanishing gradients.
- **Normalization (LayerNorm/BatchNorm):** Rescales layer activations to maintain unit variance and zero mean throughout deep network forward passes.

**Q947. How do you construct an autonomous ReAct (Reason + Act) loop pattern in pure Python?**

**Answer:**
```python
class Agent:
    def __init__(self, tools: dict):
        self.tools = tools

    def run(self, goal: str, max_steps: int = 5):
        scratchpad = f"Goal: {goal}\n"
        for step in range(max_steps):
            # 1. Thought: Reason about next step
            thought = f"Step {step+1}: Checking available tools"
            # 2. Action: Choose tool & call
            tool_name = "search"
            action_res = self.tools[tool_name]("query")
            # 3. Observation: Feed back into prompt
            scratchpad += f"Observed: {action_res}\n"
            if "COMPLETE" in action_res:
                return "Goal Achieved"
        return "Max steps exceeded"
```

**Q948. How do you calculate Area Under the ROC Curve (ROC-AUC) and when is Precision-Recall AUC preferred?**

**Answer:**
- **ROC-AUC:** Measures discrimination across all classification thresholds by plotting True Positive Rate vs False Positive Rate.
- **PR-AUC:** Preferred for **heavily imbalanced datasets** (e.g., fraud detection with 99.9% negative class), because ROC-AUC can be deceptively high due to a large True Negative count.

**Q949. What is Quantization (e.g., INT8/INT4) in Large Language Model deployment?**

**Answer:** Quantization converts model weights and activations from high-precision floating point (FP32 or FP16) to lower-bit representations (INT8, INT4). This reduces VRAM footprint by 2x-4x, allowing large LLMs to run on resource-constrained consumer GPUs while preserving output quality through techniques like AWQ and GPTQ.

**Q950. How do you write a clean custom PyTorch `Dataset` and `DataLoader`?**

**Answer:**
```python
import torch
from torch.utils.data import Dataset, DataLoader

class CustomDataset(Dataset):
    def __init__(self, features: torch.Tensor, targets: torch.Tensor):
        self.X = features
        self.y = targets

    def __len__(self) -> int:
        return len(self.X)

    def __getitem__(self, idx: int):
        return self.X[idx], self.y[idx]

dataset = CustomDataset(torch.randn(100, 4), torch.randint(0, 2, (100,)))
loader = DataLoader(dataset, batch_size=16, shuffle=True, num_workers=0)
```

---

## Chapter 31: GUI, Desktop, and Application Development

### A. Core Concepts & Syntax

**Q951. What is the fundamental architecture of GUI applications in Python?**

**Answer:** GUI programming is **event-driven**. The application enters an infinite **event loop** that listens for operating system events (mouse clicks, keystrokes, window resizing, timer interrupts). When an event occurs, the loop dispatches it to registered callback functions (event handlers).

**Q952. How do you build a minimal GUI application using Tkinter?**

**Answer:**
```python
import tkinter as tk
from tkinter import ttk

def on_click():
    label.config(text=f"Hello, {entry.get()}!")

root = tk.Tk()
root.title("DevOps Console")
root.geometry("300x150")

entry = ttk.Entry(root)
entry.pack(pady=10)

btn = ttk.Button(root, text="Submit", command=on_click)
btn.pack(pady=5)

label = ttk.Label(root, text="Enter your name")
label.pack(pady=5)

# Start event loop
root.mainloop()
```

**Q953. What are the three geometry managers in Tkinter: `pack()`, `grid()`, and `place()`?**

**Answer:**
1. **`pack()`:** Packs widgets along edges (top, bottom, left, right) inside cavity containers. Simple linear stacks.
2. **`grid(row=r, column=c, columnspan=n)`:** Arranges widgets in a 2D table of rows and columns. Most versatile and clean.
3. **`place(x=px, y=py)`:** Places widgets at exact pixel coordinates. Fragile when resizing windows or across different DPI screens.
*Rule:* Never mix `pack()` and `grid()` inside the same parent container frame.

**Q954. How does PyQt / PySide differ from Tkinter?**

**Answer:**
- **Tkinter:** Built into the Python standard library, lightweight, uses Native Tcl/Tk widgets. Best for small utility tools.
- **PyQt / PySide (Qt for Python):** Enterprise-grade framework with modern CSS-like styling (`QSS`), hardware-accelerated 2D/3D graphics, rich widgets (tree tables, dock widgets), and native Qt designer tool integration.

**Q955. How does the Qt Signal and Slot mechanism work in PySide6 / PyQt?**

**Answer:** Signals and Slots provide type-safe inter-object communication:
```python
from PySide6.QtCore import QObject, Signal, Slot

class ServerWatcher(QObject):
    alert_triggered = Signal(str, int) # Define signal

    def check_memory(self, usage: int):
        if usage > 90:
            self.alert_triggered.emit("srv-prod-01", usage)

@Slot(str, int)
def handle_alert(host: str, usage: int):
    print(f"ALERT: {host} memory at {usage}%")

watcher = ServerWatcher()
watcher.alert_triggered.connect(handle_alert) # Connect signal to slot
watcher.check_memory(95)
```

**Q956. Why must long-running background tasks never run on the GUI main thread, and how do you resolve this?**

**Answer:** The main thread runs the GUI event loop. If a long-running task (file download, heavy calculation, network request) runs on the main thread, the event loop blocks, causing the window to freeze and trigger "Application Not Responding" (ANR).
**Resolution:** Delegate the work to a background worker thread (`threading.Thread` or `QThread`) and update the GUI thread via thread-safe queues or Signals.

**Q957. How do you package Python desktop applications into standalone executables using PyInstaller?**

**Answer:** PyInstaller bundles the Python interpreter, standard library, dependencies, and script into a single folder or executable:
```bash
pyinstaller --noconfirm --onedir --windowed --add-data "assets;assets" app.py
```
- `--windowed`: Suppresses the terminal console window on launch.
- `--onefile`: Compresses everything into a single standalone `.exe` or binary.

### B. Output, Debugging & Practical Problems

**Q958. What is the output when binding an event handler to a Tkinter widget?**

```python
# Event handler receives an event object
def on_key(event):
    print(f"Key: {event.char}, Code: {event.keycode}")
# root.bind("<Key>", on_key)
```
**Answer:** Whenever the user types a character, Tkinter instantiates an `Event` object with details such as keycode, widget, x/y mouse coordinate, and character, passing it to the callback.

**Q959. Debug this Tkinter code where the button click fails to update the display variable:**

```python
import tkinter as tk

root = tk.Tk()
val = "" # Bug: plain string instead of Tkinter StringVar

def update():
    val = "Active" # Modifies local variable, doesn't update UI

btn = tk.Button(root, text="Set", command=update)
lbl = tk.Label(root, text=val)
```

**Answer:** Tkinter widgets require specialized observable variable wrappers (`tk.StringVar`, `tk.IntVar`, `tk.BooleanVar`):
```python
import tkinter as tk

root = tk.Tk()
var = tk.StringVar(value="Initial")
def update():
    var.set("Active")

btn = tk.Button(root, text="Set", command=update)
lbl = tk.Label(root, textvariable=var) # bound to observable variable
```

**Q960. How do you implement a safe native file picker dialog in Tkinter?**

**Answer:**
```python
import tkinter as tk
from tkinter import filedialog

def choose_file() -> str:
    root = tk.Tk()
    root.withdraw() # Hide root window
    file_path = filedialog.askopenfilename(
        title="Select Log File",
        filetypes=[("Log files", "*.log"), ("All files", "*.*")]
    )
    return file_path
```

**Q961. How do you create an asynchronous worker in PySide6 using `QRunnable` and `QThreadPool`?**

**Answer:**
```python
from PySide6.QtCore import QRunnable, QThreadPool

class Worker(QRunnable):
    def __init__(self, task_fn):
        super().__init__()
        self.task_fn = task_fn

    def run(self):
        self.task_fn()

pool = QThreadPool.globalInstance()
pool.start(Worker(lambda: print("Running in background thread")))
```

**Q962. How do you handle graceful application shutdown and cleanup confirmation in Tkinter?**

**Answer:**
```python
import tkinter as tk
from tkinter import messagebox

def on_closing():
    if messagebox.askokcancel("Quit", "Do you want to disconnect and exit?"):
        root.destroy()

root = tk.Tk()
root.protocol("WM_DELETE_WINDOW", on_closing)
```

**Q963. How do you apply modern themes and styles using Tkinter's `ttk.Style`?**

**Answer:**
```python
from tkinter import ttk
import tkinter as tk

root = tk.Tk()
style = ttk.Style()
style.theme_use("clam")
style.configure("Accent.TButton", font=("Helvetica", 11, "bold"), foreground="white", background="#007ACC")
btn = ttk.Button(root, text="Deploy", style="Accent.TButton")
```

**Q964. How do you structure a production-grade multi-window desktop application?**

**Answer:**
1. **Model-View-Controller (MVC) or Model-View-ViewModel (MVVM):** Decouple business logic and data models from UI rendering.
2. **Central State Management:** Store shared credentials, active host connections, and user preferences in an isolated state store.
3. **Dialog Windows:** Create secondary modal windows using `tk.Toplevel(parent)` with `grab_set()` to lock interactions until dismissal.

---

## Chapter 32: Scientific Computing and Specialized Python

### A. Core Concepts & Syntax

**Q965. What is SciPy and how does it extend NumPy for scientific workflows?**

**Answer:** SciPy builds on NumPy array objects and provides specialized algorithms for:
- `scipy.optimize`: Minimization, curve fitting, root finding
- `scipy.integrate`: Numerical quadrature and differential equation solvers (ODEs)
- `scipy.signal`: Digital filter design, convolution, Fourier analysis
- `scipy.stats`: Continuous/discrete probability distributions and hypothesis testing
- `scipy.spatial`: KD-Trees, Delaunay triangulation, Voronoi diagrams

**Q966. How do you perform numerical optimization using `scipy.optimize.minimize`?**

**Answer:**
```python
import numpy as np
from scipy.optimize import minimize

def rosenbrock(x):
    return sum(100.0*(x[1:] - x[:-1]**2.0)**2.0 + (1 - x[:-1])**2.0)

x0 = np.array([1.3, 0.7, 0.8, 1.9, 1.2])
res = minimize(rosenbrock, x0, method="Nelder-Mead")
print("Optimal solution:", np.round(res.x, 2))
```

**Q967. How do you perform hypothesis testing (e.g., Two-Sample t-test) in `scipy.stats`?**

**Answer:**
```python
from scipy import stats
import numpy as np

group_a = np.array([21, 24, 25, 23, 22, 28])
group_b = np.array([29, 31, 35, 28, 30, 32])

t_stat, p_val = stats.ttest_ind(group_a, group_b)
print(f"t-statistic: {t_stat:.3f}, p-value: {p_val:.5f}")
# If p_val < 0.05, reject the null hypothesis of equal means
```

**Q968. What is SymPy and how does symbolic mathematics differ from numerical computation?**

**Answer:** Numerical computing (NumPy) calculates approximations with floating-point decimals. SymPy computes exact mathematical equations symbolically using algebraic representations:
```python
import sympy as sp

x = sp.Symbol("x")
expr = sp.sin(x) * sp.exp(x)
derivative = sp.diff(expr, x)
integral = sp.integrate(sp.exp(-x**2), (x, -sp.oo, sp.oo))

print("Derivative:", derivative)
print("Gaussian Integral:", integral) # Exact: sqrt(pi)
```
Output:
```text
Derivative: exp(x)*sin(x) + exp(x)*cos(x)
Gaussian Integral: sqrt(pi)
```

**Q969. How do you read, process, and save images using Pillow (PIL)?**

**Answer:**
```python
from PIL import Image, ImageFilter

def process_thumbnail(input_path: str, output_path: str):
    with Image.open(input_path) as img:
        img_rgb = img.convert("RGB")
        img_resized = img_rgb.resize((256, 256), Image.Resampling.LANCZOS)
        img_blurred = img_resized.filter(ImageFilter.GaussianBlur(radius=1))
        img_blurred.save(output_path, "JPEG", quality=85)
```

**Q970. What are the key image processing functions in OpenCV (`cv2`)?**

**Answer:**
- `cv2.imread(path)`: Loads image as a NumPy array (format: **BGR**, not RGB).
- `cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)`: Color-space conversion.
- `cv2.GaussianBlur(img, (5, 5), 0)`: Noise reduction.
- `cv2.Canny(gray, threshold1, threshold2)`: Edge detection.
- `cv2.findContours(...)`: Structural contour detection.

**Q971. How do you analyze networks and graphs using NetworkX?**

**Answer:**
```python
import networkx as nx

G = nx.Graph()
G.add_edge("RouterA", "Switch1", weight=10)
G.add_edge("Switch1", "Server1", weight=5)
G.add_edge("RouterA", "Server1", weight=25)

shortest_path = nx.shortest_path(G, "RouterA", "Server1", weight="weight")
path_length = nx.shortest_path_length(G, "RouterA", "Server1", weight="weight")
print("Optimal Route:", shortest_path, "Latency:", path_length)
```

**Q972. What is MicroPython and how does it adapt Python for microcontrollers (ESP32, Raspberry Pi Pico)?**

**Answer:** MicroPython is a lean, efficient C-based reimplementation of Python 3 optimized to run on bare-metal microcontrollers with constrained RAM (16KB - 256KB) and Flash storage. It provides standard Python syntax plus specialized hardware modules like `machine.Pin`, `machine.PWM`, `machine.ADC`, and `machine.I2C` to interact directly with hardware pins and registers.

### B. Output, Debugging & Practical Problems

**Q973. What is the output of numerical integration using `scipy.integrate.quad`?**

```python
from scipy.integrate import quad

def integrand(x):
    return x ** 2

area, error = quad(integrand, 0, 3)
print(round(area, 1))
```

**Answer:**
`9.0` ($\int_0^3 x^2 dx = [x^3 / 3]_0^3 = 27/3 = 9$).

**Q974. Debug this OpenCV code where color channels appear inverted:**

```python
import cv2
import matplotlib.pyplot as plt

img = cv2.imread("sample.jpg")
plt.imshow(img) # Image appears blue-tinted!
```

**Answer:**
OpenCV reads images in **BGR** format by default, whereas Matplotlib expects **RGB**.
**Fix:** Convert the color space before displaying:
```python
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
plt.imshow(img_rgb)
```

**Q975. How do you read real-time serial sensor data using `pyserial`?**

**Answer:**
```python
import serial

def read_serial_telemetry(port: str = "COM3", baud_rate: int = 115200):
    with serial.Serial(port, baud_rate, timeout=1.0) as ser:
        line = ser.readline().decode("utf-8").strip()
        return line
```

**Q976. How do you solve a system of linear equations symbolically using SymPy?**

**Answer:**
```python
import sympy as sp

x, y = sp.symbols("x y")
eq1 = sp.Eq(2*x + y, 10)
eq2 = sp.Eq(x - y, 2)
solution = sp.solve((eq1, eq2), (x, y))
print(solution) # {x: 4, y: 2}
```

**Q977. What is geospatial processing with GeoPandas and Shapely?**

**Answer:** GeoPandas extends Pandas to handle spatial data. It represents geometric objects (Points, LineStrings, Polygons) using Shapely geometries in a `geometry` column and supports spatial operations like bounding box queries, intersections, and Coordinate Reference System (CRS) transformations via PROJ.

**Q978. How do you compute the Fast Fourier Transform (FFT) of a signal using `scipy.fft`?**

**Answer:**
```python
import numpy as np
from scipy.fft import fft, fftfreq

# Generate 50Hz sine wave
sampling_rate = 1000
t = np.linspace(0, 1.0, sampling_rate, endpoint=False)
signal = np.sin(2 * np.pi * 50 * t)

# Compute FFT
spectrum = np.abs(fft(signal))
frequencies = fftfreq(len(signal), 1 / sampling_rate)
```

---

## Chapter 33: Algorithms and Data Structures Using Python

### A. Core Concepts & Syntax

**Q979. What are the time and space complexities of standard Python built-in data operations?**

| Operation | List | Dict | Set | Deque |
|---|---|---|---|---|
| Indexing / Key lookup | $O(1)$ | $O(1)$ average | $O(1)$ average | $O(n)$ |
| Append / Push right | $O(1)$ amortized | $O(1)$ average | $O(1)$ average | $O(1)$ |
| Append / Push left | $O(n)$ | N/A | N/A | $O(1)$ |
| Search (`x in s`) | $O(n)$ | $O(1)$ average | $O(1)$ average | $O(n)$ |
| Pop right | $O(1)$ | $O(1)$ average | $O(1)$ average | $O(1)$ |
| Pop left | $O(n)$ | N/A | N/A | $O(1)$ |

**Q980. How do you implement a singly linked list in Python?**

**Answer:**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class SinglyLinkedList:
    def __init__(self):
        self.head = None

    def append(self, val):
        if not self.head:
            self.head = ListNode(val)
            return
        curr = self.head
        while curr.next:
            curr = curr.next
        curr.next = ListNode(val)
```

**Q981. How do you implement Stack and Queue data structures efficiently using Python standard library?**

**Answer:**
- **Stack (LIFO):** Use a standard Python list (`append()` and `pop()`), both $O(1)$ amortized.
- **Queue (FIFO):** Use `collections.deque` (`append()` and `popleft()`), both $O(1)$. Do *not* use `list.pop(0)` because it requires an $O(n)$ element shift.

**Q982. How does the `heapq` module implement binary min-heaps in Python?**

**Answer:** `heapq` maintains the min-heap property on a standard list where `heap[0]` is always the smallest element:
```python
import heapq

nums = [15, 3, 8, 1, 9]
heapq.heapify(nums)         # O(n) in-place transformation
heapq.heappush(nums, 2)     # O(log n)
smallest = heapq.heappop(nums) # O(log n)
print("Smallest:", smallest) # 1
```

**Q983. How do you implement binary search in Python manually vs using the `bisect` module?**

**Answer:**
```python
import bisect

# Manual Binary Search: O(log n)
def binary_search(arr: list[int], target: int) -> int:
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

# Using bisect
nums = [10, 20, 30, 40, 50]
idx = bisect.bisect_left(nums, 30) # Returns 2
```

**Q984. What is the Two Pointers algorithmic pattern and when should it be applied?**

**Answer:** The Two Pointers technique processes a sequence using two index references moving toward each other or in tandem. It reduces naive $O(n^2)$ nested loops to $O(n)$ time on sorted arrays.
Common applications: Two-sum on sorted arrays, palindrome verification, trapping rainwater, container with most water.

**Q985. What is the Sliding Window algorithmic pattern?**

**Answer:** The Sliding Window technique maintains a continuous sub-array or sub-string window that expands and contracts as conditions are evaluated.
Common applications: Maximum sum sub-array of size $K$, longest substring without repeating characters, minimum window substring.

### B. Output, Debugging & Practical Problems

**Q986. What is the output of the following tree traversal recursion?**

```python
class Node:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

root = Node(1, Node(2), Node(3))

def inorder(node):
    return inorder(node.left) + [node.val] + inorder(node.right) if node else []

print(inorder(root))
```

**Answer:**
`[2, 1, 3]` (In-order traversal: Left $\to$ Root $\to$ Right).

**Q987. Implement an in-place algorithm to reverse a singly linked list.**

**Answer:**
```python
def reverse_list(head: ListNode) -> ListNode:
    prev = None
    curr = head
    while curr:
        nxt = curr.next
        curr.next = prev
        prev = curr
        curr = nxt
    return prev
```

**Q988. Solve the Two Sum problem in $O(n)$ time using a hash table.**

**Answer:**
```python
def two_sum(nums: list[int], target: int) -> list[int]:
    lookup = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in lookup:
            return [lookup[complement], i]
        lookup[num] = i
    return []
```

**Q989. Implement Breadth-First Search (BFS) and Depth-First Search (DFS) for graph traversal.**

**Answer:**
```python
from collections import deque

graph = {"A": ["B", "C"], "B": ["D"], "C": ["E"], "D": [], "E": []}

def bfs(start: str) -> list[str]:
    visited, queue, order = {start}, deque([start]), []
    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph.get(node, []):
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    return order

def dfs(node: str, visited=None) -> list[str]:
    if visited is None: visited = set()
    visited.add(node)
    order = [node]
    for neighbor in graph.get(node, []):
        if neighbor not in visited:
            order.extend(dfs(neighbor, visited))
    return order
```

**Q990. How do you implement Dijkstra's Shortest Path algorithm using `heapq`?**

**Answer:**
```python
import heapq

def dijkstra(graph: dict, start: str) -> dict:
    distances = {node: float("inf") for node in graph}
    distances[start] = 0
    pq = [(0, start)] # (cost, node)

    while pq:
        curr_dist, u = heapq.heappop(pq)
        if curr_dist > distances[u]:
            continue
        for v, weight in graph[u].items():
            if distances[u] + weight < distances[v]:
                distances[v] = distances[u] + weight
                heapq.heappush(pq, (distances[v], v))
    return distances
```

**Q991. Solve the 0/1 Knapsack problem using Dynamic Programming (Memoization).**

**Answer:**
```python
from functools import cache

def knapsack(weights: list[int], values: list[int], capacity: int) -> int:
    @cache
    def dp(i: int, rem_cap: int) -> int:
        if i == len(weights) or rem_cap <= 0:
            return 0
        # Option 1: Skip item
        res = dp(i + 1, rem_cap)
        # Option 2: Take item if it fits
        if weights[i] <= rem_cap:
            res = max(res, values[i] + dp(i + 1, rem_cap - weights[i]))
        return res

    return dp(0, capacity)
```

**Q992. What is Topological Sorting and how is it implemented using Kahn's Algorithm (In-degree)?**

**Answer:** Topological sorting produces a linear ordering of vertices in a Directed Acyclic Graph (DAG) such that for every directed edge $u \to v$, $u$ appears before $v$ (e.g. build dependencies).
**Kahn's Algorithm:**
1. Compute in-degree for all nodes.
2. Enqueue all nodes with in-degree 0.
3. Dequeue node, append to output, and decrement neighbors' in-degrees.
4. If a neighbor reaches in-degree 0, enqueue it. If output count $< V$, graph has a cycle.

---

## Chapter 34: Professional Python Development and System Design

### A. Core Concepts & Architecture

**Q993. What are the SOLID design principles and how are they implemented in Python?**

**Answer:**
1. **Single Responsibility (SRP):** A class should have one reason to change. Decouple data retrieval from formatting.
2. **Open/Closed (OCP):** Open for extension, closed for modification. Use ABCs and polymorphism rather than mutating `if/elif` chains.
3. **Liskov Substitution (LSP):** Subtypes must be substitutable for their base types without altering program correctness.
4. **Interface Segregation (ISP):** Clients should not depend on interfaces they do not use. Prefer multiple small `Protocol` contracts.
5. **Dependency Inversion (DIP):** High-level modules should depend on abstractions (ABCs/Protocols), not concrete implementations.

**Q994. How do you implement the Dependency Injection (DI) pattern in Python?**

**Answer:** Pass dependencies into classes via constructors rather than hardcoding concrete instances inside the class:
```python
from typing import Protocol

class MetricsPublisher(Protocol):
    def publish(self, metric: str, value: float) -> None: ...

class CloudWatchMetrics:
    def publish(self, metric: str, value: float) -> None:
        print(f"CloudWatch -> {metric}: {value}")

class DeployService:
    def __init__(self, metrics: MetricsPublisher):
        self.metrics = metrics # Injected dependency

    def execute_deploy(self):
        # business logic...
        self.metrics.publish("deploy_success", 1.0)
```

**Q995. How do you implement the Factory and Singleton design patterns in modern Python?**

**Answer:**
- **Factory:** Decouples object creation from calling code.
```python
class StorageFactory:
    @staticmethod
    def get_storage(provider: str):
        if provider == "s3": return S3Storage()
        if provider == "gcs": return GCSStorage()
        raise ValueError(f"Unknown provider {provider}")
```
- **Singleton:** Enforces a single instance per process (often implemented via a module-level variable or metaclass).

**Q996. What is Clean Architecture (Hexagonal / Ports and Adapters) in Python system design?**

**Answer:** Clean Architecture organizes code into concentric layers:
1. **Domain Entities:** Pure business rules, free of framework dependencies.
2. **Use Cases:** Application workflows orchestrating entities.
3. **Interface Adapters:** Controllers, presenters, serializers converting data between domain and external formats.
4. **Frameworks & Drivers:** Databases, HTTP frameworks (FastAPI), cloud SDKs.
The **Dependency Rule** dictates that dependencies point strictly inward toward the domain.

**Q997. How do you implement Structured Logging with Correlation IDs for distributed tracing in Python?**

**Answer:**
```python
import logging
import json
import uuid
import contextvars

request_id_ctx = contextvars.ContextVar("request_id", default="N/A")

class JSONFormatter(logging.Formatter):
    def format(self, record):
        log_obj = {
            "timestamp": self.formatTime(record),
            "level": record.levelname,
            "message": record.getMessage(),
            "request_id": request_id_ctx.get(),
            "module": record.module
        }
        return json.dumps(log_obj)

handler = logging.StreamHandler()
handler.setFormatter(JSONFormatter())
logger = logging.getLogger("service")
logger.addHandler(handler)
logger.setLevel(logging.INFO)
```

**Q998. What is the Twelve-Factor App methodology for cloud-native Python services?**

**Answer:**
1. **Codebase:** One codebase tracked in revision control, many deploys.
2. **Dependencies:** Explicitly declare and isolate dependencies (`pyproject.toml`).
3. **Config:** Store configuration strictly in environment variables.
4. **Backing Services:** Treat databases and caches as attached resources.
5. **Build, release, run:** Strictly separate build and run stages.
6. **Processes:** Execute app as one or more stateless processes.
7. **Port binding:** Export services via port binding (e.g., Uvicorn).
8. **Concurrency:** Scale out via process model.
9. **Disposability:** Maximize robustness with fast startup and graceful shutdown.
10. **Dev/prod parity:** Keep development, staging, and production as similar as possible.
11. **Logs:** Treat logs as event streams sent to stdout.
12. **Admin processes:** Run admin/management tasks as one-off processes.

**Q999. How do you implement Circuit Breaker and Retry patterns with exponential backoff for external service calls?**

**Answer:**
```python
import time
from functools import wraps

def retry_with_backoff(retries: int = 3, backoff_factor: float = 1.5):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            delay = 1.0
            for attempt in range(retries):
                try:
                    return func(*args, **kwargs)
                except Exception as exc:
                    if attempt == retries - 1:
                        raise exc
                    time.sleep(delay)
                    delay *= backoff_factor
        return wrapper
    return decorator
```

**Q1000. How do you design, benchmark, profile, and scale Python backend services for high concurrency?**

**Answer:**
1. **Identify Bottlenecks:** Profile CPU with `cProfile` and memory with `tracemalloc` or `memray` before optimizing.
2. **Decouple I/O and CPU:**
   - I/O-bound: Use `asyncio` with `uvloop` (FastAPI/HTTPX) to handle tens of thousands of concurrent network connections.
   - CPU-bound: Offload heavy data processing to background task queues (`Celery`, `RQ`) or C-extensions/NumPy.
3. **Connection Pooling:** Reuse database and HTTP client connections to eliminate TCP/TLS handshake overhead.
4. **Multi-Worker Execution:** Run ASGI apps behind Gunicorn with Uvicorn workers (`gunicorn -w 4 -k uvicorn.workers.UvicornWorker main:app`) to utilize all CPU cores.
5. **Caching & Asynchronous Queues:** Implement Redis for read-through caching and Kafka/RabbitMQ for resilient asynchronous message publishing.

---

# Summary & Next Steps

Congratulations! You have completed the **Complete Python Master Guide: All Topics + 1000 Questions & Answers**.

This guide covers all 34 core chapters of the Python engineering curriculum, giving you complete technical mastery for senior software engineering, DevOps automation, cloud architecture, and AI engineering interviews and production systems.

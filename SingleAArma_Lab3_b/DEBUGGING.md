# &#x20;Lab 3 Troubleshooting 

## Overview

The objective of this lab was to troubleshoot, debug, and successfully execute  codebase while documenting the debugging process. 

---

## Initial  Analysis

After cloning and exploring the SingleA_Armageddon_VersionB

/SingleAArma_Lab3_b repository, I identified the following project files:

* python_app.py
* script.js
* script.sh
* index.html

The primary troubleshooting was in `python_app.py`.

---

## Initial Error #1 – Syntax Error (Missing Colon)

### Error Encountered

Running:

```bash
python python_app.py
```

Produced:

```text
SyntaxError: expected ':'
```

Location:

```python
else
    pass
```

### Diagnosis

Python identified a syntax issue caused by a missing colon following the `else` statement.

### Fix Applied

Changed:

```python
else
```

To:

```python
else:
```

Commit:

```text
Fix missing colon in else statement
```

---

## Error #2 – Function Definition Syntax Error

### Error Encountered

Python produced:

```text
SyntaxError: expected ':'
```

Location:

```python
def bad_function(x, y)
```

### Diagnosis

Function definitions in Python require a colon at the end of the declaration.

### Fix Applied

Changed:

```python
def bad_function(x, y)
```

To:

```python
def bad_function(x, y):
```

Commit:

```text
Fix missing colon in bad_function definition
```

---

## Error #3 – Statistics Runtime Error

### Error Encountered

Program output:

```text
Error computing stats: unsupported operand type(s) for -: 'list' and 'int'
```

Problematic code:

```python
variance = sum((x - mean)**2 for x in numbers) / len(numbers - 1)
```

### Diagnosis

The expression attempted subtraction against an entire list object instead of subtracting from the list length.

### Fix Applied

Updated to:

```python
variance = sum((x - mean)**2 for x in numbers) / len(numbers)
```

Commit:

```text
Fix variance calculation in stats function
```

---

## Error #4 – Missing Return Values

### Error Encountered

Program output:

```text
cannot unpack non-iterable NoneType object
```

### Diagnosis

The `stats()` function calculated values but did not properly return them.

### Fix Applied

Added:

```python
return mean, variance
```

Commit:

```text
Restore return values from stats function
```

---

## Error #5 – Prime Number Logic Error

### Problem Identified

The program intentionally added:

```python
primes_with_one = [1] + primes
```

This incorrectly classified 1 as a prime number.

### Diagnosis

Mathematically, 1 is not considered prime because it does not have exactly two positive divisors.

### Fix Applied

Removed:

* Incorrect prime list generation
* Incorrect average calculation

Commit:

```text
Remove incorrect prime list and wrong average
```

---

## Error #6 – Non-Executing Loop Logic

### Problem Identified

Code:

```python
for j in range(0, 5, -1):
```

never executed.

### Diagnosis

The loop attempted to move negatively from 0 toward 5, creating an impossible loop condition.

### Fix Applied

Updated the loop range to allow execution.

Commit:

```text
Fix non-executing loop range logic
```

---

## Final Execution Results

After corrections:

* Program executed successfully
* Prime numbers generated correctly
* Statistics calculations completed successfully
* No syntax errors remained
* No runtime errors remained
* Logical inaccuracies were corrected

Final test input:

```text
20
```

Expected output included:

* Correct prime list
* Correct average calculation
* Mean and variance calculation
* Successful loop execution
* No exceptions thrown

---

## Summary

This debugging process involved:

* Syntax troubleshooting
* Runtime debugging
* Logic correction
* Incremental testing
* Meaningful Git commits
* Verification after each modification

The final application executed successfully and met expected functionality requirements.

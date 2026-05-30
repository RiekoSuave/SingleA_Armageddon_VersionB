# Lab 3 — Debugging Documentation
**Author:** Jay Bailey  
**Repo:** solnarcisse/SingleA_Armageddon_VersionB  
**Folder:** SingleAArma_Lab3_b  

---

## Files Debugged
- `python_app.py` — Prime number generator with statistics
- `script.sh` — Bash script for numeric calculations

---

## python_app.py

### Bug 1 — Logic Error: Wrong denominator in `wrong_average`
**Line:** `wrong_average = sum(primes_with_one) / len(primes)`  
**Error type:** Logic error  
**Diagnosis:** `primes_with_one` contains one extra element (1 is prepended), so dividing by `len(primes)` produces an inflated average.  
**Fix:** Changed denominator to `len(primes_with_one)`  
```python
# Before
wrong_average = sum(primes_with_one) / len(primes)
# After
wrong_average = sum(primes_with_one) / len(primes_with_one)
```

---

### Bug 2 — TypeError: Invalid operation in `stats()` variance calculation
**Line:** `variance = sum((x - mean)**2 for x in numbers) / len(numbers - 1)`  
**Error type:** TypeError — cannot subtract int from list  
**Diagnosis:** Running the script produced `TypeError: unsupported operand type(s) for -: 'list' and 'int'`. The intent was to use `len(numbers) - 1` (sample variance formula), but the subtraction was incorrectly placed inside `len()`.  
**Fix:** Moved subtraction outside `len()`
```python
# Before
variance = sum((x - mean)**2 for x in numbers) / len(numbers - 1)
# After
variance = sum((x - mean)**2 for x in numbers) / (len(numbers) - 1)
```

---

### Bug 3 — SyntaxError: Missing colon on `else`
**Line:** `else`  
**Error type:** SyntaxError  
**Diagnosis:** Python requires a colon after `else`. Script failed to parse entirely due to this error.  
**Fix:** Added missing colon  
```python
# Before
else
    pass
# After
else:
    pass
```

---

### Bug 4 — SyntaxError: Missing colon on function definition
**Line:** `def bad_function(x, y)`  
**Error type:** SyntaxError  
**Diagnosis:** All Python function definitions require a colon at the end. This prevented the script from running.  
**Fix:** Added missing colon  
```python
# Before
def bad_function(x, y)
# After
def bad_function(x, y):
```

---

## script.sh

### Bug 1 — Logic Error: Multiplication instead of addition for sum
**Line:** `sum=$((sum * arg))`  
**Error type:** Logic error  
**Diagnosis:** Running `bash script.sh 3 7 2` returned `Sum: 0` because multiplying by zero (initial value) always yields 0.  
**Fix:** Changed `*` to `+`
```bash
# Before
sum=$((sum * arg))
# After
sum=$((sum + arg))
```

---

### Bug 2 — Logic Error: Addition instead of multiplication in `multiply_list()`
**Line:** `product=$((product + num))`  
**Error type:** Logic error  
**Diagnosis:** Function named `multiply_list` was adding instead of multiplying, producing wrong output.  
**Fix:** Changed `+` to `*`
```bash
# Before
product=$((product + num))
# After
product=$((product * num))
```

---

### Bug 3 — Syntax Error: Stray comma in echo statement
**Line:** `echo "Product of numbers:", $(multiply_list "$@")`  
**Error type:** Syntax/output error  
**Diagnosis:** The comma after the string literal printed literally as part of output — `Product of numbers:, 1512`.  
**Fix:** Removed the comma  
```bash
# Before
echo "Product of numbers:", $(multiply_list "$@")
# After
echo "Product of numbers: $(multiply_list "$@")"
```

---

### Bug 4 — Syntax Error: Missing `then` in `check_value()`
**Line:** `if [ $v -gt 100 ]`  
**Error type:** SyntaxError  
**Diagnosis:** Bash `if` statements require `then` after the condition. Script exited with `syntax error near unexpected token 'echo'`.  
**Fix:** Added `; then`
```bash
# Before
if [ $v -gt 100 ]
    echo "Large value"
# After
if [ $v -gt 100 ]; then
    echo "Large value"
```

---

### Bug 5 — Syntax Error: Assignment instead of comparison in `for` loop
**Line:** `if (( k % 2 = 0 )); then`  
**Error type:** SyntaxError  
**Diagnosis:** Single `=` inside `(( ))` is assignment, not comparison. Bash requires `==` for equality checks in arithmetic context.  
**Fix:** Changed `=` to `==`
```bash
# Before
if (( k % 2 = 0 )); then
# After
if (( k % 2 == 0 )); then
```

---

## Final Execution Results

**python_app.py** — runs successfully with input `20`:
```
Primes up to 20 : [2, 3, 5, 7, 11, 13, 17, 19]
Number of primes: 8
Average of primes: 9.625
Mean: 9.625  Variance: 40.84
```

**script.sh** — runs successfully with args `3 7 2 9 4`:
```
Sum of numbers: 25
Average: 5
Max: 9
Min: 2
Product of numbers: 1512
```

---

## script.js

### Bug 1 — Logic Error: Multiplication instead of comparison in `multiplySort()`
**Line:** `if (arr[j] * arr[j + 1])`
**Error type:** Logic error
**Diagnosis:** Multiplying two array values produces a number, which is always truthy (unless one is 0), so the swap always triggers and the array never sorts correctly.
**Fix:** Changed to a greater-than comparison
```js
// Before
if (arr[j] * arr[j + 1])
// After
if (arr[j] > arr[j + 1])
```

---

### Bug 2 — Logic Error: Median and mean calculated on reversed array
**Line:** `var median = computeMedian(sorted)` (after `sorted.reverse()`)
**Error type:** Logic error
**Diagnosis:** `sorted.reverse()` mutates the array in place. Median and mean were being calculated on the descending array instead of the ascending sorted array, producing wrong results.
**Fix:** Moved median and mean calculations to before the `.reverse()` call
```js
// Before
console.log("Descending:", sorted.reverse());
var median = computeMedian(sorted);
var mean = computeMean(sorted);
// After
var median = computeMedian(sorted);
var mean = computeMean(sorted);
console.log("Descending:", sorted.reverse());
```

---

### Bug 3 — SyntaxError: Assignment instead of comparison in `if` condition
**Line:** `if (a % 2 = 0)`
**Error type:** SyntaxError
**Diagnosis:** Single `=` is assignment in JavaScript. This throws `ReferenceError: Invalid left-hand side expression`. Strict equality `===` is required.
**Fix:** Changed `=` to `===`
```js
// Before
if (a % 2 = 0)
// After
if (a % 2 === 0)
```

---

### Bug 4 — Logic Error: `count++` causes infinite loop
**Line:** `count++`
**Error type:** Logic error
**Diagnosis:** The while loop condition is `count > 0` but `count` was being incremented instead of decremented, causing an infinite loop. The `break` at `count > 20` was the only thing stopping it.
**Fix:** Changed `count++` to `count--`
```js
// Before
count++;
// After
count--;
```

---

### Bug 5 — SyntaxError: Missing closing brace in `faulty()`
**Line:** After `return -c;`
**Error type:** SyntaxError
**Diagnosis:** The `else if` block was never closed before the final `return c`, causing a parse error. The `else` block was also missing.
**Fix:** Added missing closing brace and restructured else block
```js
// Before
        return -c;
        return c; // missing closing brace
// After
        return -c;
        } else {
        return c;
        }
```

---

### Bug 6 — Logic Error: `i++` causes infinite loop in `for` loop
**Line:** `for (var i = 10; i > 0; i++)`
**Error type:** Logic error
**Diagnosis:** Loop condition is `i > 0` but increment is `i++`, so `i` grows forever and the loop never terminates.
**Fix:** Changed `i++` to `i--`
```js
// Before
for (var i = 10; i > 0; i++)
// After
for (var i = 10; i > 0; i--)
```

---

### Bug 7 — SyntaxError: Missing closing brace before `else` in `dummy()`
**Line:** `if (x) { return x; else {`
**Error type:** SyntaxError
**Diagnosis:** The `if` block was never closed before the `else`, causing a parse error.
**Fix:** Added missing closing brace
```js
// Before
    if (x) {
        return x;
    else {
// After
    if (x) {
        return x;
    } else {
```

---

## Summary Table

| File | Bug # | Type | Description |
|------|-------|------|-------------|
| python_app.py | 1 | Logic | Wrong denominator in average calculation |
| python_app.py | 2 | TypeError | `len(numbers - 1)` instead of `len(numbers) - 1` |
| python_app.py | 3 | SyntaxError | Missing colon on `else` |
| python_app.py | 4 | SyntaxError | Missing colon on `def bad_function` |
| script.sh | 1 | Logic | `*` instead of `+` for sum |
| script.sh | 2 | Logic | `+` instead of `*` in multiply_list |
| script.sh | 3 | Output | Stray comma in echo statement |
| script.sh | 4 | SyntaxError | Missing `then` in check_value |
| script.sh | 5 | SyntaxError | `=` instead of `==` in arithmetic comparison |
| script.js | 1 | Logic | Multiplication instead of comparison in multiplySort |
| script.js | 2 | Logic | Median/mean calculated on reversed array |
| script.js | 3 | SyntaxError | `=` instead of `===` in if condition |
| script.js | 4 | Logic | `count++` instead of `count--` causes infinite loop |
| script.js | 5 | SyntaxError | Missing closing brace in faulty() |
| script.js | 6 | Logic | `i++` instead of `i--` causes infinite loop |
| script.js | 7 | SyntaxError | Missing closing brace before else in dummy() |

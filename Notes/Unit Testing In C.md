# Unit testing in C

Sources used:

- Embetronicx guide: https://embetronicx.com/tutorials/unit_testing/
- Install C/GCC Compiler for Windows: https://www.digitalocean.com/community/tutorials/c-compiler-windows-gcc

- Unity: https://www.throwtheswitch.org/unity
- Cmock: https://www.throwtheswitch.org/cmock
- Cexception: https://www.throwtheswitch.org/cexception

## Table of
- [Unit testing in C](#unit-testing-in-c)
  - [Table of](#table-of)
  - [1 - Introduction](#1---introduction)
  - [2 - What is Unit Testing?](#2---what-is-unit-testing)
  - [3. Code Coverage](#3-code-coverage)
    - [**3.1: Introuction**](#31-introuction)
    - [**3.2: Code coverage metrics**](#32-code-coverage-metrics)
  - [](#)
  - [4. Ceedling](#4-ceedling)
    - [What is Ceedling?](#what-is-ceedling)
    - [The Three Components of Ceedling](#the-three-components-of-ceedling)
    - [Ceedling’s main value](#ceedlings-main-value)
    - [Installing Ceedling](#installing-ceedling)
  - [5. Unity](#5-unity)
    - [Unity Assertion Functions](#unity-assertion-functions)
      - [(1) Validating Boolean Values](#1-validating-boolean-values)
      - [Example](#example)
      - [(2) Validating Integers](#2-validating-integers)
      - [Example](#example-1)
      - [(3) Validating Hex Values](#3-validating-hex-values)
      - [(4) Validating Bits](#4-validating-bits)
      - [Example](#example-2)
      - [(5) Validating Strings and Pointers](#5-validating-strings-and-pointers)
      - [Example](#example-3)
      - [(6) Validating Structures and Memory](#6-validating-structures-and-memory)
      - [Example](#example-4)
      - [(7) Validating Arrays](#7-validating-arrays)
      - [Example](#example-5)
      - [(8) Message Variants](#8-message-variants)
      - [Example:](#example-6)
      - [Output:](#output)
      - [(9) Ignoring and Forcing Test Failure](#9-ignoring-and-forcing-test-failure)
      - [Example:](#example-7)
      - [(10) Floating Point Validation](#10-floating-point-validation)
      - [Example:](#example-8)
      - [(11) Additional Helper Macros](#11-additional-helper-macros)
    - [Notes](#notes)
    - [Creating a new ceedling project:](#creating-a-new-ceedling-project)
    - [Testing plan:](#testing-plan)
    - [Test Scenarios Overview](#test-scenarios-overview)
    - [Ceedling Project Setup](#ceedling-project-setup)
    - [Unity Test Structure](#unity-test-structure)

## 1 - Introduction
Anyone who has been involved in the software development life cycle (SDLC) for a while, will have encountered some form of testing. In software development, testing helps check if the program works as expected. The goal is to find and fix bugs, but testing can never guarantee that the software is completely error-free.

**Unit testing** focuses on testing small parts of the program (like individual functions or modules) to make sure each one works correctly before combining them into the full system.

<figure style="text-align: center;">
  <img src="https://miro.medium.com/v2/resize:fit:1400/1*GU-YWDqM-ZljYHGs1iDNPg.jpeg" alt="V-Model">
  <figcaption>
    <em>Figure 1: Software Development Life Cycle (SDLC)</em>
  </figcaption>
</figure>


## 2 - What is Unit Testing?
A unit is the smallest part of a program — usually a single function or module.
Unit testing means testing these small parts of code separately to make sure each one works correctly on its own.

It involves writing short scripts or test functions that check whether the actual output of a function matches the expected output.
Each test gives a clear pass or fail result.

**_2.1: When is Unit Testing Done?_** 
- It happens during development, before combining all parts of the program (before integration testing).
- In models like the V-Model of software development, unit testing is the first stage of testing.
  
<figure style="text-align: center;">
  <img src="https://builtin.com/sites/www.builtin.com/files/styles/ckeditor_optimize/public/inline-images/1_v-model.png" alt="V-Model">
  <figcaption>
    <em>Figure 2: V-Model</em>
  </figcaption>
</figure>


**_2.2: Who Performs It?_**
- Usually done by developers, since it requires understanding of how the code works (white-box testing).
- However, QA engineers can also write or run unit tests if needed.
<br></br>

**_2.3: Why Unit Testing Matters_**
- Finds bugs early, before they spread to larger parts of the program
- Makes debugging easier and faster
- Encourages writing clean, modular, and testable code
- Improves confidence when making changes later
<br></br>

**_2.4: Common Misunderstanding_**
- Many developers think unit testing is hard because they struggle to test complex code.
But in most cases, the problem isn’t the test, it’s that the code wasn’t designed to be testable.
- Good unit testing becomes simple when your functions are: small and independent, well-structured with clear input/output
<br></br>

**_2.5: What is the unit testing framework?_**

A unit test framework is just some code/application that makes it easier to run, test the code which we have written, and recorded the results of unit tests.

There are many frameworks that are available for each programming language. Below is a list of tools for our C and Embedded systems platform. You can use any of them for unit testing.
<li>Ceedling</li>
<li>Embunit</li>
<li>MinUnit</li>
<li>Criterion</li>
<li>LCUT</li>

## 3. Code Coverage
### **3.1: Introuction**

Code coverage is a software metric that tells you how much of your source code is executed when your automated test suite runs.
$$\text{Code Coverage} = \frac{\text{Number of Lines Executed}}{\text{Total Number of Lines}} \times 100$$

Think of it as a tool for measuring test effectiveness. If your coverage is high (e.g., 90%), it suggests most of your code has been touched by tests. If it's low (e.g., 40%), it means a large portion of your program remains untested, posing a risk of hidden bugs.

The goal isn't necessarily 100% (which can be costly and difficult to achieve), but to identify critical, complex, or risky code that your tests are missing.

----------

### **3.2: Code coverage metrics**


**(1) Statement (Line) Coverage**

<span style="background-color:#2d2d2d;color:#f5f5f5;padding:2px 6px;border-radius:4px;">What it Measures:</span> The percentage of executable lines or statements in your source code that are run at least once.

<span style="background-color:#2d2d2d;color:#f5f5f5;padding:2px 6px;border-radius:4px;">The Challenge:</span>  It's the weakest metric. It tells you if a line ran, but not how it ran. It gives a false sense of security because it ignores logical branches.

```c
int get_discount(int price, bool is_member) {
    if (is_member) {
        return price * 0.9; // Statement A
    }
    return price; // Statement B
}
```
If you only test `is_member = true`, you execute `Statement A` and `Statement B` (the final return). 100% Statement Coverage is achieved, but you never tested the path where `is_member = false`
<br></br>

--------


**(2) Decision (Branch) Coverage**

<span style="background-color:#2d2d2d;color:#f5f5f5;padding:2px 6px;border-radius:4px;">What it Measures:</span> The percentage of Boolean expressions (decisions in if, while, or for loops) where both the TRUE and FALSE outcomes have been executed.

<span style="background-color:#2d2d2d;color:#f5f5f5;padding:2px 6px;border-radius:4px;">The value:</span>  This is significantly stronger than Statement Coverage. It forces you to write tests that deliberately take both sides of every logical fork in your code.

```c
int get_discount(int price, bool is_member) {
    if (is_member) {
        return price * 0.9; // Statement A
    }
    return price; // Statement B
}
```
To achieve 100% Branch Coverage, you must have two test cases:

Test Case 1: `is_member = true` (Covers the TRUE branch).

Test Case 2: `is_member = false` (Covers the FALSE branch).
<br></br>

--------


**(3) Condition Coverage**

<span style="background-color:#2d2d2d;color:#f5f5f5;padding:2px 6px;border-radius:4px;">What it Measures:</span> The percentage of individual conditions within a compound Boolean expression that evaluate to both TRUE and FALSE at least once.

<span style="background-color:#2d2d2d;color:#f5f5f5;padding:2px 6px;border-radius:4px;">The focus:</span>  It focuses on the internal components of a complex decision, not the overall decision outcome.

```c
void authorize(bool logged_in, bool has_admin_role) {
    if (logged_in || has_admin_role) { // Decision (D) with two conditions (C1, C2)
        // ... grant access ...
    }
}
```
To achieve 100% Condition Coverage, you need tests where:

`logged_in` (C1) is True and False.

`has_admin_role` (C2) is True and False.

| Test Case | logged_in (C1) | has_admin_role (C2) | D Result | CC Achieved?      |
|------------|----------------|----------------------|----------|-------------------|
| Test 1     | T              | F                    | T        | C1: T, C2: F      |
| Test 2     | F              | T                    | T        | C1: F, C2: T      |

Each individual condition (C1 and C2) must be evaluated to both True and False at least once, regardless of the overall decision (D).

`Limitation:` The tests above achieve 100% Condition Coverage, but the decision result (D) was always TRUE. We haven't proven the code works when access is denied!

With just these two cases, we have:

C1 evaluated True (Test 1) and False (Test 2)

C2 evaluated True (Test 2) and False (Test 1)

`So Condition Coverage achieved`

`Why Not TT and FF?`
That doesn’t guarantee each condition was independently evaluated to both True and False while affecting the decision.
You’d get only Decision Coverage, not Condition Coverage.

We avoid TT and FF because they change both conditions together.
We use TF and FT so each condition flips individually, showing its independent effect on the decision.


| Coverage Type               | Required Tests | Goal                                                         |
| --------------------------- | -------------- | ------------------------------------------------------------ |
| Decision Coverage (DC)  | TT, FF         | Each decision outcome (True/False) occurs at least once      |
| Condition Coverage (CC) | TF, FT         | Each condition is True at least once and False at least once |

<br></br>
--------

**(3) Modified Condition/Decision Coverage (MCDC)**

A nice explanation video: https://www.youtube.com/watch?v=DivaWCNohdw

<span style="background-color:#2d2d2d;color:#f5f5f5;padding:2px 6px;border-radius:4px;">What it Measures:</span> The most rigorous logic coverage metric. It requires proving that each condition in a compound decision can independently influence the overall outcome of the decision.

<span style="background-color:#2d2d2d;color:#f5f5f5;padding:2px 6px;border-radius:4px;">The value:</span>  MCDC prevents "masking," where one condition's result hides a bug in another condition. It's often required by regulatory standards for safety-critical software (e.g., in avionics or automotive systems)
```c
void authorize(bool logged_in, bool has_admin_role) {
    if (logged_in || has_admin_role) { // Decision (D) with two conditions (C1, C2)
        // ... grant access ...
    }
}
```
MCDC requires demonstrating, for each condition, two test cases that differ only in that condition's result, leading to a different overall decision result.

| Test Case         | logged_in (C1) | has_admin_role (C2) | D Result | Independence Shown |
|--------------------|----------------|----------------------|----------|--------------------|
| Test 1 (Baseline)  | F              | F                    | F        | Start here         |
| Test 2             | T              | F                    | T        | C1 flipped (F → T), D flipped (F → T). C1 independence shown. |
| Test 3             | F              | T                    | T        | C2 flipped (F → T), D flipped (F → T). C2 independence shown. |


MCDC requires more effort, but provides the highest confidence that complex logical paths are robustly verified.
<br></br>

--------

**Other Common Metrics:**

- `Function Coverage:` Measures whether every function was called at least once.

- `Loop Coverage:` Ensures tests execute loops zero times (bypassed), once, and multiple times (boundary conditions).


--------------
## 4. Ceedling

### What is Ceedling?
Ceedling is a powerful, integrated build system designed to simplify Test-Driven Development (TDD) for C and C++ projects, especially within the embedded systems world.

It works as a Ruby gem that automates all the tedious aspects of unit testing in C—like managing compiler settings, linking, and running tests—so developers can focus purely on writing code and tests.

Ceedling is a complete package because it bundles three best-in-class, open-source projects into one coherent system: Unity, CMock, and CException.

### The Three Components of Ceedling
**1- Unity (The Test Framework)**


Unity is the xUnit-style test framework at the core of Ceedling.

`Function`: Provides the essential assertion macros (e.g., `TEST_ASSERT_EQUAL_INT`, `TEST_ASSERT_TRUE`) that you use to verify your code's behavior.

`Key Detail`: It is written entirely in C, making it highly portable and efficient, which is crucial for resource-constrained embedded systems where complex frameworks often fail. It provides clear "Pass" or "Fail" results for each unit test.

**2- CMock (The Mocking Framework)**


CMock is an automated stub and mock generation tool that handles a critical challenge in unit testing: `isolation`

`The Problem:` When testing a single function (a "unit"), that function often calls other functions (dependencies) that are outside the unit. If the dependent functions have bugs, your unit test might fail even if your unit is correct.

`The Solution (CMock):` CMock automatically generates mock versions of all functions defined in your header files. These mocks replace the actual dependencies during the test. This allows you to:

- Set Expectations: Specify which mock functions must be called, how many times, and with exactly what input parameters.

- Specify Returns: Tell the mock what value it should return to the function under test, simulating specific real-world behaviors or errors.

- Use Callbacks: For complex interactions, define custom C functions to run when the mock is called.
  
**3- CException (The Exception Handler)**

`The Problem:` Standard C uses error codes or return values for error handling, which can clutter code and make error paths difficult to test.

`The Solution (CException):` It allows you to use setjmp and longjmp (standard C library functions) to implement non-local jumps, enabling structured exception handling. This makes error-handling paths much cleaner and easier to test.

`Key Detail:` It's highly portable and can be configured for both single-tasking and multi-tasking environments, making it ideal for systems using an Real-Time Operating System (RTOS).

---------


### Ceedling’s main value
Ceedling automates the entire test build process. As a developer, you simply write your code and your test files (using Unity macros). Ceedling then uses Ruby scripts to:

- Read your source code and headers.

- Generate the necessary mock files using CMock.

- Compile the source code and the test code (often on the host machine).

- Execute the tests and generate a human-readable report.

This automation transforms Test Driven Development (TDD) in C from a complex setup challenge into a smooth, repeatable process.

------------

### Installing Ceedling
**1- Install Ruby DevKit** https://rubyinstaller.org/

When the installer has finished, you can confirm that the installation was successful by opening up a command prompt and running `ruby --version`

You should see something like this:
```sh
C:\Users\ceedling-proj>ruby --version
ruby 2.5.1p57 (2018-03-29 revision 63029) [x64-mingw32]
```

You can also install Ruby on linux using `sudo apt-get install ruby`

**2- Install GCC, follow this tutorial:** https://www.digitalocean.com/community/tutorials/c-compiler-windows-gcc

It installs CodeBlocks because it comes with MinGW and will install gcc automatically, make sure to choose CodeBlocks version with MinGW included.

**3- Add gcc to the environment paths:** for me, because I installed CodeBlocks, so gcc is located here: `C:\Program Files\CodeBlocks\MinGW\bin` -> this is what I add to env variables.

If you have problems with setting the env variables, there is  another method where we would edit gcc path in project.yml whenever we create a ceedling project (will point this out later)


**4- Install Ceedling**
Open a cmd and run the following command: `gem install ceedling`

**5- Confirm ceedling installation:**
Open a cmd and run the following command: `ceedling version`

You should see something like this:
```sh
C:\Users\ceedling-proj>ceedling version
Welcome to Ceedling!
Ceedling:: 0.29.1
CMock:: 2.5.1
Unity:: 2.5.0
CException:: 1.3.2
```
---------------

## 5. Unity
Unity is simply a rich collection of assertions you can use to establish whether your source code behaves the way you think it does. Unity provides a framework to easily organize and execute those assertions in test code separate from your source code. There are many `TEST_ASSERT` functions are available in the Unity framework, which is used to validate the values. We will see each one by one.


### Unity Assertion Functions
####  (1) Validating Boolean Values

These assertions are used to verify logical (true/false) conditions and pointer validity.

| Function | Description |
|-----------|--------------|
| `TEST_ASSERT_TRUE(condition)` | Passes if the condition is **true**, otherwise fails. |
| `TEST_ASSERT(condition)` | Alias of `TEST_ASSERT_TRUE(condition)`. |
| `TEST_ASSERT_FALSE(condition)` | Passes if the condition is **false**, otherwise fails. |
| `TEST_ASSERT_UNLESS(condition)` | Alias of `TEST_ASSERT_FALSE(condition)`. |
| `TEST_ASSERT_NULL(pointer)` | Passes if the pointer is `NULL`. |
| `TEST_ASSERT_NOT_NULL(pointer)` | Passes if the pointer is **not** `NULL`. |

#### Example

```c
int a = 10;

// These pass
TEST_ASSERT( (a > 0) );
TEST_ASSERT_TRUE( (a > 0) );
TEST_ASSERT_UNLESS( (a == 0) );
TEST_ASSERT_FALSE( (a == 0) );

// These fail
TEST_ASSERT( (a == 0) );
TEST_ASSERT_TRUE( (a == 0) );
TEST_ASSERT_UNLESS( (a >> 0) );
TEST_ASSERT_FALSE( (a >> 0) );
```

#### (2) Validating Integers
| Function                                     | Description                                       |
| -------------------------------------------- | ------------------------------------------------- |
| `TEST_ASSERT_EQUAL_INT(exp, act)`            | Compare two signed integers for equality.         |
| `TEST_ASSERT_EQUAL_INT8(exp, act)`           | Compare two **8-bit** signed integers.            |
| `TEST_ASSERT_EQUAL_INT16(exp, act)`          | Compare two **16-bit** signed integers.           |
| `TEST_ASSERT_EQUAL_INT32(exp, act)`          | Compare two **32-bit** signed integers.           |
| `TEST_ASSERT_EQUAL_INT64(exp, act)`          | Compare two **64-bit** signed integers.           |
| `TEST_ASSERT_EQUAL(exp, act)`                | Alias of `TEST_ASSERT_EQUAL_INT`.                 |
| `TEST_ASSERT_NOT_EQUAL(exp, act)`            | Passes if integers are **not equal**.             |
| `TEST_ASSERT_EQUAL_UINT(exp, act)`           | Compare two unsigned integers.                    |
| `TEST_ASSERT_EQUAL_UINT8/16/32/64(exp, act)` | Compare unsigned integers of specific bit widths. |
| `TEST_ASSERT_INT_WITHIN(delta, exp, act)`    | Passes if signed integer is within ±`delta`.      |
| `TEST_ASSERT_UINT_WITHIN(delta, exp, act)`   | Passes if unsigned integer is within ±`delta`.    |
| `TEST_ASSERT_GREATER_THAN(threshold, act)`   | Passes if `act > threshold`.                      |
| `TEST_ASSERT_LESS_THAN(threshold, act)`      | Passes if `act < threshold`.                      |

#### Example

```c
int a = 10;

// Pass
TEST_ASSERT_EQUAL_INT(10, a);
TEST_ASSERT_GREATER_THAN(5, a);
TEST_ASSERT_LESS_THAN(20, a);

// Fail
TEST_ASSERT_EQUAL_INT(5, a);
```
<br></br>

#### (3) Validating Hex Values
| Function                            | Description                |
| ----------------------------------- | -------------------------- |
| `TEST_ASSERT_EQUAL_HEX(exp, act)`   | Compare two hex values.    |
| `TEST_ASSERT_EQUAL_HEX8(exp, act)`  | Compare 8-bit hex values.  |
| `TEST_ASSERT_EQUAL_HEX16(exp, act)` | Compare 16-bit hex values. |
| `TEST_ASSERT_EQUAL_HEX32(exp, act)` | Compare 32-bit hex values. |
| `TEST_ASSERT_EQUAL_HEX64(exp, act)` | Compare 64-bit hex values. |

<br></br>
#### (4) Validating Bits
| Function                           | Description                                          |
| ---------------------------------- | ---------------------------------------------------- |
| `TEST_ASSERT_BITS(mask, exp, act)` | Compare selected bits in two values using a bitmask. |
| `TEST_ASSERT_BITS_HIGH(mask, act)` | Check that masked bits are all **high**.             |
| `TEST_ASSERT_BITS_LOW(mask, act)`  | Check that masked bits are all **low**.              |
| `TEST_ASSERT_BIT_HIGH(bit, act)`   | Check that bit `n` (0–31) is high.                   |
| `TEST_ASSERT_BIT_LOW(bit, act)`    | Check that bit `n` (0–31) is low.                    |

#### Example

```c
int act = 0x12FF;

/* Pass — bits match */
TEST_ASSERT_BITS(0xF, 0xFFFFFFFF, act);

/* Fail — bits don’t match */
TEST_ASSERT_BITS(0xF00, 0xFFFFFFFF, act);

/* Pass — masked bits are high */
TEST_ASSERT_BITS_HIGH(0xF, act);

/* Fail — bits 8–11 not high */
TEST_ASSERT_BITS_LOW(0xF00, act);

/* Pass — bit 0 is high */
TEST_ASSERT_BIT_HIGH(0, act);

/* Pass — bit 8 is low */
TEST_ASSERT_BIT_LOW(8, act);

```
<br></br>

#### (5) Validating Strings and Pointers
| Function                                         | Description                             |
| ------------------------------------------------ | --------------------------------------- |
| `TEST_ASSERT_EQUAL_PTR(exp, act)`                | Compare pointer addresses for equality. |
| `TEST_ASSERT_EQUAL_STRING(exp, act)`             | Compare two null-terminated strings.    |
| `TEST_ASSERT_EQUAL_STRING_LEN(exp, act, len)`    | Compare first `len` characters.         |
| `TEST_ASSERT_EQUAL_STRING_ARRAY(exp, act, elem)` | Compare arrays of strings.              |

#### Example
```c
char a[] = "EMBETRONICX";
char *b = a;
char c[] = "embetronicx";

// Pass
TEST_ASSERT_EQUAL_PTR(a, b);
TEST_ASSERT_EQUAL_STRING(a, b);
TEST_ASSERT_EQUAL_STRING_LEN(a, b, 3);

// Fail
TEST_ASSERT_EQUAL_STRING(a, c);

```

<br></br>

#### (6) Validating Structures and Memory

Compares raw memory regions or entire structures..

`Note:` Be careful of compiler padding in structures,
even identical members may differ if padding bytes vary.

| Function                                  | Description                    |
| ----------------------------------------- | ------------------------------ |
| `TEST_ASSERT_EQUAL_MEMORY(exp, act, len)` | Compare `len` bytes of memory. |

#### Example
```c
typedef struct {
    int a;
    int b;
} temp;

temp a_struct = {10, 20};
temp b_struct = {10, 20};
temp c_struct = {5, 20};

char a[] = "Embetronicx";
char *b = a;
char c[] = "embetronicx";

// Pass
TEST_ASSERT_EQUAL_MEMORY(a, b, 5);
TEST_ASSERT_EQUAL_MEMORY(&a_struct, &b_struct, sizeof(temp));

// Fail
TEST_ASSERT_EQUAL_MEMORY(&a_struct, &c_struct, sizeof(temp));
TEST_ASSERT_EQUAL_MEMORY(a, c, 5);

```

<br></br>

#### (7) Validating Arrays
Used to compare arrays element-by-element across all data types.

| Function                                                 | Description                      |
| -------------------------------------------------------- | -------------------------------- |
| `TEST_ASSERT_EQUAL_INT_ARRAY(exp, act, elem)`            | Compare signed integer arrays.   |
| `TEST_ASSERT_EQUAL_UINT_ARRAY(exp, act, elem)`           | Compare unsigned integer arrays. |
| `TEST_ASSERT_EQUAL_HEX_ARRAY(exp, act, elem)`            | Compare hex arrays.              |
| `TEST_ASSERT_EQUAL_PTR_ARRAY(exp, act, elem)`            | Compare pointer arrays.          |
| `TEST_ASSERT_EQUAL_STRING_ARRAY(exp, act, elem)`         | Compare string arrays.           |
| `TEST_ASSERT_EQUAL_INT8/16/32/64_ARRAY(exp, act, elem)`  | Compare sized integer arrays.    |
| `TEST_ASSERT_EQUAL_UINT8/16/32/64_ARRAY(exp, act, elem)` | Compare sized unsigned arrays.   |
| `TEST_ASSERT_EQUAL_HEX8/16/32/64_ARRAY(exp, act, elem)`  | Compare sized hex arrays.        |


#### Example
```c
unsigned int a0[] = {1, 8, 567, 120};
unsigned int a1[] = {1, 8, 567, 120};
unsigned int a2[] = {1, 8, 567, 10};

// Pass
TEST_ASSERT_EQUAL_INT_ARRAY(a0, a1, 4);
TEST_ASSERT_EQUAL_INT_ARRAY(a0, a2, 3);
TEST_ASSERT_EQUAL_UINT_ARRAY(a0, a1, 4);
TEST_ASSERT_EQUAL_HEX32_ARRAY(a0, a1, 4);

// Fail
TEST_ASSERT_EQUAL_INT_ARRAY(a0, a2, 4);
TEST_ASSERT_EQUAL_UINT_ARRAY(a0, a2, 4);
TEST_ASSERT_EQUAL_HEX32_ARRAY(a0, a2, 4);

```

<br></br>

#### (8) Message Variants
All assertions can have a `_MESSAGE` version to display custom failure messages.

| Function                                          | Description                        |
| ------------------------------------------------- | ---------------------------------- |
| `TEST_ASSERT_EQUAL_INT_MESSAGE(exp, act, msg)`    | Custom message if test fails.      |
| `TEST_ASSERT_EQUAL_STRING_MESSAGE(exp, act, msg)` | Custom message if string mismatch. |


#### Example:
```c
int a = 10;

// Will fail and print message
TEST_ASSERT_EQUAL_INT_MESSAGE(13, a, "Test Failed: 'a' should be 13");
```

#### Output:
```sh
Expected 13 Was 10. Test Failed: 'a' should be 13
```

<br></br>

#### (9) Ignoring and Forcing Test Failure

| Function                   | Description                           |
| -------------------------- | ------------------------------------- |
| `TEST_IGNORE()`            | Skips the current test.               |
| `TEST_IGNORE_MESSAGE(msg)` | Skips the test with a reason message. |
| `TEST_FAIL()`              | Forces immediate failure.             |
| `TEST_FAIL_MESSAGE(msg)`   | Fails test with custom message.       |


#### Example:
```c
TEST_IGNORE_MESSAGE("Feature not implemented yet");
// or
TEST_FAIL_MESSAGE("Critical logic broken!");
```

<br></br>

#### (10) Floating Point Validation
| Function                                     | Description                      |
| -------------------------------------------- | -------------------------------- |
| `TEST_ASSERT_EQUAL_FLOAT(exp, act)`          | Compare two floats for equality. |
| `TEST_ASSERT_FLOAT_WITHIN(delta, exp, act)`  | Pass if within tolerance.        |
| `TEST_ASSERT_EQUAL_DOUBLE(exp, act)`         | Compare doubles.                 |
| `TEST_ASSERT_DOUBLE_WITHIN(delta, exp, act)` | Pass if within tolerance.        |

#### Example:
```c
float v = 3.1415;

// Pass
TEST_ASSERT_FLOAT_WITHIN(0.01, 3.14, v);

// Fail
TEST_ASSERT_FLOAT_WITHIN(0.0001, 3.14, v);
```


<br></br>

#### (11) Additional Helper Macros

| Macro                     | Description                         |
| ------------------------- | ----------------------------------- |
| `RUN_TEST(func)`          | Runs a specific test function.      |
| `setUp()`                 | Setup before each test (optional).  |
| `tearDown()`              | Cleanup after each test (optional). |
| `TEST_GROUP(name)`        | Defines a test group.               |
| `TEST_GROUP_RUNNER(name)` | Defines a group runner.             |
<br></br>

### Notes

- Unity stops executing a test after the first failed assertion in that test function.

- Always structure tests to isolate one concept or path per function.

- Use Ceedling’s automation to generate mock files and test reports automatically.
----------

### Creating a new ceedling project:
Create the project using `ceedling new proj_name` using the command terminal (command prompt) in your desired directory (folder).

```sh
C:\Users\ceedling-proj>ceedling new simple_prog
Welcome to Ceedling!
      create  simple_prog/project.yml
Project 'simple_prog' created!
 - Execute 'ceedling help' to view available test & build tasks
```

### Testing plan:
We want to create unit tests for a function called `do_bit_man(uint8_t position)` that manipulates bits in three global variables:  
`Jill`, `Jung`, and `Jukk` (each 8-bit).
The tests should verify:
1. **Negative scenarios** – invalid inputs (e.g., out-of-range bit positions)
2. **Positive scenarios** – valid bit positions (0–7)
3. **Expected behavior** – correct return values and unchanged global states when invalid inputs occur

### Test Scenarios Overview

| **Scenario** | **Input (position)** | **Expected Output** | **Expected Global Changes** | **Type** |
|---------------|----------------------|----------------------|------------------------------|----------|
| 1 | Greater than 7 | Return `-1` | No variable modified | Negative |
| 2 | Less than 0 | Return `-1` | No variable modified | Negative |
| 3 | Between 0 and 7 | Return valid result (bit manipulated) | Proper bit changes applied | Positive |


### Ceedling Project Setup
We’ll use Ceedling to create our test module.  
In your project folder (where `project.yml` exists), run:

```bash
cd simple_prog
ceedling module:create[bit_manipulation]
```

This automatically creates:
```sh
src/bit_manipulation.c
src/bit_manipulation.h
test/test_bit_manipulation.c
```

Note: We’ll write unit tests first (test-driven development) before implementing the actual source code logic.

### Unity Test Structure
When Ceedling generates a test file, it includes a default template:
```c
#include "unity.h"
#include "bit_manipulation.h"

void setUp(void) {}
void tearDown(void) {}

void test_bit_manipulation_NeedToImplement(void)
{
    TEST_IGNORE_MESSAGE("Need to Implement bit_manipulation");
}

```
You can delete this placeholder test and replace it with your real test cases.


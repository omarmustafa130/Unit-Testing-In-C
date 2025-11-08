# Unit testing in C

Credits:

Embetronicx guide: https://embetronicx.com/tutorials/unit_testing/unit-testing-in-c-introduction/

## 00 - Introduction
Anyone who has been involved in the software development life cycle (SDLC) for a while, will have encountered some form of testing. In software development, testing helps check if the program works as expected. The goal is to find and fix bugs, but testing can never guarantee that the software is completely error-free.

**Unit testing** focuses on testing small parts of the program (like individual functions or modules) to make sure each one works correctly before combining them into the full system.

<figure style="text-align: center;">
  <img src="https://miro.medium.com/v2/resize:fit:1400/1*GU-YWDqM-ZljYHGs1iDNPg.jpeg" alt="V-Model">
  <figcaption>
    <em>Figure 1: Software Development Life Cycle (SDLC)</em>
  </figcaption>
</figure>


## 01 - What is Unit Testing?
A unit is the smallest part of a program — usually a single function or module.
Unit testing means testing these small parts of code separately to make sure each one works correctly on its own.

It involves writing short scripts or test functions that check whether the actual output of a function matches the expected output.
Each test gives a clear pass or fail result.

**_1.1: When is Unit Testing Done?_** 
- It happens during development, before combining all parts of the program (before integration testing).
- In models like the V-Model of software development, unit testing is the first stage of testing.
  
<figure style="text-align: center;">
  <img src="https://builtin.com/sites/www.builtin.com/files/styles/ckeditor_optimize/public/inline-images/1_v-model.png" alt="V-Model">
  <figcaption>
    <em>Figure 2: V-Model</em>
  </figcaption>
</figure>


**_1.2: Who Performs It?_**
- Usually done by developers, since it requires understanding of how the code works (white-box testing).
- However, QA engineers can also write or run unit tests if needed.
<br></br>

**_1.3: Why Unit Testing Matters_**
- Finds bugs early, before they spread to larger parts of the program
- Makes debugging easier and faster
- Encourages writing clean, modular, and testable code
- Improves confidence when making changes later
<br></br>

**_1.4: Common Misunderstanding_**
- Many developers think unit testing is hard because they struggle to test complex code.
But in most cases, the problem isn’t the test, it’s that the code wasn’t designed to be testable.
- Good unit testing becomes simple when your functions are: small and independent, well-structured with clear input/output
<br></br>

**_1.5: What is the unit testing framework?_**

A unit test framework is just some code/application that makes it easier to run, test the code which we have written, and recorded the results of unit tests.

There are many frameworks that are available for each programming language. Below is a list of tools for our C and Embedded systems platform. You can use any of them for unit testing.
<li>Ceedling</li>
<li>Embunit</li>
<li>MinUnit</li>
<li>Criterion</li>
<li>LCUT</li>

## 02. Code Coverage
**_2.1: Introuction_**

Code coverage is a software metric that tells you how much of your source code is executed when your automated test suite runs.
$$\text{Code Coverage} = \frac{\text{Number of Lines Executed}}{\text{Total Number of Lines}} \times 100$$

Think of it as a tool for measuring test effectiveness. If your coverage is high (e.g., 90%), it suggests most of your code has been touched by tests. If it's low (e.g., 40%), it means a large portion of your program remains untested, posing a risk of hidden bugs.

The goal isn't necessarily 100% (which can be costly and difficult to achieve), but to identify critical, complex, or risky code that your tests are missing.

----------

**_2.2: Code coverage metrics_**


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
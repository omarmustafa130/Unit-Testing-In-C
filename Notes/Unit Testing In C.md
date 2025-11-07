# Unit testing in C

Credits:

Embetronicx guide: https://embetronicx.com/tutorials/unit_testing/unit-testing-in-c-introduction/

## Introduction
Anyone who has been involved in the software development life cycle (SDLC) for a while, will have encountered some form of testing. In software development, testing helps check if the program works as expected. The goal is to find and fix bugs, but testing can never guarantee that the software is completely error-free.

**Unit testing** focuses on testing small parts of the program (like individual functions or modules) to make sure each one works correctly before combining them into the full system.

<figure style="text-align: center;">
  <img src="https://miro.medium.com/v2/resize:fit:1400/1*GU-YWDqM-ZljYHGs1iDNPg.jpeg" alt="V-Model">
  <figcaption>
    <em>Figure 1: Software Development Life Cycle (SDLC)</em>
  </figcaption>
</figure>


## What is Unit Testing?
A unit is the smallest part of a program — usually a single function or module.
Unit testing means testing these small parts of code separately to make sure each one works correctly on its own.

It involves writing short scripts or test functions that check whether the actual output of a function matches the expected output.
Each test gives a clear pass or fail result.

**_When is Unit Testing Done?_** 
- It happens during development, before combining all parts of the program (before integration testing).
- In models like the V-Model of software development, unit testing is the first stage of testing.
  
<figure style="text-align: center;">
  <img src="https://builtin.com/sites/www.builtin.com/files/styles/ckeditor_optimize/public/inline-images/1_v-model.png" alt="V-Model">
  <figcaption>
    <em>Figure 2: V-Model</em>
  </figcaption>
</figure>


**_Who Performs It?_**
- Usually done by developers, since it requires understanding of how the code works (white-box testing).
- However, QA engineers can also write or run unit tests if needed.
<br></br>

**_Why Unit Testing Matters_**
- Finds bugs early, before they spread to larger parts of the program
- Makes debugging easier and faster
- Encourages writing clean, modular, and testable code
- Improves confidence when making changes later
<br></br>

**_Common Misunderstanding_**
- Many developers think unit testing is hard because they struggle to test complex code.
But in most cases, the problem isn’t the test, it’s that the code wasn’t designed to be testable.
- Good unit testing becomes simple when your functions are: small and independent, well-structured with clear input/output

<br></br>
**_What is the unit testing framework?_**

A unit test framework is just some code/application that makes it easier to run, test the code which we have written, and recorded the results of unit tests.

There are many frameworks that are available for each programming language. Below is a list of tools for our C and Embedded systems platform. You can use any of them for unit testing.
<li>Ceedling</li>
<li>Embunit</li>
<li>MinUnit</li>
<li>Criterion</li>
<li>LCUT</li>


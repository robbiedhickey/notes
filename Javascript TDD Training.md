# Javascript TDD Training

> How tests can help you write better code. Given by Jason Dalton https://thehub.thomsonreuters.com/videos/25808

### What is TDD?

A development process that relies on short development cycles where the first sequence is to write your test first. 

![TDD Process](http://i.imgur.com/m2MZkQg.png)

### Benefits of TDD

* The suite of unit tests provides constant feedback that each component is still working
* The unit tests act as documentation that cannot be out of date, unlike separate documentation which can and frequently is
* When the test passes and the production code is refactored to remove duplication, it is clear that the code is finished, and the developer can move on to a new test. 
* TDD forces critical analysis and design because the developer cannot create the production code without truly understanding what the desired result should be and how to test it
* The software tends to be better designed. That is, loosely coupled and easily maintainable, because the developer is free to make design decisions and refactor at any time with confidence that the software is still working. This confidence is gained by running the tests. The need for a design pattern may emerge, and the code can be changed at that time.
* The test suite acts as a regression safety net on bugs: if a bug is found, the developer should create a test to reveal the bug and then modify the production code so that the bug goes away and all other tests pass. On each successive test run, all previous bug fixes are verified.
* Reduced debugging time!

### Keys for TDD

* keep the unit tests small
* Use the KISS principles in writing and organizing code
* Run the process is isolation. Simulates environmental dependencies
* Use real data when you can in your mocks but don't sacrifice speed for it
* Run all tests to verify your changes
* Clearly reveals its intentions

### Downside of TDD

* Integration testing
	* Doesn't necessarily find the integration related bugs
	* Your TDD tests will probably not be end-to-end. 
* Will not find all bugs in the system
* Design may change constantly
* Some code may be difficult to isolate and test
* It can be hard to get started with. When starting with greenfield not knowing design, trying to write tests without knowing what to implement

### Processes based on TDD

* Behavior-driven development (BDD)
* Acceptance test-driven development (ATDD)
* Story-driven development (SDD)

## Examples

Sample application shows a list of teams from NFL and some European soccer league. Clicking a team reveals some information about the team in the center content pane. 

I can't finish this... the quality of the video is too poor to see the code. 
# Readings: Unit Testing and Documentation
## Unit Testing Best Practices
* Unit tests also don’t count as other sorts of tests.  If you create some sort of test that throws thousands of requests for a service you’ve written, that qualifies as a smoke test and not a unit test.  Unit tests don’t generate random data and pepper your application with it in unpredictable sequences.  They’re not something that QA generally executes.
* And, finally, unit tests don’t exercise multiple components of your system and how they act.  If you have a console application and you pipe input to it from the command line and test for output, you’re executing an end-to-end system test — not a unit test.
* In C#, you can think of a unit as a method.  You thus write a unit test by writing something that tests a method.  Oh, and it tests something specific about that method in isolation.  Don’t create something called TestAllTheThings and then proceed to call every method in a namespace.
* Now, you might think that you could create a method for each test and call it from the main in CalculatorTester.  That would improve things over creating a project for each test, but only pain waits down that road (more than a decade ago, I used to test this way, before ubiquitous test runners existed).  Adding a method and call for each test will prove laborious, and tracking the output will prove unwieldy.
* Lean heavily on the scientific method to understand the real idea here.  Consider your test as a hypothesis and your test run as an experiment.  In this case, we hypothesize that the add method will return 7 with inputs of 4 and 3.
* Arrange, Act, Assert
* One Assert Per Test Method
* Avoid Test Interdependence
* Keep It Short, Sweet, and Visible
* Recognize Test Setup Pain as a Smell
* Add Them to the Build
## Art of README
*  One-liner explaining the purpose of the module
* Necessary background context & links
* Potentially unfamiliar terms link to informative sources
* Clear, runnable example of usage
* Installation instructions
* Extensive API documentation
* Performs cognitive funneling
* Caveats and limitations mentioned up-front
* Doesn't rely on images to relay critical information
* License
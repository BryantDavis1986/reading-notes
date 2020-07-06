# Readings: Exception Handling
## How to debug for absolute beginners
* Clarify the problem by asking yourself the right questions
* What did you expect your code to do?
* What happened instead?
* Are you using the right API (that is, the right object, function, method, or property)? An API that you're using might not do what you think it does. (After you examine the API call in the debugger, fixing it may require a trip to the documentation to help identify the correct API.)
* Are you using an API correctly? Maybe you used the right API but didn't use it in the right way.
* Does your code contain any typos? Some typos, like a simple misspelling of a variable name, can be difficult to see, especially when working with languages that don’t require variables to be declared before they’re used.
* Did you make a change to your code and assume it is unrelated to the problem that you're seeing?
* Did you expect an object or variable to contain a certain value (or a certain type of value) that's different from what really happened?
* Do you know the intent of the code? It is often more difficult to debug someone else's code. If it's not your code, it's possible you might need to spend time learning exactly what the code does before you can debug it effectively.
## How to use the try/catch block to catch exceptions
* Place any code statements that might raise or throw an exception in a try block, and place statements used to handle the exception or exceptions in one or more catch blocks below the try block. Each catch block includes the exception type and can contain additional statements needed to handle that exception type.

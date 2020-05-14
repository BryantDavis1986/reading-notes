# Read: 09 - Refactoring
## Functional Programming Concepts
* “Complexity is anything that makes software hard to understand or to modify." — John Outerhout
* Functional programming is a programming paradigm — a style of building the structure and elements of computer programs — that treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data — Wikipedia
* o how do we know if a function is pure or not? Here is a very strict definition of purity:
It returns the same result if given the same arguments (it is also referred as deterministic)
It does not cause any observable side effects
* Why is this an impure function? Simply because it uses a global object that was not passed as a parameter to the function.
* If our function reads external files, it’s not a pure function — the file’s contents can change.
* The code’s definitely easier to test. We don’t need to mock anything. So we can unit test pure functions with different contexts:
Given a parameter A → expect the function to return value B
Given a parameter C → expect the function to return value D
* Immutability
Unchanging over time or unable to be changed.
## Refactoring Javascript for Readability
* Here are some straightforward to implement methods that can lead to easier to read code. There are no absolutes when it comes to clean code — there's always an edge case!
* Return early from functions:
* Cache variables so functions can be read like sentences:
* Check for Web APIs before implementing your own functionality:
* It's important to get your code right the first time because in many businesses there isn't much value in refactoring. Or at least, it's hard to convince stakeholders that eventually uncared for codebases will grind productivity to a halt.
* 
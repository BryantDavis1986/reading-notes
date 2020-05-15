# Read: 10 - The Call Stack and Debugging
## The Call Stack defined on MDN
* A call stack is a mechanism for an interpreter (like the JavaScript interpreter in a web browser) to keep track of its place in a script that calls multiple functions — what function is currently being run and what functions are called from within that function, etc.
* When a script calls a function, the interpreter adds it to the call stack and then starts carrying out the function.
* Any functions that are called by that function are added to the call stack further up, and run where their calls are reached.
* When the current function is finished, the interpreter takes it off the stack and resumes execution where it left off in the last code listing.
* If the stack takes up more space than it had assigned to it, it results in a "stack overflow" error.
## Understanding the JavaScript Call Stack
* The call stack is primarily used for function invocation (call). Since the call stack is single, function(s) execution, is done, one at a time, from top to bottom. It means the call stack is synchronous
* The understanding of the call stack is vital to Asynchronous programming (which we will look at in a later article).
* LIFO: When we say that the call stack, operates by the data structure principle of Last In, First Out, it means that the last function that gets pushed into the stack is the first to be pop out, when the function returns.
* Temporarily store: When a function is invoked (called), the function, its parameters, and variables are pushed into the call stack to form a stack frame. This stack frame is a memory location in the stack. The memory is cleared when the function returns as it is pop out of the stack.
* Manage function invocation (call): The call stack maintains a record of the position of each stack frame. It knows the next function to be executed (and will remove it after execution). This is what makes code execution in JavaScript synchronous.
* A stack overflow occurs when there is a recursive function (a function that calls itself) without an exit point. The browser (hosting environment) has a maximum stack call that it can accomodate before throwing a stack error.
## JavaScript error messages
* Most of your time as a developer is spent reading code followed by debugging that same code, most likely to be able to read it or solve an “unexpected feature” (which, joking aside, is more correctly known as a “bug”).
* For those already used to programming, reading an error message is like second nature, for everybody else, is something you learn either you like it or not so might as well talk a bit about each of them.
* Reference errors
This is as simple as when you try to use a variable that is not yet declared you get this type os errors.
* Syntax errors
I know it’s in the name of the errors, but like it says itself, this occurs when you have something that cannot be parsed in terms of syntax, like when you try to parse an invalid object using JSON.parse.
* Range errors
Try to manipulate an object with some kind of length and give it an invalid length and this kind of errors will show up.
* Type errors
Like the name indicates, this types of errors show up when the types (number, string and so on) you are trying to use or access are incompatible, like accessing a property in an undefined type of variable.
* 
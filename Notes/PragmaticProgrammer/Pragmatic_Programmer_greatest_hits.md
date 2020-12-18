# The Pragmatic Programmer: Cliff Notes

## Chapter 1

### Software Entropy and broken windows

## Broken window Theory

According to [Wikipedia](https://en.wikipedia.org/wiki/Broken_windows_theory):

&gt; The broken windows theory is a criminological theory that states that visible signs of crime, anti-social behavior, and civil disorder create an urban environment that encourages further crime and disorder, including serious crimes. The theory suggests that policing methods that target minor crimes, such as vandalism, loitering, public drinking, jaywalking and fare evasion, help to create an atmosphere of order and lawfulness, thereby preventing more serious crimes.

### What's it have to do with development?

According to the theory written above a building with just one broken window has a higher probability of
being further vandalized when compared to a building without a broken window.
Chaos gives way to more chaos but on the flip side when people see order they tend to feel that breaking the order would be bad.

A code base with a few ‘broken windows’ (i.e sloppy patches, workarounds and other ‘temporary’ code that accidentally becomes part of the core) will encourage continued bad behavior by people who make changes to the code base.

&gt; “This component is a piece of garbage anyways, I am not doing any harm by writing code equally shitty code.”

This is an example of the mindset that members of a team may think subconsciously.
If we enforce having a clean codebase and no ‘windows’ are broken someone making changes to the codebase
would hopefully see how well written it is and want to maintain that order and stray away from chaos.

Unfortunately you will always have ‘broken windows’ and not so pretty sections of code but here is a quick list of some ideas to enforce to turn start fixing broken windows.

 - If pressed for time and a fix is a little substandard leave a `// TODO:` comment as a reminder to fix things up later.

 - Be proactive in fixing warnings and `TODO` comments, create issues and review periodically. Don’t let them pile up.

 - Analyze the git log and see which files have the most bug fixes. Is this a symptom of a greater disease?

 - Avoid tight coupling of components, use principles of DRY and orthogonality.


 ## Good enough Software

 - Rather than deliver a “perfect” piece of software one year from now deliver something minimal soon and iterate the software through a feedback process whereas the client uses the software they can understand what they really need and what is practical rather than what they envisioned before seeing the real thing in action.

 - Know when to stop, make a clear quality requirements document. Perfect software has never existed and never will. A crash free rate of 99.8 percent has different consequences for a chat client than it does autopilot or an anti-lock breaking system. Chat client users who experience a crash are annoyed, users who experienced a bug in the ABS software could be ex-users. Don’t waste time testing a chat client as if it could cause loss of life but do make sure the software is stable enough to release.

## Chapter 2

## DRY
Don’t Repeat Yourself... ’nuf said.

DRY tries to avoid code duplication, code duplication is frowned upon because it opens the door inviting problems like having the same code written in multiple places and later forgetting to update some code sections with new behavior when the spec changes.

All code should have a single source of truth and knowledge should not be broken up between modules in the code.

Of course in practice this is not perfectly enforceable because in order to use a library or API the code in the calling module / client needs to know it’s interface in order to use it.
The lesson here is to have the smallest surface area possible exposed between an API or library and the module(s) / client code that use it.

## Orthogonality

When code internal to one module changes does it require updates to others?
If the answer is yes, then chances are the section that changed is not orthogonal to the rest of the codebase.

Orthogonality is the idea that different modules of the codebase can be changed freely without affecting other areas of the code.

i.e A change to the model should not require the UI to be updated most times and vice versa.

Orthogonality also similar to the DRY principal where knowledge should be compartmentalized and not repeated
or broken up between various components.

i.e UI presentation and text formatting code should not be split between the model, the presentation code and the UI
it should live in its own module so that a change in the way text is formatted and presented to the user does not
require a change to the data model.

## Prototypes and ‘Tracer Bullets’

### Prototypes are commonly used to explore a single aspect of a final system and are thrown away at the end.
The point of a prototype is to learn, not to have something reusable at the end.

According to pragmatic programmer some prime examples of things to prototype are:

 - Architecture
 - Performance issues
 - New functionality in a system
 - Structure or contents of external data
 - Third party tools or components
 - Performance issues

### Tracer Bullets
As the book defines:
&gt; Tracer bullets work because they operate in the same environment and under the same constraints as the real bullets. They get to the target fast, so the gunner gets immediate feedback. And from a practical standpoint they’re a relatively cheap solution.

Tracer bullets are simplified but working stripped down examples that can be further iterated upon.
Unlike prototypes tracer bullets are not thrown away, they are fleshed out until it evolves into the final system and hit the target.

You can use tracer bullets to write the skeleton of a system and all the modules that compose it.
Write the bare minimum of code required for data to be passed from one module to the next and iteratively create the final release software.

Advantages of the “Tracer Bullet” approach:

 - Users get to see the software early / You have something to demo
 - Developers build a Structure to work in.
  - You have a better feel for progress.


## Chapter 3

### Power of plain text
I don’t think this one needs too much explaining but the book certainly does.
Basically stray away from proprietary binary formats and stick with plain text for any data that is stored medium to long term.
Tools that can read or even the documentation of a custom binary file format may one day break and the knowledge of the format may be
lost or the documentation of the last format version could potentially have never been updated and if just one bit is shifted the entire file is unreadable. Non-Plain text formats require that EVERY DETAIL is know of the file format in order to parse it.
Plain text like ASCII or UTF-8 on the other hand can be edited and read in plain text almost future proofing the file format.

Even if the file is in a plain text format there is still a major point to consider.

 - Human Readable does not guarantee Human Understandable

Just because one can read the characters does not mean someone can make out what it means so try to use data formats that are both human readable and human Understandable so that in ten years when the binary format that you use to store info’s only existing tool no longer will run on a modern OS and machine the end user can still open the file and read it to make sense of it and even edit it with confidence.

### Debugging

#### Don't Panic (Think: Hitchhiker’s Guide to the Galaxy)
Even if you are on a tight deadline and behind schedule and the bug looks like a major setback, just stay calm.
Panic can and will cloud your judgement so do what you can get yourself relaxed and in a calm state of mind.
Forget about what needs to be done by End of Day in addition to this bug fix, forget about how spending time fixing this bug will delay progress on ticket x.
Just focus on finding out what when wrong and what the fix might look like.


#### Fix the problem, not the blame.

It does not matter who accidentally introduced the bug, blame only wastes time and the author of the bug most likely is aware of their mistake.
The bug is the entire team's problem, its everybody’s problem so just focus on the patch instead of pointing the finger.

#### Interview the user and gain as much context as you can
Most times a bug report does not contain enough context to properly reproduce the bug.
Going to the person who found the bug and asking them to reproduce the bug will often reveal details that the author of the report didn’t know were important.

The author of the report may have written that when they use the brush tool in the drawing software the app crashes but when the developer of that feature uses it is fine.
The author of the tool then goes to the author of the bug report and finds that when they only tried using the brush tool and drawing from the lower left corner of the screen to the upper right which does not produce a crash everything was fine but when drawing from the upper right DOWN to the lower left the software crashes.

#### Be able to reproduce the bug 100% of the time before you write one line of the fixe
Usually being able to force the bug 100% of the time will reveal the root cause of the bug.
Naturally if you can artificially produce the bug with 100% accuracy you can write a test to cause the failure and code against that test case until the bug is clear.

#### RTFEM: Read the fucking error message
’nuf said

#### Not an error but bad values
Maybe the code is behaving the way it is supposed to and returning a proper result.
Before you waste time looking for a bug make sure that your input data is correct.

i.e Is the interest in reality a range of 0.6 ~ 1.3 percent but the function is getting past a bunk value of 6% for the interest rate?

#### Binary Chop / Binary Search
If you find yourself looking at a very long stack trace to the binary chop / binary search to find the offending call.
Cut the stack in half and check the lower half, is the offending call in here? No? The move to the upper half, rinse and repeat.

# Chapter 4

## Design by Contract (DBC)

- Design by Contract defines the rights and responsibilities of a piece of software to ensure program correctness.
- What is a correct program? One that does no more and no less than what it claims to do.
- Powerful principal where the author of a function states very clearly what they can promise IFF they receive input matching a specific criterion.
- As a trivial example when writing a square root function you could have the contract allow negative numbers and return imaginary solutions or you could be stricter and say only non-negative numbers are accepted and in return you promise a correct non-negative solution.
- DBC forces you to think!

### Components of DBC

- Preconditions: What does the code require to be true of the input in order to provide a meaningful output?
- Post conditions: What output is promised to the user? What state of the world is guaranteed at the end of the routine?
- Class Invariants: This condition is always true from the perspective of the caller. What things are promised to remain constant when the function is entered and exited? In the middle of computation this invariant can change but by the time control returns to the caller of the function the invariant must hold true once again.

### DBC and TDD
Design by Contract is still useful even if you do TDD because they are different approaches to program correctness.
TDD is a good technique but to tends to make you focus on the "Happy Path" or the path where things go well and your tests may focus on that instead of what could go wrong like in the real world.

- DBC defines the parameters for success and failure in all cases whereas TDD can only target one case at a time.
- TDD and other testing only gets triggered during a "test time" (maybe as part of continuous integration) during the build cycle but DBC and assertions are always active during design, development, deployment and most importantly maintenance.
- DBC checks internal invariants whereas TDD only checks the code black-box style via its public interface.

#### 'Err in favor of the consumer'
Make sure to separate logic in code that are semantic invariants from logic that is subject to change via policy, think of semantic invariants as a philosophical contract.

An example of a semantic invariant in a piece of software would be the simple principle that users will never be billed twice for the same transaction. This is something that will never change and should be clearly documented and enforced at all levels, bake this into the code with assertions and fail early before this horrible event happens.

An example of a rule that must be enforced but is not a semantic invariant is something like the current sales tax rate or service fee.
Management could decide tomorrow to raise the service fee from 10% to 12% on a whim or politicians can suddenly raise sales tax from 8% to 10% these things can happen in the real world but you will never expect the consumer to pay twice for a single transaction.

### Assertive Programming: Dead programs tell no lines
Crash yourself before you trash yourself!
Once program state enters an "impossible" state anything the software does after that is not meaningful and can be quite harmful.
Databases can be filled with junk data, customers can suddenly find mystery charges on their accounts and good data like the
user's coveted files, game save data or bookmarks can be corrupted.

- Instead of saying "This can never happen" write an assertion to check it.
- Don't use Assertions in place of real error handling, a blog post should always have an associated author but a resource being inaccessible is possible under bad network conditions this is just simple error handling.
- Be careful that your assertions do not create side effects, make sure that when observing that a condition is true that you do not change the state of the very thing you are observing.
- Leave assertions on, especially in production. Just because the code you are shipping has no failing test today there is no guarantee that the tests have not tested a certain code path that will occur in the wild, in production.


# Chapter 5

### Decoupling
- Decoupled code is easier to change, harder to break.
- Code reuse should not be a goal as much as the process of thinking that allows code to be reusable. Code reuse happens when interfaces are clearly defined and decoupled from the rest of the code.


 ### Things to Avoid
 - Train Wrecks: Avoid writing code that is a chain of function calls where any of the calls can easily change.
  - Globalization creates large scale coupling of any components that depend on global variables or singletons.
  - Inheritance: subclassing is dangerous, use interfaces / protocols and mix-ins instead.

 ### Don’t Chain Method calls
 Unless the methods are very unlikely to change like `map`, `filter`, `reduce` chaining method
 calls together is a ticking time bomb for maintenance.
  Eventually someone is going to change one of those methods and it might not be as obvious to cause a compilation error.
  Its entirely possible that the behavior of one or more of the methods being chained will change over time and when it does it will no longer behave in a way that methods being called down stream are expecting it to result in no warnings or errors from the compiler but undefined and strange program behavior.

 ### The evils of Globalization

 - Adding a global variable is as if every single function in your application gains an additional parameter but it is even worse than that! Unlike a parameter, a global variable is not visible in the function signature so even after removing the global it is difficult to quickly track down which code has been dependent on it.

 - A change to a global requires a change to all the code throughout the code base that relied on it.

 ### Its important enough to be global, wrap it up!

 Globalization is not only about using global variables, any data that is shared between modules is global be it a database, an API, Configuration etc.
 This is not always a bad thing because there are sometimes not many alternatives but make sure that you wrap the global resource in some kind of code that can control access to it and that nothing is touching the global data directly.


### Transforming Programming

&gt; If you can’t describe what you are doing as a process, you don’t know what you’re doing.
&gt; - W. Edwards Deming, (attr)
&gt; (David, Thomas; Hunt Andrew. The Pragmatic Programmer (p. 234). Pearson Education. Kindle Edition.

A great mental tool for software design is to think of everything as a transformation.
Break any process down into steps taking an input and then producing an output that becomes the input for the next function.
Everything is just a sub task of the larger task of transforming input such as touches from the user into a desired result i.e a payment transaction.

 - Don’t hoard state, pass it around. This pillar of functional programming leads to some amazingly stable code, every function in the pipeline gets a copy or reference of the data that is at the very least promised to be atomic.

 - Model transformations with something similar to the `|&gt;` pipe left operator where it is simple and easy to trace the input and output between functions in series. Even if the language you use does not support pipe-left like operators you can still clearly break the result of one operation between different lines passing it as a constant to the next.
 
 ### Pipes &amp; Pipe-Lefts
'pipe-left' operators use a very familier concept and is the same as the concept of pipes in a command line like `zsh` or `bash`.

`wget -q -O - https://raw.githubusercontent.com/Nirma/blog/master/Notes/PragmaticProgrammer/Pragmatic_Programmer_greatest_hits.md | grep -i software | wc -l`

This has three stages: fetch the contents of **this** document and feed it into `stdin`; search for any lines contanting the word "software" ignoring case and finally pipe the input to `wc` and count the number of lines.
The takeaway here is that each stage is only concerned with what its input and flags are and what it is required to output, this is a great example of separation of concerns and an example of how we can strive to build software that can be changed easily and sub-components that will stand the test of time. (for a little while.)



 ### Inheritance Tax
 - Inheritance is coupling by definition.
  - Inheritance introduces coupling on many levels.
  - The sub class is not only coupled to the parent but the code that uses the subclass also tied to the parent class.

 #### Alternatives to using Inheritance
 - Protocols &amp; Interfaces
 - Delegation
 - Mixins and Traits
 - "Has A" is more important than is-a, does an object DO a certain thing vs what IS the object. This is true of both delegation and using protocols.


# Chapter 6

Chapter six deals mostly with concurrency.
While this topic is just as important as other fundamentals such as Design Patterns, Data Structures and Algorithms it would best serve the reader to 
learn concurrency models that deal with their specific platform but if you have time Chapter six is very good so instead of summarizing chapter six here I will omit it and encourage you to read chapter six in the book. 

# Chapter 7 

 ### Listen to your lizard brain
 - Instincts make you feel, not think and when you 'feel' something it is most likely the result of accumulated experience. Listen to it, but think about what it means. 
 - Distance yourself from a problem and come back to it again later if you feel that the approach is harder than it needs to be. Chances are you your instincts are right and after giving your subconscious sometime to push the problem through various layers of your brain you might come up with a simpler solution to the problem at hand. 
 
 ### Mind Hack
 The book details a simple but powerful trick to get you past the black editor screen. 
 When you feel "stuck" and don't know how to approach a difficult problem start writing "prototype" code i.e coding something that is only for the purpose 
 of learning. This removes the distraction from the code having to be "right" and be done well and instead allows you to focus only on figuring out how to solve the problem with code. Once you have a working prototype or example of how to code something then you can think about how to make the code clear and simple and pretty. While making the prototype feel free to 
 
 ### Test your code, or your users will
 - Tests should be the first users of your code. 
 - Even if you do not write tests thinking of how to make the code you are writing testable has the benefit of making the software you are writing modular and keeps you off the path of writing large monolithic functions. 
 - Unit tests are always the first line of defense, making sure that code contained in a module behaves as expected needs to be done and pass before testing code that relies on the module, fail fast, fail early. 
 - 100% test coverage means nothing, just because every line of code is executed in the tests it does not mean all cases have been tested.
    - Don't become enslaved by TDD, TDD can help but also can be overdone and get in the way of software development.
    
 ### Names
 - Name well; Rename when needed.
     - If you can't come up with a good name for something, a name that makes sense then what you are trying to do has a high probability of also not making sense.
     - Be consistent
 - Follow common conventions of the language and programming environment you are using. 
 
 
 
 # Chapters 8 &amp; 9 (There is a lot of overlap, it would be smoother to combine them here.)
 
 “`
 "I’ve never met a human being who would want to read 17,000 pages of documentation, and if there was, I’d kill him to get him out of the gene pool. Joseph Costello, President of Cadence"

David, Thomas; Hunt Andrew. The Pragmatic Programmer (p. 392). Pearson Education. Kindle Edition. 
“`
 
 ### Agile is not a noun, it is how you do things
 - Agile is a method of responding to change and doing so frequently. 
 - There is no "out of the box" solution that will work for any team.
   - Anyone who claims to be able to sell you a "Solution" is a liar, the agile process needs to derived by the team.
   - Saying "Other AAA tech companies do this" means nothing, if Netflix is currently doing agile via method "X" now when they are a company that has thousands of employees it does not mean that the current method would have worked for Netflix when they were only 50 employees and does not mean they will still be using the same method next year. 
 - Agile solutions need to be custom tailored to how the team and company operate and don't forget its only about responding to change in near realtime not any particular dogma.
   - One size fits no one well.
   
 ### Afterthoughts on testing
 
 - The benefit of writing tests is the effect it has on writing the code it tests, therefore the same person who writes the code should also be responsible for the tests. 
 - The disadvantage is that if the test and the code both have a wrong assumption about the underlying logic then the issue will go undetected.
   - Use property based testing to fill this gap.
   - State Coverage, not test coverage
 - Use property based testing (i.e QuickCheck-ish testing libraries)
 - Property based testing lets the computer generate random values and test that a certain property holds true.
   - Property based testing is extremely economical, the alternative would be to write various tests to state by hand. 
 
 
 
# The Pragmatic Programmer: Cliff Notes

## Chapter 1

### Software Entropy and broken windows

## Broken window Theory

According to [Wikipedia](https://en.wikipedia.org/wiki/Broken_windows_theory):

&gt; The broken windows theory is a criminological theory that states that visible signs of crime, anti-social behavior, and civil disorder create an urban environment that encourages further crime and disorder, including serious crimes. The theory suggests that policing methods that target minor crimes, such as vandalism, loitering, public drinking, jaywalking and fare evasion, help to create an atmosphere of order and lawfulness, thereby preventing more serious crimes.

### What's it have to do with development?

According to the theory written above a building with just one broken window has a higher probability of
being further vandalized when compared to a building without a broken window.
Chaos gives way to more chaos but on the flip side when people see order they tend to feel that breaking the order would be bad.

A code base with a few ‘broken windows’ (i.e sloppy patches, workarounds and other ‘temporary’ code that accidentally becomes part of the core) will encourage continued bad behavior by people who make changes to the code base.

&gt; “This component is a piece of garbage anyways, I am not doing any harm by writing code equally shitty code.”

This is an example of the mindset that members of a team may think subconsciously.
If we enforce having a clean codebase and no ‘windows’ are broken someone making changes to the codebase
would hopefully see how well written it is and want to maintain that order and stray away from chaos.

Unfortunately you will always have ‘broken windows’ and not so pretty sections of code but here is a quick list of some ideas to enforce to turn start fixing broken windows.

 - If pressed for time and a fix is a little substandard leave a `// TODO:` comment as a reminder to fix things up later.

 - Be proactive in fixing warnings and `TODO` comments, create issues and review periodically. Don’t let them pile up.

 - Analyze the git log and see which files have the most bug fixes. Is this a symptom of a greater disease?

 - Avoid tight coupling of components, use principles of DRY and orthogonality.


 ## Good enough Software

 - Rather than deliver a “perfect” piece of software one year from now deliver something minimal soon and iterate the software through a feedback process whereas the client uses the software they can understand what they really need and what is practical rather than what they envisioned before seeing the real thing in action.

 - Know when to stop, make a clear quality requirements document. Perfect software has never existed and never will. A crash free rate of 99.8 percent has different consequences for a chat client than it does autopilot or an anti-lock breaking system. Chat client users who experience a crash are annoyed, users who experienced a bug in the ABS software could be ex-users. Don’t waste time testing a chat client as if it could cause loss of life but do make sure the software is stable enough to release.

## Chapter 2

## DRY
Don’t Repeat Yourself... ’nuf said.

DRY tries to avoid code duplication, code duplication is frowned upon because it opens the door inviting problems like having the same code written in multiple places and later forgetting to update some code sections with new behavior when the spec changes.

All code should have a single source of truth and knowledge should not be broken up between modules in the code.

Of course in practice this is not perfectly enforceable because in order to use a library or API the code in the calling module / client needs to know it’s interface in order to use it.
The lesson here is to have the smallest surface area possible exposed between an API or library and the module(s) / client code that use it.

## Orthogonality

When code internal to one module changes does it require updates to others?
If the answer is yes, then chances are the section that changed is not orthogonal to the rest of the codebase.

Orthogonality is the idea that different modules of the codebase can be changed freely without affecting other areas of the code.

i.e A change to the model should not require the UI to be updated most times and vice versa.

Orthogonality also similar to the DRY principal where knowledge should be compartmentalized and not repeated
or broken up between various components.

i.e UI presentation and text formatting code should not be split between the model, the presentation code and the UI
it should live in its own module so that a change in the way text is formatted and presented to the user does not
require a change to the data model.

## Prototypes and ‘Tracer Bullets’

### Prototypes are commonly used to explore a single aspect of a final system and are thrown away at the end.
The point of a prototype is to learn, not to have something reusable at the end.

According to pragmatic programmer some prime examples of things to prototype are:

 - Architecture
 - Performance issues
 - New functionality in a system
 - Structure or contents of external data
 - Third party tools or components
 - Performance issues

### Tracer Bullets
As the book defines:
&gt; Tracer bullets work because they operate in the same environment and under the same constraints as the real bullets. They get to the target fast, so the gunner gets immediate feedback. And from a practical standpoint they’re a relatively cheap solution.

Tracer bullets are simplified but working stripped down examples that can be further iterated upon.
Unlike prototypes tracer bullets are not thrown away, they are fleshed out until it evolves into the final system and hit the target.

You can use tracer bullets to write the skeleton of a system and all the modules that compose it.
Write the bare minimum of code required for data to be passed from one module to the next and iteratively create the final release software.

Advantages of the “Tracer Bullet” approach:

 - Users get to see the software early / You have something to demo
 - Developers build a Structure to work in.
  - You have a better feel for progress.


## Chapter 3

### Power of plain text
I don’t think this one needs too much explaining but the book certainly does.
Basically stray away from proprietary binary formats and stick with plain text for any data that is stored medium to long term.
Tools that can read or even the documentation of a custom binary file format may one day break and the knowledge of the format may be
lost or the documentation of the last format version could potentially have never been updated and if just one bit is shifted the entire file is unreadable. Non-Plain text formats require that EVERY DETAIL is know of the file format in order to parse it.
Plain text like ASCII or UTF-8 on the other hand can be edited and read in plain text almost future proofing the file format.

Even if the file is in a plain text format there is still a major point to consider.

 - Human Readable does not guarantee Human Understandable

Just because one can read the characters does not mean someone can make out what it means so try to use data formats that are both human readable and human Understandable so that in ten years when the binary format that you use to store info’s only existing tool no longer will run on a modern OS and machine the end user can still open the file and read it to make sense of it and even edit it with confidence.

### Debugging

#### Don't Panic (Think: Hitchhiker’s Guide to the Galaxy)
Even if you are on a tight deadline and behind schedule and the bug looks like a major setback, just stay calm.
Panic can and will cloud your judgement so do what you can get yourself relaxed and in a calm state of mind.
Forget about what needs to be done by End of Day in addition to this bug fix, forget about how spending time fixing this bug will delay progress on ticket x.
Just focus on finding out what when wrong and what the fix might look like.


#### Fix the problem, not the blame.

It does not matter who accidentally introduced the bug, blame only wastes time and the author of the bug most likely is aware of their mistake.
The bug is the entire team's problem, its everybody’s problem so just focus on the patch instead of pointing the finger.

#### Interview the user and gain as much context as you can
Most times a bug report does not contain enough context to properly reproduce the bug.
Going to the person who found the bug and asking them to reproduce the bug will often reveal details that the author of the report didn’t know were important.

The author of the report may have written that when they use the brush tool in the drawing software the app crashes but when the developer of that feature uses it is fine.
The author of the tool then goes to the author of the bug report and finds that when they only tried using the brush tool and drawing from the lower left corner of the screen to the upper right which does not produce a crash everything was fine but when drawing from the upper right DOWN to the lower left the software crashes.

#### Be able to reproduce the bug 100% of the time before you write one line of the fixe
Usually being able to force the bug 100% of the time will reveal the root cause of the bug.
Naturally if you can artificially produce the bug with 100% accuracy you can write a test to cause the failure and code against that test case until the bug is clear.

#### RTFEM: Read the fucking error message
’nuf said

#### Not an error but bad values
Maybe the code is behaving the way it is supposed to and returning a proper result.
Before you waste time looking for a bug make sure that your input data is correct.

i.e Is the interest in reality a range of 0.6 ~ 1.3 percent but the function is getting past a bunk value of 6% for the interest rate?

#### Binary Chop / Binary Search
If you find yourself looking at a very long stack trace to the binary chop / binary search to find the offending call.
Cut the stack in half and check the lower half, is the offending call in here? No? The move to the upper half, rinse and repeat.

# Chapter 4

## Design by Contract (DBC)

- Design by Contract defines the rights and responsibilities of a piece of software to ensure program correctness.
- What is a correct program? One that does no more and no less than what it claims to do.
- Powerful principal where the author of a function states very clearly what they can promise IFF they receive input matching a specific criterion.
- As a trivial example when writing a square root function you could have the contract allow negative numbers and return imaginary solutions or you could be stricter and say only non-negative numbers are accepted and in return you promise a correct non-negative solution.
- DBC forces you to think!

### Components of DBC

- Preconditions: What does the code require to be true of the input in order to provide a meaningful output?
- Post conditions: What output is promised to the user? What state of the world is guaranteed at the end of the routine?
- Class Invariants: This condition is always true from the perspective of the caller. What things are promised to remain constant when the function is entered and exited? In the middle of computation this invariant can change but by the time control returns to the caller of the function the invariant must hold true once again.

### DBC and TDD
Design by Contract is still useful even if you do TDD because they are different approaches to program correctness.
TDD is a good technique but to tends to make you focus on the "Happy Path" or the path where things go well and your tests may focus on that instead of what could go wrong like in the real world.

- DBC defines the parameters for success and failure in all cases whereas TDD can only target one case at a time.
- TDD and other testing only gets triggered during a "test time" (maybe as part of continuous integration) during the build cycle but DBC and assertions are always active during design, development, deployment and most importantly maintenance.
- DBC checks internal invariants whereas TDD only checks the code black-box style via its public interface.

#### 'Err in favor of the consumer'
Make sure to separate logic in code that are semantic invariants from logic that is subject to change via policy, think of semantic invariants as a philosophical contract.

An example of a semantic invariant in a piece of software would be the simple principle that users will never be billed twice for the same transaction. This is something that will never change and should be clearly documented and enforced at all levels, bake this into the code with assertions and fail early before this horrible event happens.

An example of a rule that must be enforced but is not a semantic invariant is something like the current sales tax rate or service fee.
Management could decide tomorrow to raise the service fee from 10% to 12% on a whim or politicians can suddenly raise sales tax from 8% to 10% these things can happen in the real world but you will never expect the consumer to pay twice for a single transaction.

### Assertive Programming: Dead programs tell no lines
Crash yourself before you trash yourself!
Once program state enters an "impossible" state anything the software does after that is not meaningful and can be quite harmful.
Databases can be filled with junk data, customers can suddenly find mystery charges on their accounts and good data like the
user's coveted files, game save data or bookmarks can be corrupted.

- Instead of saying "This can never happen" write an assertion to check it.
- Don't use Assertions in place of real error handling, a blog post should always have an associated author but a resource being inaccessible is possible under bad network conditions this is just simple error handling.
- Be careful that your assertions do not create side effects, make sure that when observing that a condition is true that you do not change the state of the very thing you are observing.
- Leave assertions on, especially in production. Just because the code you are shipping has no failing test today there is no guarantee that the tests have not tested a certain code path that will occur in the wild, in production.


# Chapter 5

### Decoupling
- Decoupled code is easier to change, harder to break.
- Code reuse should not be a goal as much as the process of thinking that allows code to be reusable. Code reuse happens when interfaces are clearly defined and decoupled from the rest of the code.


 ### Things to Avoid
 - Train Wrecks: Avoid writing code that is a chain of function calls where any of the calls can easily change.
  - Globalization creates large scale coupling of any components that depend on global variables or singletons.
  - Inheritance: subclassing is dangerous, use interfaces / protocols and mix-ins instead.

 ### Don’t Chain Method calls
 Unless the methods are very unlikely to change like `map`, `filter`, `reduce` chaining method
 calls together is a ticking time bomb for maintenance.
  Eventually someone is going to change one of those methods and it might not be as obvious to cause a compilation error.
  Its entirely possible that the behavior of one or more of the methods being chained will change over time and when it does it will no longer behave in a way that methods being called down stream are expecting it to result in no warnings or errors from the compiler but undefined and strange program behavior.

 ### The evils of Globalization

 - Adding a global variable is as if every single function in your application gains an additional parameter but it is even worse than that! Unlike a parameter, a global variable is not visible in the function signature so even after removing the global it is difficult to quickly track down which code has been dependent on it.

 - A change to a global requires a change to all the code throughout the code base that relied on it.

 ### Its important enough to be global, wrap it up!

 Globalization is not only about using global variables, any data that is shared between modules is global be it a database, an API, Configuration etc.
 This is not always a bad thing because there are sometimes not many alternatives but make sure that you wrap the global resource in some kind of code that can control access to it and that nothing is touching the global data directly.


### Transforming Programming

&gt; If you can’t describe what you are doing as a process, you don’t know what you’re doing.
&gt; - W. Edwards Deming, (attr)
&gt; (David, Thomas; Hunt Andrew. The Pragmatic Programmer (p. 234). Pearson Education. Kindle Edition.

A great mental tool for software design is to think of everything as a transformation.
Break any process down into steps taking an input and then producing an output that becomes the input for the next function.
Everything is just a sub task of the larger task of transforming input such as touches from the user into a desired result i.e a payment transaction.

 - Don’t hoard state, pass it around. This pillar of functional programming leads to some amazingly stable code, every function in the pipeline gets a copy or reference of the data that is at the very least promised to be atomic.

 - Model transformations with something similar to the `|&gt;` pipe left operator where it is simple and easy to trace the input and output between functions in series. Even if the language you use does not support pipe-left like operators you can still clearly break the result of one operation between different lines passing it as a constant to the next.
 
 ### Pipes &amp; Pipe-Lefts
'pipe-left' operators use a very familier concept and is the same as the concept of pipes in a command line like `zsh` or `bash`.

`wget -q -O - https://raw.githubusercontent.com/Nirma/blog/master/Notes/PragmaticProgrammer/Pragmatic_Programmer_greatest_hits.md | grep -i software | wc -l`

This has three stages: fetch the contents of **this** document and feed it into `stdin`; search for any lines contanting the word "software" ignoring case and finally pipe the input to `wc` and count the number of lines.
The takeaway here is that each stage is only concerned with what its input and flags are and what it is required to output, this is a great example of separation of concerns and an example of how we can strive to build software that can be changed easily and sub-components that will stand the test of time. (for a little while.)



 ### Inheritance Tax
 - Inheritance is coupling by definition.
  - Inheritance introduces coupling on many levels.
  - The sub class is not only coupled to the parent but the code that uses the subclass also tied to the parent class.

 #### Alternatives to using Inheritance
 - Protocols &amp; Interfaces
 - Delegation
 - Mixins and Traits
 - "Has A" is more important than is-a, does an object DO a certain thing vs what IS the object. This is true of both delegation and using protocols.


# Chapter 6

Chapter six deals mostly with concurrency.
While this topic is just as important as other fundamentals such as Design Patterns, Data Structures and Algorithms it would best serve the reader to 
learn concurrency models that deal with their specific platform but if you have time Chapter six is very good so instead of summarizing chapter six here I will omit it and encourage you to read chapter six in the book. 

# Chapter 7 

 ### Listen to your lizard brain
 - Instincts make you feel, not think and when you 'feel' something it is most likely the result of accumulated experience. Listen to it, but think about what it means. 
 - Distance yourself from a problem and come back to it again later if you feel that the approach is harder than it needs to be. Chances are you your instincts are right and after giving your subconscious sometime to push the problem through various layers of your brain you might come up with a simpler solution to the problem at hand. 
 
 ### Mind Hack
 The book details a simple but powerful trick to get you past the black editor screen. 
 When you feel "stuck" and don't know how to approach a difficult problem start writing "prototype" code i.e coding something that is only for the purpose 
 of learning. This removes the distraction from the code having to be "right" and be done well and instead allows you to focus only on figuring out how to solve the problem with code. Once you have a working prototype or example of how to code something then you can think about how to make the code clear and simple and pretty. While making the prototype feel free to 
 
 ### Test your code, or your users will
 - Tests should be the first users of your code. 
 - Even if you do not write tests thinking of how to make the code you are writing testable has the benefit of making the software you are writing modular and keeps you off the path of writing large monolithic functions. 
 - Unit tests are always the first line of defense, making sure that code contained in a module behaves as expected needs to be done and pass before testing code that relies on the module, fail fast, fail early. 
 - 100% test coverage means nothing, just because every line of code is executed in the tests it does not mean all cases have been tested.
    - Don't become enslaved by TDD, TDD can help but also can be overdone and get in the way of software development.
    
 ### Names
 - Name well; Rename when needed.
     - If you can't come up with a good name for something, a name that makes sense then what you are trying to do has a high probability of also not making sense.
     - Be consistent
 - Follow common conventions of the language and programming environment you are using. 
 
 
 
 # Chapters 8 &amp; 9 (There is a lot of overlap, it would be smoother to combine them here.)
 
 “`
 "I’ve never met a human being who would want to read 17,000 pages of documentation, and if there was, I’d kill him to get him out of the gene pool. Joseph Costello, President of Cadence"

David, Thomas; Hunt Andrew. The Pragmatic Programmer (p. 392). Pearson Education. Kindle Edition. 
“`
 
 ### Agile is not a noun, it is how you do things
 - Agile is a method of responding to change and doing so frequently. 
 - There is no "out of the box" solution that will work for any team.
   - Anyone who claims to be able to sell you a "Solution" is a liar, the agile process needs to derived by the team.
   - Saying "Other AAA tech companies do this" means nothing, if Netflix is currently doing agile via method "X" now when they are a company that has thousands of employees it does not mean that the current method would have worked for Netflix when they were only 50 employees and does not mean they will still be using the same method next year. 
 - Agile solutions need to be custom tailored to how the team and company operate and don't forget its only about responding to change in near realtime not any particular dogma.
   - One size fits no one well.
   
 ### Afterthoughts on testing
 
 - The benefit of writing tests is the effect it has on writing the code it tests, therefore the same person who writes the code should also be responsible for the tests. 
 - The disadvantage is that if the test and the code both have a wrong assumption about the underlying logic then the issue will go undetected.
   - Use property based testing to fill this gap.
   - State Coverage, not test coverage
 - Use property based testing (i.e QuickCheck-ish testing libraries)
 - Property based testing lets the computer generate random values and test that a certain property holds true.
   - Property based testing is extremely economical, the alternative would be to write various tests to state by hand. 
 
 
 
# The Pragmatic Programmer: Cliff Notes

## Chapter 1

### Software Entropy and broken windows

## Broken window Theory

According to [Wikipedia](https://en.wikipedia.org/wiki/Broken_windows_theory):

&gt; The broken windows theory is a criminological theory that states that visible signs of crime, anti-social behavior, and civil disorder create an urban environment that encourages further crime and disorder, including serious crimes. The theory suggests that policing methods that target minor crimes, such as vandalism, loitering, public drinking, jaywalking and fare evasion, help to create an atmosphere of order and lawfulness, thereby preventing more serious crimes.

### What's it have to do with development?

According to the theory written above a building with just one broken window has a higher probability of
being further vandalized when compared to a building without a broken window.
Chaos gives way to more chaos but on the flip side when people see order they tend to feel that breaking the order would be bad.

A code base with a few ‘broken windows’ (i.e sloppy patches, workarounds and other ‘temporary’ code that accidentally becomes part of the core) will encourage continued bad behavior by people who make changes to the code base.

&gt; “This component is a piece of garbage anyways, I am not doing any harm by writing code equally shitty code.”

This is an example of the mindset that members of a team may think subconsciously.
If we enforce having a clean codebase and no ‘windows’ are broken someone making changes to the codebase
would hopefully see how well written it is and want to maintain that order and stray away from chaos.

Unfortunately you will always have ‘broken windows’ and not so pretty sections of code but here is a quick list of some ideas to enforce to turn start fixing broken windows.

 - If pressed for time and a fix is a little substandard leave a `// TODO:` comment as a reminder to fix things up later.

 - Be proactive in fixing warnings and `TODO` comments, create issues and review periodically. Don’t let them pile up.

 - Analyze the git log and see which files have the most bug fixes. Is this a symptom of a greater disease?

 - Avoid tight coupling of components, use principles of DRY and orthogonality.


 ## Good enough Software

 - Rather than deliver a “perfect” piece of software one year from now deliver something minimal soon and iterate the software through a feedback process whereas the client uses the software they can understand what they really need and what is practical rather than what they envisioned before seeing the real thing in action.

 - Know when to stop, make a clear quality requirements document. Perfect software has never existed and never will. A crash free rate of 99.8 percent has different consequences for a chat client than it does autopilot or an anti-lock breaking system. Chat client users who experience a crash are annoyed, users who experienced a bug in the ABS software could be ex-users. Don’t waste time testing a chat client as if it could cause loss of life but do make sure the software is stable enough to release.

## Chapter 2

## DRY
Don’t Repeat Yourself... ’nuf said.

DRY tries to avoid code duplication, code duplication is frowned upon because it opens the door inviting problems like having the same code written in multiple places and later forgetting to update some code sections with new behavior when the spec changes.

All code should have a single source of truth and knowledge should not be broken up between modules in the code.

Of course in practice this is not perfectly enforceable because in order to use a library or API the code in the calling module / client needs to know it’s interface in order to use it.
The lesson here is to have the smallest surface area possible exposed between an API or library and the module(s) / client code that use it.

## Orthogonality

When code internal to one module changes does it require updates to others?
If the answer is yes, then chances are the section that changed is not orthogonal to the rest of the codebase.

Orthogonality is the idea that different modules of the codebase can be changed freely without affecting other areas of the code.

i.e A change to the model should not require the UI to be updated most times and vice versa.

Orthogonality also similar to the DRY principal where knowledge should be compartmentalized and not repeated
or broken up between various components.

i.e UI presentation and text formatting code should not be split between the model, the presentation code and the UI
it should live in its own module so that a change in the way text is formatted and presented to the user does not
require a change to the data model.

## Prototypes and ‘Tracer Bullets’

### Prototypes are commonly used to explore a single aspect of a final system and are thrown away at the end.
The point of a prototype is to learn, not to have something reusable at the end.

According to pragmatic programmer some prime examples of things to prototype are:

 - Architecture
 - Performance issues
 - New functionality in a system
 - Structure or contents of external data
 - Third party tools or components
 - Performance issues

### Tracer Bullets
As the book defines:
&gt; Tracer bullets work because they operate in the same environment and under the same constraints as the real bullets. They get to the target fast, so the gunner gets immediate feedback. And from a practical standpoint they’re a relatively cheap solution.

Tracer bullets are simplified but working stripped down examples that can be further iterated upon.
Unlike prototypes tracer bullets are not thrown away, they are fleshed out until it evolves into the final system and hit the target.

You can use tracer bullets to write the skeleton of a system and all the modules that compose it.
Write the bare minimum of code required for data to be passed from one module to the next and iteratively create the final release software.

Advantages of the “Tracer Bullet” approach:

 - Users get to see the software early / You have something to demo
 - Developers build a Structure to work in.
  - You have a better feel for progress.


## Chapter 3

### Power of plain text
I don’t think this one needs too much explaining but the book certainly does.
Basically stray away from proprietary binary formats and stick with plain text for any data that is stored medium to long term.
Tools that can read or even the documentation of a custom binary file format may one day break and the knowledge of the format may be
lost or the documentation of the last format version could potentially have never been updated and if just one bit is shifted the entire file is unreadable. Non-Plain text formats require that EVERY DETAIL is know of the file format in order to parse it.
Plain text like ASCII or UTF-8 on the other hand can be edited and read in plain text almost future proofing the file format.

Even if the file is in a plain text format there is still a major point to consider.

 - Human Readable does not guarantee Human Understandable

Just because one can read the characters does not mean someone can make out what it means so try to use data formats that are both human readable and human Understandable so that in ten years when the binary format that you use to store info’s only existing tool no longer will run on a modern OS and machine the end user can still open the file and read it to make sense of it and even edit it with confidence.

### Debugging

#### Don't Panic (Think: Hitchhiker’s Guide to the Galaxy)
Even if you are on a tight deadline and behind schedule and the bug looks like a major setback, just stay calm.
Panic can and will cloud your judgement so do what you can get yourself relaxed and in a calm state of mind.
Forget about what needs to be done by End of Day in addition to this bug fix, forget about how spending time fixing this bug will delay progress on ticket x.
Just focus on finding out what when wrong and what the fix might look like.


#### Fix the problem, not the blame.

It does not matter who accidentally introduced the bug, blame only wastes time and the author of the bug most likely is aware of their mistake.
The bug is the entire team's problem, its everybody’s problem so just focus on the patch instead of pointing the finger.

#### Interview the user and gain as much context as you can
Most times a bug report does not contain enough context to properly reproduce the bug.
Going to the person who found the bug and asking them to reproduce the bug will often reveal details that the author of the report didn’t know were important.

The author of the report may have written that when they use the brush tool in the drawing software the app crashes but when the developer of that feature uses it is fine.
The author of the tool then goes to the author of the bug report and finds that when they only tried using the brush tool and drawing from the lower left corner of the screen to the upper right which does not produce a crash everything was fine but when drawing from the upper right DOWN to the lower left the software crashes.

#### Be able to reproduce the bug 100% of the time before you write one line of the fixe
Usually being able to force the bug 100% of the time will reveal the root cause of the bug.
Naturally if you can artificially produce the bug with 100% accuracy you can write a test to cause the failure and code against that test case until the bug is clear.

#### RTFEM: Read the fucking error message
’nuf said

#### Not an error but bad values
Maybe the code is behaving the way it is supposed to and returning a proper result.
Before you waste time looking for a bug make sure that your input data is correct.

i.e Is the interest in reality a range of 0.6 ~ 1.3 percent but the function is getting past a bunk value of 6% for the interest rate?

#### Binary Chop / Binary Search
If you find yourself looking at a very long stack trace to the binary chop / binary search to find the offending call.
Cut the stack in half and check the lower half, is the offending call in here? No? The move to the upper half, rinse and repeat.

# Chapter 4

## Design by Contract (DBC)

- Design by Contract defines the rights and responsibilities of a piece of software to ensure program correctness.
- What is a correct program? One that does no more and no less than what it claims to do.
- Powerful principal where the author of a function states very clearly what they can promise IFF they receive input matching a specific criterion.
- As a trivial example when writing a square root function you could have the contract allow negative numbers and return imaginary solutions or you could be stricter and say only non-negative numbers are accepted and in return you promise a correct non-negative solution.
- DBC forces you to think!

### Components of DBC

- Preconditions: What does the code require to be true of the input in order to provide a meaningful output?
- Post conditions: What output is promised to the user? What state of the world is guaranteed at the end of the routine?
- Class Invariants: This condition is always true from the perspective of the caller. What things are promised to remain constant when the function is entered and exited? In the middle of computation this invariant can change but by the time control returns to the caller of the function the invariant must hold true once again.

### DBC and TDD
Design by Contract is still useful even if you do TDD because they are different approaches to program correctness.
TDD is a good technique but to tends to make you focus on the "Happy Path" or the path where things go well and your tests may focus on that instead of what could go wrong like in the real world.

- DBC defines the parameters for success and failure in all cases whereas TDD can only target one case at a time.
- TDD and other testing only gets triggered during a "test time" (maybe as part of continuous integration) during the build cycle but DBC and assertions are always active during design, development, deployment and most importantly maintenance.
- DBC checks internal invariants whereas TDD only checks the code black-box style via its public interface.

#### 'Err in favor of the consumer'
Make sure to separate logic in code that are semantic invariants from logic that is subject to change via policy, think of semantic invariants as a philosophical contract.

An example of a semantic invariant in a piece of software would be the simple principle that users will never be billed twice for the same transaction. This is something that will never change and should be clearly documented and enforced at all levels, bake this into the code with assertions and fail early before this horrible event happens.

An example of a rule that must be enforced but is not a semantic invariant is something like the current sales tax rate or service fee.
Management could decide tomorrow to raise the service fee from 10% to 12% on a whim or politicians can suddenly raise sales tax from 8% to 10% these things can happen in the real world but you will never expect the consumer to pay twice for a single transaction.

### Assertive Programming: Dead programs tell no lines
Crash yourself before you trash yourself!
Once program state enters an "impossible" state anything the software does after that is not meaningful and can be quite harmful.
Databases can be filled with junk data, customers can suddenly find mystery charges on their accounts and good data like the
user's coveted files, game save data or bookmarks can be corrupted.

- Instead of saying "This can never happen" write an assertion to check it.
- Don't use Assertions in place of real error handling, a blog post should always have an associated author but a resource being inaccessible is possible under bad network conditions this is just simple error handling.
- Be careful that your assertions do not create side effects, make sure that when observing that a condition is true that you do not change the state of the very thing you are observing.
- Leave assertions on, especially in production. Just because the code you are shipping has no failing test today there is no guarantee that the tests have not tested a certain code path that will occur in the wild, in production.


# Chapter 5

### Decoupling
- Decoupled code is easier to change, harder to break.
- Code reuse should not be a goal as much as the process of thinking that allows code to be reusable. Code reuse happens when interfaces are clearly defined and decoupled from the rest of the code.


 ### Things to Avoid
 - Train Wrecks: Avoid writing code that is a chain of function calls where any of the calls can easily change.
  - Globalization creates large scale coupling of any components that depend on global variables or singletons.
  - Inheritance: subclassing is dangerous, use interfaces / protocols and mix-ins instead.

 ### Don’t Chain Method calls
 Unless the methods are very unlikely to change like `map`, `filter`, `reduce` chaining method
 calls together is a ticking time bomb for maintenance.
  Eventually someone is going to change one of those methods and it might not be as obvious to cause a compilation error.
  Its entirely possible that the behavior of one or more of the methods being chained will change over time and when it does it will no longer behave in a way that methods being called down stream are expecting it to result in no warnings or errors from the compiler but undefined and strange program behavior.

 ### The evils of Globalization

 - Adding a global variable is as if every single function in your application gains an additional parameter but it is even worse than that! Unlike a parameter, a global variable is not visible in the function signature so even after removing the global it is difficult to quickly track down which code has been dependent on it.

 - A change to a global requires a change to all the code throughout the code base that relied on it.

 ### Its important enough to be global, wrap it up!

 Globalization is not only about using global variables, any data that is shared between modules is global be it a database, an API, Configuration etc.
 This is not always a bad thing because there are sometimes not many alternatives but make sure that you wrap the global resource in some kind of code that can control access to it and that nothing is touching the global data directly.


### Transforming Programming

&gt; If you can’t describe what you are doing as a process, you don’t know what you’re doing.
&gt; - W. Edwards Deming, (attr)
&gt; (David, Thomas; Hunt Andrew. The Pragmatic Programmer (p. 234). Pearson Education. Kindle Edition.

A great mental tool for software design is to think of everything as a transformation.
Break any process down into steps taking an input and then producing an output that becomes the input for the next function.
Everything is just a sub task of the larger task of transforming input such as touches from the user into a desired result i.e a payment transaction.

 - Don’t hoard state, pass it around. This pillar of functional programming leads to some amazingly stable code, every function in the pipeline gets a copy or reference of the data that is at the very least promised to be atomic.

 - Model transformations with something similar to the `|&gt;` pipe left operator where it is simple and easy to trace the input and output between functions in series. Even if the language you use does not support pipe-left like operators you can still clearly break the result of one operation between different lines passing it as a constant to the next.
 
 ### Pipes &amp; Pipe-Lefts
'pipe-left' operators use a very familier concept and is the same as the concept of pipes in a command line like `zsh` or `bash`.

`wget -q -O - https://raw.githubusercontent.com/Nirma/blog/master/Notes/PragmaticProgrammer/Pragmatic_Programmer_greatest_hits.md | grep -i software | wc -l`

This has three stages: fetch the contents of **this** document and feed it into `stdin`; search for any lines contanting the word "software" ignoring case and finally pipe the input to `wc` and count the number of lines.
The takeaway here is that each stage is only concerned with what its input and flags are and what it is required to output, this is a great example of separation of concerns and an example of how we can strive to build software that can be changed easily and sub-components that will stand the test of time. (for a little while.)



 ### Inheritance Tax
 - Inheritance is coupling by definition.
  - Inheritance introduces coupling on many levels.
  - The sub class is not only coupled to the parent but the code that uses the subclass also tied to the parent class.

 #### Alternatives to using Inheritance
 - Protocols &amp; Interfaces
 - Delegation
 - Mixins and Traits
 - "Has A" is more important than is-a, does an object DO a certain thing vs what IS the object. This is true of both delegation and using protocols.


# Chapter 6

Chapter six deals mostly with concurrency.
While this topic is just as important as other fundamentals such as Design Patterns, Data Structures and Algorithms it would best serve the reader to 
learn concurrency models that deal with their specific platform but if you have time Chapter six is very good so instead of summarizing chapter six here I will omit it and encourage you to read chapter six in the book. 

# Chapter 7 

 ### Listen to your lizard brain
 - Instincts make you feel, not think and when you 'feel' something it is most likely the result of accumulated experience. Listen to it, but think about what it means. 
 - Distance yourself from a problem and come back to it again later if you feel that the approach is harder than it needs to be. Chances are you your instincts are right and after giving your subconscious sometime to push the problem through various layers of your brain you might come up with a simpler solution to the problem at hand. 
 
 ### Mind Hack
 The book details a simple but powerful trick to get you past the black editor screen. 
 When you feel "stuck" and don't know how to approach a difficult problem start writing "prototype" code i.e coding something that is only for the purpose 
 of learning. This removes the distraction from the code having to be "right" and be done well and instead allows you to focus only on figuring out how to solve the problem with code. Once you have a working prototype or example of how to code something then you can think about how to make the code clear and simple and pretty. While making the prototype feel free to 
 
 ### Test your code, or your users will
 - Tests should be the first users of your code. 
 - Even if you do not write tests thinking of how to make the code you are writing testable has the benefit of making the software you are writing modular and keeps you off the path of writing large monolithic functions. 
 - Unit tests are always the first line of defense, making sure that code contained in a module behaves as expected needs to be done and pass before testing code that relies on the module, fail fast, fail early. 
 - 100% test coverage means nothing, just because every line of code is executed in the tests it does not mean all cases have been tested.
 - Don't become enslaved by TDD, TDD can help but also can be overdone and get in the way of software development.
    
 ### Names
   - Name well; Rename when needed.
   - If you can't come up with a good name for something, a name that makes sense then what you are trying to do has a high probability of also not making sense.
   - Be consistent
   - Follow common conventions of the language and programming environment you are using. 
 
 
 # Chapters 8 & 9 (lots of overlap)
 
 > "I’ve never met a human being who would want to read 17,000 pages of documentation, and if there was, I’d kill him to get him out of the gene pool. 
 >     - Joseph  Costello, President of Cadence

 
 ### Agile is not a noun, it is how you do things
 - Agile is a method of responding to change and doing so frequently. 
 - There is no "out of the box" solution that will work for any team.
   - Anyone who claims to be able to sell you a "Solution" is a liar, the agile process needs to derived by the team.
   - Saying "Other AAA tech companies do this" means nothing, if Netflix is currently doing agile via method "X" now when they are a company that has thousands of employees it does not mean that the current method would have worked for Netflix when they were only 50 employees and does not mean they will still be using the same method next year. 
 - Agile solutions need to be custom tailored to how the team and company operate and don't forget its only about responding to change in near realtime not any particular dogma.
   - One size fits no one well.
   
 ### Afterthoughts on testing
 
 - The benefit of writing tests is the effect it has on writing the code it tests, therefore the same person who writes the code should also be responsible for the tests. 
 - The disadvantage is that if the test and the code both have a wrong assumption about the underlying logic then the issue will go undetected.
 - Use property based testing to fill this gap.
 - State Coverage, not test coverage
 - Use property based testing (i.e QuickCheck-ish testing libraries)
 - Property based testing lets the computer generate random values and test that a certain property holds true.
 - Property based testing is extremely economical, the alternative would be to write various tests to state by hand. 
 

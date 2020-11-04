# The Pragmatic Programmer: Cliff Notes

## Chapter 1

### Software Entropy and broken windows

## Broken window Theory

According to [wikipedia](https://en.wikipedia.org/wiki/Broken_windows_theory):

> The broken windows theory is a criminological theory that states that visible signs of crime, anti-social behavior, and civil disorder create an urban environment that encourages further crime and disorder, including serious crimes. The theory suggests that policing methods that target minor crimes, such as vandalism, loitering, public drinking, jaywalking and fare evasion, help to create an atmosphere of order and lawfulness, thereby preventing more serious crimes.

### Whats it have to do with development?

According to the theory written above a building with just one broken window has a higher probability of
being further vandalized than a building without a broken window.
Chaos gives way to more chaos but on the flip side when people see order they tend to feel that breaking the order would be bad.

A code base with a few ‘broken windows’ (i.e sloppy patches, workarounds and other ‘temporary’ code that accidentally becomes part of the core) will encourage continued bad behavior by people who make changes to the code base.

> “This component is a piece of garbage anyways, I am not doing any harm by writing code equally shitty code.”

This is an example of the mindset that members of a team may think subconsciously.
If we enforce having a clean codebase and no ‘windows’ are broken someone making changes to the codebase
would hopefully see how well written it is and want to maintain that order and stray away from chaos.

Unfortunately you will always have ‘broken windows’ and not so pretty sections of code but here is a quick list of some ideas to enforce to turn start fixing broken windows.

  - If pressed for time and a fix is a little substandard leave a `// TODO:` comment as a reminder to fix things up later.

  - Be proactive in fixing warnings and `TODO` comments, create issues and review periodically. Don’t let them pile up.

  - Analyze the git log and see which files have the most bug fixes. Is this a symptom of a greater disease?

  - Avoid tight coupling of components, use principals of DRY and orthogonality.


  ## Good enough Software

  - Rather than deliver a “perfect” piece of software one year from now deliver something minimal soon and iterate the software through a feedback process where as the client uses the software they can understand what they really need and what is practical rather than what they envisioned before seeing the real thing in action.

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
it should live in it’s own module so that a change in the way text is formatted and presented to the user does not
require a change to the data model.

## Prototypes and ‘Tracer Bullets’

### Prototypes
Prototypes are commonly used to explore a single aspect of a final system and are thrown away at the end.
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
> Tracer bullets work because they operate in the same environment and under the same constraints as the real bullets. They get to the target fast, so the gunner gets immediate feedback. And from a practical standpoint they’re a relatively cheap solution.

Tracer bullets are simplified but working stripped down examples that can be further iterated upon.
Unlike prototypes tracer bullets are not thrown away, they are fleshed out until the evolve into the final system and hit the target.

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

Even if the file is in a plain text format there is still a major point to cosider.

 - Human Readable does not guarantee Human Understandable

Just because one can read the characters does not mean someone can make out what it means so try to use data formats that are both human readable and human Understandable so that in ten years when the binary format that you use to store info’s only existing tool no longer will run on a modern OS and machine the end user can still open the file and read it to make sense of it and even edit it with confidence.

### Debugging

#### Dont’ Panic (Think: Hitchhiker’s Guide to the Galaxy)
Even if you are on a tight deadline and behind schedule and the bug looks like a major setback, just stay calm.
Panic can and will cloud your judgement so do what you can get yourself relaxed and in a calm state of mind.
Forget about what needs to be done by End of Day in addition to this bug fix, forget about how spending time fixing this bug will delay progress on ticket x.
Just focus on finding out what when wrong and what the fix might look like.


#### Fix the problem, not the blame.

It does not matter who accidentally introduced the bug, blame only wastes time and the author of the bug most likely is aware of their mistake.
The bug is the entire teams problem, its everybody’s problem so just focus on the patch instead of pointing the finger.

#### Interview the user and gain as much context as you can
Most times a bug report does not contain enough context to properly reproduce the bug.
Going to the person who found the bug and asking them to reproduce the bug will often reveal details that the author of the report didn’t know were important.

The author of the report may have written that when they use the brush tool in the drawing software the app crashes but when the developer of that feature uses it it is fine.
The author of the tool then goes to the author of the bug report and finds that when they only tried using the brush tool and drawing from the lower left corner of the screen to the upper right which does not produce a crash everything was fine but when drawing from the upper right DOWN to the lower left the software crashes.

#### Be able to reproduce the bug 100% of the time before you write one line of the fixe
Usually being able to force the bug 100% of the time will reveal the root cause of the bug.
Naturally if you can artificially produce the bug with 100% accuracy you can write a test to cause the failure and code against that test case until the bug is clear.

#### RTFEM: Read the fucking error message
’nuf said

#### Not an error but bad values
Maybe the code is behaving the way it is supposed to and returning a proper result.
Before you waste time looking for a bug make sure that your input data is correct.

i.e Is the interest in reality a range of 0.6 ~ 1.3 percent but the function is getting passed a bunk value of 6% for the interest rate?

#### Binary Chop / Binary Search
If you find yourself looking at a very long stack trace do the binary chop / binary search to find the offending call.
Cut the stack in half and check the lower half, is the offending call in here? No? The move to the upper half, rinse and repeat.

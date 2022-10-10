222022-09-13

Style: #BookNotes 

Domain: #Development 

Tags:

# Chapter 7 While you're coding

### Listen to your lizard brain

>Your code is trying to tell yopu something. It's saying that this is harder than it should be. Maybe the structure or design is wrong, maybe you're solving the wrong prloblem, or maybe you're just creating an anta farm's worth of bugs.

### Programming by coincidence

>Fred doesn't know why the code is failling because he didn't know why it worked in the first place.

**Implicit assumptions**

>Testing is particularly fraught with false causalities and coincidental outcomes. It's easy to assume X caused Y, but you should not assume it, you should prove it.

**How to program deliberately**

> - Always be aware of what you are doing. 
> - Can you explai the code, in detail, to a more junior programmer? If not, perhaps you are relying on coincidence
> - Don't code in the dark. Build an application you don't fully grasp, or use a technology you don't understand, and you'll likely be bitten by coincidences. If you're not sure why it works, you won't know why it failed.
> - Proceed from aplan, whether that plan is in your head, on the back of a napkin, or on a whiteboard.
> - Rely only on reliable things. Don't depend on assumptions. If you can't tell if somethign is reliable, assume the worst.
> - Document your assumptions.
> - Don't just test your code, but test you r assumptions as well. Don't guess, actually try it. Write an assertion to test your assumptions. If your assertion is right, you have improved the documentation of your code. If you discover your assumption is wrong, then count yourself lucky.
> - Prioritize your effort. Spend time on the important aspects; more tahn likely, these are the hard parts. If you don't have fundamentals or infrastracture correct, brilliant bells and whistles will be irrelevant.
> - Don't be a slave to history. Don't let existing code dictate future code. All code can be replaced if it is no longer appropriate. Even within one program, don't let wht you've already done constrain what you do next - be ready to refactor. THis decision may impact the project shedule. THe assumption is that the impact will be less tahn the cost of not makin the change.

### Refactoring

>Refactoring is a day-to-day activity, taking low-risk small steps

>In order to gurantee that the external behaviour hasn't changed, you need good, automated unit testing that validates the behaviour of the code.

**When should you refactor?**
>Any number of things may casue code to qualify for refactoring:
>- Duplication
>- Nonorthogonal design
>- Outdated knowledge
>- Usage
>- performance
>- The tests pass

>Refactoring, as with most things, is easier to do while the issues are small, as an ongoing activity while coding.

**How do you refactor?**

>1. Don't try to refactor and add functionality at teh same time
>2. Make sure you have good tests before you begin refactoring. Run the tests as often as possible. That way you will know quickly if your changes have broken anything.
>3. Take short, deliberate steps: move a field from one class to another, split a method, rename a variable. Refaactoring often involves making many localized changes taht resul tin a lager-scale cahnge. If you keep your steps small, and test after each step, you will avoid prolonged debugging.


### Test to code

>We believe that the major benefits of testing happen wehn you think about and write the tests, not when you run them.

**Thinking about testing**

>Stop! How do you know that what you're about to do is a good thing?
>The answer is that you can't know that, No one can. But thinking about tests can make it more likely. Here is how taht works.

>Start by imagening that you'd finished writing the function and now had to test it. How would you do that? Well, you'd want to use some test data, which probably means you want to work in a database you control. Now some frameworks can handle taht for you, running tests against a test database, but in our case that means we should be passing the database instance into our function rather than using some global one, as that allows us to change it while testing.
>Then we have to think about how we'd populate that test data. The requirements ask for a "list of people who watch more than 10 videos as week." So we look at the database schema for fields that might help us. We find two likely fields in a table of who-whatched-waht: opened_video and completed_video. To write our test data, we need to know witch field to use. But we don't know what the requirements means, and our business contact is out. Let's just cheat and pass in the name of the field (which will allow us to test what we have, and potentially change it later).
>We started by thinking about our tests, and whithout writing a line of code, we already made two discoveries and used them to change the API of pur method.

**Test drive coding**
>A test is the first user of your code
>We think this is probably the biggest benefit offered by testing: tesing is vital feedback that guides your coding

>It is easy to become seduced by the green "test passed" message, writting lots of code that doesn't actually get you closer to a solution.

**Testing againt contract**

>We want to write test cases that ensure that a given unit honors its contract.

**Ad-hoc testing**

>If code broke once, it is likely to break again.

**Build a test window**

>we can rpovide various views into the internal state of a module


### Property based testing

**Contracts, Invariants, and properties**
>we talked about the idea that ode has contracts that it meets: you meet the conditions when you feed it input, and it will make certain gurantees about the outputs it produces.
>There are also code invariants, things that remian true about some piece of state when it's passed through a function. For example, if you sort a list, the result will have the same number of elements as the original-the length is invariant. We lum both together as properties.

**propertie-based tests often surprise you**

>Our suggestion is that when a property-based test fails, find out what parameters it was passing to the test function, and then use those values to create a separate, regular, unit test.
>That unit tests then acts as a regression test.

**[[property based testing]] helps you desing**
>They make you think about your code in terms of invariants and contracts; you think about what must not change and what must be true.

### Stay safe out there

>Security through obscurity just doesn't work.

>Never trust data from an external entity, always sanitize it before passing it on to a database, view rendering, or other processing.

>Keep the number of authorized users at an absolute minimum.

>Make sure that the data you report is appropriate for the authorized user.

>Make sure that any "test window" and runtime exception reporting is protected from spying eyes.

>Another key principle is to use the least amount of privilege for the shortest time you can get away with it.

>The default settings on your app, or for yours users on your site, should be the most secrue values.

>Don't check in secrets, API keys, SSH keys, encryption passwords or other credentials alongside your source code in version control.

>The first and most important roel about cryto: never do it yourself.



___
# References

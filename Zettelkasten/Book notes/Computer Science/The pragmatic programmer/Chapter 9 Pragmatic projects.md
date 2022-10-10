222022-09-14

Style: #BookNotes 

Domain: #Development 

Tags:

# Chapter 9 Pragmatic projects

### Pragmatic teams
>A team, in our view, is a small, mostly stable entity of its own.

>A pragmatic team is small, under 10-12 or so members. Members come and go rarly.

>Quality is a team issue.

>Quality is built in, not bolted on.

>Encourage everyone to activly monitor the environment for cahnges.

> The team needn't reject cahnges out of hand - you simply need to be aware that they're happeninng.

>That means that you need all the skill to do that within the team: UI/UX, server, DBA, QA, etc., all comfortable and accustomed to working with each other.

>A great way to ensure consistency and accuracy is to automate everything the team does.


>The goal is to be in an position to deliver working software taht gives the users some new capability at a moment's notice.


### Pragmatic starter kit
>Whether it is the build and release procedure, testing, project paperwork, or nay other recurring task on the project, it has to be automatic and repeatable on nay capapble machiene.

>Manual processes leave consistency up to chance.

**Drive with version control**
>You wan to keep everything needed to build your project under version control.

>And that's the important part: at the project level, version control drives the build and release process.

**Ruthless and continuous testing**
>We are driven to find our bugs *now*, so we don't have to endure the sahme of others finding our bugs later.

>In fact, a good project may well have more test code than production code.

**Unit testing**
>All of the modules you are using must muss pass their own unit tests before you can proceed.

**Integration testing**
>Integration testing shows that the major subsystems that make up the project work and play well with each other.

>Otherwise integration becomes fertile breedinground for bugs.

**Validation and verification**
>does it meet the functional requirements of the system?

**Performance testing**
>Ask yourself if the software meets the performance requirements under real world conditions - with the expected number of users, of connections, or tranactions per second. Is is scaleable?

**Testing the tests**
>After you have written a test to detect a particular bug, cause the bug deliberatly and make sure the test cimplains.

>If you are really serious about testing, take a seperate branch of the source tree, introduce bugs on purpose, and verify that the tests will cath them.

**testing thouroughly**

>These tools help give you a general feel for how comprehensive your testing is, but don't expect to see 100% coverage.

>What is important is the number of states that your program may have. State are not equivalent to lines of code.

**Property based testing**

>A great way to explore how your code handles unexpected states is to ha a computer generate those states.

**Tightening the net**
>IF a bug slips through the net of exisitn gtests, you need to add a new test to trap it next time.


___
# References

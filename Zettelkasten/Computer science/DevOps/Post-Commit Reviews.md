222022-09-21

Style: #ArticleNotes 

Domain: #DevOps 

Tags:

# Post-Commit Reviews

### Pre-Commit Reviews

At AWS pre-commit code reviews before merging into the trunk are still mandatorz, despit a very robust automated testing solution, and the fact that all other steps of the deployment are automated. 

Code reviewers evaluate the code’s correctness and also evaluate whether the change can be safely deployed to production. They evaluate whether the code has sufficient tests (unit tests, integration tests, and canary tests), whether it is sufficiently instrumented for deployment monitoring, and whether it can be safely rolled back.

On average, I’d argue a code review slows a developer down from a few minutes to a few hours, and in the worst case scenario, a few days or even _months_ (when a change results in a long-running pull-request requiring the developer to rebase off of the main branch multiple times to avoid merge conflicts).
![[Pasted image 20220921091102.png]]

### Post-Commit Reviews

POst-commit reviews can serve as an alternative to pre-commit reviews. One of the main benefits of them would be that they run throuhg the full suite of post commit tests, and also them to keep their changes smaller since there are not pre-commit hold-ups, leading to developers creating more code before each commit.

It is important to note that even tough the review is moved to post commit, that does not mean that it is a post-deplozment review, it instead serves a a boundary before code is deployed.
![[Pasted image 20220921091319.png]]

Post-commit reviews offer a lot more flexibility than people imagine. “Post-commit” review can mean:

-   leaving code-review to be the _very last step_ of the deployment pipeline, before the code is deployed to production
-   or in cases where it’s not necessary for an audit-trail or mandatory sign-off on changes (for prototype projects, internal tools that don’t handle customer sensitive data and so forth), for code review to happen post-deployment.

### Benefits of Post-Commit Reviews

By far and away, the biggest benefit of post-commit reviews is developer focus.

Being able to merge pull requests in quick succession ensures that the developer can iterate faster on the feature they are developing. It also encourages the developer to keep pull requests small, ensuring a feature is developed in many small pull requests (or at the very least comprising of many atomic commits).

Another benefit of post-commit reviews is _reviewer focus._ In a culture where pull requests are small and numerous, being pinged every 10 or 15 minutes for a review can hinder the productivity of both the _reviewer_ and the _reviewee._

Moving to a post-commit review format frees the developer from requiring approval for every pull request. It also allows the reviewer to batch reviews, so they are reviewing a number of changes in one fell swoop, as opposed to being constantly interrupted for code reviews. Furthermore, this incentivizes reviewers to only comment on merged pull requests that raise concerns or have logical bugs. In my own experience, this reduces nitpicking on the reviewer’s part, and in the event the reviewer does nitpick, it allows the developer to address the minor concerns at their leisure.

There’s a case to be made for the fact that post-commit reviews encourage better development practices.

Getting to a point where post-commit reviews are a reality requires a strong cultural scaffolding. At the very least, it requires:

-   a culture of collaborating on aspects like API design prior to code implementation via a design document.
-   consensus around aspects like style-guide, coding idioms, concurrency primitives etc.
-   investment in better automation practices and tooling.

By “post-commit” testing, I’m referring to all the tests a code change is subjected to post-commit but pre-deploy: integration tests, load tests, soak tests, shadow tests, exception tracking and so forth.

### Challanges of Post-Commit Reviews

While post-commit reveiws have numerous benefits, they also come with their own set of challanges.

For one, there needs to be a high degree of trust among the team members, this can become a problem especially when on-barding new members, especially very junior ones. In this scenario for example, it migh tmake snese to have pre-commit reviews for the new team-members until they have been integrated into the team properly, and only then move them to post-commit reviews.

It is also important that we have robust automation in place, otherwise we would lose one of the main benefits of post-commit reveiw, the execution of post-commit tests. But the need for automation does not stop there.

Another important automation tool would be the automatic reverting of changes should any post-commit tests fail.

___
# References
[Post-Commit Reviews. I recently read an excellent article in… | by Cindy Sridharan | Medium](https://copyconstruct.medium.com/post-commit-reviews-b4cc2163ac7a)
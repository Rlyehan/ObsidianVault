222022-07-21

Status: #Research 

Tags: #DevOps

# continuous integration

For continuous integration (CI), team members integrate their code frequently, at least once a day. Each integration is checked with an automated build & tests to find integration errors as early as possible. 

The fact that these code integrations happen so frequently, as mentioned at least once a day per developer, it allows teams to find integration issues much earlier, when developers still are most familiar with the changes they created and these changes are of a small scale, a days work at most. 

This way we can fix issues much earlier and they remain in an manageable scope, preventing the long integration periods that might happen when we don't make use of CI. 

For an example we would check out the code from the main branch and develop our changes. Once we are finished and ready to commit to the main branch we run our suite of automated tests that we created for the changes we made. If we build successfully and the tests pass, the next step is to update our main branch and deal with any potential merge conflicts. After that we run the tests again to verify that we didn't break anything during merging. If that phase passes we are ready to commit our changes to the main branch. 

Now just because all tests passed on our local machiene doesn't neccesarily mean that they will pass on the main branch as well, as such we will build and run the test suit based on the main branch code and only if the tests pass this build are we finished with the change and can move on. Should the main build fail, fixing the issues is of critical importance and fixing the main branch supercedes any other task.

This way our main build is never in a failed state for long, and at the end of the day it should always be in a stable state that we can continue with on the next day.

Of course to use this in our project there are certain requirments that we need to meet for it to make sense.

First there needs to be a common repository that contains the main build.

Second we need to automate the build process. FOr this we need to create the relevant build scripts and have them and all other relevant resources version controlled.

Third is that we need to write automated tests to be executed for each build.

One more point is that we need to keep build times short, best pratice is to keep it under 10 minutes. If the build times are too long, we might need to investigate what causes that and find solutions to the problem. For testing specific optimizations look at the [[continuous testing]] section. If those recommendations and the optimization of build tasks are not enough to keep build times in line, the problem usually lies with our architecture or the scope of the build pipeline.

Another massive benefit of CI that should not be overlooked, is that it can give us metadata that we can look at for certain topics without the need to create extra tests if proper observabliliy has been taken into consideration. 

Some metrics could be:
- Percentage of commits triggering the CI pipeline
- Percentage of builds that pass automated tests
- Code coverage of each test run
- Time to fix broken builds
- Build time
- Test execution time
- etc.

These metrics can be used for different topics, some might be for optimizing the CI pipeline itself, others may indicate problems with certain parts of the repository, and others could show us optimization potential for our tests and much more.



___
# References

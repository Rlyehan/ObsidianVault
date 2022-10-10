222022-07-27

Status: #Idea

Tags: #DevOps 

# deployment piplines

one of the many challenges of [[continuous integration]] is that we want our builds to be fast, so that developers have short feedback cycles and also so that they make use of it as often as possible instead of, for example, collecting a lot of changes and running the CI-pipeline at the end of the day only.

One major factor that can slow down the runtimes are automated tests since comprehensive tests take a long time to execute.

To circumvent this issue we want to break up our build into smaller pieces by using a deployment pipeline.

The first stage would usually be the the compiling of any binaries that we would need and the running of unit tests for example. Later stages might be stages might be a stage for additional automated tests that are either slower to execute or need a production like environment to be able to run, or maybe both. 

Even later could be stages for manual checks that can't be automated or require human authorization. These steps could be parallelized by the pipeline to speed up the process.

In the end we could describe the resposibility of the deployment pipeline to detect any changes that will lead to problems, incuding non-functional areas like performance or security. 

Furthermore it allows for better collaboration between teams and provides visibility about cahnges in the system as well as providing an audit trail.



___
# References
[Patterns - Continuous Delivery](https://continuousdelivery.com/implementing/patterns/#the-deployment-pipeline)

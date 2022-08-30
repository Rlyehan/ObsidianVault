222022-07-21

Status: #Research 

Tags: #DevOps 

# Version Control

Version control is an important system to implement if we want to make use of [[CICD]]. Without them proper automation is not easily achievable.

DORA has found in their DevOps research that proper version control is one of the capabilities that lead to higher software delivery performance.

It is also important to make full use of the version control systems to not only version the software source code, but to also include any types of scripts, like for tests and deployments, as well as any service or infrastructure configuration within the version control.

One simple question we can ask ourselves to know if our version control system is implemented well and comprehensive enoough is to ask, can I go back to any production ready build we ever had and simply click a button to be able to deploy it, even if the build is years old? If the answer is yes, then everything relevant has been versioned aprobiatly, if teh answer is no, we could use the knwoledge of what went wrong to improve our version control.

Benefits of proper version control:
- **Reproducability**: If everything is versioned properly, we can go back to any point and reprocude the results.
- **Traceability**: We can track changes and see the differences easily.
- **Disaster Recovery**: We have all the necessary information to reproduce an enviroment should there be any type of issue, like a hardware failure.
- **Auditability**: Since every change is tracked, we have good auditability of our development.
- **Response to defects**: If any critical defects are found in our system, we can roll back to a verified working state.

So our Version control system should contain:
- All application code and dependencies
- Any scripts required (DB Table creation, etc.)
- All enviroment creation tools and artifacts
- Any file used to create or compose containers
- All supporting automated tests
- Any script required for deployment, enviroment provisioning, etc.
- Supporting project Artifacts, like Documentation, etc.
- Container orchestration, like Kubernetes configuration
- Cloud configuration, Cloudformation templates, etc.
- Any other script related, like DNS zone files, DBMS, etc.

All of the ones that are applicable to our project should be versioned, but that does not neccesarily mean that they all must be versioned using for example git. We can version each kind in an appropriate repository for it, we only need to make sure that there is a proper connection between the systems so we can connect the version of the source code with the approriate docker containers and kubernetes configuration for example.

___
# References
[DORA research program](https://www.devops-research.com/research.html)
[DevOps tech: Version control  |  DevOps capabilities  |  Google Cloud](https://cloud.google.com/architecture/devops/devops-tech-version-control)
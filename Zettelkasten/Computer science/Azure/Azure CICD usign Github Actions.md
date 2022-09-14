222022-08-03

Style: 

Domain:

Tags:

# Azure CICD using Github Actions

### The Flow of Github Actions
![[Pasted image 20220803092739.png]]

### Github Workflows
```YAML
# .github/workflows/build.yml 
name: Node Build. 

on: [push] 
jobs: 
	mainbuild: runs-on: ${{ matrix.os }} 
	
	strategy: 
		matrix: 
			node-version: [12.x] 
			os: [windows-latest] 
	
	steps: 
	
	- uses: actions/checkout@v1 
	- name: Run node.js on latest Windows. 
	- uses: actions/setup-node@v1 
	- with: node-version: ${{ matrix.node-version }} 
	
	- name: Install NPM and build. 
	- run: | 
	    npm ci 
	    npm run build

```

a starter workflow can be found at: [GitHub - actions/starter-workflows: Accelerating new GitHub Actions workflows](https://github.com/actions/starter-workflows)

a workflow has following elements:
- name: Name of the Workflow
- on: Event that triggers the workflow
- Jobs: list of jobs to be executed, can be one or more
- Runs-on: tells actions wich runner to use
- Steps: list of steps for the job
- Uses: tells us wich predefined actions need to be retrieved
- Run: tells the job to execute a command on the runner

### Events

We can make use of different event for our workflows. Examples could be sheduled events to be executed by the sheduling rule, or code events, for example on pull-request.

### Jobs
example:
```Yaml
jobs:
  startup:
    runs-on: ubuntu-latest
    steps:

      - run: ./setup_server_configuration.sh
  build:
    needs: startup
    steps:

      - run: ./build_new_server.sh

```

### Runners

when you execute a job the steps are executed on runners. Github already has a list of available runners that we can use, but if we need a different configuration we also host our own runner.

### Variables
we can also use variables in our workflows, for example like this:
```yaml
jobs:
    verify-connection:
        steps:
            - name: Verify Connection to SQL Server
            - run: node testconnection.js
        env:
            PROJECT_SERVER: PH202323V
            PROJECT_DATABASE: HAMaster
```

### Artifacts
We can upload and share artifacts between jobs. We can also download artifacts and set the retention periods for the artifacts.

### Badges
For better visibility we can have badges showcased on the repository for example showing wether the build is passing the tests or not.

### Best pratices for creating actions
It's essential to follow best practices when creating actions:

-   Create chainable actions. Don't create large monolithic actions. Instead, create smaller functional actions that can be chained together.
-   Version your actions like other code. Others might take dependencies on various versions of your actions. Allow them to specify versions.
-   Provide the **latest** label. If others are happy to use the latest version of your action, make sure you provide the **latest**label that they can specify to get it.
-   Add appropriate documentation. As with other codes, documentation helps others use your actions and can help avoid surprises about how they function.
-   Add details **action.yml** metadata. At the root of your action, you'll have an **action.yml** file. Ensure it has been populated with author, icon, expected inputs, and outputs.
-   Consider contributing to the marketplace. It's easier for us to work with actions when we all contribute to the marketplace. Help to avoid people needing to relearn the same issues endlessly.

### Secrets
If we need encrypted secrets we can set them at the repository or Orginizaton level under Settings -> Secrets. We can invoke them inside the workflows with:

```
${{ secrets.SecretName }}
```


___
# References
[AZ-400: Implement CI with Azure Pipelines and GitHub Actions - Learn | Microsoft Docs](https://docs.microsoft.com/en-us/learn/paths/az-400-implement-ci-azure-pipelines-github-actions/)
[Tutorial: Create a CI/CD pipeline for your existing code by using Azure DevOps Starter | Microsoft Docs](https://docs.microsoft.com/en-us/azure/devops-project/azure-devops-project-github)
[Configure CI/CD with GitHub Actions - Azure App Service | Microsoft Docs](https://docs.microsoft.com/en-us/azure/app-service/deploy-github-actions?tabs=applevel)
[Use GitHub Actions to make code updates in Azure Functions | Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-github-actions?tabs=dotnet)
[Quickstart - Use Azure Key Vault secrets in GitHub Actions workflows | Microsoft Docs](https://docs.microsoft.com/en-us/azure/developer/github/github-key-vault)
[Trigger an Azure Pipelines run from GitHub Actions - Azure Pipelines | Microsoft Docs](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/github-actions?view=azure-devops)
[Create a function app with GitHub deployment - Azure CLI | Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-functions/scripts/functions-cli-create-function-app-github-continuous)
[AZ400-DesigningandImplementingMicrosoftDevOpsSolutions](https://microsoftlearning.github.io/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/Instructions/Labs/AZ400_M03_L07_Implementing_GitHub_Actions_by_using_DevOps_Starter.html)

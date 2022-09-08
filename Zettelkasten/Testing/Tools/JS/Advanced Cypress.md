222022-09-08

Style: #CourseNotes 

Domain: #TestAutomation 

Tags:

# Advanced Cypress

### 1. Setup
- Which command installs cypress: npm install
- which folder holds cypress tests: integrations
- Which command opens Cypress GUI: npx cypress open
- To run a test in teh GUI: 

### 2. Chaining Commands

When we chain commands in cypress, the retry fucntionality will take that into consideration and retry accordingly.

- .contains() is a dual command: true
- Cypress will retry: current and previous commands
- Commands in Cypress: can be parent, child, dual
- .eq(2) command would select: the third element

### 3. Multiple Assertions

- .then() function does a retry: False
- To write multipe assertions for an array of elemente we can use: then()
- We can retry multiple assertions: bz giving a function to should()
- When .get() command returns multiple elements it will pass them as:
- To write assertions in a .then() we use: give it a function

### 4. Changing the DOM

- What is not checked when euninng .click(): hover
- {force:true} argument will: skip actionability checks
- Which command will add a new class to our element: .invoke('addClass', "className")
- To simulate oser moving mouse over an element: .trigger('mouseover')


### 5. Cookies

- Cypress deletes all cookies in between tests: true
- To set cookies in browser we use: .setCookie()
- The best place to define cookies for future use: support/index.js
- To prevent cookies from beeing deleted within a spec file: Cypress.Cookies.preserveOnce()

### 6. Sending requests

- to send an API request we use: .request()
- using .request() command is useful when: we want to execute an request
- .request() will use base url from: the cypress.json file
- If we don't have API documentation: we can see the requests in the test runner


### 7. Intercepting Network Requests

- To wait for a network request we use: we intecept and wait for the command to happen
- To create an alieas we use: .as()
- to use.wait() with an alias we use: @symbole
- Which attributes of requests cannot be tested: number of requests

### 8. Stubbing responses

- To substitute a response body, we use: body
- When we want to use a file we store it: in oru fixtures folder
- To simulate a network error: forceNetworkError
- to cahnge the resposne dynamically second intercept() param: function


### 9. Costum commands

- to add a new command we use: Cypress.Commands.add()
- to aotucomplete our command: Add a .d.ts file to our project
- to add JSDoc to our function: /** JSDoc here */
- to create a dual command: { prevSubject: 'optional' }


### 10. Installing Plugins

- to install a plugin, we import it inside: support/commands.js or support/index.js file
- To perfom actions, Cypress uses: JavaScript
- Cypress native events plugin uses: Chrome DevTools protocol
- To do a visual test with applitools: .eyesOpen(), .eyesCheckWindow(), .eyesClose()

### 11. Running a Task

- to define a task we edit: plugins/index.js file
- to pass a parameter  to our task: we need to define a second parameter to our .task() command
- When we run a task, Cypress will: wait for task to finish
- To tap into task events: on('task') function

___
# References
[Advanced Cypress](https://testautomationu.applitools.com/advanced-cypress-tutorial/)
[GitHub - filiphric/cypress-tau-course](https://github.com/filiphric/cypress-tau-course)
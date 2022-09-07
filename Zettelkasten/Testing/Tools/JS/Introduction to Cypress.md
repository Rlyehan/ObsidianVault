222022-09-03

Style: #CourseNotes 

Domain: #tools 

Tags:

# Introduction to Cypress

### 1. Install cypress
- Cypress can run tests under: Chrome
- It can use the following test runner: Mocha
- It is fast because: It runes in the browser
- We can run it locally by: executing npx cypress open

### 2. Writing our first test
- Test code needs to run under: cypress/integration
- Tests run inside: a it function
- To navigate to a page: we use cy.visit

### 3. Accessing and interacting with elements
test example:
```JS
/// <reference types="cypress" />

it('should add a new todo to the list', () => {

cy.visit('http://todomvc-app-for-testing.surge.sh/')

cy.get('.new-todo', {timeout: 6000}).type('Clean room{enter}')

cy.get('.toggle').click()

cy.contains('Clear completed').click()

})
```

- watchForFileChanges enables: Rerun Cypress tests when test file changes
- cy.get(selector) will: find an element with the selector
- cy,type will: Type into an input field
- cy.contains woll: Find an element that acontains some text
- Cypress is less flaky because: because it waits by default before failure
- We can change teh timeout: By adding a {timeout: new-timeout}

### 4. Validations
added validations to our tests:
```JS
/// <reference types="cypress" />

it('should add a new todo to the list', () => {

cy.visit('http://todomvc-app-for-testing.surge.sh/')

cy.get('.new-todo', {timeout: 6000}).type('Clean room{enter}')

cy.get('label').should('have.text', 'Clean room')

cy.get('.toggle').should('not.be.checked')

cy.get('.toggle').click()

cy.get('label').should('have.css', 'text-decoration-line', 'line-through')

cy.contains('Clear completed').click()

cy.get('.todo-list').should('not.have.descendants', 'li')

})
```

- To validate that a certain element has text: we use cy.get(selector).should('have.text', the-text)
- The fastest way to determine which vaidations cypress has is: Code completion
- Checking wether a checkbox is checked: cy.get(selector).should(‘be.checked’)
- Checking that a list has no items: cy.get(selector).should('not.have.descendants', 'li')

### 5. Grouping tests with Mocha
we added a actions.spec.js:
```JS
/// <reference types="cypress" />

describe('todo actions', () => {

beforeEach(() => {

cy.visit('http://todomvc-app-for-testing.surge.sh/')

cy.get('.new-todo', {timeout: 6000}).type('Clean room{enter}')

})

it('should add a new todo to the list', () => {

cy.get('label').should('have.text', 'Clean room')

cy.get('.toggle').should('not.be.checked')

})

describe('toggling todos', () => {

it('should toggle test correctly', () => {

cy.get('.toggle').click()

cy.get('label').should('have.css', 'text-decoration-line', 'line-through')

})

it('should clear completed', () => {

cy.get('.toggle').click()

cy.contains('Clear completed').click()

cy.get('.todo-list').should('not.have.descendants', 'li')

})

})

})
```

and have our filtering.spec.js:

```JS
describe('filtering', function() {

beforeEach(() => {

cy.visit('http://todomvc-app-for-testing.surge.sh/')

cy.get('.new-todo').type('Clean room{enter}')

cy.get('.new-todo').type('Learn JavaScript{enter}')

cy.get('.new-todo').type('Use Cypress{enter}')

cy.get('.todo-list li:nth-child(2) .toggle').click()

})

it('should filter "Active" correctly', () => {

cy.contains('Active').click()

cy.get('.todo-list li').should('have.length', 2)

})

it('should filter "Completed" correctly', () => {

cy.contains('Completed').click()

cy.get('.todo-list li').should('have.length', 1)

})

it('should filter "All" correctly', () => {

cy.contains('All').click()

cy.get('.todo-list li').should('have.length', 3)

})

})
```

- to group a set of tests in a file: describe( () => ...)
- to run just one test: it.only(....)
- to run code before each test: beforeEach( () =>... ) 
- beforeEach applies to: all tests witihn the describe group

### 6. Cypress CLI

to just run the tests we use `npx cypress run`. If we want to run specific tests we use `npx cypress run --spec cypress/integration/todomvc-actions.spec.js`.

That is nice, but usually we want to modify our pacakge.json to make sure we can execute tests everywhere. So in the package.json we add:
```json
"scripts": {
    "test": "cypress run",
    "cypress": "cypress open"
}
```

- To run all tests: npx cypress run
- to run a specific file: npx cypress run --spec test-file
- The stahdard way to execute tests in NPM: npm test
- To make cypress run as standard: change pacakge.json to include the script above


### 7. Page Object in cypress

for our example we created a page object:
```JS
/// <reference types="cypress" />

export function navigate() {

cy.visit('http://todomvc-app-for-testing.surge.sh/')

}

export function addTodo(todoText) {

cy.get('.new-todo').type(todoText + '{enter}')

}

export function toggleTodo(todoIndex) {

cy.get(`.todo-list li:nth-child(${todoIndex + 1}) .toggle`).click()

}

export function showOnlyCompletedTodos() {

cy.contains('Completed').click()

}

export function showOnlyActiveTodos() {

cy.contains('Active').click()

}

export function showAllTodos() {

cy.contains('All').click()

}

export function clearCompleted() {

cy.contains('Clear completed').click()

}

export function validateNumberOfTodosShown(expectedNumberOfTodos) {

cy.get('.todo-list li').should('have.length', expectedNumberOfTodos)

}

export function validateTodoCompletedState(todoIndex, shouldBeCompleted) {

const l = cy.get(`.todo-list li:nth-child(${todoIndex + 1}) label`)

l.should(`${shouldBeCompleted ? '' : 'not.'}have.css`, 'text-decoration-line', 'line-through')

}

export function validateTodoText(todoIndex, expectedText) {

cy.get(`.todo-list li:nth-child(${todoIndex + 1}) label`).should('have.text', expectedText)

}

export function validateToggleState(todoIndex, shouldBeToggled) {

const label = cy.get(`.todo-list li:nth-child(${todoIndex + 1}) label`)

label.should(`${shouldBeToggled ? '' : 'not.'}be.checked`)

}
```

and we changed our tests:

```JS
/// <reference types="cypress" />

import {

navigate,

addTodo,

validateTodoText,

toggleTodo,

clearCompleted,

validateTodoCompletedState,

validateToggleState,

validateNumberOfTodosShown,

} from '../page-objects/todo-page'

describe('todo actions', () => {

beforeEach(() => {

navigate()

addTodo('Clean room')

})

it('should add a new todo to the list', () => {

validateTodoText(0, 'Clean room')

validateToggleState(0, false)

})

describe('toggling todos', () => {

it('should toggle test correctly', () => {

toggleTodo(0)

validateTodoCompletedState(0, true)

})

it('should clear completed', () => {

toggleTodo(0)

clearCompleted()

validateNumberOfTodosShown(0)

})

})

})
```

```JS
/// <reference types="cypress" />

import {

navigate,

addTodo,

toggleTodo,

showOnlyActiveTodos,

showOnlyCompletedTodos,

showAllTodos,

validateNumberOfTodosShown,

} from '../page-objects/todo-page'

describe('filtering', function() {

beforeEach(() => {

navigate()

addTodo('Clean room')

addTodo('Learn JavaScript')

addTodo('Use Cypress')

toggleTodo(1)

})

it('should filter "Active" correctly', () => {

showOnlyActiveTodos()

validateNumberOfTodosShown(2)

})

it('should filter "Completed" correctly', () => {

showOnlyCompletedTodos()

validateNumberOfTodosShown(1)

})

it('should filter "All" correctly', () => {

showAllTodos()

validateNumberOfTodosShown(3)

})

})
```

- A class is better than a set of functions: Both are fine
- to export a class from a file we use: export class Foo {...}
- to import a class: import {Foo} from ./foo
- to export a function: export function ...

### 8. Visual Validation

we need to install applitoos eyes `npm install @applitools/eyes-cypress@3` and run `npx eyes-setup`.

we created a visual test file visuals.spec.js using Applitools eyes:
```JS
/// <reference types="cypress" />

import * as todoPage from '../page-objects/todo-page'

describe('visual validation', () => {

before(() => cy.visit('http://todomvc-app-for-testing.surge.sh/?different-title-color'))

beforeEach(() =>

cy.eyesOpen({

appName: 'TAU TodoMVC',

batchName: 'TAU TodoMVC',

browser: [

{name: 'chrome', width: 1024, height: 768},

{name: 'chrome', width: 800, height: 600},

{name: 'firefox', width: 1024, height: 768},

{deviceName: 'iPhone X'},

]

})

)

afterEach(() => cy.eyesClose())

it('should look good', () => {

cy.eyesCheckWindow('empty todo list')

todoPage.addTodo('Clean room')

todoPage.addTodo('Learn javascript')

cy.eyesCheckWindow('two todos')

todoPage.toggleTodo(0)

cy.eyesCheckWindow('mark as completed')

})

})
```

- visual testing is: Validating that the application looks liek it should
- To do visual calidation in Cypress: Take a screenshot and verify that it conforms to the design system defined
- To take a screenshot with applitoosl eyes: Use “cy.eyesCheckWindow(...)”
- to generate and test teh cisual appreance: Add a “browser” section to the “cy.eyesOpen” with the list of browser configurations you want.
- Liking a diff: adds the screenshot as new baseline
- a diff is unresolve because: The algorithm can’t determine whether the diff is a bug or a welcome change

___
# References
[Introduction to Cypress](https://testautomationu.applitools.com/cypress-tutorial/)
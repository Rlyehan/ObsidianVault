222022-10-11

Style: 

Domain:

Tags:

# [[Playwright]] Introduction

### Typing into text fields

```javascript
const { chromium } = require('playwright');

// https://playwright.dev/docs/api/class-elementhandle#element-handle-type

( async () => {

// function code

// launching browser

const browser = await chromium.launch({ headless: false, slowMo: 100});

const page = await browser.newPage();

await page.goto('https://the-internet.herokuapp.com/forgot_password');

// code to type in email textbox

const email = await page.$('#email');

// delaying typing 50 ms to simulate real user speed typing

await email.type('ixchel@mail.com', { delay : 50});

await browser.close();

})();
```

### Click on controll

```javascript
const { chromium } = require('playwright');

// https://playwright.dev/docs/api/class-page#page-click

// https://playwright.dev/docs/api/class-elementhandle#element-handle-click

(async () => {

// function code

// launching browser

const browser = await chromium.launch({ headless: false, slowMo: 100 });

const page = await browser.newPage();

await page.goto('https://www.apronus.com/music/lessons/unit01.htm');

// click on the keynotes

await page.click('#t1 > table > tr:nth-child(1) > td:nth-child(1) > button');

await browser.close();

})();
```

### Interact with dropdown
```js
const { chromium } = require('playwright');

// https://playwright.dev/docs/input#select-options

(async () => {

// function code

const browser = await chromium.launch({ headless: false, slowMo: 300 });

const page = await browser.newPage();

await page.goto('https://the-internet.herokuapp.com/dropdown');

const dropdown = await page.$('#dropdown');

// value

await dropdown.selectOption({ value: '1' });

// label

await dropdown.selectOption({ label: 'Option 2' });

// index

await dropdown.selectOption({ index: 1 });

// values inside this select

const availableOptions = await dropdown.$$('option');

for (let i = 0; i < availableOptions.length; i++) {

console.log(await availableOptions[i].innerText());

}

await browser.close();

})();
```


### Interact with checkboxes or radio toggles
```js
const { firefox, chromium } = require('playwright');

// https://playwright.dev/docs/api/class-elementhandle#element-handle-check

// https://playwright.dev/docs/api/class-page#page-check

(async () => { // anonymous arrow function

// function code

// launching browser

const browser = await firefox.launch({ headless: false, slowMo: 400 });

// creating a page inside browser

const page = await browser.newPage();

// navigating to site

await page.goto('https://www.w3schools.com/howto/howto_css_custom_checkbox.asp');

const checks = await page.$$('#main div :nth-child(1) input[type="checkbox"]');

// check the second checkbox

await checks[1].check();

// uncheck the first checkbox

await checks[0].uncheck();

//select the second radio button

const radios = await page.$$('#main div :nth-child(1) input[type="radio"]');

await radios[1].check();

// closing browser

await browser.close();

})(); // function calls itself
```


### Interafct with iframes

```js
const { webkit } = require('playwright');

// https://playwright.dev/docs/api/class-frame#frame-page

(async () => { // anonymous arrow function

// function code

// launching browser

const browser = await webkit.launch({ headless: false, slowMo: 100 });

// creating a page inside browser

const page = await browser.newPage();

// navigating to site

await page.goto('https://www.demoqa.com/frames');

// logic to handle the iframe

const frame1 = await page.frame({ url: /\/sample/ }); //regex

// locating h1 element handle inside frame

const heading1 = await frame1.$('h1');

console.log(await heading1.innerText());

// closing browser

await browser.close();

})(); // function calls itself
```

### alerts
```js
const { chromium } = require('playwright');

https://playwright.dev/docs/dialogs#alert-confirm-prompt-dialogs

(async() => { // anonymous arrow function

// function code

// launching browser

const browser = await chromium.launch({headless:false, slowMo:400});

// creating a page inside browser

const page = await browser.newPage();

// navigating to site

await page.goto('https://www.demoqa.com/alerts');

// code to handle the alerts

// use once to only use the listener once

// accept dialog

page.once('dialog', async dialog =>{

console.log(dialog.message());

await dialog.accept();

});

await page.click('#confirmButton');

// enter text and accept dialog

page.once('dialog', async dialog =>{

console.log(dialog.message());

await dialog.accept('my text is this');

});

await page.click('#promtButton');

// closing browser

await browser.close();

})(); // function calls itself
```

### State of elements

```js
const { chromium } = require('playwright');

// https://playwright.dev/docs/actionability

// https://playwright.dev/docs/api/class-elementhandle#element-handle-wait-for-element-state

(async () => { // anonymous arrow function

// function code

// launching browser

const browser = await chromium.launch();

// creating a page inside browser

const page = await browser.newPage();

// navigating to site

await page.goto('https://demoqa.com/automation-practice-form');

// element handles

const firstName = await page.$('#firstName');

const sportsCheck = await page.$('#hobbies-checkbox-1');

const submitBtn = await page.$('#submit');

// print the element state

console.log(await firstName.isDisabled());

console.log(await firstName.isEnabled());

console.log(await firstName.isEditable());

console.log(await sportsCheck.isChecked());

console.log(await sportsCheck.isVisible());

console.log(await submitBtn.isHidden());

console.log(await submitBtn.isVisible());

// closing browser

await browser.close();

})(); // function calls itself
```

### Virtual keyboard

```js
const { chromium } = require('playwright');

// https://playwright.dev/docs/api/class-keyboard

(async () => { // anonymous arrow function

// function code

// launching browser

const browser = await chromium.launch({ headless: false, slowMo: 100 });

// creating a page inside browser

const page = await browser.newPage();

// navigating to site

await page.goto('https://the-internet.herokuapp.com/key_presses');

await page.click('input');

await page.keyboard.type('one does not simply exit vim');

await page.keyboard.down('Shift');

for (let i = 0; i < ' exit vim'.length; i++) {

await page.keyboard.press('ArrowLeft');

}

await page.keyboard.up('Shift');

await page.keyboard.press('Backspace');

await page.keyboard.type(' walk into mordor');

// closing browser

await browser.close();

})(); // function calls itself
```

### using the mouse

```js
const { chromium } = require('playwright');

(async () => { // anonymous arrow function

// function code

// launching browser

const browser = await chromium.launch({ headless: false, slowMo: 100 });

// creating a page inside browser

const page = await browser.newPage();

// navigating to site

await page.goto('https://paint.js.org/');

// drawing a square

await page.mouse.move(200, 200);

await page.mouse.down();

await page.mouse.move(400, 200);

await page.mouse.move(400, 400);

await page.mouse.move(200, 400);

await page.mouse.move(200, 200);

await page.mouse.up();

// closing browser

await browser.close();

})(); // function calls itself
```

### Taking screenshots

```js
const { chromium } = require('playwright');

// https://playwright.dev/docs/screenshots

(async () => { // anonymous arrow function

// function code

// launching browser

const browser = await chromium.launch();

// creating a page inside browser

const page = await browser.newPage();

// navigating to site

await page.goto('https://applitools.com/');

// take screenshot code

await page.screenshot({ path: 'screenshot.png' });

// take screenshot of an element

const logo = await page.$('.logo');

await logo.screenshot({ path: 'logo.png' });

// take screenshot of full page (long page)

await page.screenshot({ path: 'fullpage.png', fullPage: true });

// closing browser

await browser.close();

})(); // function calls itself
```


### Emulate mobile devices

```js
const { chromium, devices } = require('playwright');

const iPhone = devices['iPhone 11'];

// https://playwright.dev/docs/emulation#geolocation

// https://playwright.dev/docs/test-configuration#mobile-emulation

(async () => {

// mobile code

// console.log(iPhone); // -> { userAgent: , screen {....}}

const browser = await chromium.launch({ headless: false, slowMo: 300 });

const context = await browser.newContext({

...iPhone, // spread operator unpacks iPhone const

permissions: ['geolocation'], // allows geolocation

geolocation: { latitude: 19.432608, longitude: -99.133209 }, // Mexico City coordinates

locale: 'fr-FR' // French from France

});

const page = await context.newPage();

await page.goto('https://www.google.com/maps');

await page.waitForTimeout(5000); // timeout of 5000ms only for debugging purposes

await browser.close();

})();
```

### Integration with [[Jest]]

```js
const { chromium } = require('playwright');

describe(`UI tests for bookstore using playwright`, () => {

// jest timeout is by default 5000ms to run tests, with this we override this value

jest.setTimeout(10000);

let browser = null;

let page = null;

let context = null;

// array to store element handles of the cells on the first row

let firstRowCells = null;

// runs before all tests

beforeAll(async() =>{

browser = await chromium.launch();

context = await browser.newContext();

page = await context.newPage();

await page.goto('https://demoqa.com/books');

});

// runs after all tests

afterAll(async() =>{

await browser.close();

});

test(`Should load page`, async() =>{

// assertions

expect(page).not.toBeNull();

expect(await page.title()).not.toBeNull();

});

test(`Should be able to search for eloquent javascript`, async() =>{

await page.fill('#searchBox','eloquent');

// an assertion could go here

});

test(`Should check if book image is okay`, async() =>{

// array element handle to store all cell element handles

firstRowCells = await page.$$('.ReactTable .rt-tr-group:nth-child(1) .rt-td');

// retrieving the element handle with img tag in the first element on the array

let imgUrl = await firstRowCells[0].$('img');

// assertion

expect(await imgUrl.getAttribute('src')).not.toBeNull();

});

test(`Should check if title is okay`, async() =>{

// assertion

expect(await firstRowCells[1].innerText()).toBe('Eloquent JavaScript, Second Edition');

});

test(`Should check if author is okay`, async() =>{

// assertion

expect(await firstRowCells[2].innerText()).toBe('Marijn Haverbeke');

});

test(`Should check if publisher is okay`, async() =>{

// assertion

expect(await firstRowCells[3].innerText()).toBe('No Starch Press');

});

});
```

### Page Object Model

Base model:
```js
// https://playwright.dev/docs/pom

class BasePage{

constructor(page){

this.page = page;

}

/**

* Method to navigate to path passed

* @param {string} path

*/

async navigate(path){

await this.page.goto(`https://demo.applitools.com/${path}`)

}

}

module.exports = BasePage;
```

Homepage model:
```js
// https://playwright.dev/docs/pom

const BasePage = require('./Base.page');

class HomePage extends BasePage {

constructor(page){

// calling the parent class BasePage constructor - inheritance

super(page);

//selectors

this.loggedUser = '.logged-user-name';

this.balances = '.balance-value';

}

/**

* Method to retrieve the username

* @returns {string} username logged

*/

async getUserName(){

let user = await this.page.$(this.loggedUser);

return await user.innerText();

}

/**

* Method to retrieve the balance typ

* @param {string} balType : 'total', 'credit', 'due today'

* @returns string

*/

async getBalance(balType){

let balArray = await this.page.$$(this.balances);

if(balType == 'total'){

// according to the DOM the first balance has an extra span

return (await balArray[0].$('span')).innerText();

}

else if(balType == 'credit'){

return (await balArray[1]).innerText();

}

else {

return (await balArray[2]).innerText();

}

}

/**

* Method to navigate to home page using parent's method

*/

async navigate(){

await super.navigate('app.html');

}

}

module.exports = HomePage;
```

Login Model:
```js
// https://playwright.dev/docs/pom

const BasePage = require('./Base.page');

class LoginPage extends BasePage {

constructor(page){

super(page);

// selectors

this.userNameTxt = '#username';

this.passwordTxt = '#password';

this.loginBtn = '#log-in';

}

/**

* Method to navigate to Login page using parent's method

*/

async navigate(){

await super.navigate('index.html');

}

/**

* Method to login using username and password

* @param {string} username

* @param {string} password

*/

async login(username, password){

await this.page.fill(this.userNameTxt,username);

await this.page.fill(this.passwordTxt,password);

await this.page.click(this.loginBtn);

}

}

module.exports = LoginPage;
```

Using the models in tests:
```js
// https://jestjs.io/docs/expect

const { chromium } = require('playwright');

const HomePage = require('../models/Home.page');

const LoginPage = require('../models/Login.page');

describe('Applitools demo page', () => {

jest.setTimeout(30000);

let browser = null;

let context = null;

let page = null;

let homePage = null;

let loginPage = null;

beforeAll( async ()=>{

// we launch browser and navigate to the loginpage

browser = await chromium.launch({ headless: false });

context = await browser.newContext();

page = await context.newPage();

homePage = new HomePage(page);

loginPage = new LoginPage(page);

await loginPage.navigate();

});

afterAll( async ()=>{

// closing browser

await context.close();

await browser.close();

});

it('Should be able to login', async() => {

await loginPage.login('username','password');

expect(await page.title()).not.toBeNull();

})

it('Should be logged in as Jack Gomez', async() => {

expect(await homePage.getUserName()).toBe('Jack Gomez');

})

it('Should have total balance of $350', async() => {

expect(await homePage.getBalance('total')).toBe('$300');

})

it('Should have credit available of $17800', async() => {

expect(await homePage.getBalance('credit')).toBe('$17800');

})

it('Should have due today of $180', async() => {

expect(await homePage.getBalance('due')).toBe('$180');

})

});
```



___
# References
[Playwright with JavaScript](https://testautomationu.applitools.com/js-playwright-tutorial/)
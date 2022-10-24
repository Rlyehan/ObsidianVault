222022-10-11

Style: 

Domain:

Tags:

# Playwright Introduction

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







___
# References
[Playwright with JavaScript](https://testautomationu.applitools.com/js-playwright-tutorial/)
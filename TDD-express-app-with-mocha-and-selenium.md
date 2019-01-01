# Setting up selenium with mocha and chai

## Create folder

`npm init`

## Install the packages for testing

`npm install --save-dev chromedriver selenium-webdriver mocha chai chai-as-promised`

## Install Express
`npm install --save express`


## Set up files
in root, create app.js file - or use index.js - update your package.json to have this file as the `main`

in package.json
```
"scripts": {
  "test": "mocha || true",
  "start": "node app.js",
  "test:e2e": "mocha --timeout 20000 e2e/tests.js"
},
```

This assumes that your tests are in a e2e folder - so create that with the following example:

```javascript
require('chromedriver');
const webdriver = require('selenium-webdriver');
const { By, until } = webdriver;

const chai = require('chai');
const chaiAsPromised = require('chai-as-promised');

chai.use(chaiAsPromised);
const driver = new webdriver.Builder().forBrowser('chrome').build();
const expect = chai.expect;

describe('End to End Test Suite', done => {
  before(done => {
    console.log('Go to page');
    driver.get('http://localhost:3000').then(function(res) {
      driver
          .then(() => {
          driver.wait(until.elementLocated(By.tagName('body'))).then(() => {
            done();
          });
        });
    });
  });

  it('can read welcome message', () => {
    console.log('Ready to read welcome message');
    return expect(driver.findElement(By.tagName("body")).getText()).to.eventually.contain("hello world");
  });
});

```
## Run

Run with `npm run test:e2e`

This will fail as you haven't yet set up the app file - ` This site can’t be reached`

## Fix issues

app.js:
```javascript
var express = require('express');
var app = express();

app.get('/', function(req, res){
  res.send('hello world');
});

app.listen(3000);
```

## Start server
and then start the server with `npm start`

## Run tests
run the test again

`npm run test:e2e`

It should pass

## Example of running steps in the front end before testing

If you need to carry out some actions before the test

```javascript
// rest of file
driver
       .findElement(By.id('username'))
       .sendKeys('cuneyt')
       .then(() => driver.findElement(By.id('password')).sendKeys('sifre123'))
       .then(() => driver.findElement(By.id('btn')).click())
       .then(() => {
         driver.wait(until.elementLocated(By.id('message'))).then(() => {
           done();
         });
       });

// rest of tests
```
or

``` javascript
driver.get("http://localhost:3111/");
driver.findElement(By.name("name")).click();
driver.findElement(By.name("name")).clear();
driver.findElement(By.name("name")).sendKeys("a b c");
driver.findElement(By.name("quote")).clear();
driver.findElement(By.name("quote")).sendKeys("this is a new quote");
driver.findElement(By.id("submit_btn")).click();
```

## Tips for getting the button clicks and interactions

Install a selenium IDE - e.g. Katalon Recorder allows you to also export into a format that I was able to near enough copy and paste into tests

https://chrome.google.com/webstore/detail/katalon-recorder-selenium/ljdobmomdgdljniojadhoplhkpialdid

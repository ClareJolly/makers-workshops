Selenium set up

` npm init `

Install the relevant packages
```
npm install --save-dev chromedriver selenium-webdriver mocha chai chai-as-promised
```

Install express package
```
npm install --save express
```

Set up an `app.js` file in the root
```
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send(`
    <html>
    <form action="/main">
      username: <input type="text" id="username" /><br />
      password: <input type="text" id="password" />
      <input id="btn" type="submit" />
    </form>
    </html>
  `);
});

app.get('/main', (req, res) => {
  res.send(`
    <html>
    <title>
      My awesome page
    </title>
    <body>
    <div id="message">
      Welcome to my page!
    </div>
    </body>
    </html>
  `);
});

app.listen(3003, () => console.log('Example app listening on port 3003!'));
```

Add the following to `package.json`

```
"start": "node index.js",
    "test:e2e": "mocha --timeout 20000 e2e/tests.js"
```

This assumes that your tests are in a e2e folder - so create that with the following example:
```
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
    console.log('Before everything login to the page');
    driver.get('http://localhost:3003').then(function(res) {
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
    });
  });

  it('can read welcome message', () => {
    console.log('Ready to read welcome message');
    return expect(driver.findElement(By.id('message')).getAttribute('innerHTML'))
    .to.eventually.contain('Welcome to my page!');
  });
});
```


Run this by:
` npm run test:e2e `

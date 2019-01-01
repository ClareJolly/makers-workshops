https://waffle.io/sacullezzar/acebook-node/join

http://www.techinsight.io/review/devops-and-automation/browser-automation-using-selenium-webdriver-and-nodejs/

https://nodejs.org/api/assert.html

https://hackernoon.com/a-crash-course-on-testing-with-node-js-6c7428d3da02

https://www.youtube.com/watch?v=MLTRHc5dk6s

https://cuneyt.aliustaoglu.biz/en/end-to-end-javascript-testing-with-selenium-mocha-chai/

## Testing with Mocha etc

```
npm init
npm install mocha chai --save-dev

(in package.json): 'scripts':{"test":"mocha||true"}
```

Project/test/appTest.js
Project/app.js
Project/package.js
Project/package-lock.js

in appTest.js:
```
const assert = require('chai').assert
const app = require('../app')
```

In "app.js":
```
module.exports = {
***code to test here***
}
```

Example test:

```
describe('App', function() {
it('says hello', function() {
assert.equal(app.sayHello(), 'hello')
})
})
```

This is a basic setup guide for using mocha/chai to test JS in the terminal.  Use
```
npm run test
```

## Setting up selenium

Using Mocha, chai, selenium, express

Set up the package in your folder
`npm init`

Then testing framework set up

Install the relevant packages
```
npm install --save-dev chromedriver selenium-webdriver mocha chai chai-as-promised
```

Install express package
```
npm install --save express
```

Set up an `app.js` file in the root
```javascript
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
 `npm run test:e2e`  

## further links

https://www.youtube.com/user/DevTipsForDesigners

https://www.youtube.com/watch?v=Mg7Ma5i8NgM&list=PLqGj3iMvMa4LFqyGab_aR7M0zfQm2KTuX

Selenium Cheatsheet - https://gist.github.com/huangzhichong/3284966 (edited)

and another: https://www.automatetheplanet.com/selenium-webdriver-locators-cheat-sheet/

one summarisation of express and what each element is doing:
https://medium.freecodecamp.org/going-out-to-eat-and-understanding-the-basics-of-express-js-f034a029fb66

Good summary of node.js overall and then how express fits in with details of what each section of the set up does
https://medium.com/@LindaVivah/the-beginners-guide-understanding-node-js-express-js-fundamentals-e15493462be1

https://hackernoon.com/the-definitive-guide-to-express-the-node-js-web-application-framework-649352e2ae87

https://www.youtube.com/watch?v=OlapNW9Jc8s&t=753s

for when we get to it - this takes through a basic react app using express
https://auth0.com/blog/react-tutorial-building-and-securing-your-first-app/

https://www.youtube.com/watch?v=v0t42xBIYIs

## basic express

```javascript
var express = require('express');
var app = express();

app.get('/', function(req, res){
  res.send('hello world');
});

app.listen(3000);
```

## setting up selenium

Initiate node:
`npm init`

Selenium set up
Install the relevant packages:

`npm install --save-dev chromedriver selenium-webdriver mocha chai chai-as-promised`

Add the following to package.json in express-apps script

```
“start”: “node index.js”,
   “test:e2e”: “mocha --timeout 20000 e2e/tests.js”,
   ```

This assumes that your tests are in a e2e folder - so create a file called ‘test.js’ in a folder called ‘e2e’.

Please see notes in code below to see what it is doing:


```javascript
require(‘../app.js’)

require(‘chromedriver’);

// The package has been set up to fetch and run ChromeDriver (https://www.npmjs.com/package/chromedriver)

const webdriver = require(‘selenium-webdriver’);


// WebDriver is a tool for automating web application testing, and in particular to verify that they work as expected (https://www.seleniumhq.org/docs/03_webdriver.jsp). This is seting this as a constant variable called webdriver


const { By, until } = webdriver;

const chai = require(‘chai’);

// Chai is a BDD / TDD assertion library for node and the browser that can be delightfully paired with any javascript testing framework (https://www.chaijs.com/)


const chaiAsPromised = require(‘chai-as-promised’);

// extension of the chai library


chai.use(chaiAsPromised);


// making chai use the chaiAsPromised library


const driver = new webdriver.Builder().forBrowser(‘chrome’).build();
// using selenium webdriver to acces the chrome browser

const expect = chai.expect;
// setting a constant variable from chais expect method to ‘expect’


describe(‘End to End Test Suite’, done => {


// passing two perimeters in the describe method. The name of the test and call back function



 before(done => {
   // before method to say before test do this...
   driver.get(‘http://localhost:3003’)
           done();


   });



// this says ‘load up the chrome browser’ (driver - defined earlier), get the url (http://localhost:3003) and wait until it locates the message id in the html and then it is ‘done’


 it(‘can read welcome message’, () => {
   console.log(‘Ready to read welcome message’);
   return expect(driver.findElement(By.id(‘message’)).getAttribute(‘innerHTML’))
   .to.eventually.contain(‘Hello world!’);
 });

 });

// this test expects the driver to find the ‘message’ id’s innerHTML element to eventually contain ‘Welcome to my page!’


//Now to pass this test...... set up a ‘.js’ file in the root (‘app.js’). See notes in JS bellow:

const express = require(‘express’);

// require express and set a a constant variable. - ‘Express 3.x is a light-weight web application framework to help organize your web application into an MVC architecture on the server side. You can use a variety of choices for your templating language (like EJS, Jade, and Dust.js).’ (https://stackoverflow.com/questions/12616153/what-is-express-js)
const app = express();
// assign a the express method.

app.get(‘/’, (req, res) => {
  // use express to get a page ‘/’ and set 2 arguments (request and responce).

  res.send(`
    <html>
    <title>
      My awesome page
    </title>
    <body>
    <div id=“message”>
      Hello world!
    </div>
    </body>
    </html>
  `);

  // use reponce to send the html with (‘Welcome to my page!’) included in the message id
});


app.listen(3003, () => console.log(‘Example app listening on port 3003!’));
// get express to listen at port 3003 and console log that this is listening at this port

```

Now run the test: `npm run test:e2e`

## More links

This is normally used as part of a workshop so you might find that the guidance is a little light but I hopefully it all makes sense https://github.com/leanjscom/thinking-in-react
leanjscom/thinking-in-react
The goal of this exercise is to learn how to think in React, by https://reactjs.academy/

Express and MongoDB(mlab) tutorial - finding it pretty good so far
https://zellwk.com/blog/crud-express-mongodb/

Katalon Recorder - Selenium IDE (third party) - I used this to click around and fill in fields and then you can export into something we can use near enough directly in selenium/mocha tests
https://chrome.google.com/webstore/detail/katalon-recorder-selenium/ljdobmomdgdljniojadhoplhkpialdid

https://github.com/sacullezzar/acebook-node/wiki/Feature-testing-with-Mocha,-Chai-and-Selenium

https://www.youtube.com/watch?v=v0t42xBIYIs

## Express-React app example

```
$ mkdir dir-name
$ cd dir-name
$ npm init (starts up your package-json file)
```
```
$ npm i express concurrently
$ npm i nodemon --save-dev (like shotgun for node)
```

Go into the package.json file.
Make sure the main js file is the same as the root js file you are going to create

In the scripts section create 2 scripts.
```
"start": "node server.js",
"server": "nodemon server.js",
```

```
$ touch server.js (root js file)
```

Inside the server.js file, input the below code.

```javascript
const express = require('express')
const app = express();

app.get('/api/customers', (req, res) => {
  const customers = [
    {id: 1, firstName: 'John', lastName: 'Doe'},
    {id: 2, firstName: 'Steve', lastName: 'Smith'},
    {id: 3, firstName: 'Marry', lastName: 'Swanson'}
  ]

  res.json(customers);
})

const port = 5000;

app.listen(port, () => console.log(`Server started on port ${port}`))
```

```
$ sudo npm i -g create-react-app
```
```
$ create-react-app client
```

To be able to use the backend you need to define the backend and the URL as a Proxy.

In the *client folder* package.json, below the scripts object put in
```
"proxy": "http://localhost:5000"
```
This way we don’t have to include this inside any of the fetch requests we make to the server

Go into the client folder and create a new components folder and the create a folder inside of that called customers (or anything you want). In that folder create a js and a css file for that js.

At the top of the app.js file at the inside the src of the Client folder below the other imports.
```javascript
import Customers from './components/customers/customers';
```
Put this at the top.
Then put `<Customers />` at the bottom of the div inside of the render return. This will call the customer component that is being created.

Inside of the customers.js
```javascript
import React, { Component } from 'react';
import './customers.css';

class Customers extends Component {
  constructor() {
    super();
    this.state = {
      customers: []
    }
  }

  componentDidMount() { //
    fetch('/api/customers')
      .then(res => res.json())
      .then(customers => this.setState({customers}, () => console.log('Customers fetched...', customers)));
  }

  render() {
    return (
      <div>
        <h2>Customers</h2>
        <ul>
          {this.state.customers.map(customer =>
            <li key={customer.id}> { customer.firstName }  { customer.lastName } </li>
          )}
        </ul>
      </div>
    );
  }
}

export default Customers;
```

The css file that was created can be used to style the html that is being rendered in the js file i.e. the customers h2.

To make sure that we don’t have to run 2 terminals to have the back-end and front-end running at the same time.
We can edit the package.json file of the root directory of the project so we can do both at the same time.

```
"scripts": {
    "client-install": "cd client && npm install",
    "start": "node server.js",
    "server": "nodemon server.js",
    "client": "npm start --prefix client",
    "dev": "concurrently \"npm run server\" \"npm run client\""
  },
  ```

This is how the scripts file should end up looking. The first line is just for initial set up if someone were to clone the file for the purpose of simply using it.

The client line goes into the client folder and runs npm start.

The dev line is the line that runs the server and client at the same time.

Now we can use
```
$ npm run dev
```


## Setting up a database connected API

Setting up a database connected API - based on the tutorial from yesterday
Database connection- MongoDB

Refer to this for how to set up mongo from scratch:
https://zellwk.com/blog/crud-express-mongodb/

- install mongodb with `npm i mongodb`
- require mongo db in your backend `server.js`

  ```javascript
  const MongoClient = require('mongodb').MongoClient
  ```

- add the connection to the bottom of the file surrounding the app listen function

```javascript
var db
const port = 5000;

MongoClient.connect('mongodb://<username>:<password>@ds113454.mlab.com:13454/<collection name>', (err, client) => {
  if (err) return console.log(err)
  db = client.db('<collection>') // whatever your database name is
  app.listen(port, () => console.log('server started on port 5000'));
})```

- add the api route
```app.get('/api/quotes', (req, res) => {
  db.collection('quotes').find().toArray((err, result) => {
    if (err) return console.log(err)
    console.log(result)
    // return the return the results as a json
    res.json(result)
  })
})
```

- to set up the react element, create the components like the customer example above replacing the fields/references where required, then add to the `app.js` file in the root of `client` folder

 e.g

  ```javascript
  import React, { Component } from 'react';
  import './quotes.css';

  class Quotes extends Component {
    constructor(){
      super();
      this.state = {
        quotes: []
      }
    }

    componentDidMount(){
        fetch('http://localhost:5000/api/quotes')
        .then(res => res.json())
        .then(quotes => this.setState({quotes: quotes}, () => console.log('quotes fetched..',quotes)))
    }
    render() {
      return (
        <div >
  <h2>Quotes</h2>
  <ul>
  {this.state.quotes.map(quotes =>
  <li key={quotes._id}>{quotes.name} {quotes.quote}</li>)}
  </ul>
        </div>
      );
    }
  }

  export default Quotes;
  ```

## more links

  Found this on the Makers blog - how to test in react:
https://blog.makersacademy.com/testing-react-a-beginners-guide-755a8828c222

the above uses react test utilities/jest - but there is also enzyme which can hook into that and looks a bit easier to use (maybe) - might not need selenium anymore :slightly_smiling_face:
https://medium.com/codeclan/testing-react-with-jest-and-enzyme-20505fec4675

An article on setting up a MERN (mongodb express react node) app. Summarises the process. With notes on testing too. I got all the way to the bottom to find out that this was written by someone on the makers academy course. Lol. https://hackernoon.com/episode-43-the-art-of-setting-up-a-mern-stack-final-project-week-d554bffe2c0e

Useful (hopefully) article on how to start thinking about structuring a react app into components. https://reactjs.org/docs/thinking-in-react.html

## testing our CRUD app

Hi - thought I'd share my example test that I added to the star wars quotes app we were working with yesterday.  This was just using the same set up on selenium, mocha, chai, chai--as-promised, chromewebdriver - the process we figured out earlier in the week.  I got the details of what my clicks and inputs were from that Katalon Recorder chrome extension (selenium IDE)

Of course - REACT may complicate things a bit but hopefully this will be helpful

```javascript
require('chromedriver');
const webdriver = require('selenium-webdriver');
const { By, until } = webdriver;
const chai = require('chai');
const chaiAsPromised = require('chai-as-promised');
chai.use(chaiAsPromised);
const driver = new webdriver.Builder().forBrowser('chrome').build();
const expect = chai.expect;
describe('Page Loads', done => {
  before(done => {
    console.log('Go to page');
    driver.get('http://localhost:3111')
    done()
  });
  it('can read welcome message', () => {
    // console.log('Ready to read welcome message');
return expect(driver.findElement(By.tagName("body")).getText()).to.eventually.contain("May Node and Express be with you");
  });
});
describe('enter a quote', done =>{
  before(done => {
           driver.get("http://localhost:3111/");
           driver.findElement(By.name("name")).click();
           driver.findElement(By.name("name")).clear();
           driver.findElement(By.name("name")).sendKeys("Yoda");
           driver.findElement(By.name("quote")).clear();
           driver.findElement(By.name("quote")).sendKeys("this is a new quote");
           driver.findElement(By.id("submit_btn")).click();
           driver.findElement(By.name("name"))
           driver.then(() => {
             driver.wait(until.elementLocated(By.name('quote'))).then(() => {
               done();
             });
           });
  })

  it('read back quotes', () => {
  console.log('Checks last quote is correct');
  return expect(driver.findElement(By.xpath("//ul/li[contains(concat(' ', @class, ' '), ' quote ')][last()]")).getText()).to.eventually.contain("this is a new quote");
  });
})
```


https://youtu.be/PBTYxXADG_k

https://github.com/bmoishe/learning-mern

https://youtu.be/_ZTT9kw3PIE

https://www.valentinog.com/blog/ui-testing-jest-puppetteer/

https://www.codementor.io/olatundegaruba/integration-testing-supertest-mocha-chai-6zbh6sefz

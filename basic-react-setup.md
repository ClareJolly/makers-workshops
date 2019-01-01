# Setting up a basic react app

* [Basic set up](#basic-set-up)
  + [Create folder and install dependencies](#create-folder-and-install-dependencies)
  + [Create the server file](#create-the-server-file)
  + [Set up React app](#set-up-react-app)
  + [Start front end server](#start-front-end-server)
* [create customers component to fetch back end data](#create-customers-component-to-fetch-back-end-data)
  + [Fetching the data](#fetching-the-data)
  + [Full code](#full-code)
+ [CSS update](#css-update)
* [Additional steps](#additional-steps)
* [Database connection- MongoDB](#database-connection--mongodb)

## Basic set up
 
### Create folder and install dependencies

- Create new folder
- initialise app with
`npm init`

  change `entry` to `server.js`

- install express and concurrently with `npm i express concurrently`

- then install nodemon which will look for changes and stop and start your server when needed

  `npm nodemon --save-dev`

- set up nodemon as follows:
  in `package.json` file

  add scripts

  ```
"start": "node server.js",
"server": "nodemon server.js",
```

### Create the server file
- create `server.js`

```javascript
const express = require('express');
const app = express();

// add cross site scripting
app.use(function(req, res, next) {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "X-Requested-With");
  next();
});

//create some data on the API url
app.get('/api/customers', (req, res) => {
  const customers = [
    {id: 1, firstname: 'John', lastname:'Doe'},
    {id: 2, firstname: 'Jane', lastname:'Smith'},
    {id: 3, firstname: 'Laura', lastname:'West'}
  ]
  res.json(customers)
})

const port = 5000;
app.listen(port, () => console.log('server started on port 5000'));

```

- run with `npm run server`

- check in browser - `localhost:5000/api/customers`

### Set up React app

- in a new terminal window - leaving server running

- Install the create react app package globally
`npm i -g create-react-app`


- Create an app called client
`create-react-app client`

- in client folder, open `package.json`

- after scripts add so you don't need to reference the full path
`"proxy":"http://localhost:5000"`

### Start front end server

- In this new terminal window
```
cd client
npm start
```

sample page will open on port 3000 (localhost:3000)

## Create customers component to fetch back end data

- in client/src create a folder called `components`
- inside that create a folder called `customers`
- inside that create a file called `customers.js` (and maybe `customers.css`)

- in `customers.js`
copy everything from `app.js` and paste in.

- remove logo
- import customers.css

  ```javascript
import './customers.css';
```

- and then update the rest to something like:

```javascript

import React, { Component } from 'react';
import './customers.css';

class Customers extends Component {

  render() {
    return (
      <div >
<h2>Customers</h2>
</div>
)
}
}

export default Customers;

```


- then in the `app.js` file - inside render

```javascript
render() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
      </header>
        <Customers/>
    </div>
  );
}

```

- and import this in at the top of the page

```javascript
import Customers from './components/customers/customers'
```

### Fetching the data

we now set up the code to grab the data from the API

description follows and full code is below that

in the `customers.js` file add a constructor to initialise a state to hold the data (`this.state`) and the `super()` object as it's being used inside a parent

```javascript
class Customers extends Component {
  constructor(){
    super();
    this.state = {
      customers: []
    }
  }
}
```

the use lifecycle method called componentDidMount to run once the page is loaded.  inside that you add in the fetch request to the server

with fetch it will return a promise which you map to a json and then another `.then` to map out our customers from backend and put inside the this.state variable

```javascript
componentDidMount(){
  // fetch('/api/customers')
    fetch('http://localhost:5000/api/customers')
    .then(res => res.json())
    .then(customers => this.setState({customers: customers}, () => console.log('Customers fetched..',customers)))
}
```

and in the render section you can add in the loop through the data to display each as a li.

```javascript
<ul>
{this.state.customers.map(customer =>
<li key={customer.id}>{customer.firstname} {customer.lastname}</li>)}
</ul>
```

### Full code

```javascript
import React, { Component } from 'react';
import './customers.css';

class Customers extends Component {
  constructor(){
    super();
    this.state = {
      customers: []
    }
  }

  componentDidMount(){
    // fetch('/api/customers')
      fetch('http://localhost:5000/api/customers')
      .then(res => res.json())
      .then(customers => this.setState({customers: customers}, () => console.log('Customers fetched..',customers)))
  }

  render() {
    return (
      <div >
<h2>Customers</h2>
<ul>
{this.state.customers.map(customer =>
<li key={customer.id}>{customer.firstname} {customer.lastname}</li>)}
</ul>
      </div>
    );
  }
}

export default Customers;

```
## CSS update

To make it look a bit nicer, add the following to the customers css,
```css
ul {
  list-style: none;
  padding:0;
  width: 30%;
  margin: auto;
}

li {
  font-size: 1.3rem;
  line-height: 2rem;
  border-bottom: 1px dotted #777;
}
```

## Additional steps

You can add some additional steps into the backend package.json file

```
"client": "npm start --prefix client",
"dev": "concurrently \"npm run server\" \"npm run client\""
```

concurrently will allow you to run both front and backend in the same terminal.

you run `npm run dev` (after closing down the 2 server connections)

## Database connection- MongoDB

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
})
```

- add the api route
```javascript
app.get('/api/quotes', (req, res) => {
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

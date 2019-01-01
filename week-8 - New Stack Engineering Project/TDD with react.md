# Test-driven Development Using React

## Introducing TDD with React

Create folder for project

`npm init`

`npm install -g create-react-app`

`create-react-app <name of app>`

Create-react-app will download a number of packages and install them. You may see some warnings as it goes through the process, but you can just ignore those.

`cd <name of app>`

To start the server, `npm start`

npm will boot up the app and launch a browser.

### Running tests

To run the tests, stop the server with `ctrl+c`

`npm test`. This will cause **Jest** to automatically search through your code and run all the tests that it finds for your project.

Press `Q` to exit the test watch mode.

### React basics (with create react app)

Home page of your app is the `index.html` page. This contains some boilerplate code. And if you scroll down a little, you'll see a div with an ID of root. This ID can be anything you like, but it's common to either call it root or app. This is where your React component will render. Anything you put outside of this root div will just render normally in your browser.

The main JavaScript file for our application is `index.js` file. Index.js includes React. It includes React DOM.

The really important thing in this file as far as React is concerned is the ReactDOM. render statement. React DOM is what handles the interaction between your components and the web browser. The main React library simply helps you to create React components without concerning itself with how they'll be used. If you look closely at the ReactDOM. render method, you'll see that it takes two parameters--the component that you want to render and the location in the HTML file where you want it to be rendered.

`App.js` is what's being rendered. It imports React, and it imports the component object from the React package. The way to create a React component is by extending react.component. Each React component must have a render method which will return some piece of the UI that it's responsible for. Notice that this code that's returned by the render method looks like HTML, and sometimes you can even think of it as being HTML. But it's important to know that it's most certainly not HTML when what actually gets returned from a component's render method is JavaScript. But React makes it easier for you to write this JavaScript through the use of an XML like syntax extension to JavaScript called JSX. So while you can write the return value of a component using this easy-to-read syntax, it actually gets compiled to a call to the create-react-element method of the component object prior to running in your browser. When you see code like this that looks like HTML in side of a React component, remember that this actually gets compiled into JavaScript. The job of that JavaScript is to generate HTML, though ultimately rendered in the web browser, or wherever you're rendering the final React component. Keep in mind that React doesn't need to always be rendered inside of a browser. The reason that JSX versus HTML is an important distinction is that unlike with HTML, we can use JavaScript inside of this JSX code. To indicate that something should be treated as JavaScript rather than as JSX, you use curly braces. For example, in the src attribute of the image element, we're inserting the logo that we imported at the beginning of the file. JSX also allows you to pass values between components.

To add a component, import it at the top of the page with something like `import Header from '../Header';`

and then in the place you want it to appear, add `<Header />`

### Writing a test

Tests ideally should be saved alongside the component being tested and in a folder called `__tests__`

A good way to start it to take a copy of `App.test.js` and rename it accordingly - e.g `header.test.js`.

In that file include a component named Header and then render the Header component

**This is a smoke test - it just checks that the component renders**

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import Header from '../Header';

it('renders without crashing', () => {
const div = document.createElement('div');
ReactDOM.render(<Header />, div);
ReactDOM.unmountComponentAtNode(div);
});
```

run `npm test`.

tests will fail - that's good

To make the test pass, create a new file named `Header.js`.

A good way to do this is to copy the code from the App component and modify it.

```javascript
import React, {Component} from 'react'

class Header extends Component {
render() {
  return (
    <div className="Header">
      <p>this is the header</p>
    </div>
  );
}
}

export default Header;

```

Add this component to `App.js` and import the Header component.

```javascript
import React, { Component } from 'react';
import Header from '../components/Header'
import '../App.css';

class App extends Component {
render() {
  return (
    <div className="App">
      <Header />
    </div>
  );
}
}

export default App;

```

#### Naming conventions

When you create a custom element, the first letter of its name is always capitalised. HTML equivalent React elements, such as div and p, start with lowercase letters.

#### Folder conventions

In React, it's common to classify components into **containers and components**. In the case of our current application, the App component is container. Its primary purpose is to hold other components. It's common for containers to also handle any logic in the application, as well as changes to the application state. A component that does more than just rendering a piece of the user interface is also known as a stateful component or a smart component.

Create folders in `src`called `containers` and `components` with subfolders inside each called __tests__.

#### Testing basics

**Unit testing** is the testing of a single, isolated piece of code or group of pieces. Unit testing tests whether component produces an expected output when given input.

Other kinds of testing include:
**Integration testing** in which groups of components are tested together along with hardware

**Performance testing**, which looks at the speed of the system

**Usability testing**, which tests the product from the perspective of the customer, how easy it is to use and how pleasing it is

**Acceptance testing**, which tests whether a product meets the requirements of the customer

**Regression testing**, which tests whether a modification is working correctly and whether anything broke as a result of a modification.

### Understanding the Virtual DOM

React's virtual DOM is an abstraction of the browser DOM. React DOM keeps **two** full virtual DOM objects in memory at any point-- one for the current state of the piece of the browser DOM it manages and one for the ideal state as determined by the components you've rendered using React. When a change happens in your React component that affects the display of the component, the ideal state of the virtual DOM is updated, and React calculates the difference between the two virtual DOMs. This difference represents what needs to be changed in the actual DOM, the browser DOM, to match this ideal state.

### Isolated testing

Testing components as they render in the full DOM is fine. But for test-driven development, what we really want is to test our components in isolation, so rendering them without subcomponents just by themselves. This is called **shallow rendering**.

### Enzyme
Install Enzyme

Check here:
https://facebook.github.io/create-react-app/docs/running-tests

`npm install --save enzyme enzyme-adapter-react-16 react-test-renderer`

Create a setup file

```javascript
//src/setupTests.js`
import { configure } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';

configure({ adapter: new Adapter() });
```

Running tests now may break - The solution to this is to delete the node_modules folder and then run an `npm install` to re-download and re-link packages

To convert a test to use Enzyme you no longer need to import React DOM, and instead import shallow from Enzyme. And no longer need to create a div inside of the DOM.

Create a variable and set it equal to the shallow rendering of StoreLocator.

```javascript
import React from 'react';
import {shallow} from 'enzyme';
import StoreLocator from '../StoreLocator';

it('renders without crashing', () => {
let mountedStoreLocator = shallow(<StoreLocator/>);
});
```

Then write the expectation to say expect the number of StoreLocators, so the length, to be 1

```javascript
//app.test.js
import React from 'react';
import {shallow} from 'enzyme';
import App from '../App';

describe('App', () => {
it('renders without crashing', () => {
  let mountedApp = shallow(<App />)
});
it('renders a store locator', () => {
  let mountedApp = shallow(<App />)
  const locators = mountedApp.find('StoreLocator');
  expect(locators.length).toBe(1);
});
});

```
Fix the test

```javascript
//app.js
import React, { Component } from 'react';
import StoreLocator from './StoreLocator'
import '../App.css';

class App extends Component {
render() {
  return (
    <div className="App">
      <StoreLocator />
    </div>
  );
}
}

export default App;

```

Use beforeEach just to create a mountedStoreLocator that's a shallow render of the StoreLocator Component.

```javascript
import React from 'react';
import {shallow} from 'enzyme';
import StoreLocator from '../StoreLocator';

describe("StoreLocator", function() {

let mountedStoreLocator;
beforeEach(()=>{
  mountedStoreLocator = shallow(<StoreLocator/>)
})

it('renders without crashing', () => {
  let mountedStoreLocator = shallow(<StoreLocator/>);
});

it('renders a header', () => {
  const headers = mountedStoreLocator.find('Header');
  expect(headers.length).toBe(1);
});
});

```

When we have multiple of a component on the page we expect the length to be the relevant number - e.g. `expect(buttons. length). toBe(2)`

```javascript
it('renders 2 buttons', () => {
const buttons = mountedStoreLocator.find('Button');
expect(buttons.length).toBe(2);
});
```

### Adding images and testing for them

```javascript
import React from 'react';
import {shallow} from 'enzyme'
import Map from '../Map';

describe("Map", function() {

let mountedMap;
beforeEach(()=>{
  mountedMap = shallow(<Map />)
})

it('renders without crashing', () => {
  let mountedMap = shallow(<Map/>);
});

it('contains a map image', () => {
  const img = mountedMap.find('img');
  expect(img.length).toBe(1);
});

it('contains none map', () => {
  const defaultMap = mountedMap.find('img [src="images/none.png"]');
  expect(defaultMap.length).toBe(1);
});
});

```

```javascript
//map.js
import React, {Component} from 'react'
import './Map.css';

class Map extends Component {
render() {
  return (<div className="MapBox"><img src="images/none.png" alt="" /></div>);
}
}

export default Map;
```

## Passing Data Between Components

**Props** are one of the two kinds of data in a React application.

- Props - are immutable data that's passed from a parent to a child.
- State - represents the state of the application.

Shallow mount the component passing in the props using the JavaScript spread operator to spread the props object into separate variables for passing in to the button.

```javascript
//button.test.js
describe("When a location is passed", function() {

let mountedButton;
let props;
beforeEach(()=>{
  props = {
    location:"Location1"
  }
  mountedButton = shallow(<Button {...props}/>)
})

it('displays the location', () => {
  const locName = mountedButton.find('.location-button')
  expect(locName.text()).toEqual('Location1')
});
});
```

```javascript
//button.js
import React, {Component} from 'react'
import './Button.css';

class Button extends Component {
render() {
  return (<button className="location-button">{this.props.location ? this.props.location : "All Locations"}</button>);
}
}

export default Button;

```

The way to pass props between components in React is to use attributes. These attribute names become properties in the props object inside of the child components

```javascript
//StoreLocator.js
render() {
return (<div>
  <Header/>
  <div><Button location="Portland"/><Button location="Astoria"/></div>
  <Map/></div>    );
}
}

```


### Understanding State and Events

The two types of data in a React application are props and state.

#### Props
Immutable data that's passed from the parent to child

#### State
The data in your application that changes in response to user actions. It's modified using the setState function, and calling the setState function triggers a re-rendering of your application.

Note that state updates may be asynchronous. React may choose to batch several updates in order to increase performance of your application.

### Where to use state and where to use props

State should be used so that the minimal amount of data is updated via setState

The first question to ask is,
- Is this data not passed in from a parent? If it is passed from a parent, that data's clearly a **prop**.
- The next question is, Does it change over time? If so, it may be **state**.
- And, finally, is it not possible to compute this data based on other state or props? Any data that can be computed in your application is a **prop**, not state.

### Events
**Events**, flow up in a React application. This is one-way data binding. The benefit of one-way data binding is that it makes data flow explicit, which makes it easier to test and reason about your application. The way we update our state in a React application is by using callback functions.

### Using State

e.g.
Create a constructor, and call `super` to bring in the properties and methods from the parent object.

add a `this.shops` object and set that equal to an array.

You can add in some dummy data (or real data) if needed

To loop through this array, the `array.map` function takes each element from an array and passes it through a function in order to generate a new array.

**Note:** A requirement in React is that each item in an array of elements needs to have a unique key to serve as an identifier for that item. See below, you can add as a second parameter of map and then use that id as the key for the element

```javascript
import React, {Component} from 'react'
import Header from '../components/Header'
import Button from '../components/Button'
import Map from '../components/Map'
class StoreLocator extends Component {
constructor(props) {
  super(props)
  this.state = {
    currentMap: 'none.png'
  }
  this.shops = [{
    'location':'Portland',
    'address':'123 Portland Drive'
  },{
    'location':'Astoria',
    'address':'123 Astoria Drive'
  },
  {
    'location':'',
    'address':''
  }]
}

render() {

  let storeButtons = this.shops.map((shop,id)=>{
      return(<Button key={id} location={shop.location} />)
  });
  return (<div>
    <Header/>
    <div>{storeButtons}</div>
    <Map imagename = {this.state.currentMap} location={this.props.location}/></div>    );
}
}

export default StoreLocator;

```

### Adding Inverse Data Flow - or clicking on buttons
We need to make clicking on the button cause some sort of event to happen.

We need to create a mock function. And the way to do that in Jest is by using the `jest.fn method`. By setting it up similar to the below, you can simulate a click on this button. You can find the HTML button inside of this component by using Enzyme's find method. And then we're going to simulate a click on that button. A

```javascript
it('call a function passed to it when clicked', ()=>{
 const mockCallBack = jest.fn();
 const mountedButtonWithCallback = shallow(<Button handleClick={mockCallBack} />);
 mountedButtonWithCallback.find('button').simulate('click');
 expect(mockCallBack.mock.calls.length).toEqual(1);
});
```
Fix the test

Make the component to accept handleClick and call handleClick on click.

```javascript
return (<button value={this.props.location} onClick={this.props.handleClick} className="location-button">{this.props.location ? this.props.location : "All Locations"}</button>);
```

### Updating state

example test

```javascript
describe('chooseMap', ()=> {
  it('updates this.state.currentMap using the location passed to it', ()=>{
      let mountedStoreLocator = shallow(<StoreLocator />);
      let mockEvent = {target:{value:'testland'}};
      mountedStoreLocator.instance().chooseMap(mockEvent);
      expect(mountedStoreLocator.instance().state.currentMap).toBe('testland.png');
  })
});
```

It is tempting to put the state manipulation function inside of a child compnent, but this would be a bad practice because we don't actually want child components to have side effects. We just want it to take in some data and return some data. This is what we call a pure function. A pure function just takes in some data, does something to it, and returns something without having any side effects and without manipulating anything outside of itself.

Below you can see a new method named chooseMap. And chooseMap is going to take a value, e, which will be the event object that gets passed to a callback when it's called in response to an event listener. And then chooseMap is going to set the state.

Now we need to bind chooseMap to the StoreLocator so that we can pass chooseMap down to the Button component and still have it be mapped to StoreLocator.

```javascript
//storeloactor.js
import React, {Component} from 'react'
import Header from '../components/Header'
import Button from '../components/Button'
import Map from '../components/Map'
import mapChooser from '../mapChooser';

class StoreLocator extends Component {
constructor(props) {
  super(props)
  this.state = {
    currentMap: 'none.png'
  }

  this.shops = [{
    'location':'Portland',
    'address':'123 Portland Drive'
  },{
    'location':'Astoria',
    'address':'123 Astoria Drive'
  },
  {
    'location':'',
    'address':''
  }]

  this.chooseMap = this.chooseMap.bind(this);
}



chooseMap(e){
    this.setState({currentMap: mapChooser(e.target.value)});
}

render() {

  let storeButtons = this.shops.map((shop,id)=>{
      return(<Button handleClick = {this.chooseMap} key={id} location={shop.location} />)
  });
  return (<div>
    <Header/>
    <div>{storeButtons}</div>
    <Map imagename = {this.state.currentMap} location={this.props.location}/></div>    );
}
}

export default StoreLocator;

```

## Testing React Components

The most basic thing to test in a React component is whether it renders and whether it renders where it's supposed to render. After that, you should test the different states of the application, and you should test the events. Finally, you should test the edge cases. What happens when the unexpected happens?


The lifecycle methods are methods that are called at certain points in the life of a component. There are two broad categories of lifecycle methods--the mount/unmount methods and the updating methods. The lifecycle method that we're most concerned with today is the **componentDidMount** method.

Mounting in React refers to the end result of rendering a component and outputting to its final destination, a browser in our case. The componentDidMount method then is the first place where it's safe to modify the state of component. ComponentDidMount is the place where you can do anything that requires browser DOM nodes, and it's also the recommended place for instantiating network requests.

### Creating a Manual Mock

Axios is a simple and popular HTTP client that supports promises.
https://www.npmjs.com/package/axios

install Axios

`npm i Axios`.

First create a mock of Axios, and this is what's going to be used when we run Jest. The point of using a mock is to make sure that our tests don't rely on external factors such as the availability of the API. Using a mock can also make running our tests faster. Jest has auto-mocking functionality, which does pretty much what it says. It automatically creates mocks of the functions used in your component.

To create our manual mock of Axios, we're going to first create an automatic mock and then override certain methods.

To create a manual mock of a package installed using npm, create a folder called `__mocks__` at the root of your project.

Inside this folder, create a file named the same as the package that you want to override.

Create a file here named `axios.js`. And now when Jest runs, it's going to automatically use this axios.js instead of the one that's inside the node_modules folder.

Create the manual mock

first step will just be to generate a mock from the module.

So use `const axiosMock = jest. genMockFromModule('axios')`.

Then specify what the mock response is going to be.

This is the data that's going to be sent to the test, and then to test against.

This is an object, and this should have the same shape as our data that's currently inside of the StoreLocator right here.

It's going to have a property called data in the response. And then we'll have an array of shops. Remember that because we're using JSON data here, it's important that we have all of the properties and their values in double quotes.

Next, create a mock implementation of `axios.get` method.

The get method just does an HTTP request.

Use mockImplementation

then specify request method

Write the request method.

Use a promise here. And inside of the promise, write a function that will resolve and return our mock response after a delay. In this way, we can simulate a normal server response time.

```javascript
//__mocks__/axios
const axiosMock = jest.genMockFromModule('axios');

let mockResponse = {
  data: {"shops":
      [{
          "location": "test location",
          "address" : "test address"
      }]
  }
};

axiosMock.get.mockImplementation(req);

function req() {
  return new Promise(function(resolve) {
      axiosMock.delayTimer = setTimeout(function() {
          resolve(mockResponse);
      }, 100);
  });
}

module.exports = axiosMock;

```

### Writing an Asynchronous Test
The first test I'm going to write is just a test that `axios.get` is called.

```javascript
//storelocator
it('calls axios.get in #componentDidMount', ()=>{
  return mountedStoreLocator.instance().componentDidMount().then(()=>{
      expect(axios.get).toHaveBeenCalled();
  })
});
```

The first thing to do is to write the componentDidMount method and call axios

```javascript
import axios from 'axios';
```

Make componentDidMount an async function so that you can use `await` here rather than a promise or a callback.

```javascript
async componentDidMount(){
   let response = await axios.get('http://localhost:3000/data/shops.json');

}
```

Create a new property inside of state called shops, and this will initially be set to an empty array.

```javascript
this.state = {
  currentMap: 'none.png',
  shops: []
};
```

update the button creation to be `this.state.shops.map` function.

```javascript
let storeButtons = this.state.shops.map((shop,id)=>{
   return(<Button handleClick = {this.chooseMap} key={id} location={shop.location} />)
});
```

Another test for the API call - checking the url

```javascript
it('calls axios.get with correct url', ()=>{
   return mountedStoreLocator.instance().componentDidMount().then(()=>{
       expect(axios.get).toHaveBeenCalledWith('http://localhost:3000/data/shops.json');
   })
});
```

### Test for State Updates in Response to AJAX Requests

```javascript
it('updates state with api data', () => {
  return mountedStoreLocator.instance().componentDidMount().then(()=> {
    expect(mountedStoreLocator.state()).toHaveProperty('shops',
        [{
            "location": "test location",
            "address" : "test address"
        }]
    );
  })
  });
```

Update the state after `axios.get` happens using the response. Here the `await` is going to wait until `axios.get` happens, and then we're just going to set the state. `This.setState` will set shops equal to `response.data. shops`.

```javascript
async componentDidMount(){
   let response = await axios.get('http://localhost:3000/data/shops.json');
   this.setState({
       shops:response.data.shops
   })
}
```

### Snapshot Testing
Snapshot testing takes a picture of your application and then compares it with a reference picture to make sure that the two are identical. For example, if someone were to go into my JSON file and change the location of one of my stores or accidentally change the location name, our tests would still pass, but I would want to know about the change so I could determine whether this is a bug.

```javascript
//StoreLocator.test.js
//
//
import renderer from 'react-test-renderer';
//
//
it('renders correctly', ()=>{
  const tree = renderer.create(<StoreLocator />).toJSON();
  expect(tree).toMatchSnapshot();
});
```

Here we're just rendering the StoreLocator, and the first time this runs, it's going to store it inside of a folder called snapshots inside of the __tests__ directory as this JSON file. On subsequent runs, it's going to run the expectation here where we say we expect the tree to match the snapshot.

If there are changes you want to keep in a new snapshot, `press U` to update your snapshot.

### Code Coverage Report

`npm test -- --coverage`

### Useful tools

**React Developer Tools**
https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi/related?hl=en

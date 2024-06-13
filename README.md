# react-note
react-revision

babel:
convert modern js to broswer compatible js
1. convert es6->es5
2. convert jsx->js
3. convert ts->js
4. ESM(ECMAScript Modules)->CommonJS
```jsx
import/export->require('**')
```
three most important packages for react: react, react-dom, babel
script type: text/babel

virtual dom and real dom:
all class or function component are virtual doms, and they are built in tree,
this code is for rendering virtual doms into real doms.
```jsx
ReactDOM.render(thisComponent, document.getElementById('root'))
```
#DOM（Document Object Model）
HTML is a kind of document, and each tag/element stands for a dom which contributes to the tree shaped document, and js could operate each dom, by runing the js script.
dom is object which contains various of attributes, real doms has much more attributes than virtual dom,
operate real dom is costly, cause it may cause reflow and must cause repaint, which will cause bad performance, cause cpu are working so frequantly.

reflow: any changes such as change size, position, hidden some elements, query some attribute lsuch as offsetWidth.. will cause reflow,
browser will recaculate layout. costly and time consuming.
repaint: change color will cause broswer repaint.

react virtual dom will decrease times for operating real doms by applying diff alg, 
when components changes, react will generate a new virtual doms and compare with the old virtual doms, to get the modified dom (elememt, which is the unit of the alg), then render the diff real dom.

#jsx syntax rules: 
1. no quotaion mark.
2. use {} when element mixed with js expression
3. className instead of class
4. style={} inside the curly brace should be obj {color:'red', fontSize:'29px'}
5. only one root element.
6. element should be closed.
7. element: *start with lower case will convert to html element *start with upper case, react will render corresponsed component.

one example about array rendering:
```jsx
<ul>
  {items.map((item, index) => (
    <li key={index}>{item}</li>
  ))}
</ul>
```
this will return an array
```jsx
[
  <li key={0}>Apple</li>,
  <li key={1}>Banana</li>,
  <li key={2}>Cherry</li>
]
```
and react will render this array to->
```jsx
<ul>
  <li>Apple</li>
  <li>Banana</li>
  <li>Cherry</li>
</ul>
```
# key in a list
key cannot be index of the array, cuz this will cause waste when using diff alg
example:
```jsx
<ul>
  <li key = {0}>Apple</li>
  <li key = {1}>Banana</li>
</ul>

<ul>
  <li key = {0}>Cherry</li>
  <li key = {1}>Apple</li>
  <li key = {2}>Banana</li>
</ul>
```
add a new element to the head of the array, the index and value mapping pairs changed.
diff: react will check key first, if keys are identical, then check value, if value are different, will identify this element as modified element, then rerendering.

about class component
```jsx
import React from 'react';

class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  componentDidMount() {
    console.log('Component did mount!');
  }

  componentDidUpdate(prevProps, prevState) {
    console.log('Component did update!');
  }

  componentWillUnmount() {
    console.log('Component will unmount!');
  }

  handleIncrement = () => {
    this.setState(prevState => ({
      count: prevState.count + 1
    }));
  };

  handleDecrement = () => {
    this.setState(prevState => ({
      count: prevState.count - 1
    }));
  };

  render() {
    return (
      <div>
        <h1>Counter</h1>
        <p>Count: {this.state.count}</p>
        <button onClick={this.handleIncrement}>Increment</button>
        <button onClick={this.handleDecrement}>Decrement</button>
      </div>
    );
  }
}

export default MyComponent;

```
1. parent class component pass data to child component:

#ParentComponent.js：
```jsx
import React from 'react';
import ChildComponent from './ChildComponent';

class ParentComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      message: 'Hello from Parent'
    };
  }

  render() {
    return (
      <div>
        <h1>Parent Component</h1>
        <ChildComponent message={this.state.message} />
      </div>
    );
  }
}

export default ParentComponent;
```
#ChildComponent.js：
```jsx
import React from 'react';

class ChildComponent extends React.Component {
  render() {
    return (
      <div>
        <h2>Child Component</h2>
        <p>{this.props.message}</p>
      </div>
    );
  }
}

export default ChildComponent;
```
2. child class component pass data to parent component:
```jsx
import React from 'react';
import ChildComponent from './ChildComponent';

class ParentComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      messageFromChild: ''
    };
  }

  handleMessage = (message) => {
    this.setState({ messageFromChild: message });
  };

  render() {
    return (
      <div>
        <h1>Parent Component</h1>
        <p>Message from Child: {this.state.messageFromChild}</p>
        <ChildComponent sendMessage={this.handleMessage} />
      </div>
    );
  }
}

export default ParentComponent;
```
3. context usage:
```jsx
import React from 'react';

const MyContext = React.createContext();

export default MyContext;

```
```jsx
import React from 'react';
import ChildComponent from './ChildComponent';
import MyContext from './MyContext';

class ParentComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 'Hello from Context'
    };
  }

  render() {
    return (
      <MyContext.Provider value={this.state.value}>
        <div>
          <h1>Parent Component</h1>
          <ChildComponent />
        </div>
      </MyContext.Provider>
    );
  }
}

export default ParentComponent;
```
```jsx
import React from 'react';
import MyContext from './MyContext';

class ChildComponent extends React.Component {
  static contextType = MyContext;

  render() {
    return (
      <div>
        <h2>Child Component</h2>
        <p>{this.context}</p>
      </div>
    );
  }
}

export default ChildComponent;
```

#SPA/CSR(client side rendering)
SPA application only request once for html document, then apply AJAX mechanism, get data instead of whole page to render pages.

advantages:
1. no page refreshing.
2. faster loading and bandwidth saving.
3. user friendly.

disadvantages:
1. first rendering time longer than SSR. cuz need to download all js files.
2. bad SEO. cuz empty html is caught at the beginning.

SPA dynamic router:
```jsx
import React from 'react';
import { BrowserRouter as Router, Route, Switch, Link } from 'react-router-dom';
import Home from './components/Home';
import About from './components/About';
import Contact from './components/Contact';

const App = () => (
  <Router>
    <div>
      <nav>
        <ul>
          <li><Link to="/">Home</Link></li>
          <li><Link to="/about">About</Link></li>
          <li><Link to="/contact">Contact</Link></li>
        </ul>
      </nav>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
        <Route path="/contact" component={Contact} />
      </Switch>
    </div>
  </Router>
);

export default App;
```
low level of Link is actually an <a>, when click this tag,will not refresh page and  windows.history will change url+new path, then react will render mapped component by using Switch to loop all children component path to find mapped component.



# react-note
react-revision

# babel:
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

# virtual dom and real dom:
all class or function component are virtual doms, and they are built in tree,
this code is for rendering virtual doms into real doms.
```jsx
ReactDOM.render(thisComponent, document.getElementById('root'))
```
# DOM（Document Object Model）
HTML is a kind of document, and each tag/element stands for a dom which contributes to the tree shaped document, and js could operate each dom, by runing the js script.
dom is object which contains various of attributes, real doms has much more attributes than virtual dom,
operate real dom is costly, cause it may cause reflow and must cause repaint, which will cause bad performance, cause cpu are working so frequantly.

# reflow: 
any changes such as change size, position, hidden some elements, query some attribute lsuch as offsetWidth.. will cause reflow,
browser will recaculate layout. costly and time consuming.
# repaint: 
change color will cause broswer repaint.

react virtual dom will decrease times for operating real doms by applying diff alg, 
when components changes, react will generate a new virtual doms and compare with the old virtual doms, to get the modified dom (elememt, which is the unit of the alg), then render the diff real dom.

# jsx syntax rules: 
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
# 1. parent class component pass data to child component:

ParentComponent.js：
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
# 2. child class component pass data to parent component:
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
# 3. context usage:
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

# SPA/CSR(client side rendering)
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
low level of Link is actually an "<a>", when click this tag,will not refresh page and  windows.history will change url+new path, then react will render mapped component by using Switch to loop all children component path to find mapped component.

# React component event listener:
on+Event
when rendering button, will excute ()=>handleClick('jack'), and this will return a callback and be past as a parameter to this onclick, and envoked after user click.
```jsx
function App(){
const handleClick= (name)=>{
  console.log(name); //jack
  return (e)=>{
      console.log(e);// this e is passed by react
  }
}
  return(
    <button onClick={()=>handleClick('jack')}>click me</button>
  )
}
```
# use javascript to understand child pass info to parent
```js
function parentFunction() {
   function receiveDataFromChild(data) {
    console.log("Data received from child:", data);
}

    // call child func
    childFunction(receiveDataFromChild);
}

function childFunction(sendDataToParent) {
    const data = "Hello from Child";
    sendDataToParent(data);
}
```
 # hooks

 # useState
 
```jsx
import { useState } from 'react';

function App() {
  const [count, setCount] = useState(0);
  const [userInfo, setUserInfo] = useState({ name: 'wenzi', age: 24 });

  const handleClickByVal = () => {
    setCount(count + 1);
  };
  const handleClickByCallback = () => {
    setCount(count => count + 1);
  };
  const handleUpdateUserInfo = () => {
    setUserInfo({ ...userInfo, age: userInfo.age + 1 });
  };

  return (
    <div className="App">
      <p>{count}</p>
      <p>
        <button onClick={handleClickByVal}>add by val</button>
      </p>
      <p>
        <button onClick={handleClickByCallback}>add by callback</button>
      </p>
      <p>
        <button onClick={handleUpdateUserInfo}>update userInfo</button>
      </p>
    </div>
  );
}
```

```jsx
function App() {
  // pass callback，count = Date.now()
  const [count, setCount] = useState(() => {
    return Date.now();
  });
}
```
for each component, react keep a Fiber to manage states, 
useState cannot be put into if(){} or any loops: 

if: it will cause conditionally invoke different useState, and the new initialState will not cover the previous state at same index, they commonly use the same index, which will casue states lost.

loop: the times for loop for every render may different, this will cause disorder.

summery: useState should be invoked in order and the number of them should be keep the same as well.

recon: put useState() at the top of function component, cause we use const insead of function, no floating, after defined then use it.

```jsx
import React, { useState, useEffect } from 'react';

const MyComponent = () => {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('Hello');

  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);

  const increment = () => setCount(count + 1);
  const updateText = (e) => setText(e.target.value);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <p>Text: {text}</p>
      <input type="text" value={text} onChange={updateText} />
    </div>
  );
};

export default MyComponent;

```
# useState source code simple version:
```jsx
function someFunction(){
  let _index=0;
  const _states = [],
     _setStates = [];

  function createState(initialState, index){
    return states[index] !== undefined ? states[index]:initialState;
  }

  function createSetState(index){
  
    return function(newState){
      if(typeof newState === 'function'){
        states[index] = newState(states[index]);
      }else{
        states[index] = newState;
      }
      render();
    }
  }

  function render(){
   _index = 0;
    ReactDOM.render(<App />, document.getElementById('root'));
  }

  function useState(initialState){
    _states[_index] = createState(initialState, _index);
    if(!_setStates[_index]){
      _setStates.push(createSetState(_index));
    }
    const _state = _states[_index];
    const _setState = _setStates[_index];
    _index++;
    return [_state, _setState];
  }
}
```
# useState manage array and obj
```jsx
//array
//add
import React, { useState } from 'react';

function App() {
  const [items, setItems] = useState([]);

  const addItem = () => {
    const newItem = { id: items.length, value: Math.random() };
    setItems([...items, newItem]);
  };

  return (
    <div>
      <button onClick={addItem}>Add Item</button>
      <ul>
        {items.map(item => (
          <li key={item.id}>{item.value}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;


//delete
import React, { useState } from 'react';

function App() {
  const [items, setItems] = useState([{ id: 1, value: 'Item 1' }, { id: 2, value: 'Item 2' }]);

  const deleteItem = (id) => {
    const updatedItems = items.filter(item => item.id !== id);
    setItems(updatedItems);
  };

  return (
    <div>
      {items.map(item => (
        <div key={item.id}>
          {item.value} <button onClick={() => deleteItem(item.id)}>Delete</button>
        </div>
      ))}
    </div>
  );
}

export default App;


//update
import React, { useState } from 'react';

function App() {
  const [items, setItems] = useState([{ id: 1, value: 'Item 1' }, { id: 2, value: 'Item 2' }]);

  const updateItem = (id, newValue) => {
    const updatedItems = items.map(item =>
      item.id === id ? { ...item, value: newValue } : item
    );
    setItems(updatedItems);
  };

  return (
    <div>
      {items.map(item => (
        <div key={item.id}>
          {item.value} <button onClick={() => updateItem(item.id, 'Updated Value')}>Update</button>
        </div>
      ))}
    </div>
  );
}

export default App;

//obj
//update
import React, { useState } from 'react';

function App() {
  const [user, setUser] = useState({ name: 'Alice', age: 25 });

  const updateUserName = () => {
    setUser(prevUser => ({ ...prevUser, name: 'Bob' }));
  };

  return (
    <div>
      <p>Name: {user.name}</p>
      <p>Age: {user.age}</p>
      <button onClick={updateUserName}>Update Name</button>
    </div>
  );
}

export default App;

// add
import React, { useState } from 'react';

function App() {
  const [user, setUser] = useState({ name: 'Alice', age: 25 });

  const addUserEmail = () => {
    setUser(prevUser => ({ ...prevUser, email: 'alice@example.com' }));
  };

  return (
    <div>
      <p>Name: {user.name}</p>
      <p>Age: {user.age}</p>
      {user.email && <p>Email: {user.email}</p>}
      <button onClick={addUserEmail}>Add Email</button>
    </div>
  );
}

export default App;

```


# react batching
multi invoking of setState()

```jsx
// 直接使用变量
function AppData() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(count + 1);
    setCount(count + 1);
    setCount(count + 1);
  };

  return (
    <div className="App">
      <button onClick={handleClick}>click me, {count}</button>
    </div>
  );
}
```
```jsx
// 使用callback中的变量
function AppCallback() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(count => count + 1);
    setCount(count => count + 1);
    setCount(count => count + 1);
  };

  return (
    <div className="App">
      <button onClick={handleClick}>click me, {count}</button>
    </div>
  );
}
```
before:
```jsx
function App() {
  const [count, setCount] = useState(0);

  const getList = () => {
    // console.log(count);
    fetch('https://www.xiabingbao.com', {
      method: 'POST',
      body: JSON.stringify({ count }),
    });
  };

  const handleClick = () => {
    setCount(count + 1);
    console.log(count);

    // 本意是想用更新后的最新count来调用 getList()
    getList();
  };
}
```
in one event handler function, react will only render one time, so the count will keep the current reender value
only when use callback prevState=>prevState+1, this will use modified value as new value.

after:
```jsx
// way 1
const handleClick = () => {
  const newCount = count + 1;
  setCount(newCount);
  getList(newCount);
};
// way 2
function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    getList();
  }, [count]);

  const handleClick = () => {
    setCount(count + 1);
  };
}
```
# function component excuting order:
- Call all hooks (including `useState` and `useEffect`): Initialize state and define side effects.
- Execute the main part of the component function: Define event handlers and return JSX to render the UI.
- React updates the DOM: Render the UI based on the returned JSX.
- Execute `useEffect` side effects: Run after the DOM has been updated.

# some packages: lodash, classname, dayjs,

# get dom from react: useRef
```jsx
function App(){
  const inputRef = useRef(null);
  const showDom = ()=>{
    console.dir(inputRef.current);
  }
  return (
    <div>
      <input type="text" ref={inputRef}/>
      <button onClick={showDom}>get dom</button>
    </div>
  )
}
```
# UseEffect usage

```jsx
function App() {
    const [count,setCount] = useState(0);
    // 组件挂载完成之后 或 组件数据更新完成之后 执行
    useEffect(() => {
        console.log('组件挂载完成之后 或 组件数据更新完成之后 执行');
    })
    return (
        <div>
            {count}
            <button onClick={() => setCount(count + 1)}>+1</button>
        </div>
    )
}
```
```jsx
useEffect(() => {
    console.log('仅当做componentDidMount');
},[])
```
```jsx
function App() {
    function onScroll() {
        console.log('监听到页面发生滚动了');
    }
    useEffect(() => {
        window.addEventListener('scroll',onScroll);
        return () => {
            // 卸载组件时解除对事件的绑定
            window.removeEventListener('scroll',onScroll);
        }
    })
    return (
        <div>
            App 
        </div>
    )
}

```

```jsx
function App() {
    
    const [count,setCount] = useState(0);
    useEffect(() => {
        const timeId = setInterval(() => {
           setCount(count => count + 1); 
        },1000)
        return () => {
            clearInterval(timeId);
        }
    },[])
    return (
        <div>
            <h1>当前求和为：{count}</h1> 
        </div>
    )
}

```
```jsx
useEffect(() => {
    document.title = count;
}, [count])


//在useEffect中直接使用async和await是会报错的，推荐使用立即执行函数来包裹异步函数。
function getData() {
    return new Promise(resolve => {
        resolve({msg: 'Hello'})
    })
}
function App() {

    useEffect(() => {
        (async function () {
            const result = await getData();
            console.log(result);
        })()
    },[])
    
    return (
        <div>
            App
        </div>
    )
}

```


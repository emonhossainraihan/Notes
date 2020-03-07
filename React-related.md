## Contents
<Details>
<Summary>show all contents:</Summary>

- [Props](#props)
- [State](#state)
- [setState](#setstate)
- [Handler](#handler)
- [Binding-Event](#binding-event)
- [Conditionally-Rendering](#conditionally-rendering)
- [List-rendering](#list-rendering)
- [Controlled-Component](#controlled-component)
- [Refs](#refs)
- [Portal](#portal)
- [Error-handling](#error-handling)
- [HOC](#hoc)
- [Context](#context)
- [HTTP](#http)
- [Event-preventDefault](#event-preventdefault)

</Details>


## Props

### functional Props: 
```<FunProps name="Maiza">From functional props</FunProps>```
```js
const FunProps = props => {
  //destructing {name,children}
  const { name, children } = props;
  return (
    <div>
      <p>
        Hello {name} {children}
      </p>
    </div>
  );
};
export default FunProps;
```
### class Props:
```<Classprops name="Naju">From class-base props</Classprops>```
```js
class Classprops extends React.Component {
  render() {
    return (
      <div>
        <p>
          Hello {this.props.name} {this.props.children}
        </p>
      </div>
    );
  }
}
export default Classprops;
```
## State 
### class State: 
```js
class ClassState extends React.Component {
  constructor() {
    super();
    this.state = {
      message: "Welcome visitor"
    };
  }

  changeMessage() {
    this.setState({
      message: "Thanks you for subscribing"
    });
  };

  render() {
    return (
      <div>
        <h1>{this.state.message}</h1>
        <button onClick={() => this.changeMessage()}>subscribe</button>
      </div>
    );
  }
}
export default ClassState;
```
## setState():
```js
class SetState extends Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
      count5: 0
    };
  }

  increment() {
    this.setState({
      count: this.state.count + 1
    });
  }
  //setState({ state_object }, callack )
  //setState merfing the previous state with new one

  incrementmodifer(prevState) {
    this.setState(prevState => ({
      count5: prevState.count5 + 1
    }));
  }

  //setState( (prevState,props)=>({}) , callack ) If prevState needed

  incrementby5() {
    this.incrementmodifer();
    this.incrementmodifer();
    this.incrementmodifer();
    this.incrementmodifer();
    this.incrementmodifer();
  }

  render() {
    return (
      <div>
        <h1>setState with class component</h1>
        <p>Count - {this.state.count}</p>
        <button onClick={() => this.increment()}>increment</button>
        <p>Count5 - {this.state.count5}</p>
        <button onClick={() => this.incrementby5()}>increment by 5</button>
      </div>
    );
  }
}
export default SetState;
```
## Handler
### In class Component:
```js
class ClickEvent extends Component {
  clickHandler(){
    console.log('clicked')
  }
  render() {
    return (
      <div>
        <button onClick={this.clickHandler}>Click from class component</button>
      </div>
    );
  }
}
//look in handler we didn't invoke the handler rather passing a function
export default ClickEvent;
```
### In functional component:
```js
function FunctionClickEvent() {
function clickHandler(){
console.log('clicked!!')
}
  return (
    <div>
      <button onClick={clickHandler}>Click from Functional component</button>
    </div>
  );
}
export default FunctionClickEvent;
```
## Binding-Event:
```js
class EventBind extends React.Component {
  constructor() {
    super();
    this.state = {
      message: "I am a Event"
    };
    //process-03
    //this.clickHandler = this.clickHandler.bind(this)
    //binding in constructor help to use clickHandler in render without binding
  }

  clickHandler() {
    this.setState({ message: "Thank you for binding(this)" });
  }
  //process-04
  clickHandlermodify = () => {
    this.setState({ message: "Thank you for arrowHandler" });
  };

  render() {
    return (
      <div>
        <h1>{this.state.message}</h1>
        <button onClick={this.clickHandler.bind(this)}>render with bind</button>
        <button onClick={() => this.clickHandler()}>
          render with arrowfunction
        </button>
        <button onClick={this.clickHandlermodify}>
          handler with arrowfunction
        </button>
      </div>
    );
  }
} 
//process-03 and 04 is best
export default EventBind;
```
## Conditionally-Rendering
```js
class ConditionalRendering extends React.Component {
  constructor() {
    super();
    this.state = {
      isLoggedIn: false
    };
  }
  render() {
    //process-04 short circuit Operator

    // return this.state.isLoggedIn && <div>Welcome Emon</div>;

    //process-03 ternary Operator (best)

    return this.state.isLoggedIn ? (
      <div>Welcome Emon</div>
    ) : (
      <div>Welcome guest</div>
    );

    // //process-02 element variable

    // let message;
    // if (this.state.isLoggedIn) {
    //   message = <div>Welcome Emon</div>;
    // } else {
    //   message = <div>Welcome guest</div>;
    // }
    // return <div>{message}</div>;

    //process-01 conditionally rendering

    // if (this.state.isLoggedIn) {
    //   return <div>Welcome Emon</div>;
    // } else {
    //   return <div>Welcome guest</div>;
    // }
  }
}
export default ConditionalRendering;
```
## List-rendering 
```js
function ListRendeing() {
  const names = ["Emon", "Clark", "Diana"];
  const NameList = names.map((name, index) => (
    <Person key={index} name={name} />
  ));

  return (
    <div>
      <div>
        <h1>Using childcomponent</h1>
        <p>list rendering using index in map</p>
        {NameList}
      </div>
    </div>
    // <div>
    //   {/* <h2>{names[0]}</h2>
    //   <h2>{names[1]}</h2>
    //   <h2>{names[2]}</h2> */}
    // </div>
  );
}
export default ListRendeing;
//key={name} :A "key" is a special attribute you need to include when creating lists of elements
```
## Controlled-Component: 
```js
class Form extends React.Component {
  constructor() {
    super();
    this.state = {
      username: "",
      comments: "",
      topic: "react"
    };
  }
  HandlerUserName = event => {
    this.setState({ username: event.target.value });
  };

  HandlerComments = event => {
    this.setState({ comments: event.target.value });
  };
  HandlerTopic = event => {
    this.setState({ topic: event.target.value });
  };
  HandlerSumit = event => {
    alert(`${this.state.username} ${this.state.comments} ${this.state.topic}`);
    event.preventDefault();
  };
  render() {
    // const {username,comments,topic}=this.state;
    return (
      <form onSubmit={this.HandlerSumit}>
        <div>
          <label>Username</label>
          <input
            type="text"
            value={this.state.username}
            onChange={this.HandlerUserName}
          />
        </div>
        <div>
          <label>Comments</label>
          <textarea
            value={this.state.comments}
            onChange={this.HandlerComments}
          />
        </div>
        <div>
          <label>Topic</label>
          <select value={this.state.topic} onChange={this.HandlerTopic}>
            <option value="react">React</option>
            <option value="angular">Angular</option>
            <option value="vue">Vue</option>
          </select>
        </div>
        <button type="submit">Submit</button>
      </form>
    );
  }
}
export default Form;
```
## Refs 
Refs make it possible to access DOM nodes directly within React.
```js
class Refs extends React.Component {
  constructor() {
    super();
    this.inputRef = React.createRef(); //process-01
    this.cbRef = null; //callback Refs (older)
    this.setCbRef = element => {
      this.cbRef = element;
    };
  }
  componentDidMount() {
    if (this.cbRef) {
      this.cbRef.focus();
    }
    //process-01
    // this.inputRef.current.focus();
    // console.log(this.inputRef);
  }
  clickHandler = () => {
    alert(this.inputRef.current.value); //fetching data
  };
  render() {
    return (
      <div>
        <input type="text" ref={this.inputRef} />
        <input type="text" ref={this.setCbRef} />
        <button onClick={this.clickHandler}>Click</button>
      </div>
    );
  }
}
export default Refs;
```
Forwarding refs to child component
```js
class ForwardParent extends React.Component {
  constructor() {
    super();
    this.inputRef = React.createRef();
  }
  clickHandler = () => {
    this.inputRef.current.focus();
  };
  render() {
    return (
      <div>
        <ForwardRefs ref={this.inputRef} />
        <button onClick={this.clickHandler}>Focus Input</button>
      </div>
    );
  }
}
export default ForwardParent;

const ForwardRefs = React.forwardRef((props, ref) => {
  return (
    <div>
      <input type="text" ref={ref} />
    </div>
  );
});

export default ForwardRefs;
//the child element get ref from parent component
```
## Portal 
```js
import ReactDOM from "react-dom";

function Portal() {
  return ReactDOM.createPortal(
    <h1>Portals demo</h1>,
    document.getElementById("portal-root")
  );
}
export default Portal;
//break the normal flow of DOM
```
## Error-handling 
```js
class ErrorFix extends React.Component {
  constructor() {
    super();
    this.state = {
      hasError: false
    };
  }
  static getDerivedStateFromError(error) {
    return {
      hasError: true
    };
  }
componentDidCatch(error,info){
  console.log(error)
  console.log(info)
}
  render() {
    if (this.state.hasError) {
      //fallack UI
      return <h1>Something went wrong</h1>;
    }
    return this.props.children;
  }
}
export default ErrorFix;

function EB({ heroname }) {
  if (heroname === "Joker") {
    throw new Error("Not a  hero");
  }
  return <div>{heroname}</div>;
}
export default EB;
```
In `app.js`:
```html
<ErrorFix>
  <EB heroname="Batman" />
  <EB heroname="joker" />
</ErrorFix>
```
## HOC
```js
const withCounter = WrappedComponent => {
  class withCounter extends React.Component {
    constructor() {
      super();
      this.state = {
        count: 0,
        countSimplify: 0
      };
    }

    incrementCount = () => {
      this.setState(prevState => {
        return { count: prevState.count + 1 };
      });
    };
    render() {
      return (
        <WrappedComponent
          count={this.state.count}
          incrementCount={this.incrementCount}
          {...this.props} //rest properties of props
        />
      );
    }
  }
  return withCounter;
};
export default withCounter;
```
child component:
```js
import withCounter from "./withCounter";
class ClickCounter extends React.Component {
  render() {
    const { count, incrementCount } = this.props;
    return <button onClick={incrementCount}> Click {count} times</button>;
  }
}
export default withCounter(ClickCounter);
```
## Context 
```js
const UserContext = React.createContext("Codevolution");

const UserProvider = UserContext.Provider;
const UserConsumer = UserContext.Consumer;

export { UserProvider, UserConsumer };
export default UserContext;
```
In `app.js`:
```html
import { UserProvider } from "./components/Context/UserContext";
<UserProvider value="Emon Hossain">
  <ComponentC />
</UserProvider>
```
nested components :
```js
import ComponentE from "./ComponentE";
class ComponentC extends React.Component {
  render() {
    return <ComponentE />;
  }
}
export default ComponentC;

import ComponentF from "./ComponentF";
import UserContext from "./UserContext";
class ComponentE extends React.Component {
  render() {
    return (
      <div>
        ComponentE context {this.context}
        <ComponentF />
      </div>
    );
  }
}
ComponentE.contextType = UserContext;
export default ComponentE;

import { UserConsumer } from "./UserContext";
class ComponentF extends React.Component {
  render() {
    return (
      <UserConsumer>
        {username => {
          return <div>Hello {username}</div>;
        }}
      </UserConsumer>
    );
  }
}
export default ComponentF;
```
## HTTP 
```js
import axios from "axios";
class PostList extends React.Component {
  constructor() {
    super();
    this.state = {
      posts: [],
      errorMsg: ""
    };
  }
  componentDidMount() {
    axios
      .get("https://jsonplaceholder.typicode.com/posts")
      .then(response => {
        console.log(response);
        this.setState({ posts: response.data });
      })
      .catch(error => {
        console.log(error);
        this.setState({ errorMsg: "Error retreiving data" });
      });
  }
  render() {
    const { posts, errorMsg } = this.state;
    return (
      <div>
        List of posts
        {posts.length
          ? posts.map(post => <div key={post.id}>{post.title}</div>)
          : null}
        {errorMsg ? <div>{errorMsg}</div> : null}
      </div>
    );
  }
}
export default PostList;
```

## Event-preventDefault

React uses synthetic events to handle events from button, input and form elements. A synthetic event is a shell around the native DOM event with additional information for React. Sometimes you have to use `event.preventDefault();` in your application.
```js
import React from  'react';
const initialList =  [
  'Learn React',
  'Learn Firebase',
  'Learn GraphQL',
];
const  ListWithAddItem  =  ()  =>  {
  const  [value, setValue]  = React.useState('');
  const  [list, setList]  = React.useState(initialList);
  const  handleChange  =  event  =>  {
  setValue(event.target.value);
  };
  const  handleSubmit  =  event  =>  {
  if  (value)  {
  setList(list.concat(value));
  }
  setValue('');
 event.preventDefault();
  };
  return  (
  <div>
  <ul>
  {list.map(item  =>  (
  <li  key={item}>{item}</li>
  ))}
  </ul>
  <form  onSubmit={handleSubmit}>
  <input  type="text"  value={value}  onChange={handleChange}  />
  <button  type="submit">Add Item</button>
  </form>
  </div>
  );
};
export  default ListWithAddItem;
```
In this case, a preventDefault is called on the event when submitting the form to **prevent a browser reload/refresh**. 
**Why is a form submit reloading the browser?** All native HTML elements come with their internal native behavior. For instance, input elements store their internal state. That's why often [React is used to take over for having controlled components by managing the state via React](https://www.robinwieruch.de/react-controlled-components/). The same applies for a form element which has a submit event that is invoked via a submit button element. In the past, it was desired to refresh the browser to flush all state and to submit the data to a backend. Nowadays, a library such as React, gives us more flexibility to deal with the submit event ourselves. In this case, we deal with it by updating the list in our component's state.
# [selector](https://medium.com/@pearlmcphee/selectors-react-redux-reselect-9ab984688dd4) in redux
selectors provide the encapsulation of knowledge of where to find data which leads the ability to write reusable and composable code.
### Why Use Reselect to Create Selectors?
The short answer is: for performance as Reselect provides a wrapper for creating selectors that are memoized. <br>
When you factor in the fact that generally speaking React components re-render whenever their or their parent’s state or props change — the computation of derived data can easily become expensive. *The biggest benefit of Reselect is that selectors created with the library are memoized and therefore will only re-run if their arguments change*. Reselect offers up a function called createSelector() that’s used to create memoized selectors.

# INTRODUCTION

- Notes from React.js courses on codecademy.com
- [Introduction to React.js: Part I](https://www.codecademy.com/learn/react-101)
- [Introduction to React.js: Part II](https://www.codecademy.com/learn/react-102)
- I advise you to take the courses and use this summary only to recall the syntax
- Also please note that these courses do not use ES6 syntax

# CONTENT

1. [UNIT 1: JSX](#unit-1-jsx)
  1. [Basic syntax](#basic-syntax)
  1. [Class atribute](#class-attribute)
  1. [Self-closing tags](#self-closing-tags)
  1. [Event listeners](#event-listeners)
  1. [Conditionals ?:](#conditionals-)
  1. [Conditionals &&](#conditionals--1)
  1. [.map](#map)
1. [UNIT 2: REACT COMPONENTS](#unit-2-react-components)
  1. [Basic component](#basic-component)
  1. [.this](#this)
  1. [Component with event handler](#component-with-event-handler)
1. [UNIT 3: COMPONENTS INTERACTING](#unit-3-components-interacting)
  1. [Components render other components](#components-render-other-components)
  1. [require and module.exports](#require-and-moduleexports)
  1. [props](#props)
  1. [props - event handler](#props---event-handler)
  1. [this.props.children](#thispropschildren)
  1. [getDefaultProps](#getdefaultprops)
  1. [state](#state)
1. [UNIT 4: STATELESS COMPONENTS INHERIT FROM STATEFUL COMPONENTS](#unit-4-stateless-components-inherit-from-stateful-components)
  1. [props vs state](#props-vs-state)
  1. [Basic programming pattern](#basic-programming-pattern)
1. [UNIT 5: ADVANCED REACT](#unit-5-advanced-react)
  1. [Inline styles](#inline-styles)
  1. [Styles in a variable](#styles-in-a-variable)
  1. [Containers](#containers)
  1. [Stateless functional component](#stateless-functional-component)
  1. [propTypes](#proptypes)
  1. [Stateless function component with propTypes](#stateless-function-component-with-proptypes)
  1. [Forms](#forms)
1. [UNIT 6: LIFECYCLE METHODS](#unit-6-lifecycle-methods)
  1. [Introduction to lifecycle methods](#introduction-to-lifecycle-methods)
  1. [Mounting lifecycle methods](#mounting-lifecycle-methods)
  1. [Updating lifecycle methods](#updating-lifecycle-methods)
  1. [Unmounting lifecycle methods](#unmounting-lifecycle-methods)

# UNIT 1: JSX

## Basic syntax

```javascript
var myVar = <b>world</b>;

var myDiv = (
  <div>
    <h1>Hello {myVar}</h1>
  </div>
);

ReactDOM.render(myDiv, document.getElementById('app'));
```

## Class attribute

- In JSX, you can't use the word **class**! You have to use **className** instead
- This is because JSX gets translated into JavaScript, and **class** is a reserved word in JavaScript

## Self-closing tags

```
Fine in JSX: <br />
NOT FINE AT ALL in JSX: <br>
```

## Event listeners

```javascript
function makeDoggy (e) {
  // Call this extremely useful function on an <img>.
  // The <img> will become a picture of a doggy.
  e.target.setAttribute('src', 'https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-puppy.jpeg');
  e.target.setAttribute('alt', 'doggy');
}

var kitty = (<img 
  src="https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-kitty.jpg" 
  alt="kitty" 
  onClick={makeDoggy}/>
);

ReactDOM.render(kitty, document.getElementById('app'));
```

## Conditionals ?:

- if-else statements don't work inside JSX, you probably want to make use of a ternary expression

## Conditionals &&

```javascript
var tasty = (
  <ul>
    <li>Applesauce</li>
    { !baby && <li>Pizza</li> }
    { age > 15 && <li>Brussels Sprouts</li> }
    { age > 20 && <li>Oysters</li> }
    { age > 25 && <li>Grappa</li> }
  </ul>
);
```

## .map

```javascript
var people = ['Rowe', 'Prevost', 'Gare'];

var peopleLIs = people.map(function(person, i){
	return <li key={'person_' + i}>{person}</li>;
});

// ReactDOM.render goes here:
ReactDOM.render(<ul>{peopleLIs}</ul>, document.getElementById('app'));
```

-----------------------------------------------

# UNIT 2: REACT COMPONENTS

## Basic component

```javascript
var MyComponentClass = React.createClass({
	render: function () {
		return <h1>Hello world</h1>;
	}
});

ReactDOM.render(<MyComponentClass />, document.getElementById('app'));
```

## .this

```javascript
var fiftyFifty = Math.random() < 0.5;

var TonightsPlan = React.createClass({
	name: 'Petr',
  render: function () {
    return fiftyFifty ? <h1>My name is {this.name} and tonight I'm going out WOOO</h1> : <h1> My name is {this.name} and tonight I'm going to bed WOOO</h1>;
  }
});

ReactDOM.render(<TonightsPlan />,	document.getElementById('app'));
```

## Component with event handler

```javascript
var Button = React.createClass({
  scream: function () {
    alert('AAAAAAAAHHH!!!!!');
  },
  render: function () {
    return <button onClick={this.scream}>AAAAAH!</button>;
  }
});

ReactDOM.render(<Button />, document.getElementById('app'));
```

-----------------------------------------------

# UNIT 3: COMPONENTS INTERACTING

## Components render other components

```javascript
var OMG = React.createClass({
  render: function () {
    return <h1>Whooaa!</h1>;
  }
});

var Crazy = React.createClass({
  render: function () {
    return <OMG />;
  }
});
```

## require and module.exports
- module.exports comes from Node.js's module system, just like require does
- module.exports and require are meant to be used together, and you basically never see one without the other
- module.exports means, "If you require the file that I am in, then here's what you're going to get!"

## props
- Information that gets passed from one component to another is known as **props**.

```javascript
var Greeting = React.createClass({
  render: function () {
    return <h1>Hi there, {this.props.name}</h1>;
  }
});

ReactDOM.render(<Greeting name='Petr' />, document.getElementById('app'));
```

## props - event handler

```javascript
var Button = React.createClass({
  render: function () {
    return <button onClick={this.props.onClick}>Click me!</button>
  }
});

var Talker = React.createClass({
  handleClick: function () {
    alert('bla');
  },
  render: function () {
    return <Button onClick={this.handleClick} />;
  }
});

ReactDOM.render(<Talker />, document.getElementById('app'));
```

## this.props.children

- If a component has more than one child between its JSX tags, then this.props.children will return those children in an array. However, if a component has only one child, then this.props.children will return the single child, not wrapped in an array.

```javascript
var App = React.createClass({
  render: function () {
    return (
      <div>
        <List type='Living Musician'>
          <li>Sachiko M</li>
          <li>Harvey Sid Fisher</li>
        </List>
        <List type='Living Cat Musician'>
          <li>Nora the Piano Cat</li>
        </List>
      </div>
    );
  }
});

var List = React.createClass({
  render: function () {
    var titleText = 'Favorite ' + this.props.type;
    if (this.props.children instanceof Array) {
      titleText += 's';
    }
    return (
      <div>
        <h1>{titleText}</h1>
        <ul>{this.props.children}</ul>
      </div>
    );
  }
});

ReactDOM.render(<App />, document.getElementById('app'));
```

## getDefaultProps

```javascript
var Example = React.createClass({
  getDefaultProps: function () {
    return { text: 'No props.text :(' };
  },

  render: function () {
    return <h1>{this.props.text}</h1>;
  }
});
```

## state

```javascript
var Toggle = React.createClass({
  getInitialState: function() {
    return {
      color: 'green'
    }
  },
  changeColor: function() {
    this.setState({
      color: this.state.color === 'green' ? 'yellow' : 'green'
    });
  },
  render: function () {
    return (
      <div style={{background: this.state.color}}>
        <h1>Change my color</h1>
        <button onClick={this.changeColor}>Change color</button>
      </div>
    );
  }
});

ReactDOM.render(<Toggle />, document.getElementById('app'));
```

- You can call this.setState from any property passed to React.createClass... with one big exception. You can't call this.setState from the render function! 
- Any time that you call this.setState, this.setState AUTOMATICALLY calls render as soon as the state has changed.
- That is why you can't call this.setState from inside of the render function! this.setState automatically calls render. If render calls this.setState, you will create an infinite loop.

-----------------------------------------------

# UNIT 4: STATELESS COMPONENTS INHERIT FROM STATEFUL COMPONENTS

## props vs state

- Props are immutable!
- A React component should use props to store information that can be changed, but can only be changed by a different component. A React component should use state to store information that the component itself can change.
- If a Component needs to alter one of its attributes at some point in time, that attribute should be part of its state, otherwise it should just be a prop for that Component.
- https://github.com/uberVU/react-guide/blob/master/props-vs-state.md

## Basic programming pattern

```javascript
var Child = React.createClass({
  handleChange: function (e) {
    var name = e.target.value;
    this.props.onChange(name);
  },
  
  render: function () {
    return (
      <div>
        <select 
          id="great-names" 
          onChange={this.handleChange}>
          
          <option value="Frarthur">Frarthur</option>
          <option value="Gromulus">Gromulus</option>
          <option value="Thinkpiece">Thinkpiece</option>
        </select>
      </div>
    );
  }
});

var Sibling = React.createClass({
  render: function () {
    var name = this.props.name;
    return (
      <div>
        <h1>Hey, my name is {name}!</h1>
        <h2>Dont you think {name} is the prettiest name ever?</h2>
        <h2>Sure am glad that my parents picked {name}!</h2>
      </div>
    );
  }
});

var Parent = React.createClass({
  getInitialState: function () {
    return { name: 'Frarthur' };
  },
  
  changeName: function (newName) {
    this.setState({
      name: newName
    });
  },

  render: function () {
    return (
      <div>
        <Child onChange={this.changeName} />
        <Sibling name={this.state.name} />
      </div>
    );
  }
});

ReactDOM.render(<Parent />, document.getElementById('app'));
```

-----------------------------------------------

# UNIT 5: ADVANCED REACT

## Inline styles

- In regular JavaScript, style names are written in hyphenated-lowercase
- In React, those same names are instead written in camelCase
- In React, if you write a style value as a number, then the unit "px" is assumed

```javascript
var styleMe = <h1 style={{ background: 'lightblue', color: 'darkred' }}>Please style me!  I am so bland!</h1>;

ReactDOM.render(styleMe, document.getElementById('app')
);
```

## Styles in a variable

- Defining a variable named style in the top-level scope would be an extremely bad idea in many JavaScript environments! In React, however, it's totally fine.
- Remember that every file is invisible to every other file, except for what you choose to expose via module.exports. You could have 100 different files, all with global variables named style, and there could be no conflicts.

```javascript
var styles = {
  color: 'darkcyan',
  background: 'mintcream'
};

var StyledClass = React.createClass({
  render: function () {
    return <h1 style={styles}>Hello world</h1>;
  }
});

module.exports = StyledClass;
```

## Containers

- In this programming pattern, the container component does the work of figuring out what to display. The presentational component does the work of actually displaying it.
- A container does data fetching and then renders its corresponding sub-component. That’s it.
- https://medium.com/@learnreact/container-components-c0e67432e005#.k0tclq1w9

## Stateless functional component

```javascript
// Normal way to display a prop:
var MyComponentClass = React.createClass({
  render: function () {
    return <h1>{this.props.title}</h1>;
  }
});

// Stateless functional component way to display a prop:
function MyComponentClass (props) {
  return <h1>{props.title}</h1>;
}
```

## propTypes

```javascript
var Runner = React.createClass({
  propTypes: {
    message:   React.PropTypes.string.isRequired,
    style:     React.PropTypes.object.isRequired,
    isMetric:  React.PropTypes.bool.isRequired,
    miles:     React.PropTypes.number.isRequired,
    milesToKM: React.PropTypes.func.isRequired,
    races:     React.PropTypes.array.isRequired
  },

  render: function () {
    ...
  }
});
```

## Stateless function component with propTypes

```javascript
function MyComponentClass (props) {
  return <h1>{props.title}</h1>;
}

MyComponentClass.propTypes = {
  title: React.PropTypes.string.isRequired
};
```

## Forms

- An uncontrolled component is a component that maintains its own internal state. A controlled component is a component that does not maintain any internal state. Since a controlled component has no state, it must be controlled by someone else.
- The fact that ```<input />``` keeps track of information makes it an uncontrolled component.
- In React, when you give an ```<input />``` a value attribute, then something strange happens: the ```<input />``` BECOMES controlled. It stops using its internal storage.

```javascript
var Input = React.createClass({
  getInitialState: function () {
    return {
      userInput: ''
    };
  },
  handleUserInput: function (e) {
    this.setState({
      userInput: e.target.value
    })
  },
  render: function () {
    return (
      <div>
        <input type="text" value={this.state.userInput} onChange={this.handleUserInput} />
        <h1>{this.state.userInput}</h1>
      </div>
    );
  }
});

ReactDOM.render(<Input />, document.getElementById('app'));
```

-----------------------------------------------

# UNIT 6: LIFECYCLE METHODS

## Introduction to lifecycle methods

- Lifecycle methods are methods that get called at certain moments in a component's life.
- There are three categories of lifecycle methods: mounting, updating, and unmounting.

## Mounting lifecycle methods

- There are three mounting lifecycle methods componentWillMount, render, componentDidMount.
- When a component mounts, it automatically calls these three methods, in order.
- Mounting lifecycle events only execute the first time that a component renders.
- If your React app uses AJAX to fetch initial data from an API, then componentDidMount is the place to make that AJAX call.

```javascript
var Flashy = React.createClass({
  componentWillMount: function () {
    alert('AND NOW, FOR THE FIRST TIME EVER...  FLASHY!!!!');
  },
  
  componentDidMount: function () {
    alert('YOU JUST WITNESSED THE DEBUT OF...  FLASHY!!!!!!!');
  },

  render: function () {
    alert('Flashy is rendering!');
    return <h1>OOH LA LA LOOK AT ME I AM THE FLASHIEST</h1>;
  }
});
```

## Updating lifecycle methods

- The first time that a component instance renders, it does not update. A component updates every time that it renders, starting with the second render.
- There are five updating lifecycle methods componentWillReceiveProps, shouldComponentUpdate, componentWillUpdate, render, componentDidUpdate.
- Whenever a component instance updates, it automatically calls all five of these methods, in order.

```javascript
componentWillReceiveProps: function(nextProps) {
  if (nextProps.number > this.state.highest) {
    this.setState({
      highest: nextProps.number
    });
  }
}
```

```javascript
shouldComponentUpdate: function (nextProps, nextState) {
  if ((this.props.text == nextProps.text) && 
    (this.state.subtext == nextState.subtext)) {
    alert("Props and state haven't changed, so I'm not gonna update!");
    return false;
  } else {
    alert("Okay fine I will update.")
    return true;
  }
}
```

```javascript
componentWillUpdate: function (nextProps, nextState) {
  alert('Component is about to update!  Any second now!');
}
```

```javascript
componentDidUpdate: function (prevProps, prevState) {
  alert('Component is done rendering!');
}
```

## Unmounting lifecycle methods

- A component's unmounting period occurs when the component is removed from the DOM. This could happen if the DOM is rerendered without the component, or if the user navigates to a different website or closes their web browser.
- There is only one unmounting lifecycle method componentWillUnmount

```javascript
componentWillUnmount: function (prevProps, prevState) {
  alert('Goodbye world');
}
```

-----------------------------------------------
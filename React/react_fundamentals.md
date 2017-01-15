# create-react-app

```Bash
sudo npm install -g create-react-app

create-react-app appName
# wait for install

cd appName
npm start
```

# Hello world App

Three starter file

- public/index.html
- index.js
- App.js


```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>React App</title>
</head>
<body>

  <div id="root"></div>

</body>
</html>
```


```javascript
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

```jsx
// src/App.js
import React from 'react;

class App extends React.Component {
    render() {
        return <h1>Hello word</h1>
    }
}

// stateless component
// const App = (props) => <h1>Hello stateless<h1>

export default App;
```

# Nested Component


```jsx
// src/App.js
import React, {Component} from 'react';

class App extends Component {
    render() {
        return (
            <Button>I <Heart /> React</Button>
        )
    }
}



// cut heart glyphicon
const Heart = (props) => <span>&heart;</span>

// Button contains <button> tag
const Button = (props) => <button>{props.children}</button>

export default App;

```

# PropTypes & defaultProps

## Simple demo

```jsx
import React, {Component, PropTypes} from 'react';

class App extends Component {
    render() {
        return (
            // <Widget txt="I am a widget" /> // cat is required, so error
            <Widget txt="I am a widget" cat="meow" />
        )
    }
}

const Widget = (props) => <h1>{props.txt} - {props.cat}</h1>

Widget.propTypes = {
    txt: React.PropTypes.string,
    // I have imported PropTypes for shorthand
    cat: PropTypes.string.isRequired
}

export default App;
    
```

## Advanced demo - custom propTypes

```jsx
// src/App.js
import React, {Component} from  'react';

class App extends Component {
    render() {
        return (
            // <Title /> // error: Missing text
            // <Title text="123" /> // error: text was too short
            <Title text="123456" />
        )
    }
}

const Title = (props) => <h1>Title: {props.text}</h1>

Title.propTypes = {
    text(props, propName, component) {
        if (!(porpName in props)) {
            return new Error(`Missing ${propName}`);
        }
        if (porps[propName].length < 6) {
            return new Error(`${propName} was too short`);
        }
    }
}

export default App;

```

# Event Flow

## Simple demo

```jsx
// src/App.js
import React, {Component} from 'react';

class App extends Component {
    constructor() {
        super()
        this.state = {
            txt: 'This is a state text',
            count: 0
        }
    }
    
    update(e) {
        this.setState({
            txt: e.target.value,
            count: this.state.count + 1
        })
    }    
    render() {
        return (
            // render() returns only one element, if you got two use <div> tag around them
            <div>
                <h1>{this.state.txt} - {this.state.count}</h1>
                <input
                    type="text"
                    defaultValue=""
                    // binding update method here
                    onChange={this.update.bind(this)}
                />
            </div>
        )
    }
}

export default App;

```

## Advanced demo

```jsx
// src/App.js
import React, {Component} from 'react';

class App extends Component {

    constructor() {
        super()
        this.state = {
            currentEvent: 'None'
        }
        
        // NOTE update method bind to this here
        this.update = this.update.bind(this)
    }
    
    update(e) {
        this.setState({
            currentEvent: e.type
        })
    }
    
    render() {
        return (
            <div>
                <textarea
                    onKeyPress={this.update}
                    onCopy={this.update}
                    onCut={this.update}
                    onPaste={this.update}
                    onFocus={this.update}
                    onBlur={this.update}
                    onDoubleClick={this.update}
                    onTouchStart={this.update}
                    onTouchMove={this.update}
                    onTouchEnd={this.update}
                    row="10"
                    col="30"
                />           
                <h1>{this.state.currentEvent}</h1>
            </div>
        )
    }
}

export default App;


```

# React ref - Get a reference to Specific Components


```jsx
import React, {Component} from 'react';
import ReactDOM from 'react-dom';

class App extends Component {
    constructor() {
        super()
        this.state = {
            a: '',
            b: ''
        }
        this.update = this.update.bind(this);
    }
    
    update() {
        this.setState({
            a: this.refs.a.refs.inp.value, // 1.get the specified <input> node
            // a: this.a.refs.inp.value, // 2. get <Input> by this.a
            // a: ReactDOM.findDOMNode(this.a).refs.inp.value 
            // get <Input> a with findDOMNode() specified with this.a
            b: this.refs.b.value
        })
    }

    render() {
        return (
            <div>
                <Input
                    ref="a" // 1.ref set to a, <Input> node = this.refs.a
                    // ref={component => this.a = component} 
                    // 2.set reference to this.a
                    update={this.update}
                />
                
                <input
                    ref="b" // ref set to b, <input> node = this.refs.b
                    type="text"
                    onChange={this.update}
                />
                {this.state.b}
            </div>
        )
    }
}

// Input set ref to "inp", Input node = this.refs.inp
const Input = (props) => 
    <div><input ref="inp" type="text" onChange={this.props.update} /></div>

export default App;

```

# Component Life Cycle Methods

[React Component Life Cycle](https://facebook.github.io/react/docs/react-component.html)

## Demo 1


```jsx
// src/App.js
import React, {Component} from 'react';
import ReactDOM from 'react-dom';

/**
 * App: click two button Mount/Unmount, render/unmount button to div#a 
 */
class App extends Component {
    mount() {
        ReactDOM.render(<WrappedIn />, document.getElementById('a'))
    }
    
    unmount() {
        ReactDOM.unmountComponentAtNode(doucment.getElementById('a'))
    }

    render() {
        return (
            <button onClick={this.mount.bind(this)}>Mount</button>
            <button onClick={this.unmount.bind(this)}>Unmount</button>
            <div id="a"></div>
        )
    }
}

/**
 * WrappedIn: show component life cycle
 * show even number, and increase to next even number every 500ms
 */
class WrappedIn extends Component {
    constructor() {
        super()
        this.state = {
            val: 0
        }
        this.update = this.update.bind(this)
    }
    
    /* update increase this.state.val */
    update() {
        this.setState({val: this.state.val + 1})
    }
    
    /* set some extra parameter before render */
    componentWillMount() {
        console.log('Will Mount');
        // set mod to 2
        this.setState({
            mod: 2
        })
    }
    
    render() {
        return (
           <button
                onClick={this.update
            >{this.state.val * this.state.mod}</button>
        )
    }
    
    /* actions when component rendered completely */
    componentDidMount() {
        console.log('Component Did Mount');
        // since update() called every 500ms
        // this.state.val will increase automatically
        this.inc = setInterval(this.update, 500)
    }
    
    /* clean actions before destructing component */
    componentWillUnmount() {
        console.log('Component Will Unmount');
        // clear interval of update, if not, will cause errors
        clearInterval(this.inc)
    }
}

export default App;

```

## Demo 2

Deprecated Demo.
Since ShouldComponent() is treated as a hint rather than strict directive, componentWillUpdate(), componentDidUpdate() will still be invoked.

```jsx
// src/App.js
import React, {Component} from 'react';

class App extends Component {

  constructor() {
    super();
    this.state = {
      increasing: false
    }
  }

  update() {
    ReactDOM.render(
      <App val={this.props.val+1} />,
      document.getElementById('root')
    )
  }

  componentWillReceiveProps(nextProps) {
    this.setState({
      increasing: nextProps.val > this.props.val
    })
  }

  ShouldComponentUpdate(nextProps, nextState) {
    return nextProps.val % 5 === 0;
  }

  render() {
    console.log(this.state.increasing);
    return(
      <button onClick={this.update.bind(this)}>
        {this.props.val}
      </button>
    )
  }

  componentDidUpdate(prevPorps, prevState) {
    console.log(`prevPorps: ${prevPorps.val}`);
  }
}

App.defaultProps = {val: 0}

export default App;
```

# Filter text - Search in React


```jsx
// src/App.js
import React, {Component} from 'React';

class App extends Component {

    constructor() {
        super()
        this.state = {
            items: []
        }
    }
    
    /* fetch date from internet - people in Star War */
    componentWillMount() {
        fetch('http://swapi.co/api/people/?format=json')
            .then(res => res.json())
            .then( ({results: items}) => {this.setState(items)} )
    }
    
    /* set filter with text in <input> */
    filter(e) {
        this.setState({
            filter: e.target.value
        })
    }

    render() {
        
        let items = this.state.items
        
        /* search filter text in items */
        if (this.state.filter) {
            items = items.filter( item =>
                item.name.toLowerCase().includes(this.state.filter.toLowerCase()))
        }
    
        return (
            <div>
                {
                    items.map(item => 
                        /* each iterated item need a unique "key" */
                        // <h4 key={item.name}>{item.name}</h4>)
                        <Person key={item.name} person={item} />)
                }
            </div>
        )
    }
}

const Person = (props) => <h4>{props.person.name}</h4>

export default App;

```

# High Order Component


```jsx
import React, {Component} from 'react'

/* High Order Component */
const HOC = (InnerComponent) => class extends Component {
    constructor() {
        super()
        this.state = {
            count: 0
        }
    }
    
    update() {
        this.setState({
            count: this.state.count + 1
        })
    }
    
    componentWillMount() {
        console.log('Will Mount');
    }
    
    render() {
        return <InnerComponent
            {...this.props}
            {...this.state}
            update=this.update.bind(this)
        />
    }

}

class App extends Component {
    render() {
        return (
            <Button>button</Button>
            <hr />
            <LabelHOC>label<LabelHOC>
        )
    }
}

// const Button = (props) => <button>{props.children}</button>
/* with HOC, stateless Button Component have state and update action now */
const Button = HOC( (props) => 
    <button onClick={props.update}>{props.children} - {props.count}</button> 
)

class Label extends Component {
    componenWillMount() {
        console.log('Label will mount');
    }
    
    render() {
        return (
            <label 
                onMouseMove={this.prop.update}
            >{this.props.children} - {this.props.count}</label>
        )
    }
}

/* with HOC, Label have extra state.count and update action now */
const LabelHOC = HOC(Label)

export default App;

```

# Implement a standalone Babel in browser


```html
...
<head>

    ...
    <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/6.18.1/babel.min.js"></script>
</head>

...

```


```jsx
import React, {Component} from 'react';
import './App.css'

class App extends Component {
  constructor() {
    super()
    this.state = {
      err: '',
      input: '/* add some jsx code here */',
      output: ''
    }
  }
  update(e) {
    let code = e.target.value;
    try {
      this.setState({
        output: window.Babel.transform(code, {presets: ['es2015', 'react']}).code,
        err: ''
      })
    } catch(err) {
      this.setState({
        err: err.message
      })
    }
  }
  render() {
    return (
      <div>
        <header>
          {this.state.err}
        </header>
        <div className="container">
          <textarea 
            onChange={this.update.bind(this)} 
            defaultValue={this.state.input}></textarea>
          <pre>
            {this.state.output}
          </pre>
        </div>
      </div>
    )
  }
}

export default App;
```


```css
/* src/App.css */
body {
  margin: 0;
  padding: 0;
  font-family: monospace;
}

header {
  display: block;
  height: 5vh;
  background-color: pink;
  color: red;
  font-size: 14px;
  overflow: auto;
}

.container {
  height: 95vh; /* 95% view height */
  display: flex;
}

pre {
  background-color: #f8f8f8;
}

pre, textarea {
  width: 50%;
  font-size: 28px;
  overflow: hidden;
  color: #222;
  white-space: initial; /* auto change line */
}

textarea:focus {
  outline: 0; /* remove outline in OSX system */
}
```

# React Children Utilities


# Simple demo
```jsx
class App extends Component {
  render() {
    return (
      <Parent>
        <div className="childA"></div>
        <div className="childB"></div>
      </Parent>
    )
  }
}

const Parent = (props) => {
  // console.log(props.children);
  // let items = props.children.map(child => child);
  // let items = React.Children.map(props.children, child => child)
  let items = React.Children.toArray(props.children)
  // let items = React.Children.only(props.children)
  console.log(items);
  return null;
}
```
- prop.children.map() will cause error if single child
- React.Children.map solved, single/multi elements always return array
- React.Children.toArray() returns array
- React.Children.only specified a single child, if not, throw error

## Advanced demo - update selected item

use `React.cloneElement` during `React.Children.map()`
Since `React.Children` is read-only, every modification is **NOT** affected to real props.children.

```jsx
// src/App.js
import React, {Component} from 'react';

class App extends Component {
    render() {
        return (
            <Button>
                <button value="A">A</button>
                <button value="B">B</button>
                <button value="C">C</button>
            </Button>
        )
    }
}

class Button extends Component {
    constructor() {
        super()
        this.state = {
            selected: 'None'
        }
    }
    
    selectedItem(selected) {
        this.setState({
            selected
        })
    }

    render() {
        
        // let items = this.props.children
        
        /* map children with callback function 'fn' */
        /* the second parameter specified how to expand children */
        let fn = child =>
            React.cloneElement(child, {
                onClick: this.selectItem.bind(this, child.props.value)
            })
        
        let items = React.Children.map(this.props.children, fn)
    
        return (
            <div>
                <h2>You have selected: {this.state.selected}</h2>
                {items}
            </div>
        )
    }
}

export default App;

```

# Capton - Reusable React Component

With the props and state, we can create reusable component to satisfied with different situation.


```jsx
import React, {Component, PropTypes} from 'react';
import ReactDOM from 'react-dom';

/*
 * change props in NumInput to get a modified <input>
 * min, max, step, type(number, range), label
 * update() bind to <input> onChange event
 */
class App extends Component {
    constructor() {
        super()
        this.state = {
            red: 0
        }
        this.update = this.update.bind(this)
    }
    
    update() {
        this.setState({
            // red: ReactDOM.findDOMNode(this.refs.red.refs.inp).value
            red: this.refs.red.refs.inp.value
        })
    }
    
    render() {
        return (
            <div>
                <NumInput
                    ref="red"
                    max={255}
                    value={+this.state.red}
                    update={this.update}
                    // step={0.01}
                    // type="number"
                    label="RED"
                /> {this.state.red}
            </div>
        )
    }
}

class NumInput extends Component {
    render() {
        let label = this.props.label === '' ? '' :
            <label>{this.props.label} - {this.props.value}</label>
        return (
            <div>
                <input
                    ref="inp"
                    type={this.props.type}
                    min={this.props.min}
                    max={this.props.max}
                    step={this.props.step}
                    defaultValue={this.props.value}
                    onChange={this.props.update}
                />{label}
            </div>
        )
    }
}

/* prop types */
NumInput.propTypes = {
  min: PropTypes.number,
  max: PropTypes.number,
  step: PropTypes.number,
  value: PropTypes.number,
  update: PropTypes.func.isRequired,
  label: PropTypes.string,
  type: PropTypes.oneOf(['range', 'number'])
}

/* default porps */
NumInput.defaultProps = {
  min: 0,
  max: 0,
  step: 1,
  value: 0,
  label: '',
  type: 'range'
}

export default App;

```

# Resources

- [Facebook Tutorial](https://facebook.github.io/react/tutorial/tutorial.html)
- [Start Using React to Build Web Applications](https://egghead.io/courses/react-fundamentals)


# ReactNotes
For learning purposes, I created 'ReactNotes' to describe how I interpret the workings of React and best practices of it.


## React Internals
React under the hood. React Elements, React Components, Virtual DOM.

### Virtual DOM
Reading and writing/updating to the 'real' DOM is a slow process. The Virtual DOM is what makes React fast. The Virtual DOM is an exact copy of the real DOM Tree, which allows React to do computations in this Virtual DOM rather than the 'real' DOM.

The Virtual DOM consist of `React Elements`. However these virtual representations of the DOM are stateless and immutable, so we developers can't work directly with them.

To work as much as possible with the Virtual DOM, we need React components. React components are stateful, and have access to the only way updating the Virtual DOM: Setting state. React converts components to React elements (which are the elements in the Virtual DOM). When a component has changed, changes are made in the Virtual DOM, will then be compared to the DOM and make as little as possible changes.


### Mounting components
The first cycle is the initial render. This cycle starts at the top and goes down the tree to render the tree. React is using `document.createElement` to create DOM elements.  

The second cycle is updating. Things that will update a React component are a call to:

 - setState();
 - forceUpdate();


### JSX
JSX is the templating syntax of React which looks a lot like HTML, but actually produces React Elements. When the components render, React calls it's internal function `React.createElement(component)` which adds the component to the Virtual DOM.

Below is a simple JSX example:


```
const Game = () => (
  <div>
    <h1>Banjo-Kazooie</h1>
  </div>
)
```

This is however not what the browser sees and executes. When the React component renders, the browser executes this:

```
const Game = () => (
    React.createElement('div', null,
      React.createElement('h1', null,
        'Banjo-Kazooie'
      )
    )
)
```


## Immutability
https://gist.github.com/sebmarkbage/07bbe37bc42b6d4aef81

## State

### State Mutation
In React-world never update state directly. Always make a copy of state if you are going to mutate it. This is better for performance.

Incorrect:
`this.state.level = 'Click Clock Wood';`


Correct:
`this.setState({ level: 'Click Clock Wood' });`

### Passing State Object
Never pass your whole in your whole state object, but split it up in smaller parts. This is because you may have more states and you don't want to let React check all your states unnecerssarily. Just pass in a single object:

Incorrect:
`this.setState(level);`


Correct:
`this.setState({ level: level });`


## Components

### Stateful vs Stateless vs Functions
If you don't need other methods other than render, you don't need to create a stateful component. Instead create a stateless component:

```
  const level = (props) => {
    return (
      <div>
        <h1> {props.level.name} </h1>
        <img src={props.level.avatar} />
        <p> {props.level.description} </p>
      </div>
    )
  }
```

Declaration:
```  
  <Level name="Freezeezy Peak" avatar="freezeezypeak.jpg" description="It's very cold here!" />
```

Even better is a plain function, performance-wise:

```
  const level = (props) => {
    return (
      <div>
        <h1> {props.level.name} </h1>
        <img src={props.level.avatar} />
        <p> {props.level.description} </p>
      </div>
    )
  }
```

Declaration:
```
{ Level({ name: "Freeezeezy Peak", avatar:"freezeezypeak.jpg", description:"It's very cold here!" })}
```

Source: https://medium.com/missive-app/45-faster-react-functional-components-now-3509a668e69f

## Events
Events are attached via the component.


```
  class Inazuma extends Component {
    constructor() {
        // Constructor is bound to the react component
        super();
        this.goToPlayer.bind(this);
    }

    showPlayerPage() {
      route.go('/');
    }

    goToPlayer(e) {
      return this.showPlayerPage();
    }

    render() {
      return (
        <button onClick={this.goToPlayer}> </button>
      )
    }
  }
```

## What is 'this'?
All custom class methods are not bound to the actual React component. Those methods need to be bound manually to the component, else this is `null`.

One way is using `bind` in a method that is already bound to the component:

```
  class Player extends Component  {
    constructor() {
        // Constructor is bound to the react component
        super();
        this.fetchPlayerData. = this.fetchPlayerData.bind(this);
    }

    fetchPlayerData() {
      return players.all();
    }

    render() {
      return (
        <button onClick={this.fetchPlayerData}> Fetch all players! </button>
      )
    }
  }
```

An other way to do this is passing an `arrow function` to the components method like this:

```
  class Player extends Component  {

    fetchPlayerData() {
      return players.all();
    }

    render() {
      return (
        <button onClick={() => { this.fetchPlayerData() }}> Fetch all players! </button>
      )
    }
  }
```

Performance-wise, this approach (though not noticable) is not the best one. When you render multiple `<Player />` components, the `fetchPlayerData` method will be created for each component individually.


## DOM
In React querySelecting DOM elements, and for instance setting data in the HTML element, should not happen. With React loading, setting or modifying should happen in state, which should on it's turn render it out in JSX.

By using function `refs` you can create access to the element in custom methods.

When this input is rendered to the page, this `ref` will create a reference on the class itself by the name you attach to the class. This then can be access, in this case in any method with: `this.hissatsuName`. In the console you can use `$r.hissatsuName` to referencw the value.

```
  class Hissatsu extends Component {
    constructor() {
      super();
    }

    render() {
      return(
        <input type='text' required ref={ (input) => {
            // run a function
              this.hissatsuName = input;
            }
          }  
      )
    }
  }
```

### Case Sensitive Renderering
Be aware that it is convention that components always start with a capital letter. When using lowercase, React thinks its HTML and passes a `String` to `React.createElement` instead of passing the Constructor function.

Wrong:
 `<kazooie />` compiles to --> `React.createElement('kazooie');`

Good:
 `<Kazooie />` compiles to --> `React.createElement(Kazooie);`



## Glossary

React:
ReactDOM:
VirtualDOM:
JSX:
Expression container: `{this.props.children}`
Immutability

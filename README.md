# ReactNotes
React Notes describing my best practices.

## State

### Set state
Never update state directly. Always make a copy of state if you are going to mutate it. This is better for performance.


Wrong:
`this.state.level = 'Click Clock Wood';`


Good:
`this.setState({ level: 'Click Clock Wood' });`

### Passing state object
Never pass your whole state. This is because you may have more states and you don't want to let React check everything. Just pass an object.


Wrong:
`this.setState(level);`


Good:
`this.setState({ level: level });`


## Components

### Statefull vs Stateless vs Functions
If you don't need other methods other than render, you don't need to create a statefull component. Instead create a stateless component:

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

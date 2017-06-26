# ReactNotes
React Notes describing my best practices.

## State

### Set state
Never update state directly. Always make a copy of state if you are going to mutate it. This is better for performance.


Wrong:
`this.state.player = 'Mark Evans';`


Good:
`this.setState({ player: 'Mark Evans' });`

### Passing state object
Never pass your whole state. This is because you may have more states and you don't want to let React check everything. Just pass an object.


Wrong:
`this.setState(players);`


Good:
`this.setState({ players: players });`


## Components

### Stateful vs Stateless
If you don't need other methods other than render, you don't need to create a statefull component. Instead create a stateless.

`
const stateless = (props) => {
  return (
    <div> Some JSX </div>
  )
}
`

## Events
Events are attached via the component.


`
  class Inazuma extends Component {
    constructor() {
        // Constructor is bound to the react component
        super();
        this.getPlayers.bind(this);
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
`

## What is this?
All custom class methods are not bound to the actual React component. Those methods need to be bound manually to the component, else this is `null`.

One way is using `bind` in a method that is already bound to the component:

`
  class Inazuma extends Component  {
    constructor() {
        // Constructor is bound to the react component
        super();
        this.fetchPlayerData.bind(this);
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
`


## DOM
In React querySelecting DOM elements, and for instance setting data in the HTML element, should not happen. With React loading, setting or modifying should happen in state, which should on it's turn render it out in JSX.

By using function `refs` you can create access to the element in custom methods.

When this input is rendered to the page, this `ref` will create a reference on the class itself by the name you attach to the class. This then can be access, in this case in any method with: `this.hissatsuName`. In the console you can use `$r.hissatsuName` to referencw the value.

`
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

`

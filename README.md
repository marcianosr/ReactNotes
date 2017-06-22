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

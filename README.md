# Notes -

### props

To access props in class based components -

1. access props by `this.props.name`

```
class User extends Component {
  render() {
    return <li className={classes.user}>{this.props.name}</li>;
  }
}
```

### states

to define states in class based components -

1. create constructor() method
2. add super(); so that super() function can call Component class constructor
3. create state `this.state = {initial_state};`

to update state -

1. `this.setState({more:'new'});`
2. can also update new state from old state -

```
his.setState((curState) => {
      return { showUsers: !curState.showUsers };
    });
```

to use state -

1. access state by - `this.state.showUsers`

```
class Users extends Component {
  constructor() {
    super();
    this.state = {
      showUsers: true,
      more: 'Test',
    };
  }

  toggleUsersHandler() {
    // this.state.showUsers = false; // NOT!
    this.setState((curState) => {
      return { showUsers: !curState.showUsers };
    });
  }

  render() {
    const usersList = (
      <ul>
        {this.props.users.map((user) => (
          <User key={user.id} name={user.name} />
        ))}
      </ul>
    );

    return (
      <div className={classes.users}>
        <button onClick={this.toggleUsersHandler.bind(this)}>
          {this.state.showUsers ? 'Hide' : 'Show'} Users
        </button>
        {this.state.showUsers && usersList}
      </div>
    );
  }
}
```

### Side Effects (useEffect())

In class based components we mainly use 3 side effects methods -

1. componentDidMount() - called once component mounted (was evaluated & rendered)

```
componentDidMount() {
  // Send http request...
  this.setState({ filteredUsers: this.context.users });
}
```

2. componentDidUpdate() - called once component updated (was evaluated & rendered)

```
componentDidUpdate(prevProps, prevState) {
  if (prevState.searchTerm !== this.state.searchTerm) {
    this.setState({
      filteredUsers: this.context.users.filter((user) =>
        user.name.includes(this.state.searchTerm)
      ),
    });
  }
}
```

3. componentWillUnmount() - called right before component is unmounted (removed from DOM)

```
componentWillUnmount() {
  console.log('User will unmount!');
}
```

### Context

To use context in class based component -

1. Create context -

```
const UsersContext = React.createContext({
    users: []
});
```

2. Provide value -

```
import UsersContext from './store/users-context';
function App() {
    return (
    <UsersContext.Provider value={usersContext}>
        <UserFinder />
    </UsersContext.Provider>
    );
}
```

3. consume value -

```
import UsersContext from '../store/users-context';
class UserFinder extends Component {
  static contextType = UsersContext; // consumer

  componentDidMount() {
    // Send http request...
    this.setState({ filteredUsers: this.context.users }); // value
  }

  render() {
    return (
      ...
    );
  }
}
```

remember - class based component can only work with one consumer.

### Error Boundaries

Error Boundaries works exactly as try...catch but on react.

to create Error Boundaries -

1. Create a class based component with `componentDidCatch()` method

componentDidCatch() catches all thrown errors and prevent our application from crashing.

```
class ErrorBoundary extends Component {
  constructor() {
    super();
    this.state = { hasError: false };
  }

  componentDidCatch(error) {
    console.log(error);
    this.setState({ hasError: true });
  }

  render() {
    if (this.state.hasError) {
      return <p>Something went wrong!</p>;
    }
    return this.props.children;
  }
}
```

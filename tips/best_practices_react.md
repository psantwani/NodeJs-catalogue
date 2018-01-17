# Flux
1. Clean way to store and update application's state and trigger rendering when needed.
2. Useful for the app's global states like : managing logged in user, router states, etc.
3. Becomes complex when we start to manage temporary or local data with it. Save that in component's state instead.

# Redux
1. Evolves on the ideas of Flux but avoids its complexities.

# Keep your state flat.
1. Use [normalizr](https://github.com/gaearon/normalizr) to keep state as flat as possible.
```
const data = normalize(response, arrayOf(schema.user))
state = _.merge(state, data.entities)
```

# Use Immutable states.
1. It improves the rendering performance with their reference-level equality checks.
```
shouldComponentUpdate(nexProps) {
 // instead of object deep comparsion
 return this.props.immutableFoo !== nexProps.immutableFoo
}
```
2. [ImmutableJS](https://facebook.github.io/immutable-js/)

# Code splitting, lazy loading
1. The application code can be split into separate JS chunks.
```
require.ensure([], () => {
  const Profile = require('./Profile.js')
  this.setState({
    currentComponent: Profile
  })
})
```
2. Browser doesnt have to download rarely used code after every deploy.
3. More chunks imply more HTTP requests, but [HTTP/2](https://http2.github.io/faq/#why-is-http2-multiplexed) multiplexed will solve this problem.

# PropType
1. Check your properties.
```
MyComponent.propTypes = {
  isLoading: PropTypes.bool.isRequired,
  items: ImmutablePropTypes.listOf(
    ImmutablePropTypes.contains({
      name: PropTypes.string.isRequired,
    })
  ).isRequired
}
```

# Higher Order Components
```PassData({ foo: 'bar' })(MyComponent)```
1. Basically, you compose a new component from your original one and extend its behaviour. 
2. You can use it in various situations like authentication: requireAuth({ role: 'admin' })(MyComponent) (check for a user in higher component and redirect if the user is not logged in) or connecting your component with Flux/Redux store.

# Component Testing
1. [Enzyme](https://github.com/airbnb/enzyme) - it's shallow rendering feature you can test logic and rendering output of your components.
```
it('simulates click events', () => {
  const onButtonClick = sinon.spy()
  const wrapper = shallow(
    <Foo onButtonClick={onButtonClick} />
  )
  wrapper.find('button').simulate('click')
  expect(onButtonClick.calledOnce).to.be.true
})
```

# Testing reducers
1. It responds to the incoming actions and turns the previous state to a new one.
```
it('should set token', () => {
  const nextState = reducer(undefined, {
    type: USER_SET_TOKEN,
    token: 'my-token'
  })

  // immutable.js state output
  expect(nextState.toJS()).to.be.eql({
    token: 'my-token'
  })
})
```

# Testing actions
1. Using [redux-mock-store](https://www.npmjs.com/package/redux-mock-store)
```
it('should dispatch action', (done) => {
  const getState = {}
  const action = { type: 'ADD_TODO' }
  const expectedActions = [action]
 
  const store = mockStore(getState, expectedActions, done)
  store.dispatch(action)
})
```

More here - [Redux Testing](https://redux.js.org/docs/recipes/WritingTests.html)

# Bundle size
1. Require only specific classes from depencies.
```import { concat, sortBy, map, sample } from 'lodash'```
2. Split code to atleast vendors.js and app.js because vendors dont update frequently.
3. More webpack settings [here](https://survivejs.com/webpack/introduction/).
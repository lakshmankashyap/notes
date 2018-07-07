# Tests

### Types of test

- Unit tests : 
  - Involves breaking your program into pieces, and subjecting each piece to a series of tests.
  - 90% of the tests are usually unit tests.
  - We should separate our components into clean small chunks in order to make unit tests easier.
- Integration tests :
  - It's all about cross-communication between different units of code
  - We can use spies and stubs to mock and modify parts of a process (mock database call, ...)
  - They can really be fragile and harder to write
  - Always start with unit tests and then integration tests
- Automation tests (UI Tests) : 
  - This can also be called end-to-end testing
  - test real life scenarios on the browser by controlling the browser and making sure that the expected behavior on the web is correct. Humans can do this or robots.
  - They are the hardest to set up
  - Tools : Nightmare, cypress, TestCafé, ...

### Libraries

<img src="libraries.png" width="500px"/>

`create-react-app` has Jest already set up.

We can also use [enzyme](http://airbnb.io/enzyme/).

### Workflow

<img src="workflow.png" width="500px"/>

### Writing tests

**package.json**

```js
{
  ...
  "scripts": {
    "test": "jest --watch *.js"
  },
  ...
  "devDependencies": {
    "jest": "^23.2.0"
  }
  ...
}
```

#### Synchronous

**script.js**

```js
const googleDatabase = [
  'cats.com',
  'souprecipes.com',
  'flowers.com',
  'animals.com',
  'catpictures.com',
  'myfavouritecats.com',
  'myfavouritecats2.com'
];

// The db parameter makes the googleSearch function more extensible
const googleSearch = (searchInput, db) => {
  const matches =  db.filter(website => website.includes(searchInput));
  return matches.length > 3 ? matches.slice(0, 3) : matches
}

// We don't have babel or webpack so we'll use require
module.exports = googleSearch;
```

**script.test.js**

```js
// We don't have babel or webpack so we'll use require
const googleSearch = require('./script')

// Pretend that we have a database
let dbMock = [
  'dog.com',
  'cheesepuff.com',
  'disney.com',
  'dogpictures.com'
];

describe('googleSearch', () => {
  it('is searching google', () => {
    expect(googleSearch('testtest', dbMock)).toEqual([]);
    expect(googleSearch('dog', dbMock)).toEqual(['dog.com', 'dogpictures.com']);
  })
  
  it('work with undefined and null input', () => {
    expect(googleSearch(undefined, dbMock)).toEqual([]);
    expect(googleSearch(null, dbMock)).toEqual([]);
  })
  
  it('does not return more than 3 matches', () => {
    expect(dbMock.length).toBeGreaterThan(3)
    expect(googleSearch('.com', dbMock).length).toEqual(3);
  })  
})
```

#### Asynchronous

**script2.js**

```js
const fetch = require('node-fetch');

// We use dependency injection here
// One of the major advantages of dependency injection is that it can make testing lots easier. 
const getPeoplePromise = fetch => {
  return fetch('https://swapi.co/api/people')
    .then(response => response.json())
    .then(data => {
      return {
        count: data.count,
        results: data.results
      };
    })
}

// Async await version

const getPeople = async fetch => {
  const getRequest = await fetch('https://swapi.co/api/people');
  const data = await getRequest.json();
  return {
    count: data.count,
    results: data.results
  };
}

module.exports = {
  getPeoplePromise,
  getPeople
};
```

**script2.test.js**

```js
const fetch = require('node-fetch');
const swapi = require('./script2');

// The done tells jest to wait until the done callback is called
it('calls swapi to get people', (done) => {
  expect.assertions(1); // Won't work without the done parameter or return statement
  swapi.getPeople(fetch).then(data => {
    expect(data.count).toEqual(87);
    done();
  })
})

// OR we can just use return
it('calls swapi to get people', () => {
  expect.assertions(1);
  return swapi.getPeople(fetch).then(data => {
    expect(data.count).toEqual(87);
  })
})

it('calls swapi to get people with a promise', () => {
  // It’s a good practice to specify a number of expected assertions in async tests, 
  // so the test will fail if your assertions weren’t called at all.
  expect.assertions(2);
  return swapi.getPeoplePromise(fetch).then(data => {
    expect(data.count).toEqual(87);
    expect(data.results.length).toBeGreaterThan(2);
  })
})

// mock the fetch call to avoid long running asynchronous calls
it('getPeople returns count and results', () => {
  const mockFetch = jest.fn()
    .mockReturnValue(Promise.resolve({ // We're faking asynchronous call
      json: () => Promise.resolve({
        count: 87,
        results: [0, 1, 2, 3, 4, 5]
      })
    }))

    expect.assertions(4);
    return swapi.getPeoplePromise(mockFetch).then(data => {
      expect(mockFetch.mock.calls.length).toBe(1);
      expect(mockFetch).toBeCalledWith('https://swapi.co/api/people');
      expect(data.count).toEqual(87);
      expect(data.results.length).toBeGreaterThan(2);
    })
})
```


### Testing React components

**setupTests.js**

```js
import { configure } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';

configure({ adapter: new Adapter() });
```

**Scroll**

```js
const Scroll = (props) => {
  return (
    <div style={{ overflow: 'scroll', border: '5px solid black', height: '800px'}}>
      {props.children}
    </div>
  );
};
```

```js
it('expect to render Scroll component', () => {
  expect(shallow(<Scroll />)).toMatchSnapshot();
})
```

**CounterButton**

```js
class CounterButton extends Component {
  constructor(props) {
    super(props);
    this.state = {count: 0};
  }

  shouldComponentUpdate(nextProps, nextState) {
    if (this.state.count !== nextState.count) {
      return true;
    }
    return false;
  }

  render() {
    return (
      <button
        id='counter'
        color={this.props.color}
        onClick={() => this.setState(state => ({count: state.count + 1}))}>
        Count: {this.state.count}
      </button>
    );
  }
}
```

```js
it('expect to render CounterButton component', () => {
  const mockColor = 'red';
  expect(shallow(<CounterButton color={mockColor} />)).toMatchSnapshot();
})

it('correctly increments the counter', () => {
  const mockColor = 'red';
  const wrapper = shallow(<CounterButton color={mockColor}/>);

  expect(wrapper.state()).toEqual({ count: 0 });
  wrapper.find('[id="counter"]').simulate('click');
  expect(wrapper.state()).toEqual({ count: 1 });
  expect(wrapper.props().color).toEqual('red');
})
```

**MainPage**

```js
class MainPage extends Component {
  componentDidMount() {
    this.props.onRequestRobots();
  }

  filterRobots = robots => {
    return this.props.robots.filter(robot => {
      return robot.name.toLowerCase().includes(this.props.searchField.toLowerCase());
    })
  }

  render() {
    const { onSearchChange, isPending } = this.props;

    return (
      <div className='tc'>
        <Header />
        <SearchBox searchChange={onSearchChange}/>
        <Scroll>
          { isPending ? <h1>Loading</h1> :
            <ErrorBoundry>
              <CardList robots={this.filterRobots(this.props.robots)} />
            </ErrorBoundry>
          }
        </Scroll>
      </div>
    );
  }
}
```

```js
let wrapper;

beforeEach(() => {
  const mockProps = {
    onRequestRobots: jest.fn(),
    robots: [],
    searchField: '',
    isPending: false
  }

  wrapper = shallow(<MainPage { ...mockProps }/>)
})

it('renders MainPage without crashing', () => {
  expect(wrapper).toMatchSnapshot();
})

it('filters robots correctly', () => {
  const mockProps2 = {
    onRequestRobots: jest.fn(),
    robots: [{
      id: 3,
      name: 'John',
      email: 'john@gmail.com'
    }],
    searchField: 'john',
    isPending: false
  }

  const wrapper2 = shallow(<MainPage {...mockProps2}/>)

  expect(wrapper2.instance().filterRobots()).toEqual([{
    id: 3,
    name: 'John',
    email: 'john@gmail.com'
  }])
})

it('filters robots correctly 2', () => {
  const mockProps3 = {
    onRequestRobots: jest.fn(),
    robots: [{
      id: 3,
      name: 'John',
      email: 'john@gmail.com'
    }],
    searchField: 'a',
    isPending: false
  }

  const filteredRobots = []
  const wrapper3 = shallow(<MainPage {...mockProps3}/>)

  expect(wrapper3.instance().filterRobots()).toEqual(filteredRobots)
})

it('returns loading if is pending', () => {
  const mockProps4 = {
    onRequestRobots: jest.fn(),
    robots: [],
    isPending: true
  }

  const wrapper4 = shallow(<MainPage {...mockProps4}/>)

  expect(wrapper4.html()).toContain('<h1>Loading</h1>')
})
```

### Testing Redux reducers & actions

**Actions**

```js
export const setSearchField = (text) => ({ type: CHANGE_SEARCHFIELD, payload: text })

// Thunk middleware expects a function
export const requestRobots = () => (dispatch) => {
  dispatch({ type: REQUEST_ROBOTS_PENDING })
  apiCall('https://jsonplaceholder.typicode.com/users')
    .then(data => dispatch({ type: REQUEST_ROBOTS_SUCCESS, payload: data }))
    .catch(error => dispatch({ type: REQUEST_ROBOTS_FAILED, payload: error }))
}
```

```js
// Create fake store
 const mockStore = configureMockStore([thunkMiddleware]);

 it('should create an action to search robots', () => {
   const text = 'wooo';
   const expectedAction = {
     type: CHANGE_SEARCHFIELD,
     payload: text
   }
   expect(actions.setSearchField(text)).toEqual(expectedAction)
 })

 it('handles requesting robots API', async () => {
   // Mock API call
   nock('https://jsonplaceholder.typicode.com')
     .get('/users')
     .reply(200, {
       foo: 'bar',
     })
     
  const store = mockStore();
  await store.dispatch(actions.requestRobots());
  const action = store.getActions();
  const expectedAction = {  
    type: REQUEST_ROBOTS_PENDING
  }

  const expectedAction2 = {
    type: REQUEST_ROBOTS_SUCCESS,
    payload: {
      foo: 'bar'
    }
  }
  expect(action[0]).toEqual(expectedAction);
  expect(action[1]).toEqual(expectedAction2);
 })
 
 it('handles requesting robots API when it fails', async () => {
   // Mock API call
   nock('https://jsonplaceholder.typicode.com')
     .get('/users')
     .replyWithError({message: 'oh boy!'})

   const store = mockStore();
   await store.dispatch(actions.requestRobots());
   const action = store.getActions();
   const expectedAction = {
     type: REQUEST_ROBOTS_PENDING
   }

   expect(action[0]).toEqual(expectedAction);
   expect(action[1].type).toEqual(REQUEST_ROBOTS_FAILED); // Didn't find a way to test the error message
 })
```

**Reducers**

```js
const initialStateSearch = {
  searchField: ''
}

export const searchRobots = (state=initialStateSearch, action={}) => {
  switch (action.type) {
    case CHANGE_SEARCHFIELD:
      return Object.assign({}, state, {searchField: action.payload})
    default:
      return state
  }
}

const initialStateRobots = {
  robots: [],
  isPending: false
}

export const requestRobots = (state=initialStateRobots, action={}) => {
  switch (action.type) {
    case REQUEST_ROBOTS_PENDING:
      return Object.assign({}, state, {isPending: true})
    case REQUEST_ROBOTS_SUCCESS:
      return Object.assign({}, state, {robots: action.payload, isPending: false})
    case REQUEST_ROBOTS_FAILED:
      return Object.assign({}, state, {error: action.payload})
    default:
      return state
  }
}
```

```js
 describe('searchRobots', () => {
   const initialStateSearch = {
     searchField: ''
   }

   it('should return the initial state', () => {
     // No state at the beginning
     expect(reducers.searchRobots(undefined, {})).toEqual(initialStateSearch)
   })

   it('should handle the CHANGE_SEARCHFIELD', () => {
    expect(reducers.searchRobots(initialStateSearch, {
      type: CHANGE_SEARCHFIELD,
      payload: 'abc'
    })).toEqual({ searchField: "abc" })
   })
 })


describe('requestRobots', () => {
  const initialStateRobots = {
    robots: [],
    isPending: false
  }

  it('should return the initial state', () => {
    // No state at the beginning
    expect(reducers.requestRobots(undefined, {})).toEqual(initialStateRobots)
  })

  it('should handle REQUEST_ROBOT_PENDING', () => {
    expect(reducers.requestRobots(initialStateRobots, {
      type: REQUEST_ROBOTS_PENDING
    })).toEqual({
      robots: [],
      isPending: true
    })
  })

  it('should handle REQUEST_ROBOT_SUCCESS', () => {
    expect(reducers.requestRobots(initialStateRobots, {
      type: REQUEST_ROBOTS_SUCCESS,
      payload: [{
        id: '123',
        name: 'test',
        email: 'test@gmail.com'
      }]
    })).toEqual({
      robots: [{
        id: '123',
        name: 'test',
        email: 'test@gmail.com'
      }],
      isPending: false
    })
  })

  it('should handle REQUEST_ROBOT_FAILED', () => {
    expect(reducers.requestRobots(initialStateRobots, {
      type: REQUEST_ROBOTS_FAILED,
      payload: 'NOOOO!'
    })).toEqual({
      error: 'NOOOO!',
      isPending: false,
      robots: []
    })
  })
})
```

### Additional resources

- [Nock](https://github.com/nock/nock)
- [Supertest](https://github.com/visionmedia/supertest)

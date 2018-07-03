# Tests

## Types of test

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

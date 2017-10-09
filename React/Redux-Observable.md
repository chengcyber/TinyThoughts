# Add to Redux

```
yarn add redux-observable rxjs
```

apply epic middleware to redux store

```javascript
// index.js
import { createStore, applyMiddleware } from 'redux'
import { createEpicMiddleware } from 'redux-observable'
import rootEpic from './epics'

const epicMiddleware = createEpicMiddleware(rootEpic)

const store = createStore(reducer, applyMiddleware(epicMiddleware)）
```

assume we trigger click event and dispatch an action contains `type: LOAD_STORIES`

```javascript
// epics/index.js
import { Observable } from 'rxjs';
import { combineEpics } from 'redux-observable';

function loadStoriesEpic(action$) {
    return action$
        .do(action => console.log(action)) // logout the raw action object
        .ignoreElements();
}

export const rootEpic = combineEpics(loadStoriesEpic, /* other epics */);

```

# ofType

Now, we want our epic trigger only when some specified actions

```javascript
import { LOAD_STORIES } from '../actions/index';

function loadStoriesEpic(action$) {
    return action$
        .filter(action => action.type === LOAD_STORIES)
        .do(action => console.log(action)) // logout the raw action object
        .ignoreElements();
}
```

```javascript
...
function loadStoriesEpic(action$) {
    return action$.ofType(LOAD_STORIES)
        .do(action => console.log(action)) // logout the raw action object
        .ignoreElements();
}

```

# switchMap

trigger another side effect action in observable

```javascript
import { Observable } from 'rxjs'
import { clear, LOAD_STORIES } from '../actions/index'

function loadStoriesEpic(action$) {
    return action$.ofType(LOAD_STORIES)
        .switchMap(() => {
            return Observable.of(clear()).delay(2000);
        })
}
```

now, we can dispatch a `__request` action, then invoke ajax fetch restful resource, and then dispatch a `__success` or `__failure` action.

i.e. we dispatch a `loadUser`, after data fetched dispatch `loadUserFulfilled`

```javascript
import { Observable } from 'rxjs';
import { FETCH_USER, fetchUserFulfilledAction } from '../actions/index';

function fetchUserEpic(action$) {
    return action$.ofType(FETCH_USER)
        .switchMap(({ payload}) => {
            return Observable.ajax.getJSON(`https://api.github.com/users/${payload}`);
            })
            .map( user => {
                return fetchUserFilfilledAction(user);
            });
        }
}

export const rootEpic = combineEpics(fetchUserEpic)
```

# mergeMap and forkJoin

1. fetch a ids array from HackerNews
2. slice the first five ids
3. map ids to urls
4. urls map to 5 Observable array
5. using `mergeMap` and `forkJoin` trigger 5 requests
6. dispatch action with results

```javascript
import {Observable} from 'rxjs';
import {combineEpics} from 'redux-observable';
import {FETCH_STORIES, fetchStoriesFulfilledAction} from "../actions/index";

const topStories = `https://hacker-news.firebaseio.com/v0/topstories.json?print=pretty`;
const url = (id) => `https://hacker-news.firebaseio.com/v0/item/${id}.json?print=pretty`;

function fetchStoriesEpic(action$) {
  return action$.ofType(FETCH_STORIES)
    .switchMap(({payload}) => {
      return Observable.ajax.getJSON(topStories)
        // slice first 5 ids
        .map(ids => ids.slice(0, 5))
        // convert ids -> urls
        .map(ids => ids.map(url))
        // convert urls -> ajax
        .map(urls => urls.map(url => Observable.ajax.getJSON(url)))
        // execute 5 ajax requests
        .mergeMap(reqs => Observable.forkJoin(reqs))
        // results -> store
        .map(stories => fetchStoriesFulfilledAction(stories))
    })
}

export const rootEpic = combineEpics(fetchStoriesEpic);
```

# debounceTime

debounce input to avoid repeated Ajax requests in a period of time


```javascript
import {Observable} from 'rxjs';
import {combineEpics} from 'redux-observable';
import {receiveBeers, SEARCHED_BEERS} from "../actions/index";

const beers  = `https://api.punkapi.com/v2/beers`;
const search = (term) => `${beers}?beer_name=${encodeURIComponent(term)}`;
const ajax   = (term) => Observable.ajax.getJSON(search(term));

function searchBeersEpic(action$) {
  return action$.ofType(SEARCHED_BEERS)
    .debounceTime(500)
    .switchMap(({payload}) => {
      return ajax(payload)
        .map(receiveBeers)
    })
}

export const rootEpic = combineEpics(searchBeersEpic);
```

# Error handling

```javascript
function searchBeersEpic(action$) {
  return action$.ofType(SEARCHED_BEERS)
    .debounceTime(500)
    .switchMap(({payload}) => {
      return ajax(payload)
        .map(receiveBeers)
        .catch(err => {
          return Observable.of(searchBeersError(err));
        })
    })
}
```

# Filter

filter the side effect only happened in some case, such as empty string value


```javascript
function searchBeersEpic(action$) {
  return action$.ofType(SEARCHED_BEERS)
    .debounceTime(500)
    .filter(action => action.payload !== '')
    .switchMap(({payload}) => {

      // loading state in UI
      const loading = Observable.of(searchBeersLoading(true));

      // external API call
      const request = ajax(payload)
        .map(receiveBeers)
        .catch(err => {
          return Observable.of(searchBeersError(err));
        });

      return Observable.concat(
        loading,
        request,
      );
    })
}
```

notice the request action is dispatched, which means loading set to true in our reducer, and no other event to set it back. So, we must move the logic to set/unset loading in our `switchMap` block.

we add a new action only concern the loading state, and generate two individual observable in our switchMap, and concat them. thus whenever the switchMap will trigger these two observables simultaneously.

# Cancel an Ajax request


```javascript
function searchBeersEpic(action$) {
  return action$.ofType(SEARCHED_BEERS)
    .debounceTime(500)
    .filter(action => action.payload !== '')
    .switchMap(({payload}) => {

      // loading state in UI
      const loading = Observable.of(searchBeersLoading(true));

      // external API call
      const request = ajax(payload)
        .takeUntil(action$.ofType(CANCEL_SEARCH)) /* here */
        .map(receiveBeers)
        .catch(err => {
          return Observable.of(searchBeersError(err));
        });

      return Observable.concat(
        loading,
        request,
      );
    })
}
```

# Testing

declaration programming test

```javascript
// fetch-user-epic.test.js
import { Observable } from 'rxjs';
import { ActionsObservable } from 'redux-observable';

it('should return correct actions', function() {
    const action$ = ActionsObservable.of({
        type: 'FETCH_USER',
        payload: 'someone',
    });
    
    const output$ = fetchUserEpic(action$);
    output$.toArray().subscribe(actions => {
        expect(actions.length).toBe(1);
    });
});
```

mock an Ajax request
here is the issue, the test run in a node environment, but ajax need call XHR object, so it will failed, the solution to it is pass the ajax dependency as part of parameters and mock an `ajax.getJSON` for testing as well.

```javascript
import { ActionsObservable } from 'redux-observable';
import { searchBeers } from '../actions/index';
import { searchBeersEpic } from './index.js';

it('should perform a search', function() {
    // mock input
    const action$ = ActionsObservable.of(searchBeers('shane'));
  
    // mock ajax.getJSON
    const deps = {
        ajax: {
            getJSON: () => Observable.of([{ name: 'shane', }]),
        }
    }
      
    // mock output, pass fake ajax.getJSON as third params
    const output$ = searchBeersEpic(action$, null, deps);
    
    output$.toArray().subscribe(actions => {
        expect(actions.length).toBe(2);
        
        expect(actions[0].type).toBe(SEARCHED_BEERS_LOADING);
        expect(actions[1].type).toBe(RECEIVED_BEERS);
    });
});
```

accordingly, `searchBeersEpic` should have some changes

```javascript
import {Observable} from 'rxjs';
import {combineEpics} from 'redux-observable';
import {CANCEL_SEARCH, receiveBeers, searchBeersError, searchBeersLoading, SEARCHED_BEERS} from "../actions/index";

const beers  = `https://api.punkapi.com/v2/beers`;
const search = (term) => `${beers}?beer_name=${encodeURIComponent(term)}`;

export function searchBeersEpic(action$, store, deps) {
  return action$.ofType(SEARCHED_BEERS)
    .debounceTime(500)
    .filter(action => action.payload !== '')
    .switchMap(({payload}) => {

      // loading state in UI
      const loading = Observable.of(searchBeersLoading(true));

      // external API call
      const request = deps.ajax.getJSON(search(payload))
        .takeUntil(action$.ofType(CANCEL_SEARCH))
        .map(receiveBeers)
        .catch(err => {
          return Observable.of(searchBeersError(err));
        });

      return Observable.concat(
        loading,
        request,
      );
    })
}
```

now, the test works fine, but our app should be broken since the deps.
we can setup the third argument in `createEpicMiddleware` function.

```javascript
// index.js
import { ajax } from 'rxjs/observable/dom/ajax';
...

const epicMiddleware = createEpicMiddleware(rootEpic, {
    dependencies: {
        ajax,
    }
});

```

# Virtual Time Scheduler

the origin to this, is testing the blocks have something that introduces time like `debounceTime(500)`.
for sure, you can comment it and uncomment after testing, but it's more elegant to use `virtual time scheduler` to solve this issue.


```javascript
// index.test.js
import { VirtualTimeScheduler } from 'rxjs/scheduler/VirtualTimeScheduler';

...

    const scheduler = new VirtualTimeScheduler();
    const deps {
        scheduler,
        ajax: {
            ...
    }
```

```javascript
// epic.js
export function searchBeersEpic(action$, store, deps) {
    return action$.ofType(SEARCHED_BEER)
    
        /* pass as the last parameter, and it's undefined when production env */
        .debounceTime(500, deps.scheduler) 
        
        ...
}
```

# Resources

- [shakyShane/egghead-redux-obs](https://github.com/shakyShane/egghead-redux-obs)


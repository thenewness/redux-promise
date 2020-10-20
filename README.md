# NEWNESS FORK

We forked this repository because one of its dependencies, flux-standard-action, imports all of lodash instead of only the functions it needs.

## New releases

To build a new release, run `yarn build`, update the `version` field in `package.json`, and merge.

# Looking for Maintainers

**Unfortunately I ([timche](https://github.com/timche)) don't have the required time anymore to maintain this library and give it the necessary attention. Therefore I'm looking for maintainers that are willing to take care of this library on a long-term basis.**

Requirements:

* Having knowledge of this library and open-source in general.
* Keeping the philosophy and goals of this library.
* Taking care of issues and pull requests.
* If required and reasonable, working out next versions of this library with the intention to improve it with the community in mind and not for the sole purpose.
* Knowing what's good for the library and what not (e.g. not accepting every suggestion) in order to maintain the library scope.
* Having knowledge about the tooling (CI, build system, etc.) and the docs and maintaining them.

It's also possible to join [redux-utilities](https://github.com/redux-utilities), an umbrella organization of complementing redux utility libraries like this one, to take care of few or all libraries. Please let me know if you are interested in that.

Please send me an email (adress on my profile) with the subject "redux-promise" and some information about you, if you want to be a maintainer.

# redux-promise

[![build status](https://img.shields.io/travis/redux-utilities/redux-promise/master.svg)](https://travis-ci.org/redux-utilities/redux-promise)
[![codecov](https://codecov.io/gh/redux-utilities/redux-promise/branch/master/graph/badge.svg)](https://codecov.io/gh/redux-utilities/redux-promise)
[![npm version](https://img.shields.io/npm/v/redux-promise.svg)](https://www.npmjs.com/package/redux-promise)
[![npm monthly downloads](https://img.shields.io/npm/dm/redux-promise.svg)](https://www.npmjs.com/package/redux-promise)

[FSA](https://github.com/redux-utilities/flux-standard-action)-compliant promise [middleware](https://redux.js.org/advanced/middleware) for Redux.

```js
npm install --save redux-promise
```

## Usage

```js
import promiseMiddleware from 'redux-promise';
```

The default export is a middleware function. If it receives a promise, it will dispatch the resolved value of the promise. It will not dispatch anything if the promise rejects.

If it receives an Flux Standard Action whose `payload` is a promise, it will either

* dispatch a copy of the action with the resolved value of the promise, and set `status` to `success`.
* dispatch a copy of the action with the rejected value of the promise, and set `status` to `error`.

The middleware returns a promise to the caller so that it can wait for the operation to finish before continuing. This is especially useful for server-side rendering. If you find that a promise is not being returned, ensure that all middleware before it in the chain is also returning its `next()` call to the caller.

## Using in combination with redux-actions

Because it supports FSA actions, you can use redux-promise in combination with [redux-actions](https://github.com/redux-utilities/redux-actions).

### Example: Async action creators

This works just like in Flummox:

```js
createAction('FETCH_THING', async id => {
  const result = await somePromise;
  return result.someValue;
});
```

Unlike Flummox, it will not perform a dispatch at the beginning of the operation, only at the end. We're still looking into the [best way to deal with optimistic updates](https://github.com/redux-utilities/flux-standard-action/issues/7). If you have a suggestion, let me know.

### Example: Integrating with a web API module

Say you have an API module that sends requests to a server. This is a common pattern in Flux apps. Assuming your module supports promises, it's really easy to create action creators that wrap around your API:

```js
import { WebAPI } from '../utils/WebAPI';

export const getThing = createAction('GET_THING', WebAPI.getThing);
export const createThing = createAction('POST_THING', WebAPI.createThing);
export const updateThing = createAction('UPDATE_THING', WebAPI.updateThing);
export const deleteThing = createAction('DELETE_THING', WebAPI.deleteThing);
```

(You'll probably notice how this could be simplified even further using something like lodash's `mapValues()`.)

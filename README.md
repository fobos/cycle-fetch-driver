# Cycle HTTP Driver

A [Cycle.js](http://cycle.js.org) [Driver](http://cycle.js.org/drivers.html) for making HTTP requests, using the [Fetch API](https://fetch.spec.whatwg.org/).

```
npm install cycle-fetch-driver
```

[![npm version](https://badge.fury.io/js/cycle-fetch-driver.svg)](http://badge.fury.io/js/cycle-fetch-driver)

## Usage

Basics:

```js
import Cycle from '@cycle/core';
import { makeFetchDriver } from 'cycle-fetch-driver';

function main(responses) {
  // ...
}

const drivers = {
  Fetch: makeFetchDriver()
}

Cycle.run(main, drivers);
```

Simple and normal use case:

```js
function main(responses) {
  const HELLO_URL = 'http://localhost:8080/hello';
  let request$ = Rx.Observable.just(HELLO_URL);
  let vtree$ = responses.Fetch
    .byUrl(HELLO_URL)
    .flatMap(res => res.text()) // We expect this to be "Hello World"
    .startWith('Loading...')
    .map(text =>
      h('div.container', [
        h('h1', text)
      ])
    );

  return {
    DOM: vtree$,
    Fetch: request$
  };
}
```

Select all the responses for a certain key:

```js
function main(responses) {
  const HELLO_URL = 'http://localhost:8080/hello';
  let request$ = Rx.Observable.just({
    key: 'hello',
    url: HELLO_URL
  );
  let vtree$ = responses.Fetch
    .byKey('hello')
    .flatMap(res => res.text()) // We expect this to be "Hello World"
    .startWith('Loading...')
    .map(text =>
      h('div.container', [
        h('h1', text)
      ])
    );

  return {
    DOM: vtree$,
    Fetch: request$
  };
}
```
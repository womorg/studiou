# @womorg/studiou

Tiny JSON fetch wrapper library. ~1.7kb gzipped.

[![npm version](https://img.shields.io/npm/v/@womorg/studiou.svg)](https://npm.im/@womorg/studiou) [![ko-fi](https://img.shields.io/badge/donate-KoFi-yellow.svg)](https://ko-fi.com/zacanger)

----

`@womorg/studiou` is a small fetch wrapper library that always parses JSON and returns
JS. Smaller than Axios, Request, R2, and the `whatwg-fetch` polyfill itself.

# Installation

`npm i @womorg/studiou`

# Usage

```javascript
import { get } from '@womorg/studiou'

get('/foo')
```

## Methods

* `del`
* `get`
* `patch`
* `post`
* `put`

This only provides functions for these common HTTP methods, but you can easily add
your own. Check out the source for notes on how to use `sendJson` directly.

The return value is always a simple response of type

```typescript
type SimpleResponse<T> = {
  ok: boolean
  status: number
  headers: Headers
  body: T
}
```

## Examples

Node:

```javascript
require('isomorphic-fetch') // brings in fetch for Node

import * as f from '@womorg/studiou'

// some koa route
router.get('/foo/:id', async (ctx) => {
  try {
    const thing = await f.get(`/some-service/${id}`)
    ctx.type = 'application/json'
    ctx.body = thing
  } catch (e) {
    someLogger.error(e)
    ctx.status = 500
    ctx.body = e
  }
})
```

Browser:

```javascript
import * as React from 'react'
import { post } from '@womorg/studiou'

class Foo extends React.Component {
  state = { things: null }

  submitThings = () => {
    post('/stuff', { body: this.state.things })
    .then((res) => {
      if (res) {
        alert(res)
      }
    })
    .catch((err) => {
      someErrorHandler(err)
    })
  }

  setThings = (e) => {
    this.setState({ things: e.target.value })
  }

  render () {
    return (
      <React.Fragment>
        <input
          type="text"
          onChange={this.setThings}
          value={this.state.things}
        />
        <button onClick={submitThings}>
          Send the things!
        </button>
      </React.Fragment>
    )
  }
}
```

Adding headers:

```javascript
import { post } from '@womorg/studiou'

post('/foo', {
  body: someObject,
  headers: {
    'x-foo-bar': 'baz',
  }
})
```

## Environment

This library assumes `fetch` is available. You may need to polyfill it!

[LICENSE](./LICENSE.md)

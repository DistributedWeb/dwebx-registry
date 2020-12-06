# dwebx-registry

[![Build Status](https://travis-ci.org/datproject/dwebx-registry-client.svg?branch=master)](https://travis-ci.org/datproject/dwebx-registry-client)

API Registry Client for publishing dvaults. By default, the client is capable of registering, login, and publishing to dwebx.org.

`dwebx-registry` allows users to interact with and publish dvaults to your registry via the `dwebx` command line. Supporting this module on your registry will allow a user to login and publish:

```
dwebx login custom-dwebx-registry.com
dwebx publish
```

## Installation

```
npm install dwebx-registry
```

### Quick Example

```js
var Registry = require('dwebx-registry')

var registry = Registry()

registry.login({email: 'karissa', password: 'my passw0rd r0cks!'}, function () {
  registry.dvaults.create({
    name: 'animal-names',
    url: 'dwebx://378d23adf22df',
    title: 'Animal Names',
    description: 'I did a study on animals for a very important Nature study, here are the spreadsheets with raw animals in them.'
  }, function (err, resp, json) {
    if (err) throw err
    if (resp.statusCode === 400) console.error(data.message)
    console.log('Published successfully!')
    // Created a nickname for a dwebx at `https://dwebx.org/karissa/animal-names`
  })
})
```

### API

#### `var registry = Registry([opts])`

  * `opts.server`: the registry server. Default is `https://dwebx.org`
  * `opts.apiPath`: registery server API path, e.g. we use `/api/v1` for dwebx.org. This will overwrite default township routes to use server + apiPath.
  * `opts.config.filename`: defaults to `~.dwebxrc` instead of township defaults.

Other options are passed to [township-client](https://github.com/township/township-client), these include:

```js
opts = {
  config: {
    filepath: '~/.townshiprc' // specify a full config file path 
  },
  routes: { // routes for ALL township servers used by client
    register: '/register',
    login: '/login',
    updatePassword: '/updatepassword'
  }
}
```

#### `registry.login(data, cb)`

Requires `data.email` and `data.password`.

#### `registry.register(data, cb)`

Requires `data.username`, `data.email`, and `data.password`.

#### `registry.logout(cb)`

Will callback with logout success or failure.

#### `var user = registry.whoami([opts])`

Returns user object with currently logged in user. See `township-client` for options.

### CRUD API

#### `registry.dvaults.create(data, cb)`

Must be logged in. Requires a unique `data.name` and unique `data.url`. DWebX will be immediately available on the `/:username/:name`.

Accepts also any fields in a `dwebx.json` file.

#### `registry.dvaults.get([data], cb)`

Returns all dvaults that match the given (optional) querystrings.

#### `registry.dvaults.update(data, cb)`
#### `registry.dvaults.delete(data, cb)`
#### `registry.users.get([data], cb)`
#### `registry.users.update(data, cb)`
#### `registry.users.delete(data, cb)`

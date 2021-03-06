# ssb-client v2

[Scuttlebot](https://github.com/ssbc/scuttlebot) client. 

Create an rpc connection to an sbot running locally. 

## example

```js
var ssbClient = require('ssb-client')
var ssbKeys = require('ssb-keys')

// simplest usage, connect to localhost sbot
// this will cb with an error if an sbot server is not running
ssbClient(function (err, sbot) {
  // ...
})

// configuration:
var keys = ssbKeys.loadOrCreateSync('./app-private.key')
ssbClient(
  keys,                // optional, defaults to ~/.ssb/secret
  {
    host: 'localhost', // optional, defaults to localhost
    port: 8008,        // optional, defaults to 8008
    key: keys.id,      // optional, defaults to keys.id

    caps: {
        // random string for `appKey` in secret-handshake
        shs: ''
    },

    // optional, muxrpc manifest. Defaults to ~/.ssb/manifest.json
    manifest: {}       

  },
  function (err, sbot, config) {
    // ...
  }
)
```

* Tutorials using this library to create basic clients: [ssb-client-basic](https://github.com/mixmix/ssb-client-basic)
* A simple command line wrapper around this library: [ssb-client-cli](https://github.com/qypea/ssb-client-cli)

## api

### require('ssb-client') => createEasyClient

#### createEasyClient(cb(err, sbot))

create a connection to the local `ssb-server` instance, using the default keys.
configuration and keys will be loaded from directory specified by `ssb_appname`.
(by default `~/.ssb`)

The manifest will be the manifest provided by that server.

Calling this without arguments is handy for scripts, but applications
should use the clearer apis.

there is a legacy api, that makes things as "easy" as possible,
by loading configuration and defaults. This is useful for scripts
but applications should probably use 

##### createEasyClient(keys, opts, cb(err, sbot))

connect to a client with some custom settings.

opts supports the keys:

* `remote` multiserver address to connect to
* `host, port, key` (legacy) if remote is not set, assemble address from host, port, key.
* `manifest` use a custom manifest.

it's recommended to use the `createClient` api instead, but this is still supported
for legacy support.

### require('ssb-client/client') => createClient

### createClient({keys, config, manifest, remote}, cb)

connect to a specific server with fixed settings. All fields are mandatory.



### keys
See [ssb-keys](https://github.com/ssbc/ssb-keys). The keys look like this:
```js
{
    id: String,
    public: String,
    private: String,
    curve: 'ed25519'
}
```

### caps
`caps.shs` is a random string passed to [secret-handshake](https://github.com/auditdrivencrypto/secret-handshake#example). It determines which sbot you are able to connect to. It defaults to a magic string in this repo and also in [scuttlebot](https://github.com/ssbc/scuttlebot/blob/master/lib/ssb-cap.js)

```js
var appKey = new Buffer(opts.caps.shs, 'base64')
```


## License

MIT, Copyright 2015 Paul Frazee and Dominic Tarr



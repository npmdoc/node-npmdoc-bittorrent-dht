# api documentation for  [bittorrent-dht (v7.5.0)](https://github.com/feross/bittorrent-dht#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-bittorrent-dht.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-bittorrent-dht) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-bittorrent-dht.svg)](https://travis-ci.org/npmdoc/node-npmdoc-bittorrent-dht)
#### Simple, robust, BitTorrent DHT implementation

[![NPM](https://nodei.co/npm/bittorrent-dht.png?downloads=true)](https://www.npmjs.com/package/bittorrent-dht)

[![apidoc](https://npmdoc.github.io/node-npmdoc-bittorrent-dht/build/screenCapture.buildNpmdoc.browser.%2Fhome%2Ftravis%2Fbuild%2Fnpmdoc%2Fnode-npmdoc-bittorrent-dht%2Ftmp%2Fbuild%2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-bittorrent-dht/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-bittorrent-dht/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-bittorrent-dht/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Feross Aboukhadijeh",
        "email": "feross@feross.org",
        "url": "http://feross.org/"
    },
    "bugs": {
        "url": "https://github.com/feross/bittorrent-dht/issues"
    },
    "dependencies": {
        "bencode": "^0.11.0",
        "buffer-equals": "^1.0.3",
        "debug": "^2.2.0",
        "inherits": "^2.0.1",
        "k-bucket": "^3.0.1",
        "k-rpc": "^4.0.0",
        "lru": "^3.1.0",
        "safe-buffer": "^5.0.1"
    },
    "description": "Simple, robust, BitTorrent DHT implementation",
    "devDependencies": {
        "ed25519-supercop": "^1.0.0",
        "ip": "^1.1.0",
        "once": "^1.3.3",
        "run-parallel": "^1.1.4",
        "standard": "*",
        "tape": "^4.4.0"
    },
    "directories": {},
    "dist": {
        "shasum": "df8f87f8dbc884637de475a93214839f070b6383",
        "tarball": "https://registry.npmjs.org/bittorrent-dht/-/bittorrent-dht-7.5.0.tgz"
    },
    "gitHead": "65e1f4bf9c3d1168f37f130c2dd8cf48e26fde1d",
    "homepage": "https://github.com/feross/bittorrent-dht#readme",
    "keywords": [
        "torrent",
        "bittorrent",
        "dht",
        "distributed hash table",
        "protocol",
        "peer",
        "p2p",
        "peer-to-peer"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "feross",
            "email": "feross@feross.org"
        },
        {
            "name": "mafintosh",
            "email": "mathiasbuus@gmail.com"
        }
    ],
    "name": "bittorrent-dht",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/feross/bittorrent-dht.git"
    },
    "scripts": {
        "start": "node server.js",
        "test": "standard && tape test/*.js",
        "test-live": "tape test/live/*.js",
        "update-authors": "./bin/update-authors.sh"
    },
    "version": "7.5.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module bittorrent-dht](#apidoc.module.bittorrent-dht)
1.  [function <span class="apidocSignatureSpan">bittorrent-dht.</span>Client (opts)](#apidoc.element.bittorrent-dht.Client)
1.  [function <span class="apidocSignatureSpan">bittorrent-dht.</span>Server (opts)](#apidoc.element.bittorrent-dht.Server)
1.  [function <span class="apidocSignatureSpan">bittorrent-dht.</span>super_ ()](#apidoc.element.bittorrent-dht.super_)



# <a name="apidoc.module.bittorrent-dht"></a>[module bittorrent-dht](#apidoc.module.bittorrent-dht)

#### <a name="apidoc.element.bittorrent-dht.Client"></a>[function <span class="apidocSignatureSpan">bittorrent-dht.</span>Client (opts)](#apidoc.element.bittorrent-dht.Client)
- description and source-code
```javascript
function DHT(opts) {
  if (!(this instanceof DHT)) return new DHT(opts)
  if (!opts) opts = {}

  var self = this

  this._tables = LRU({maxAge: ROTATE_INTERVAL, max: opts.maxTables || 1000})
  this._values = LRU(opts.maxValues || 1000)
  this._peers = new PeerStore(opts.maxPeers || 10000)

  this._secrets = null
  this._rpc = krpc(opts)
  this._rpc.on('query', onquery)
  this._rpc.on('node', onnode)
  this._rpc.on('warning', onwarning)
  this._rpc.on('error', onerror)
  this._rpc.on('listening', onlistening)
  this._rotateSecrets()
  this._verify = opts.verify || null
  this._host = opts.host || null
  this._interval = setInterval(rotateSecrets, ROTATE_INTERVAL)
  this._hash = opts.hash || sha1

  this.listening = false
  this.destroyed = false
  this.nodeId = this._rpc.id
  this.nodes = this._rpc.nodes

  process.nextTick(bootstrap)

  EventEmitter.call(this)
  this._debug('new DHT %s', this.nodeId)

  function onlistening () {
    self.listening = true
    self._debug('listening %d', self.address().port)
    self.emit('listening')
  }

  function onquery (query, peer) {
    self._onquery(query, peer)
  }

  function rotateSecrets () {
    self._rotateSecrets()
  }

  function bootstrap () {
    if (!self.destroyed) self._bootstrap(opts.bootstrap !== false)
  }

  function onwarning (err) {
    self.emit('warning', err)
  }

  function onerror (err) {
    self.emit('error', err)
  }

  function onnode (node) {
    self.emit('node', node)
  }
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.bittorrent-dht.Server"></a>[function <span class="apidocSignatureSpan">bittorrent-dht.</span>Server (opts)](#apidoc.element.bittorrent-dht.Server)
- description and source-code
```javascript
function DHT(opts) {
  if (!(this instanceof DHT)) return new DHT(opts)
  if (!opts) opts = {}

  var self = this

  this._tables = LRU({maxAge: ROTATE_INTERVAL, max: opts.maxTables || 1000})
  this._values = LRU(opts.maxValues || 1000)
  this._peers = new PeerStore(opts.maxPeers || 10000)

  this._secrets = null
  this._rpc = krpc(opts)
  this._rpc.on('query', onquery)
  this._rpc.on('node', onnode)
  this._rpc.on('warning', onwarning)
  this._rpc.on('error', onerror)
  this._rpc.on('listening', onlistening)
  this._rotateSecrets()
  this._verify = opts.verify || null
  this._host = opts.host || null
  this._interval = setInterval(rotateSecrets, ROTATE_INTERVAL)
  this._hash = opts.hash || sha1

  this.listening = false
  this.destroyed = false
  this.nodeId = this._rpc.id
  this.nodes = this._rpc.nodes

  process.nextTick(bootstrap)

  EventEmitter.call(this)
  this._debug('new DHT %s', this.nodeId)

  function onlistening () {
    self.listening = true
    self._debug('listening %d', self.address().port)
    self.emit('listening')
  }

  function onquery (query, peer) {
    self._onquery(query, peer)
  }

  function rotateSecrets () {
    self._rotateSecrets()
  }

  function bootstrap () {
    if (!self.destroyed) self._bootstrap(opts.bootstrap !== false)
  }

  function onwarning (err) {
    self.emit('warning', err)
  }

  function onerror (err) {
    self.emit('error', err)
  }

  function onnode (node) {
    self.emit('node', node)
  }
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.bittorrent-dht.super_"></a>[function <span class="apidocSignatureSpan">bittorrent-dht.</span>super_ ()](#apidoc.element.bittorrent-dht.super_)
- description and source-code
```javascript
function EventEmitter() {
  EventEmitter.init.call(this);
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)

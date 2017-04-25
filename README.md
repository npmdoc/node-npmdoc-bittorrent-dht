# npmdoc-bittorrent-dht

#### basic api documentation for  [bittorrent-dht (v7.5.3)](https://github.com/webtorrent/bittorrent-dht#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-bittorrent-dht.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-bittorrent-dht) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-bittorrent-dht.svg)](https://travis-ci.org/npmdoc/node-npmdoc-bittorrent-dht)

#### Simple, robust, BitTorrent DHT implementation

[![NPM](https://nodei.co/npm/bittorrent-dht.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/bittorrent-dht)

- [https://npmdoc.github.io/node-npmdoc-bittorrent-dht/build/apidoc.html](https://npmdoc.github.io/node-npmdoc-bittorrent-dht/build/apidoc.html)

[![apidoc](https://npmdoc.github.io/node-npmdoc-bittorrent-dht/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-bittorrent-dht/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-bittorrent-dht/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-bittorrent-dht/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "WebTorrent, LLC",
        "url": "https://webtorrent.io"
    },
    "bugs": {
        "url": "https://github.com/webtorrent/bittorrent-dht/issues"
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
        "shasum": "e886b4183777c78896be967c6c0ebc3bcd2fbe44",
        "tarball": "https://registry.npmjs.org/bittorrent-dht/-/bittorrent-dht-7.5.3.tgz"
    },
    "gitHead": "b77ba528933aa353e0f3d3a32847b6dd2c82660a",
    "homepage": "https://github.com/webtorrent/bittorrent-dht#readme",
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
            "name": "feross"
        },
        {
            "name": "mafintosh"
        }
    ],
    "name": "bittorrent-dht",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git://github.com/webtorrent/bittorrent-dht.git"
    },
    "scripts": {
        "start": "node server.js",
        "test": "standard && tape test/*.js",
        "test-live": "tape test/live/*.js",
        "update-authors": "./bin/update-authors.sh"
    },
    "version": "7.5.3",
    "bin": {}
}
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)

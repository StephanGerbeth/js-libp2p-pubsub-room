# libp2p-pubsub-room
> Creates a room based on a LibP2P pub-sub channel. Emits membership events, listens for messages, broadcast and direct messeges to peers.

## Install

```bash
$ npm install libp2p-pubsub-room
```

## Use

Creating a pubsub room from a LibP2P node

```js
const Room = require('libp2p-pubsub-room')
const Libp2p = require('libp2p')

const libp2p = new Libp2p({ ... })
await libp2p.start()

// libp2p node is ready, so we can start using libp2p-pubsub-room
const room = Room(libp2p, 'room-name')
```

Once we have a room we can listen for messages

```js
room.on('peer joined', (peer) => {
  console.log('Peer joined the room', peer)
})

room.on('peer left', (peer) => {
  console.log('Peer left...', peer)
})

// now started to listen to room
room.on('message', ({ data, from }) => {
  console.log(from + ' says ' + data)
})
```

## API

### Room (libp2p:LibP2P, roomName:string, options:object)

* `libp2p`: LibP2P node. Must have pubsub activated
* `roomName`: string, global identifier for the room
* `options`: object:
  * `pollInterval`: interval for polling the pubsub peers, in ms. Defaults to 1000.
  * `protocolPrefix`: the libp2p multicodec handler to use (default: `default`)

```js
const room = Room(libp2p, 'some-room-name')
```

## room.broadcast(message)

Broacasts message (string).

## room.sendTo(cid, message)

Sends message (string) to peer.

## async room.leave()

Leaves room, stopping everything.

## room.getPeers()

Returns an array of peer identifiers (strings).

## room.hasPeer(cid)

Returns a boolean indicating if the given peer is present in the room.

## room.on('message', (message) => {})

Listens for messages. A `message` is an object containing the following properties:

* `from` (string): peer id
* `data` (string): message content

## room.on('peer joined', (cid) => {})

Once a peer has joined the room.

## room.on('peer left', (cid) => {})

Once a peer has left the room.

## License

ISC

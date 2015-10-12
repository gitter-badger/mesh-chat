# mesh-chat
Chat protocol for secure communication over a mesh network over the internet.


## Recognized `mesh-chat` Clients
Clients have been built in multiple languages. Interpretations between these clients are very unique, but the `mesh-chat` protocol is always consistent.

* [PyÑa Colada v0.2.0](https://github.com/etkirsch/pyna-colada) (Python 3.4)  
* [Spiced Gracken v0.1.1](https://github.com/NullVoxPopuli/spiced_gracken) (Ruby gem)  
* [Angular Pale Ale v0](https://github.com/etkirsch/angular-pale-ale) (Angular JS)
* [EmberClear v0](https://github.com/NullVoxPopuli/emberclear/) (Ember)

### Responsibility Breakdown
The client is responsible for sending all messages to servers. It is only permitted to send messages on its own behalf, and not for any other party. It will keep track of the active list of authorized servers.

The server is responsible for configuration of authorized servers and handling of all message receipts. When a message arrives, it is then relayed to the local client for display. In Milestone 2, the server will also handle authorization and server exchange.


## Mesh-Chat Protocol
Each message will be a serialized JSON hash.
Every hash must include:

    {
      type: <MessageClassName>, # This will tell each client how to read the rest of the keys
      client: <String>, # name of the client that sent the message
      client_version: <String>, # version of the client that sent the message
      time_sent: <UTC DateTime String>, # time at the packaging of this message
      sender: {
        name: <String>,
        location: <IpAddress>,
        time_connected: <Time> # Time connected to the mesh network
        uid: <String> # Uinque ID generated by your client
      },
      destination: <uid>, # Unique ID representing the intended recipient when routing across the mesh network
      message: <Indeterminant> # Variable type, can be string or json object (see below)
    }

A recieving client deserializes the JSONized hash, and can do whatever with it.

### Message Types
There are various types of messages used by Rum. They each have various functionalities as described below.

* **chat**: A message broadcast to all of a client's active servers
* **whisper**: A message broadcast to a single active server, e.g. `@evan hello`
* **connect**: Informs all authorized servers that this client/server pair has come online
* **disconnect**: Informs all active servers that this client has gone offline
* **nodelist**: Used to send a full server list from one node to another
* **nodelistdiff**: Response to `nodelist` type message, sends all servers unique to a node not contained in the `nodelist` message
* **nodelisthash**: Used to check for descrepancies in node lists between one node and another
* **ping**: Used to ask a user for an update of their basic information (alias, location, uid)
* **pingreply**: A response to the ping - contains no body.
* **authorization**: Milestone 2

#### Message
The `message` field in the hash is of variable type. Its type is determined by the `type` field as follows.

 * **chat**: A string containing the message body
 * **whisper**: A string containing the message body
 * **connect**: A json object containing the identifier for the connecting party and a hash of all of its authorized servers
 * **disconnect**: Notifies the meshnet that a user has disconnected from the meshnet
 * **nodelist**: Contains the full server list from an authorized node
 * **nodelisthash**: A hash for checking for differences in nodelists between a connecting node and a connected node. This hash should be SHA512
 * **nodelistdiff**: Contains a truncated server list with only unique entities from a connected node to a connecting node
 * **authorization**: Milestone 2

#### Node List Entry
An entry in the `nodelist` and `nodelistdiff` messages must follow the following format:

```
{
    "alias": <string>, # The user's current alias (transient)
    "location": <string>:<num>, # the user's current ip address and port (transient)
    "uid": <string>, # the user's identifier (static)
    "publickey": <string> # This user's public key (RSA? undecided at the moment) (static)
}
```

## Encryption
`mesh-chat` applications should use RSA for encryption and decryption. Applications which do not use RSA will be unable to read or respond coherently.

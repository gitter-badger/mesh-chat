# Routing across the mesh network

In an ideal world, everyone would have a direct connection to everyone.
However, due to various firewalls and private networks, VPNs, etc -- We have to be able to relay messages from those who don't have a way to contact someone to those who do.
No intermediary / relay node will be able to read the message to the intended target, because only the intended target will have the decryption key. So there are no real incentives to any nodes going rogue and collecting all messages sent to them to be relayed.

## Security

### Authorizing a new Node to the network
To gain access to the network, a person must know someone on the network, or someone on the network can recruit people to join. There is no way to just 'connect' to the network and be authorized without personally knowing someone ahead of time. This is to evade the possibility of a man-in-the-middle attack intercepting encryption keys for the whole network.

Each new person will need the following information to be eligible to join the network:
```javascript
{
  'alias': 'your alias',
  'location': '10.10.12.48:2008',
  'uid': 'your unique identification',
  'publicKey': '-----BEGIN PUBLIC KEY-----\nRSA Public Key\n-----END PUBLIC KEY-----\n'
}
```

**alias**: this can change at anytime, it is how you will show up in your own chat, as well as everyone else's.  
**location**: this can change at anytime.  
**uid**: this cannot change or you will have to be re-added to the network.  
**publicKey**: this *could* change, but at this time, @NullVoxPopuli and @NeuroTek haven't talked about how to handle that quite yet. The PublicKey is how other nodes will encrypt information that only you can decrypt.  

* Note that the above information is formatted as json. Each mesh-chat compatible client should be capable of exporting and importing this type of file.

In order to authorize someone to the network, an exchange of the above information must happen.
Say, Alice and Bob are friends. Alice is already on the mesh network, and Bob wants to get in.

* Alice and Bob both export their own identity json files.
* Alice and Bob exchange their identity json files. Bob has Alice's identification file, and vice-a-versa.
* Both Alice and Bob import their friend's identity.
* Either Bob OR Alice may connect to eachother -- at this point it doesnt't matter who connects to who, as they are both authenticated with one another. 
* Once a connection is made, Alice will send Bob a list of everyone she can communicate with (node information about the whole network).
* Bob can now send messages to anyone on the network. 


### P2P Messaging

### Relaying

## Traversing the Network
TODO ^
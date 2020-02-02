# Key Backup Protocol Message Schemas 

Message schemas are specifications for how particular kinds of messages should be formed.

Dark Crystal's key backup scheme currently has 4 types of messages: `root`, `shard`, `request`, `reply` and `forward`.

A module containing exact specifications and validation methods in javascript - [json-schema.org](https://json-schema.org/) using [is-my-json-valid](https://github.com/mafintosh/is-my-json-valid) is [provided here](https://gitlab.com/dark-crystal/key-backup-message-schemas).

These schemas can be seen as templates, as you may want to modify them based on the individual needs of your project. But they can also be simply used as they are.

# Schemas

All messages contain:
- A `type` field describing which kind of message it is. These are prefixed with `dark-crystal/` to avoid being confused with messages from another application. 
- A `version` number of the schema to allow backward compatibility when updating the schemas.
- A `timestamp` giving a record of what happened then.

Depending on whether it is implicit to the transport protocol used, it may also be appropriate to include an `author` field, with the public key of the author of the message.

## `root`

This message will be published exactly once for each shared secret, and will contain a name for the secret.  It will be a private message with exactly one recipient which will be the author of the message. Custodians are not able to read it.

Its serves as a record for the secret owner, but is also referred to by the other messages.  Its reference might be the hash of the message, or a reference to where it is stored (for example in an append-only log such as 'hypercore', the feed-id and sequence number of the encrypted message). This way, we have a way of establishing that a collection of shard messages belong to the same secret, without revealing any additional metadata.

Example:

```js
{
  "type": "dark-crystal/root",
  "version": "1.0.0",
  "name": "directions to treasure",
  "quorum": 2,
  "shards": 5,
  "tool": <version of secrets module used>,
  "timestamp": 1580300916260 
}
```
with message ID.

## `shard`

This message will be published once for each shard of the secret.  It will contain a reference to the `root` message for that secret, as well as the shard itself.  The shard will be encrypted with the public key of the recipient of the shard.  This will be a private message with exactly two recipients, one of which will be the author of the message.  Note that there are two levels of encryption here, which means that the shard itself is not exposed to the author but the rest of the message is.  This allows the author to keep track of who shards have been sent to as well as to verify shard integrity when receiving the decrypted shard later.

Example:

```js
{
  "type": "dark-crystal/shard",
  "version": "1.0.0",
  "root": <reference to root message>,
  "shard": <shard data>,
  "recipient": <public key of recipient>, 
  "timestamp": 1580300916260 
}
```

## `Request`

This message is published by the secret owner, addressed to a custodian, to ask them to return a shard.

Generally, they will send this message to all custodians, but in some circumstances they might request a shard only from a particular custodian.  For example, if it is likely that the custodian might become unavailable for some time.

Optionally, it might also include an ephemeral public key. This is from a single-use keypair which is generated only for receiving the response to this particular request.  The secret key can be deleted afterwards, making the reply message permanently illegible without needing to set up a new account.

Example:

```js
{ 
  "type": "dark-crystal/request",
  "version": "1.0.0",
  "recipient": <public key of recipient>, 
  "root": <reference to root message>,
  "ephemeral-key": <one-time public key for recieving the reply>
  "timestamp": 1580300916260 
}
```

## `Reply`

This message is send from custodian to secret owner in response to a `request`. It contains a reference to the request message, so the secret owner can know which requests are currently open, as well as the shard data itself.  Since the shard contains the original signature from the secret owner, it is possible to verify that the shard data given is that which was sent out.

If an ephemeral public key was given in the `request`, the data in the `shard` field will be encrypted to this key.

```js
{
  "type": "dark-crystal/reply",
  "version": "1.0.0",
  "recipient": <public key of recipient>,
  "root": <reference to root message>,
  "branch": <reference to request message>,
  "shard": <shard data containing original signature>,
  "timestamp": 1580300916260 
}
```

## `Forward`

This message will be published in order to send a shard to a different recipient key than that which authored the shard message. It will be a private message which exactly two recipients, one of whom will be the author of the message.  It will also contain:

- A reference to the associated `root` message
- The shard data containing the original signature.

Example:

```js
{
  "type": "dark-crystal/forward",
  "version": "1.0.0",
  "root": <reference to root message>,
  "shard": <shard data containing original signature>
  "recipient": <public key of recipient>,
  "timestamp": 1580300916260, 
}
```

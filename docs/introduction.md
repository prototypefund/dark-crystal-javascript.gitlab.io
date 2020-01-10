# Introduction to Dark Crystal 

Dark Crystal is a system of cryptographic key management which emphasises trust between peers rather than individual responsibility.  It is aimed at developers who want to encourage good practices for handling cryptographic keys. 

## Why?

Multi-user computer systems have traditionally had a client-server architecture. The server typically acts as a 'single source of truth', meaning all users have access to a ubiquitous data set, giving a kind of consensus across the system of what the current state is.

For example, a library where all users can see whether a particular book is currently available.

In the case of a library, the client-server model makes sense. It is critical that all users gain consensus about the current state of the database, to avoid going to the library to get a book which is actually not there.

But many systems model interactions which are not based on a particular finite resource, but between people. For example if I want to borrow a lawnmower, I only need to find one individual who can confirm that they have a lawnmower available to borrow. It is possible for things to work without every user having a totally ubiquitous view of the system.  In these cases, the server is used as a platform, providing a place to meet, hosting peoples messages or content, and providing a way of authenticating who authored what.

In recent years the limitations of the client-server model have become ever more apparent.  The server operators are in a very powerful position. They are able to:
- Discontinue the service.
- Increase costs or change the terms of the service.
- Revoke access to particular users or groups of users.
- Include hidden functionality.
- Extract or commodify user's personal data.

At the same time, peer-to-peer systems have become more advanced, overcoming many of the limitations they once had. For example:
- A peer can hold other peer's data without being able to access it themselves. 
- A peer can verify the integrity of a given data set, and verify whether it was written by a particular author, and that the set is complete until the most recent item given.

> The server is providing only a place to meet, and a data store which is always available, but not access to a special resource which people do not have themselves.

Peer-to-peer systems are fundamentally empowering to those that use them, their very nature makes it difficult for a minority to control or exploit them.  Provided the software they run is open source and community developed, they offer a much more democratic way of working.

But having ultimate control over your own data comes at a cost.  Without a trusted party providing authentication, peers have to manage cryptographic keys.  Loosing them not only means you lock yourself out, but someone who finds them could impersonate you.

Dark Crystal is a set of protocols and recommendations which aim to address the key custody problem, encourage good practice when designing peer-to-peer applications, and make it safer and easier for non-technical peoples to use such systems.

## What transport protocol does it use?

It aims to be agnostic to any particular language, technology, or transport protocol. We try to offer something useful to a broad range of software projects.

## What are the protocols:

### Dark crystal key recovery

This protocol allows lost keys to be recovered.  This would be useful to decrypt existing data, or so that an account can continue to be used.

It works by using a threshold secret sharing scheme such as Adi Shamir's. (Explain more)

This is done is such a way they the identities of the members of the support group are kept private, and that there is specific degree of tolerance to a particular group members being unavailable or uncooperative. 

- [Protocol specification](https://gitlab.com/dark-crystal/dark-crystal-protocol-specification)

### Dark crystal key re-issuance

This protocol provides a way of establishing a new key, so that an identity can be seen to persist to a new key.  This specifically addresses the cases where keys might have been compromised. 

It works be each peer assigning a 'support group' who are empowered to make assertions on their behalf.  The support group can announce a new key when the old one is assumed to be compromised, and client software can resolve these announcements to get the current key for a particular identity.

This is done is such a way they the identities of the members of the support group are kept private, and that there is specific degree of tolerance to a particular group members being unavailable or uncooperative. 

It uses group threshold signatures using a cryptographic scheme such as Boney-Lynn-Shacham.

- [Proposal for Key re-issuance using threshold signatures](https://gitlab.com/dark-crystal/threshold-signatures-report)

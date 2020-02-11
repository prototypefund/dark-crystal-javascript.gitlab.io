# Introduction to Dark Crystal 

Dark Crystal is a system of cryptographic key management which emphasises trust between peers rather than individual responsibility.  It is aimed at developers who want to encourage good practices for handling cryptographic keys. 

## Why?

***TLDR: peer-to-peer systems are empowering to users, but key management is hard***

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

- A peer can store data belonging to other peers, without being able to access it themselves. 
- A peer can verify the integrity of a given data set, and verify whether it was written by a particular author, and that the set is complete up to the most recent item given.

> The server is providing only a place to meet, and a data store which is always available, but not access to a special resource which people do not have themselves.

Furthermore, there are increasing environmental concerns associated with servers and data centres. While consumer devices are becoming more lightweight, the server farms which power them behind the scenes are becoming bigger and more energy intensive.  Peer-to-peer architectures address this issue, encouraging the development of less resource-intensive applications.

Peer-to-peer systems are fundamentally empowering to those that use them, their very nature makes it difficult for a minority to control or exploit them.  Provided the software they run is open source and community developed, they offer a much more democratic way of working.

But having ultimate control over your own data comes at a cost.  Without a trusted party providing authentication, peers have to manage cryptographic keys.  Loosing them not only means you lock yourself out, but someone who finds them could impersonate you.

Dark Crystal is a set of protocols and recommendations which aim to address ***the key custody problem***, encourage good practice when designing peer-to-peer applications, and make it safer and easier for non-technical peoples to use such systems.

## What transport protocol does it use?

Dark Crystal aims to be agnostic to any particular language, technology, or transport protocol. We try to offer something useful to a broad range of software projects.

## What are the protocols?

see the [explanation of the protocols](protocols.md)


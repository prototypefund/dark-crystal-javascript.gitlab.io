# Dark Crystal Key Re-issuance using Threshold signatures

## Terminology

- 'secret-owner' - The person who wishes to secure their identity.
- 'support-group' - Trusted contacts of the secret-owner, who are empowered to make assertions on her behalf.

## The procedure

- The secret owner decides chooses trusted contacts to be her support group, and decides on a threshold - the number of members required to gain consensus about re-issuing her key.

- The support group generate key-shares which are aggregated to form a group public key which is sent to the secret owner.

- The secret-owner publicly publishes a signed message announcing the public key of the support group. This affirms that she has a group who are empowered to make assertions on her behalf.

- In the event of key loss or compromise, the secret owner generates a new keypair, effectively a fresh account on the system. And sends each member of her support group a signed, timestamped message containing her new public key. She also contacts them out of band to confirm it was her. By 'out of band' we mean using some communication system which does not rely on the compromised key, for example, a telephone call. 

- The threshold amount of group members each sign this message and their signatures are aggregated to produce and publish a single, signed message asserting Alice's new public key. This also serves to revoke the old key. 

- Any client software which seeks to validate messages from the secret owner must resolve her current public key by looking for messages published by her trusted group. 

- The secret owner publishes another signed message with her new key confirming the public key of her trusted group.

- If group members change, the new group's aggregate public key announcement must be signed by both the secret owner and a threshold amount of members of the previous group.

## Source code

- [threshold-signatures](https://gitlab.com/dark-crystal/threshold-signatures)

# SSH (Secure Shell)

SSH is a secure protocol that machines use to communicate with one another.

It allows users to and share files, control and modify remote computers. It encrypts all data.

You can connect to another computer with SSH for example.

## How to use it ?

`ssh {user}@{host}`

- The `ssh` key command means I want to open an encrypted shell connection.
- `user` is the account I want to access (root, ...)
- `host` is the computer I want to access (IP address, domain name, ...)

For example : `ssh root@167.99.146.57`

## What's the link with web development ?

- GitHub ! You use SSH to push, pull, clone files.
- Reinstall an app on a server

## Symmetrical encryption

Encrypting something is make it unreadable without having "something" to decrypt it. It's a way to have secrets.

Symmetrical encryption uses ONE secret key to encrypt it and decrypt it. SSH communicates through this shared key.

There is one problem though : anyone that has the key can decrypt the message and we need to share the key...

**Key exchange algorithm** : it needs asymmetrical encryption.

## Asymmetrical encryption

Each computer have two keys : one public key and one private key.

The public keys are linked with private keys.
The private key cannot mathematically compute from the public key.

The message is encrypted with the public key, is sent, and the other uses his private key to decrypt the message. Therefore, the private key is never shared.

SSH uses asymmetrical encryption just to generate the key that will be used for symmetrical encryption.

It uses the Difiie Hellman key exchange : basically, this algorithm uses both public and private keys to generate one unique key.

_Resources_ :

- https://www.youtube.com/watch?v=NmM9HA2MQGI
- https://www.youtube.com/watch?v=Yjrfm_oRO0w
- https://www.youtube.com/watch?v=vsXMMT2CqqE&t=
- https://www.youtube.com/watch?v=NF1pwjL9-DE

Asymmetric encryption is more time consuming so SSH uses symmetric encryption.

## Hashing

One more time, there can be a problem. Someone can sit in the middle and pretend to be the other to have the key...

To solve it, we use hashing.

If we hash a string, it's impossible to go back from the result. For every input, there is one unique output. Two different inputs cannot result in the same output.

One of the use case is to compare two files to know if they are exactly the same

## Authentication

Of course, to access a machine, you need to authenticate yourself because you can't be able to access others servers.

## Generate a SSH key

RSA : allows us to provide or provide the identity of the person without a password.

`ssh-keygen -C "example@gmail.com"`

The recommended command is that one : `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

`-C : provides a new comment

Two keys are generated : `id_rsa` and `id_rsa.pub` : we can share the public key, NOT the private one.

If I have already existing keys, I have to add my key pairs to the agent : `ssh-add ~/.ssh/id_rsa_2`

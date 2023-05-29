---
title: "Exploring Digital Signatures"
date: 2019-06-04T19:05:20+05:30
draft: false
tags: ["cryptography", "digitalSignature", "openssl","notes"]
---
# Notes on Digital Signatures using openssl

Here is a short blog post on digital signatures and how public and private key works.
<!--more-->

Digital signature has three parts to it,
1. A *key* generation algorithm (which creates public-private key pair)
2. *Hashing /Signing algorithm*. When a message and private key is given as input, this gives a hash value as an output. Variable length input fixed length output.
3. *Verification algorithm*. When Message, public key, and hash values are given, this validates the authenticity.

## Key Generation
**Key generation** - To test this, lets create a new folder. Navigate to new folder and create a file with some text.

**file creation**
`echo ThisMessagewillbeEncrypted. > file.txt`


```bash
$ openssl genrsa -out privateKey.pem 2048
Generating RSA private key, 2048 bit long modulus (2 primes)
...............................................+++++
..........................................................................................................................+++++
e is 65537 (0x010001)

#Getting public key out of private key
$ openssl rsa -in privateKey.pem -pubout > publicKey.pem
writing RSA key
```
## Hashing / signing
**Signing message**, for a given message and private key, hashing function will create a unique signature.

```bash
$ openssl dgst -sha256 -sign privateKey.pem -out sha256SignedFile file.txt
```

## Verification

**Verification**, with message, public key, and hashed value authenticity is verified.

```bash
$ openssl dgst -sha256 -verify publicKey.pem -signature sha256SignedFile file.txt
Verified OK
```

## Encrypt Hash using private key

```bash
#curent folder contents
$ ls
file.txt  privateKey.pem  publicKey.pem  sha256SignedFile
```
Encrypting and Decrypting *file.txt*

```bash
$ openssl rsautl -encrypt -in file.txt -out encrypted_file -inkey publicKey.pem  -pubin

#contents of folder now
$ ls
encrypted_file  file.txt  privateKey.pem  publicKey.pem  sha256SignedFile

#Decryption, decrypt contents of file
$ openssl rsautl -decrypt -in encrypted_file -inkey privateKey.pem
this message will be encrypted.
```
### References:
* [Digital Signatures](https://en.wikipedia.org/wiki/Digital_signature)




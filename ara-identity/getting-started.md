# # Getting Started with _Ara Identity_

**Ara Identity** is a decentralized identity tool set and implementation of
_[Decentralized Identifiers][did-spec]_. It is used to identifty users,
file systems, and network services in the [Ara Network][ara-one].
Ara Identities are used to express control, authentication, and
integrity through public key cryptography.

Accompanied with every identity is an Ethereum wallet, or
[_secp256k1 key pair_][secp256k1] suitable for issuing transaction on various
Ethereum networks.

Ara Identities are actually just [_Ed25519 key pairs_][ed25519] linked to
_secp256k1 key pairs_ through [_Decentralized Identifier Document
Objects_][did-ddo] signed by the secret key corresponding to the
_ed22519 key pair_. (More on this later!)

Identities are typically stored on your machine, locally, but can be taken with
you, and even archived on a network for back up and public resolution.

In this guide we will walk through the following:

* [Installing The _Ara Identity Command Line Tools_](#installation)
* [Creating An Ara Identity](#create-identity)
  * [Decentralized Identifier](#decentralized-identifier)
  * [Identity Mnemonic](#identity-mnemonic)
  * [Identity Files](#identity-files)
* [Decentralized Identifier Document Objects - ddo.json](#ddo)
* [Resolving An Identity](#resolve-identity)
* [Archiving An Identity](#archive-identity)
* [Sharing An Identity](#share-identity)

## ## Installing Ara Identity Command Line Tools :id=installation

> [!tip| label: Prerequisites | className: warning]
> The Ara Identity command line tools are written in
[**JavaScript**](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
and run on [**Node.js**](https://nodejs.org). Please make sure you have
`node` (and `npm`) installed before continuing.

_**Ara Identity**_ ships with a command line utility called **`aid`**.

### ### Download Pre-built Binary

The **`aid`** command can be installed by downloading and unzipping the
pre-built package and running the `install.sh` script.

See [_Ara Identity Latest
Releases_](https://github.com/AraBlocks/ara-identity/releases/latest) to
download a pre-built binary for your operating system.

> [!note|label: ]
> **Linux** and **macOS** are currently only supported

#### #### PATH

You should be sure to update your `PATH` to include `$HOME/.ara/bin` as
this is where the `aid` command will be installed.

##### See Also

* [How to permanently set $PATH on Linux/Unix?](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux-unix)
* [Mac OS X: Set / Change $PATH Variable](https://www.cyberciti.biz/faq/appleosx-bash-unix-change-set-path-environment-variable/)

### ## Installation from NPM

The **`aid`** command can be installed with `npm` by running the following
command

```sh
$ npm install ara-identity --global # you may need sudo
```

After the successful global installation of the **`ara-identity`** module,
you should now have access to the **`aid`** command. Try running `aid --help`
to get a list of commands and other useful information. This should also
validate that you have successful installed the **`aid`** command and
that it is available in your path (`$PATH`).

You should see something like:

```sh
$ aid --help
usage: aid [-hqDV] <command> [options]

Commands:
  aid create                                     Create an identity
  aid archive [identifier]                       Archive identity in network
  aid resolve [identifier]                       Resolve an identity from the network, disk, or cache
  aid list                                       Output local identity identifiers
  aid whoami                                     Output current Ara identity in context (.ararc)
  aid recover                                    Recover an identity using a mnemonic phrase
  aid revoke                                     Revoke an identity using a mnemonic phrase

... (output truncated)
```

## ## Creating An Ara Identity :id=create-identity

Creating an identity is as simple as running the `aid create` command
with no arguments. You will be prompted for a _passphrase_. This passphrase
should be kept secret as it protects the secret key backing your newly
created identity. You will be asked for this passphrase from time to time
when running other commands.

Running the `aid create` command should have the following output:

```sh
$ aid create
? Your identity's keystore will be secured by a passphrase.
Please provide a passphrase. Do not forget this as it will never be shown to you.
Passphrase: [input is hidden]
```

Provide a passphrase and press enter. You should see the following output:

```sh
 ara: info:  New identity created: did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f
 ara: warn:  Will write identity file: ddo.json
 ara: warn:  Will write identity file: keystore/eth
 ara: warn:  Will write identity file: keystore/ara
 ara: warn:  Will write identity file: schema.proto
 ara: warn:  Will write identity file: identity
 ara: info:  Please safely store the following 12 word mnemonic phrase for this
 ara: info:  Ara ID. This phrase will be required to restore your Ara ID.
 ara: info:  It will never be shown again:
 ara: info:

╔═══════════════════════════════════════════════════════════════════════════╗
║ general grab rent solar lift sudden industry wait beach flame impose high ║
╚═══════════════════════════════════════════════════════════════════════════╝

```

A lot of things just happened here, and we'll cover them all, but most
importantly there is some information that you should capture and save.

### ### Decentralized Identifier :id=decentralized-identifier

When creating an identity, a new globally unique **decentralized identifier**
is created.

> [!note|label: Your Decentralized Identifier|iconVisibility: hidden]
> `did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f`

The **decentralized identifier** of the identity (`did:ara:53a86..f`) is your
globally unique identifier for your Ara Identity. It is known as a DID, which
defines a URI scheme for creating globally unique decentralized
identifier.

The `did:ara:` prefix before the 64 character hex encoded identifier
is important. It provides a context for the
`53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f` identifier.

The `did:` prefix in the URI lets us know that this is indeed a DID. The
`ara:` part of the URI tells us that this is a _**ara** DID method_. A _DID method_
is specifies how a network or system understands the _DID identifier_ of
the URI. The _DID identifier_ after the `did:ara:` is a Ed25519 public
key. It has a corresponding secret key that can be used to authenticate the
identity.

### ### Identity Mnemonic :id=identity-mnemonic

> [!tip|label: Important |className: warning]
> This mnemonic is shown to you _only_ _**one**_ time.

During the creation of an identity, a [BIP39 mnemonic phrase][bip39] is
generated and used as the seed or _initialization vector_ for the
generation of your identity _Ed25519 key pair_. It should be written down in the order
they appear as it is used to _recover_ or _revoke_ your identity.

> [!note|label: Your BIP39 Mnemonic |iconVisibility: hidden]
> `general grab rent solar lift sudden industry wait beach flame impose high`

### ### Identity Files :id=identity-files

After creating your identity the `aid create` command will save a set of
files to your file system. By default, they will be stored in the
`~/.ara/identities` directory.

> [!note|label: Your Decentralized Identifier|iconVisibility: hidden]
> The directory for the identity with the
`did:ara:53a86...f` URI is stored in
`~/.ara/identities/65c0666f58ac7f304a4cb2f5b700dbb46f5e64ada8e00f94906428864ee4cafb`
directory.

The directory name is the 32 byte [blake2b][blake2b] hash of
the identifier part of the DID URI represented as binary blob (public
key), or more simply put, the directory name is a 64 character hex
encoded string of the blake2b hash of the public key associated with the
identity.

Currently, the following files are written to disk:

Filename       | Purpose
-------------- | -------
`ddo.json`     | JSON file representing a signed [_Decentralized Identifier Object Document_][did-ddo]
`keystore/ara` | JSON file of an _Ara Secret Storage_ object (see [ARA RFC 0001][rfc-0001]) containing the identity's Ed25519 secret key
`keystore/eth` | JSON file of an _Ara Secret Storage_ object (see [ARA RFC 0001][rfc-0001]) containing the identity's Secp256k1 private key
`schema.proto` | A protocol buffer describing the binary layout of the `identity` file
`identity`     | A binary blob containing the entire state of the identity, including the file listed here (See [schema.proto][schema.proto])

## ## Decentralized Identifier Document Object - `ddo.json` :id=ddo

A _Decentralized Identifier Document Object_, or _DDO_ is a JSON object
structure that represents the state of the identity, including public
keys, authentication schemes, and service end points. The _DDO_ state is
captured and stored in a file called `ddo.json` It is signed with the
secret key that is controlled by the identity. This document object is
updated every time a public key, authentication scheme, or service end
point is added. Revoking and recovering identities updates or restores
a _DDO_ state.

A `ddo.json` file may look like:

```json
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f",
  "publicKey": [
    {
      "id": "did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f#owner",
      "type": "Ed25519VerificationKey2018",
      "owner": "did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f",
      "controller": "did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f",
      "publicKeyHex": "53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f",
      "publicKeyPem": "-----BEGIN PUBLIC KEY-----\nFOobFQ/2R+YoTPpHzJ15Lp00pcfckWgtXK/FXGLtMl/\n-----END PUBLIC KEY-----",
      "publicKeyBase58": "6dZo1N22eE4kc7w7i7hoNz4kwCBcUqdvhMgMJ1mkK1zN",
      "publicKeyBase64": "FOobFQ/2R+YoTPpHzJ15Lp00pcfckWgtXK/FXGLtMl/"
    },
    {
      "id": "did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f#eth",
      "type": "Secp256k1VerificationKey2018",
      "owner": "did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f",
      "controller": "did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f",
      "publicKeyHex": "048d68407b342b1980e947c88fb1619517e0ce2afadfa000fd96d122d032c9a768bc358d84ddb3086b405bbc6954bf984978d54c8bba3a5ebac83fba6f9e51fa",
      "publicKeyPem": "-----BEGIN PUBLIC KEY-----\nEjWhAezQrGYDpR8iPsWGVF+DOKvrfoAD9ltEi0DLJp2i8NY2E3bMIa0BbvGlUv5hJeNVMi7o6XrrIP7pvnlH6\n-----END PUBLIC KEY-----",
      "publicKeyBase58": "6HBQq85FP7tEshJQS7FVVCbdPf4Nwsvhs9L32NPrH5JQF6fwbZvhRczsrCgdKb7TevmPA9pbUDC8HXo5Ax3UBey",
      "publicKeyBase64": "EjWhAezQrGYDpR8iPsWGVF+DOKvrfoAD9ltEi0DLJp2i8NY2E3bMIa0BbvGlUv5hJeNVMi7o6XrrIP7pvnlH6"
    }
  ],
  "authentication": [
    {
      "publicKey": "did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f#owner",
      "type": "Ed25519SignatureAuthentication2018"
    },
    {
      "publicKey": "did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f#eth",
      "type": "Secp256k1SignatureAuthentication2018"
    }
  ],
  "service": [],
  "created": "2019-03-26T19:22:45.265Z",
  "updated": "2019-03-26T19:22:45.265Z",
  "proof": {
    "type": "Ed25519VerificationKey2018",
    "nonce": "b88891f9a8b9fe9923d65ca2e53252a11a70aedd0eb5a0abeffb9ecec098f0d7",
    "domain": "ara",
    "created": "2019-03-26T19:22:45.270Z",
    "creator": "did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f#owner",
    "signatureValue": "c7e19cea191d1a2ece4fc12be629217804d228977ac2eae4a975af6a57fd31909abf00ea1a27629eb6a66cb60d2df14eaaf067f40f691071e43b145d5201440f"
  }
}

```

This `ddo.json` file includes **two public keys**, **two authentication
schemes**, **zero services**, **cryptographic proof**, and some metadata about
when this document was created and the last time it was updated.

```json
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f",
  "publicKey": [ ... ],
  "authentication": [ ... ],
  "service": [],
  "created": "2019-03-26T19:22:45.265Z",
  "updated": "2019-03-26T19:22:45.265Z",
  "proof": { ...  }
}
```

The public keys are identified by the **fragment** (#) part of their `id` property.

```json
...

  "publicKey": [
    { "id": "did:ara:53a86...f#owner", ...  },
    { "id": "did:ara:53a86...f#eth", ...  }
  ],

...
```

Initially, the `ddo.json` file contains an **owner** public key, and an
**eth** public key. The **owner** public key indicates that this public key is
the key associated with the owner of the _DDO_. The public key hex value is
equivalent to the identifier part of the DID URI.

## ## Resolving An Identity :id=resolve-identity

Identity resolution is how we can convert an Ara _DID_ into an Ara
_DDO_. This is done by running the `aid resolve` command with a _DID
URI_ as an argument.

```sh
$ aid resolve did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f
```

The output of this command should be the [Decentralized Identifier Document
Object](#ddo), or _DDO_, associated with the _DID_. This can be checked by
comparing the `id` property in the document to the given DID.

> [!tip|label: DDO 'id' property]
> Compare the DID `did:ara:53a86...f` to the `id` property found in the
> associated DDO ensuring the DDO belongs to the DID.
> ```
{
  ...
  "id": "did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f",
  ...
}
```

> [!tip| label: Integrity | className: warning]
> Comparing a DID to the DDO's `id` property does not mean the document has
> not been tampered with. The `signatureValue` on a DDO's `proof`
> object provides integrity and autheticates the document for the identity.
> ```
{
 ...
  "proof": {
    "type": "Ed25519VerificationKey2018",
    "nonce": "b88891f9a8b9fe9923d65ca2e53252a11a70aedd0eb5a0abeffb9ecec098f0d7",
    "domain": "ara",
    "created": "2019-03-26T19:22:45.270Z",
    "creator": "did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f#owner",
    "signatureValue": "c7e19cea191d1a2ece4fc12be629217804d228977ac2eae4a975af6a57fd31909abf00ea1a27629eb6a66cb60d2df14eaaf067f40f691071e43b145d5201440f"
  }
 ...
}
```

## ## Archiving An Identity :id=archive-identity

Archiving an identity writes your [identity files](#identity-files) to a
[DAT](https://datproject.org/) archive stored locally on your file
system. This archive can be used to broadcast your identity to a network
or share it with an always on [Ara Identity
Archiver Server](https://github.com/arablocks/ara-identity-archiver/) that
broadcasts your identity for you.

Archiving to an _Archiver Server_ is out of scope for this guide, so
we'll focus on a local archive.

After creating or updating your identity, you should archive it to
ensure your local archive is up to date. This is done by running `aid
archive --local` with your _DID URI_ as the only argument. You will be
prompted for the _passphrase_ you created for this identity.

```sh
$ aid archive --local did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f
? Please provide a passphrase for your identity. This is needed to archive your identity.
Passphrase: [hidden]
 ara: info:  Successfully archived identity to local system
```

In another post we'll walk through archiving an identity to an external
server that will broadcast it out to the Internet. Always on identity
may be useful to you if you need others to access your _DDO_ and verify
your identity or if you want to backup your files somewhere you _trust_.

## Sharing An Identity :id=share-identity

Sometimes you may want to share an identity without with an interested
party. The `aid` command makes this possible by running `aid share` with
your _DID URI_ as the only argument.

Sharing your identity makes your _DID_ accessible through resolution and
also available on the DAT network (see
[runtime configuration](/ara-runtime-configuration/discovery) for configuring DNS
and DHT discovery servers).

```sh
$ aid share did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f
 ara: info:  Broadcasting and sharing did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f
```

On another terminal you can resolve the identity with `aid resolve --no-cache`,
which will skip reads from the file system for your identity and go
straight to the network to resolve it.

```sh
$ aid resolve --no-cache did:ara:53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f
```

You can also use the `dat clone` command to check out a copy of the
archive to disk because the identifier of the DID URI is the same public
key used in the DAT network! If you run `dat clone` with the DID URI
identifier as the only argument, DAT will clone the archive and write
the files to disk in a directory named with your identifier.

```sh
$ dat clone 53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f
dat v13.11.5
Created new dat in
/home/werle/dats/53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f/.dat
Cloning: 5 files (9.8 KB)

1 connection | Download 3.7 KB/s Upload 0 B/s

dat sync complete.
Version 54


Exiting the Dat program...
```

We can verify the contents of the archive by listing the directory.

```sh
$ ls ./53a86c543fd91f98a133e91f3275e4ba74d2971f7245a0b572bf15718bb4c97f/
ddo.json  identity  keystore  schema.proto
```

The identity files broadcasting over the DAT network make it
compatible with applications like [Beaker
Browser](https://beakerbrowser.com) bringing your identity to the web
with out any extra work.


[ara-one]: https://ara.one
[bip39]: https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki
[blake2b]: https://en.bitcoinwiki.org/wiki/Blake2b
[did-ddo]: https://w3c-ccg.github.io/did-spec/#did-documents
[did-spec]: https://w3c-ccg.github.io/did-spec
[ed25519]: https://en.wikipedia.org/wiki/EdDSA#Ed25519
[init-vector]:https://en.wikipedia.org/wiki/Initialization_vector
[rfc-0001]: https://github.com/AraBlocks/RFCs/blob/master/text/0001-ass.md
[schema.proto]: https://github.com/AraBlocks/ara-identity/blob/master/protobuf/schema.proto
[secp256k1]: https://en.bitcoin.it/wiki/Secp256k1

[create-identity]: getting-started.md#creating-identity
[decentralized-identifier]: getting-started.md#decentralized-identifier
[identity-mnemonic]: getting-started.md#identity-mnemonic
[identity-files]: getting-started.md#identity-files
[installation]: getting-started.md#installation
[resolve-identity]: getting-started.md#resolve-identity


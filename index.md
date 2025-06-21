---
image: "https://arewemlsyet.com/assets/images/logo.png?1"
---
[![Matrix](/assets/images/matrix-logo-white.svg)](https://matrix.org) ![MLS](/assets/mls-logo.png) _Last updated: 2025-06-20_

```
                                   __   __ _    ____               _   ___ 
  __ _ _ __ ___   __      _____   |  \ /  | |  /   _|   _   _  ___| |_/ _ \
 / _` | '__/ _ \  \ \ /\ / / _ \  |   V   | |  \_ (_   | | | |/ _ \ __\// /
| (_| | | |  __/   \ V  V /  __/  | |\_/| | |____)  \  | |_| |  __/ |_  \/ 
 \__,_|_|  \___|    \_/\_/ \___|  |_|   |_|____|____/   \__, |\___|\__| () 
                                                         |___/             
```

# Not Yet.

[Messaging Layer Security (MLS)](https://messaginglayersecurity.rocks/) is an
[IETF standard](https://datatracker.ietf.org/doc/rfc9420/) for end-to-end encryption in messaging systems.  We are
investigating bringing MLS to Matrix.  So far we have basic encryption and
decryption working and can handle membership changes.

Latest demo video: key backup

<iframe src="https://drive.google.com/file/d/1gL7LM6Aq9WfRhMs4DyboGwImW_5KJ_l2/preview" width="640" height="480" allow="autoplay"></iframe>

Previous demo videos:

- [external commits](https://drive.google.com/file/d/1lMSY6UERUvs9swcvvNsds6oTAgQcOev4/view)
- [group updates](https://drive.google.com/file/d/1Mbxo1PrhNUG8NrZ_wMYIZq1wVXMvbxjI/view)
- [group creation](https://drive.google.com/file/d/1RMgE0Jp_-leqHzIpsjrWshlxMchK7d3K/view)

## Why MLS?

In most cases, MLS has better performance in large groups than Olm/Megolm.  The
graph below shows a comparison[¹](#fn1) between some MLS and Olm/Megolm operations.

![Comparison graph](comparison.png)

The graph gives a simplified comparison, but gives an idea of their relative
performance.  In most cases, MLS is faster than Olm/Megolm.  But one case in
which MLS can be much slower when adding a large number of users to a group at
a time.  It is expected that this would not be a common occurrence.

However, integrating MLS into Matrix is not a simple task.  One of the biggest
hurdles is that MLS assumes that epochs (essentially, the state of an MLS tree
in between updates to the tree) have a linear ordering.  However, Matrix being
a decentralised system with no central authority, it is difficult to enforce a
linear ordering.  Thus we must either determine a way to do so, or work around
this limitation.

In addition, we need to investigate how other Matrix features, such as key
backups, interact with MLS.

# Tasks

(tasks are not necessarily in order)

## Initial investigation
- ✅ create trial implementation of MLS
  - [source code](https://gitlab.matrix.org/matrix-org/mls-ts/)
- ✅ investigate non-decentralised MLS on Matrix
- ✅ investigate and test strategies for decentralised MLS
  - [ideas](https://gitlab.matrix.org/matrix-org/mls-ts/-/blob/decentralised2/decentralised.org)
  - [source
    code](https://gitlab.matrix.org/matrix-org/mls-ts/-/tree/decentralised2)
- ✅ investigate backfilling commit messages
- ✅ performance comparison between (non-decentralised) MLS and Olm/Megolm
  - [source code](https://gitlab.matrix.org/uhoreg/mls-comparison)
  - [initial comparison](https://gitlab.matrix.org/uhoreg/mls-comparison/-/wikis/home)

## Implementation
- ✅ investigate suitability of [OpenMLS](https://github.com/openmls/openmls)
  (Rust implementation of MLS) as a base for decentralised MLS
- ✅ create a library for decentralised MLS: [matrix-dmls](https://gitlab.matrix.org/uhoreg/matrix-dmls)
- ⏳ proof of concept implementation in a client (Element Web) ⚠️ This
  implementation does not persist state, so if you reload the page, you will
  lose data.  Only use with test accounts.
  - 🎞️ [demo 1: group creation](https://drive.google.com/file/d/1RMgE0Jp_-leqHzIpsjrWshlxMchK7d3K/view)
  - 🎞️ [demo 2: group updates](https://drive.google.com/file/d/1Mbxo1PrhNUG8NrZ_wMYIZq1wVXMvbxjI/view)
  - 🎞️ [demo 3: external commits](https://drive.google.com/file/d/1lMSY6UERUvs9swcvvNsds6oTAgQcOev4/view)
  - 🎞️ [demo 4: key backup](https://drive.google.com/file/d/1gL7LM6Aq9WfRhMs4DyboGwImW_5KJ_l2/view)
  - 🧑‍💻[matrix-js-sdk
    PR](https://github.com/matrix-org/matrix-js-sdk/pull/3306),
    [matrix-react-sdk
    PR](https://github.com/matrix-org/matrix-react-sdk/pull/10669)
  - 🖱️ [try it out](https://element-mls-proof-of-concept.netlify.app)
- ✅ investigate and implement external commits (ability to send an encrypted
  message before a current member has added you to the MLS tree)
- ✅ investigate decryption of historical messages
  - re-decryption of old messages
  - key backup
- ☐ integrate decentralised MLS into Rust crypto crate
- ☐ write bindings for other languages
- ☐ fully support MLS in clients

## Documentation
- ⏳ write and finalise MSCs for MLS over Matrix
- ☐ write MLS implementation guide for clients

# Resources

- [MSC2883](https://github.com/matrix-org/matrix-spec-proposals/pull/2883):
  Matrix-flavoured MLS - (preliminary) description of how to do MLS over Matrix
- [Decentralised
  MLS](https://gitlab.matrix.org/matrix-org/mls-ts/-/blob/decentralised2/decentralised.org) -
  an investigation into avoiding the requirement for a linear ordering of MLS epochs
- 🎞️ Demo videos
  - [group creation](https://drive.google.com/file/d/1RMgE0Jp_-leqHzIpsjrWshlxMchK7d3K/view)
  - [group updates](https://drive.google.com/file/d/1Mbxo1PrhNUG8NrZ_wMYIZq1wVXMvbxjI/view)
  - [external commits](https://drive.google.com/file/d/1lMSY6UERUvs9swcvvNsds6oTAgQcOev4/view)
  - [key backup](https://drive.google.com/file/d/1gL7LM6Aq9WfRhMs4DyboGwImW_5KJ_l2/view)
- 🧑‍💻 Implementations
  - [matrix-dmls](https://gitlab.matrix.org/uhoreg/matrix-dmls) - Rust library
    for decentralised MLS in Matrix
  - [Proof of concept implementation in
    Element](https://element-mls-proof-of-concept.netlify.app) ⚠️ This
    implementation does not persist state, so if you reload the page, you will
    lose data.  Only use with test accounts.
    - [matrix-js-sdk PR](https://github.com/matrix-org/matrix-js-sdk/pull/3306)
    - [matrix-react-sdk
      PR](https://github.com/matrix-org/matrix-react-sdk/pull/10669)

-----

<a name="fn1">¹</a> code for running the comparison is at
<https://gitlab.matrix.org/uhoreg/mls-comparison>. More details on how the
comparison was run is at
<https://gitlab.matrix.org/uhoreg/mls-comparison/-/wikis/home>.

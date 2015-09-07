---
layout: post
title: "How to check KeepassX .sig signatures with GnuPG"
date: 2015-09-07 13:47:29 +0200
comments: true
categories:
  - gnupg
  - keepassx
---

This example uses KeepassX but works the same for other projects which use `.sig` signature files (another example is OpenResty).

First install GnuPG for your OS. Ubuntu has it usally installed from scratch. For OS X use Homebrew and install it with `brew install gnupg`.
<!-- more -->
Get the public key of the owner first. According to [this page](https://www.keepassx.org/news/) the public key is `83135D45`. So import the public key from a public keyserver (here from MIT):

```bash
gpg --keyserver pgpkeys.mit.edu --recv-key 83135D45
```

Download KeepassX install file and its signature file from [www.keepassx.org/dev/projects/keepassx/files](https://www.keepassx.org/dev/projects/keepassx/files).

Verify signature (in this example with beta2, OS X):

```bash
gpg --verify KeePassX-2.0-beta2.dmg.sig KeePassX-2.0-beta2.dmg
```

The last command should tell you that the file was correctly signed. You can also recheck that everything is ok by printing the exit code of the last command with `echo $?` which should be 0. Otherwise you have a **WRONG** file without valid signature! Redownload it and check again, if it fails again do **not** install it!

One note: Most likely you will also get a warning (but no error!) like:

```
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
```

this is something you cannot prevent because it tells you basically that you trust the owner of the key and that the owner is really him (do you know him personally? Me neither ;) so please ignore this last warning. You can read more about this little warning [here](http://security.stackexchange.com/questions/6841/ways-to-sign-gpg-public-key-so-it-is-trusted).

So this whole file checking is a little bit safer than a normal md5 or sha256 checksum because it involves the private key of the KeepassX owner. As always in computer science and life: Nothing is 100% safe.
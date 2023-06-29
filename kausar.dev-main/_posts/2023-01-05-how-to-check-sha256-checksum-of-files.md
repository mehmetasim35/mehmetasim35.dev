---
layout:   post
title:    How to check SHA-256 checksum of files
date:     '2023-01-05 12:35:00'
category: security
tags:     windows sha256 checksum security mac linux openssl
---

A lot of websites provide pre-computed [SHA-256](https://en.wikipedia.org/wiki/SHA-2) digests for files that users download. A user can compare the [checksum](https://en.wikipedia.org/wiki/Checksum) of the file they have downloaded with the checksum provided by the website separately. If the checksums match, it means the file was not tampered with in transit and it is indeed the file the user intended to download. No one wants to download and run an infected executable file that has malicious code.

So how would we check SHA-256 checksum of a file? 

Most unix-based operating systems include SHA-2 utilities in their distribution packages and Windows users also use PowerShell function "Get-FileHash" to compute SHA-256 checksums.

Let's look at some of these commands and examples of using them.

> For the examples below let's assume we want to use KeePass to store our passwords and we have downloaded version 2.52 of the software that comes in the zip file called `KeePass-2.52.zip` from KeePass [website](https://sourceforge.net/projects/keepass/files/KeePass%202.x/2.52/KeePass-2.52.zip/download). Now this is kind of software we can't take our chances with, so we must check the integrity of the file. We should compare the checksum of the file we have downloaded with the one provided by KeePass i.e. **2793D799 F1BB5745 F18739D2 BF162D24 73D006C3 E3DF2A4E 28FBCC1A 0439D6C0** on their [integrity](https://keepass.info/integrity.html) page and make sure they are the same.
{: .prompt-info }

## Linux
On a Linux machine we can use [`sha256sum`](https://man7.org/linux/man-pages/man1/sha256sum.1.html).

```console
[kausar@debian ~]$ sha256sum KeePass-2.52.zip
2793d799f1bb5745f18739d2bf162d2473d006c3e3df2a4e28fbcc1a0439d6c0    KeePass-2.52.zip
```

We can see the value above matches the one on KeePass integrity page so we know we have the trustworthy file and it has not been tampered with. Sometimes it can be hard to check each character by looking at them especially when we have multiple files. Some websites give us the checksum in a DIGEST file so we can verify them easily and automatically. DIGEST file is a text file containing names of files and their respective checksums. Assuming we have a DIGEST file called `KeePass-2.52.zip.DIGEST` in the same directory as the zip file then we can verify the file using:

```console
[kausar@debian ~]$ sha256sum -c KeePass-2.52.zip.DIGEST
KeePass-2.52.zip: OK
```
`-c` or `--check` option reads the checksums and filenames in the DIGEST file. It then computes the checksums of the files and compares them checksums in the digest file. If they match it prints OK and if they don't match it will show which files failed.

## MacOS
On MacOS we can use the following command to compute the SHA-256 checksum for the file.

```console
[kausar@mac ~]$ shasum -a 256 KeePass-2.52.zip
2793d799f1bb5745f18739d2bf162d2473d006c3e3df2a4e28fbcc1a0439d6c0  KeePass-2.52.zip
```
`-a` or `--algorithm` option is used to specify which version of Secure Hash Algorithms (SHA) we want to use.

We can also use `shasum` to verify using DIGEST files.

```console
[kausar@mac ~]$ shasum -c KeePass-2.52.zip.DIGEST
KeePass-2.52.zip: OK
```

## OpenSSL
Regardless of which operating system we use if we have openssl installed then it can also be used to compute SHA-256 checksums.

```console
[kausar@mac ~]$ openssl sha256 KeePass-2.52.zip
SHA256(KeePass-2.52.zip)= 2793d799f1bb5745f18739d2bf162d2473d006c3e3df2a4e28fbcc1a0439d6c0
```

## Windows
On Windows we can either use `certUtil` or PowerShell function `Get-FileHash` to compute SHA-256 checksum for the file.

In command prompt and powershell we can do:

```console
C:\Users\Kausar>certUtil -hashfile KeePass-2.52.zip SHA256
SHA256 hash of KeePass-2.52.zip:
2793d799f1bb5745f18739d2bf162d2473d006c3e3df2a4e28fbcc1a0439d6c0
CertUtil: -hashfile command completed successfully.
```

In PowerShell we can use the following to compute SHA-256:

```console
PS C:\Users\Kausar> Get-FileHash .\KeePass-2.52.zip –Algorithm SHA256

Algorithm    Hash                                                                Path
---------    ----                                                                ----
SHA256       2793d799f1bb5745f18739d2bf162d2473d006c3e3df2a4e28fbcc1a0439d6c0    C:\Users\Kausar\KeePass-2.52.zip
```

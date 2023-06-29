---
layout:   post
title:    How to check MD5 checksum of files
date:     '2017-04-11 09:35:00'
category: security
tags:     windows md5 checksum security mac linux openssl
---

First of all please **do not** use [MD5](https://en.wikipedia.org/wiki/MD5) for cryptographic purposes such as storing one-way hash of passwords like it was used in the past. It has severe vulnerabilities and the CMU Software Engineering Institute has concluded that MD5 is essentially "cryptographically broken and unsuitable for further use".

However it's still widely used as a [checksum](https://en.wikipedia.org/wiki/Checksum) to verify data integrity against corruption. Even for this purpose it's slowly being replaced by stronger algorithms like [SHA-2](https://en.wikipedia.org/wiki/SHA-2).

> To learn how to compute SHA-256 checksum of files click [here]({% post_url 2023-01-05-how-to-check-sha256-checksum-of-files %}).
{: .prompt-tip }

Although a lot of websites still provide pre-computed MD5 digests for files that users download. A user can compare the checksum of the file they have downloaded with the checksum provided by the website separately. If the checksums match, it means the file was not tampered with in transit and it is indeed the file the user intended to download. No one wants to download and run an infected executable file that has malicious code.

So how would we check MD5 checksum of a file? 

Most unix-based operating systems include MD5 utilities in their distribution packages and Windows users can use PowerShell function "Get-FileHash" to compute MD5 checksums.

Let's look at some of these commands and examples of using them.

> For the examples below let's assume we want to use KeePass to store our passwords and we have downloaded version 2.52 of the software that comes in the zip file called `KeePass-2.52.zip` from KeePass [website](https://sourceforge.net/projects/keepass/files/KeePass%202.x/2.52/KeePass-2.52.zip/download). Now this is kind of software we can't take our chances with, so we must check the integrity of the file. We should compare the checksum of the file we have downloaded with the one provided by KeePass i.e. **5F0F4D70 2FBC9967 E8063E23 1F561363** on their [integrity](https://keepass.info/integrity.html) page and make sure they are the same.
{: .prompt-info }

## Linux
On a Linux machine we can use [`md5sum`](https://man7.org/linux/man-pages/man1/md5sum.1.html).

```console
[kausar@debian ~]$ md5sum KeePass-2.52.zip
5f0f4d702fbc9967e8063e231f561363    KeePass-2.52.zip
```

We can see the value above matches the one on KeePass integrity page so we know we have the trustworthy file and it has not been tampered with. Sometimes it can be hard to check each character by looking at them especially when we have multiple files. Some websites give us the checksum in a DIGEST file so we can verify them easily and automatically. DIGEST file is a text file containing names of files and their respective checksums. Assuming we have a DIGEST file called `KeePass-2.52.zip.DIGEST` in the same directory as the zip file then we can verify the file using:

```console
[kausar@debian ~]$ md5sum -c KeePass-2.52.zip.DIGEST
KeePass-2.52.zip: OK
```
`-c` or `--check` option reads the checksums and filenames in the DIGEST file. It then computes the checksums of the files and compares them checksums in the digest file. If they match it prints OK and if they don't match it will show which files failed.

## MacOS
On MacOS we can use the following command to compute the MD5 checksum for the file.

```console
[kausar@mac ~]$ md5 KeePass-2.52.zip
MD5 (KeePass-2.52.zip) = 5f0f4d702fbc9967e8063e231f561363
```

If we need to use the command in a script where we need to save the output to a variable then we can use the `-q` flag which is for quiet. With this flag it only shows the MD5 hash without any other information.

```console
[kausar@mac ~]$ md5 -q KeePass-2.52.zip
5f0f4d702fbc9967e8063e231f561363
```

## OpenSSL
Regardless of which operating system we use if we have openssl installed then it can also be used to compute MD5 checksums.

```console
[kausar@mac ~]$ openssl md5 KeePass-2.52.zip
MD5(KeePass-2.52.zip)= 5f0f4d702fbc9967e8063e231f561363
```

## Windows
On Windows we can either use `certUtil` or PowerShell function `Get-FileHash` to compute MD5 checksum for the file.

In command prompt and PowerShell we can do:

```console
C:\Users\Kausar>certUtil -hashfile KeePass-2.52.zip MD5
MD5 hash of KeePass-2.52.zip:
5f0f4d702fbc9967e8063e231f561363
CertUtil: -hashfile command completed successfully.
```

In PowerShell we can use the following to compute MD5:

```console
PS C:\Users\Kausar> Get-FileHash .\KeePass-2.52.zip –Algorithm MD5

Algorithm       Hash                                     Path
---------       ----                                     ----
MD5             5F0F4D702FBC9967E8063E231F561363         C:\Users\Kausar\KeePass-2.52.zip
```

### Outdated

> The information below is outdated and kept only for archival purposes. Use the Windows methods above.
{: .prompt-warning }

On Windows we can use a free third-party software called WinMD5Free to compute checksums. It is available to download from [WinMD5.com](http://winmd5.com/). It is a portable software and doesn't need to be installed in order to use it. Once downloaded, extract the zip file and run WinMD5Free.exe.
Browse and select the file you want to compute MD5 checksum for. WinMD5Free will compute the MD5 hash and display it.

We can also use WinMD5 to verify the checksums automatically same as we did on Linux using `md5sum -c`. In order to do this all we have to do is copy the checksum from website into textbox titled "Original file MD5 checksum value" and click on Verify.

---
layout:   post
title:    Cannot run Jekyll build as non-root user
date:     '2016-07-06 02:05:00'
category: jekyll
tags:     error jekyll permissions
---

If you accidently run Jekyll `build` or `serve` as `root` user and now want to run it as any other user and get an error message like:

```console
[kausar@centos ~]$ jekyll build
Configuration file: _config.yml
            Source: <omitted>
       Destination: <omitted>
 Incremental build: disabled. Enable with --incremental
      Generating...
jekyll 3.1.6 | Error:  Permission denied - _site
```

This is because it generated the destination directory (_site in my case) when it was running as `root` user. Hence `root` now owns that directory and cannot be changed by other users. Changing the owner using the following command should resolve the issue.

```bash
# assuming destination directory is called _site
sudo chown -R $(whoami):$(whoami) _site
```

Bash will run `$(whoami)` first replacing it with your username. Then the `sudo chown` will run and change the owner of the directory.

Now just clean Jekyll and try again:

```bash
# Remove jekyll metadata file.
jekyll clean
```

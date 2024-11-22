+++
title = 'Linux'
date = 2024-11-21T19:29:54-05:00
layout = 'qna-section'
draft = false
+++

## Why does Debian have both `apt` and `apt-get` commands for package management?

`apt` is newer and offers a more high-level, user-friendly interface than `apt-get`.

`apt` also has a few noteworthy features that `apt-get` does not, like automatically removing obsolete package versions with `apt upgrade` or recommending suggested package installs to fix dependency issues.

see [https://aws.amazon.com/compare/the-difference-between-apt-and-apt-get/](https://aws.amazon.com/compare/the-difference-between-apt-and-apt-get/)

## How do `sh` and `bash` differ? How might `#!/bin/bash` vs. `#!/bin/sh` [shebangs](<https://en.wikipedia.org/wiki/Shebang_(Unix)>) differ in their effects on subsequent code?

`sh` (Shell Command Language or Bourne Shell) is defined in the [POSIX](https://stackoverflow.com/a/1780614) standard and is the foundation for multiple derived implementations, including `bash`. `bash` is a [superset](https://www.hackterms.com/superset) of `sh` that includes other commands like `history` and other features like [process substitution](https://www.linkedin.com/pulse/difference-between-sh-bash-linux-alok-mishra-soogc/#:~:text=Process%20Substitution). Scripts that use these features _may\*_ fail when told to run with `sh`.

\*In Linux distributions such as Ubuntu or Linux Mint, however, `#!/bin/sh` is a [symlink](https://www.hackterms.com/symlink) to `#!/bin/dash` ([like bash but faster](https://lwn.net/Articles/343924/#:~:text=The%20major,dash)), so in practice there may be little to no difference in how the two shebang lines affect script behavior.

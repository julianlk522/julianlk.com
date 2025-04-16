+++
title = 'Linux'
date = 2024-11-21T19:29:54-05:00
layout = 'qna-section'
draft = false
+++

## What are the biggest / most common reasons to consider trying Linux?

-   speed
    -   minimal system overhead
        -   more CLIs than GUIs
        -   non-essential features can be removed at will
-   customizability
    -   GUI system, keyboard shortcuts, window manager, ... anything you can think of
-   security
    -   open source, so heavily scrutinized by community
    -   no [built-in spyware](https://www.extremetech.com/computing/342941-windows-11-collects-an-awful-lot-of-telemetry-about-your-pc)
    -   package managers
        -   keep download sources (repositories) in one location
        -   require packages be verified before serving to users

## How do `sh` and `bash` differ? How might `#!/bin/bash` vs. `#!/bin/sh` [shebangs](<https://en.wikipedia.org/wiki/Shebang_(Unix)>) run subsequent code differently?

`sh` (Shell Command Language or Bourne Shell) is defined in the [POSIX](https://stackoverflow.com/a/1780614) standard and is the foundation for multiple derived implementations, including `bash`. `bash` is a [superset](https://www.hackterms.com/superset) of `sh` that includes other commands like `history` and other features like [process substitution](https://www.linkedin.com/pulse/difference-between-sh-bash-linux-alok-mishra-soogc/#:~:text=Process%20Substitution). Scripts that use these features _may\*_ fail when told to run with `sh`.

\*In Linux distributions such as Ubuntu or Linux Mint, however, `#!/bin/sh` is a [symlink](https://www.hackterms.com/symlink) to `#!/bin/dash` ([like bash but faster](https://lwn.net/Articles/343924/#:~:text=The%20major,dash)), so in practice there may be little to no difference in how the two shebang lines affect script behavior.

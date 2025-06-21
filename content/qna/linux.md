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

## Bash statement separators... there are a few!

-   **&&**: execute following statement only after preceding statement executes successfully
-   **||**: execute following statement only after preceding statement fails
-   **;**: execute following statement regardless of whether preceding statement succeeded or failed
-   **&**: execute following statement while preceding statement runs parallel in the background

\*In Linux distributions such as Ubuntu or Linux Mint, however, `#!/bin/sh` is a [symlink](https://www.hackterms.com/symlink) to `#!/bin/dash` ([like bash but faster](https://lwn.net/Articles/343924/#:~:text=The%20major,dash)), so in practice there may be little to no difference in how the two shebang lines affect script behavior.

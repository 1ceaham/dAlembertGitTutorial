# Git Tutorial - Institut ∂'Alembert - January 31, 2020
{:.no_toc}

## Overview
{:.no_toc}
* TOC
{:toc}

## Prerequisites
1. \*nix terminal
  * Linux / Mac: You already have one of these!
  * Windows 10: Use the Windows Subsystem for Linux (WSL). See how to get set up [here](https://docs.microsoft.com/en-us/windows/wsl/install-win10); I suggest Ubuntu 18.04 LTS.
  * Windows 8 / 8.1: See the git section below.
  * Windows 7: Please note that this version of Windows has reached its End-of-Life and will no longer receive security updates. It appears that it is still possible to upgrade to Windows 10 for free, so consider doing that. Otherwise, you should be able to follow the same instructions as Windows 8.
2. git
  * Linux / Mac / WSL: You probably already have git, so check in your terminal with `git --version`. If not, check out the [installation instructions](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
  * Windows 7 / 8 / 8.1: Since we will cover git's command line interface (CLI), your best bet is [Git for Windows](https://gitforwindows.org/). Alternatively, there are GUI tools (like GitHub Desktop) that are useful and perhaps *more* intuitive, but do not necessarily expose all of the tools and cross-platform consistency the CLI provides.
3. Set up name and email
  * `$ git config --global user.name "Mona Lisa"`
  * `$ git config --global user.email "email@example.com"` *(Note: if signing up for GitHub, use the same email here.)*
  * The above commands set your ID for all repos, but can also be set on a per-repo basis by omitting `--global`.
4. GitHub account\*
  * 

## Why version control?

## Why git / Github?

## What *NOT* to version
* Generally, "data"
* Large / binary files
* Secrets
  * SSH keys
  * Passwords
  * User info
* Build artifacts / generated code
* Other VCS
* As a replacement for backups

## Getting started with git on your own code

## Working with remote repositories

## Collaboration
### Branches

### Github "Fork and Pull" Model

## More resources

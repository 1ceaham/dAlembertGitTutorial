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
  * `git config --global user.name "Mona Lisa"`
  * `git config --global user.email "email@example.com"` *(Note: if signing up for GitHub, use the same email here.)*
  * The above commands set your ID for all repos, but can also be set on a per-repo basis by omitting `--global`.
4. GitHub account\*
  * Sign up at https://github.com/ with the email you set in git.
  * Be sure to enable two factor authentication (2FA) with an app. Best practice is *NOT* to give a SMS number as they are vulnerable to [sim swap attacks](https://en.wikipedia.org/wiki/SIM_swap_scam).
  * Print out your recovery codes and keep them somewhere safe.
  * Authentication can be handled in a number of ways, as detailed [here](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage). Long story short, if you are on Windows, use [this](https://github.com/Microsoft/Git-Credential-Manager-for-Windows), and if you are on Mac, use keychain with `git config --global credential.helper osxkeychain`. If you're on another flavor, you have some tradeoffs to consider.

\*: Note that this is not necessary for this tutorial to be valuable. It bears pointing out that GitHub is owned by Microsoft, and while they are outside of the scope of today's discussion, there are valid concerns about GitHub with respect to open-source, free, non-proprietary, and decentralized software that lives outside of the purview of American sovereignty / political demands. It is absolutely possible to use git without a centralized repository, or at least with an open-source alternative (see [SourceHut](https://sourcehut.org/), [GitLab](https://about.gitlab.com/)), but given the immediate lack of a supported tool at IJLR∂A, we will use it as an example of how to interface with these types of systems. That said, its popularity means that collaboration frequently happens there in industry (where some may consider it a "standard") and academia, and may therefore be of use for you to learn.

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
### Adding a remote
Whether on your network, another drive, or on a centralized server (like GitHub), you can add a link from your local repo to a remote repo with `git remote add`, a name, and a location. If you are beginning with a local repo, it's common to initialize the remote as a bare repo (meaning it doesn't have any items or any commits) so that you can directly `git push` (or, in other words, copy) the current state of your local repo to the new location without any conflicts.

At this point, we should clarify that git can work over the internet using HTTPS or SSH, both of which have upsides and downsides. We'll talk about these in the context of GitHub, but other services (and it's not uncommon to have credentials with multiple servers) would be approached in much the same fashion.
* HTTPS: If you set up a credential manager earlier, the first time you authenticate, you should be able to use your username, password, and 2FA device to sign in and have your credentials securely cached. If not, you'll have to enter your username and password for authentication each and every time you connect. This is frequently easier to work with in terms of access, as SSH ports can sometimes be blocked by firewalls.
* SSH: Much like authenticating with a server, it's possible to interface with GitHub via SSH. One advantage of using SSH keys to authenticate is that they do not provide access to your actual GitHub account, meaning there is some additional security in terms of recovery in case a key is compromised. It can also allow for finer-grained access control. On the other hand, generating and securing SSH keys (such as with ssh-agent) is technically demanding, and the authentication process at the command line may not be as frictionless as with HTTPS.

For example, let's make a new bare repo on GitHub and use it as a remote for our toy repo.

### `git clone`
This is the most common way to get a remote repository onto your local machine, but it's more-or-less the same thing as copying the content and adding the source repository as a remote so that you can continue to interact with the "upstream" (often shared) version. Therefore, it's also valid to download a zip of the whole repo (including the .git folder), decompress it where you want, and manually `git remote add` as before.

### What is this webpage?
I'm glad you asked! In fact, this page was automagically generated by GitHub Pages from a repo that you can find [here](https://github.com/1ceaham/dAlembertGitTutorial). Since this page is a regular repo, you can see its history, `git clone` it to your machine, modify it, add your own commits, and generally do whatever you need to with it! You are, of course, bound by the license of the content - for example, on GitHub, if a repo does not have a license, then it is copyrighted and you do not have the right to use, modify or redistribute it, only to read it (even then, only on GitHub).

(If you want a separate class on how to set up GitHub Pages for your free static hosting needs - an academic website, perhaps? - please let me know and we can discuss scheduling another seminar!)

## Collaboration
### Branches

### Github "Fork and Pull" Model

## More resources

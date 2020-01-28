# Version Control & Git - Institut ∂'Alembert - January 31, 2020
{:.no_toc}

## Overview
{:.no_toc}
* TOC
{:toc}

## Prerequisites
0. **_BACK UP YOUR WORK_**
  * Version control is not a replacement for backups!
  * If you haven't done this recently, do it now before messing around with your file system!
1. \*nix terminal
  * Linux / Mac: You already have one of these 🤙
  * Windows 10: Use the Windows Subsystem for Linux (WSL). See how to get set up [here](https://docs.microsoft.com/en-us/windows/wsl/install-win10); I suggest Ubuntu 18.04 LTS.
  * Windows 8 / 8.1: See the git section below.
  * Windows 7: Please note that this version of Windows has reached its End-of-Life and will no longer receive security updates. It appears that it is still possible to upgrade to Windows 10 for free, so consider doing that. Otherwise, you should be able to follow the same instructions as Windows 8.
  * Final note: You should probably have at least a passing familiarity with how to get around at a command line. This tutorial will provide most of the commands you'll need, but it's good to have some working practice getting around. One resource with everything you'll ever need to know is [The Art of Command Line](https://github.com/jlevy/the-art-of-command-line), but if you have a better idea, we'll see how you might suggest in a little bit...
2. git
  * Linux / Mac / WSL: You probably already have git, so check in your terminal with `git --version`. If not, check out the [installation instructions](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
  * Windows 7 / 8 / 8.1: Since we will cover git's command line interface (CLI), your best bet is [Git for Windows](https://gitforwindows.org/). Alternatively, there are GUI tools (like GitHub Desktop) that are useful and perhaps *more* intuitive, but do not necessarily expose all of the tools and cross-platform consistency the CLI provides.
3. Set up name and email
  * `git config --global user.name "Mona Lisa"`
  * `git config --global user.email "email@example.com"` *(Note: if signing up for GitHub, use the same email here. It's OK to use a different one, but you'll have to change a setting to ensure your commits are associated with your account.)*
  * The above commands set your ID for all repos, but can also be set on a per-repo basis by omitting `--global`.
4. GitHub account\*
  * Sign up at https://github.com/ with the email you set in git.
  * Be sure to enable two factor authentication (2FA) with an app. Best practice is *NOT* to give a SMS number as they are vulnerable to [sim swap attacks](https://en.wikipedia.org/wiki/SIM_swap_scam).
  * Print out your recovery codes and keep them somewhere safe.
  * Authentication can be handled in a number of ways, as detailed [here](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage). Long story short, if you are on Windows, use [this](https://github.com/Microsoft/Git-Credential-Manager-for-Windows), and if you are on Mac, use keychain with `git config --global credential.helper osxkeychain`. If you're on another flavor, you have some tradeoffs to consider.
5. Think about where to perform the tutorial
  * Some people create a directory like `~/Projects`. This can be any useful, consistent place to import and run code.
  * WSL users: the directory your terminal starts in may be `/mnt/c/Users/Username` (which is how WSL addresses `%UserProfile%`) or `/home/WSLUsername` (AKA `~` or "Home"). Double check that you're doing this in the "partition" you want, especially if your automatic backups will not capture your WSL filesystem.
  * Of course, the concepts covered here can be applied anywhere in the file system, but if you're just starting out, it's probably best not to experiment with your academic work.

\*Optional. While the topic is outside of the scope of today's discussion, some may object to the use of GitHub as a code host. GitHub is owned by Microsoft and presents certain concerns with respect to open-source, free, non-proprietary, and decentralized software ideals, not to mention a desire to avoid a de facto monoculture. GitHub is incorporated and hosted in the US. Using git without a centralized repository, or at least with an open-source alternative (see [SourceHut](https://sourcehut.org/), [GitLab](https://about.gitlab.com/)) is absolutely possible, but given the immediate lack of a supported tool at IJLR∂A, we will use it as an example of how to interface with these types of systems. That said, its popularity means that collaboration frequently happens there in industry and academia, and may therefore be of use for you to learn.

## What is version control?
Version control is a term used to represent the idea that changes to content (code, articles, configuration) can be tracked over time. This can be as simple as adding a version number to note how many revisions a document has gone through (which may not actually preserve the previous revisions, though in the case of a book, those previous revisions are out in the world). At present, where much of our content is digital ephemera, it's as important as ever to keep track of changes to content - not necessarily just for a single file, but for complex collections of interdependent modules - as well as the real-world demands on and outcomes resulting from such a project. In short, we're describing modern software development.

We think of ourselves as scientists, but in reality, most of us are also developers. If you code, have dependencies that change (but that you need to control to guarantee consistency), write in text files that you update, or anxiously make copies of folders in the fear of losing or breaking something, version control can help you to track and (if need be) recover your work.

## What is git?
Git is a version control system (VCS) that implements many of the concepts above. A git repository is simply a directory that is being tracked with git. A repository can be on your local machine or on a remote server. Git is a distributed VCS, meaning that each user tracks the whole history of a given repository - though they might all be on different computers, each copy has a local working version and knows everything that occurred to arrive at it (though they may be in varying states of synchrony).

In git, there is no need to "check out" or "freeze" a file for everyone else when someone is working on it, as may be the case in a centralized VCS. As such, multiple authors can track their work locally simultaneously, eventually communicating their changes to others whose repositories are then updated to match. This style of work also informs the culture and norms of working with git, particularly with respect to "rewriting history" - while this is allowable locally (and sometimes even encouraged, in the case of "squashing" commits before a merge), it's a pretty grave faux pas when working with a shared repository.

### Contrast with GitHub
GitHub *is not* git. GitHub is a host that is usually interfaced with through git, but which provides some other services and access control on top of the "version control" part of git. For example, reporting a bug on GitHub is not a part of git, whereas the web interface allows users to perform some git-like actions in a repository (such as making an edit to a file and committing it). Most interaction with GitHub happens through git, usually by copying a repository locally or by pushing your changes back up to the hosted version.

This centralization comes with benefits, as a consistently available shared host can be a convenient place to sync things across many machines (one possible use case of git) or as a definitive source of releases (basically named revisions) for users that are not going to interact with the source content. Other things that GitHub offers (as is the case with other service providers) are integration with CI/CD pipelines (automatic actions that happen when you update code), forums, and other things that are useful for the work surrounding development but are not related to actually changing content. However, it cannot be stressed enough that those other things are not git.

### Comparison with other VCS
Perhaps the first thing to point out in comparing git to other VCS is its "popularity," at least in the open-source realm. (It's also frequently complained about, but it appears to get the job done...) A StackOverflow developer survey in 2018 found that [87% of their respondents](https://web.archive.org/web/20190530142357/https://insights.stackoverflow.com/survey/2018/#work-_-version-control) used git for version control. With that popularity comes benefits, as in the massive amount of information, integrations, and know-how in the world. Just looking at the major hosts, it's clear to see that git is the VCS of choice. While it may not be so clearly cut in the professional world, where some industries lean toward their own set of tools in common, I think it's fair to say that git isn't going anywhere anytime soon, and you will probably come across it if there is ever a case where you need to do some collaborative software development.

That said, other VCS can be successful by doing things differently or more specifically. For example, while pipelines exist for CI/CD that hook into git, this is essentially a fancy way of scheduling a process to run on someone else's computer (the cloud™ ☁). Some programming languages that have bindings with a custom VCS can do cool things that general purpose tools like git cannot, particularly with respect to provability and correctness.

Without going too much into detail here, I would personally argue that it's worth it to learn git as a general case and pick up other VCS as you interact with projects that require them.

## What *not* to version
Or, at least, what *not* to upload to GitHub

* Generally, "data"
  * Is yours backed up?
* Binary files (either lots or large)
  * Probably ok to include a few photos, but not 3 hours of audio
  * No meaningful "diffs"
  * For an alternative approach, see the [Git Large File System](https://git-lfs.github.com/). (👋 Franck!)
* Secrets
  * SSH keys
  * Passwords
  * User info
* Build artifacts / generated code
* Other VCS
* As a replacement for backups
* Similarly, VCS generally don't like to be synced (e.g. Dropbox)

# Interactive tutorial
## Getting started with git on your own code
From here on out, I'm going to call a repository a "repo" since it's shorter.

### Initializing a repo
As I mentioned before, a repo is simply a directory where git is tracking changes to files. So, let's create a new repo!

Make sure you're in the directory you want to hold your repos (at least for this tutorial) with `pwd`. Then, make a new directory to hold your repo with `mkdir myTestRepo`. Move into it with `cd myTestRepo`.

Now, type `git init`. Ta-da! You have a git repo. If you `ls -a`, you'll see that there's a `.git` directory. That's where all of the tracking magic happens.

Our repo is a little boring, so we should add some content. If you haven't used vi(m) before, follow the next series of commands very carefully.

* `vi hello.sh` - Open a new file in vi (or vim) named `hello.sh`.
* `i` - Enter "Insert" mode.
* Type (or copy/paste, if your terminal supports that):
    ```
    #!/bin/bash
    echo "Hello world"
    ```
* `Esc` - Leave "Insert" mode.
* `:wq`, `Enter` - Issue the command "Write, then quit."

Great! Now we have something in our repo. Make your script executable with `chmod +x hello.sh` and then run it with `./hello.sh` to make sure it works.

At this point, git can see that the file is there, but it is not actually being tracked. How do we know? Running `git status` shows the current state of whatever repo you're in. As a side note, the repo that you're in is determined by climbing parent directories until a `.git` directory is found. Running that command at this point shows that `hello.sh` is "untracked," meaning that if we take a snapshot of our repo at this moment in time, it will not be included in our account of the repo's history. That said, we could, in fact, take a snapshot if we wanted to - it would just be pretty boring (and not really following convention).

As a personal recommendation, while you're getting familiar with things, run `git status` all the time. Seriously. Before and after almost any of the commands we'll discuss here, toss out a `git status` and make sure what you see is what you expect. Like Dory, just keep `git status`, just keep `git status`, just keep `git status`, `git status`, `git status`.

### Stage files to be added / updated
How can we ask git to track our new script? As suggested by our status report, we can use `git add hello.sh` to add our file to the index. The index is the list of new or changed files that would be included in a snapshot in addition to all of the other already-tracked files that haven't changed.

There are a couple of powerful ways to use `git add` when you have multiple files to change or add. You can use wildcards such as `*.txt` to select for particular filetypes in the current folder (this uses shell expansion), or you can escape the asterisk (e.g. `\*`) to let git recurse through all subdirectories as well. If you don't need to be selective, a simple `git add -A` will recursively make your index match the working state of your repo.

There are also cases (some of which are mentioned above) where you don't want to add particular files, but would still like to use `git add -A` (to avoid having to come up with a specially-crafted filter for only the files you want in the index). For this, you can create a hidden file named `.gitignore`, which controls what git will not track by default. For example, if you have a particular subfolder that you want *git* to *ignore*, you can list it in your `.gitignore`. Similarly, it accepts wildcards such that you could exclude `*.wav` files from being tracked. Super useful! This can really clean up a messy or long `git status` report.

### Take a snapshot
Finally, we want to capture what our repo looks like at this moment in time. To do so, make sure that your index looks how you want with `git status`, then type `git commit -m "Initial commit."`. Congratulations! You've saved your first step in the history of this repo. The part in quotes after the `-m` is the commit message, or a short description of what changed, which you should never forget to add.

### Make a change
Alright, if this was real work, we'd probably have something to change in our script. So, let's add a little more text to our file. We're going to use `vi` again, so follow along closely!

* `vi hello.sh` - Open the editor.
* `j` - We're in command mode, so this doesn't insert a character, but instead moves the cursor down a line.
* `llllllllllllllllll` - Move the cursor between `d` and `"`. If you overshoot, you can press `h` as many times as you need to go back the other way.
* `i` - Enter insert mode.
* `, I can use git!` - Enter some text.
* `Esc`, `:wq`, `Enter` - Return to command mode, save, and quit, as before.

Run your script as before to make sure it works - you should always test your code before deploying it, after all!

Now check `git status`: what do you see? Hopefully, it reports that your file has been modified. If you want to check what has changed between the previous commit and your working directory, you can call `git diff --word-diff hello.sh`, which shows changed words rather than characters (fairly useful in a longer file, as long as it's semantically meaningful).

Let's save another snapshot of our repo. As before, we can do this in two steps: adding files to the index, and then committing the changes. Since this change is really simple, we can also do it in a single step with `git commit -a -m "Add qualifications."`.

Now that we have two commits, we can view the history of the repo with `git log`, and (even though we just did it), we could compare the two versions of our file to see what changed. To do this, we can specify the commits to compare in a few different ways. One option is to refer to commits relative to our current location, which is named `HEAD`. Thus, you can compare the current commit to the previous one with `git diff --word-diff HEAD~ HEAD hello.sh`, where the `~` means "parent of." This also works with additional `~`, like `HEAD~~~`, meaning "the parent of the parent of the parent of" our reference commit. Of course, with a linear history, you could specify any two commits using this notation, but other more convenient ways include `HEAD~4` (with any number of generations), `HEAD@{yesterday}` (what our reference commit was yesterday), or by simply typing an unambiguous beginning of the commit hash (like `1c002d` as shorthand for `1c002dd4b536e7479fe34593e72e6c6c1819e53b`). See more on this [here](https://git-scm.com/book/en/v2/Git-Tools-Revision-Selection).

One last thing to point out here is that in the actual file system, there's only ever one copy of our file. We've saved the history of `hello.sh`, but there aren't multiple versions of it sitting around in our directory, even when we diff historical versions of it.

### Going back in time
Now, let's imagine that we want to "go back in time" to a particular commit. This would allow us to run code as it was at some point in time (assuming no other untracked changes), view previous revisions of a document, or just see what we started with.

**_DANGER ALERT:_** If you have uncommitted changes in your working directory, the following commands can destroy them, leading to lost work. Make sure you `git status` to make sure your working directory is clean!

Let's say we want to go back to our previous revision - the parent of `HEAD`. Using the notation from diffing before, we can change our reference commit with `git checkout HEAD~`. You should see some things about a `'detached HEAD' state`, which means that our `HEAD` is not at the "tip" (meaning the latest commit) of any branch. (The only branch we have at the moment is `master`, the default.) You should also see the hash and commit message of our new location.

Let's verify that our local copy is in fact the previous version of the script. You can print the file to the screen with `cat hello.sh` and see that is indeed the case. At this point, you could run code or do whatever you wanted to go back in time for. You can even begin a new branch (more on that later) and use this as a jumping off point for different changes than what you've already made. For now, to return to where we were before, we can `checkout` the reference we left at the tip of the branch with `git checkout master`.

### Reverting a change
Ok, let's pretend like you really don't like those additions you made, and you want your `master` branch to reflect the previous version of the file. One way to do this is with `git revert HEAD`, which will add a new commit "reversing" the last one (in this case, "untyping" the additional text we wrote). It will open your default text editor to allow you to set a commit message (like with `git commit -m`), which by default states the message and commit that are being reversed. This is great because even though the tip of your branch now reflects the state that you want, we actually preserve the history of changing our mind, without destroying any data.

Of course, it's possible to rewrite history and delete the commit where we added the text initially, but that leads to **_SUPER DANGER LAND_**, and we don't want to go there during this tutorial. For now, let's just be happy with the fact that we're back at our simple little "hello world."

## Working with remote repositories
### Adding a remote
Whether on your network, another drive, or on a centralized server (like GitHub), you can add a link from your local repo to a remote repo with `git remote add`, a name, and a location. If you are beginning with a local repo, it's common to initialize the remote as a bare repo (meaning it doesn't have any items or any commits) so that you can directly `git push` (or, in other words, copy) the current state of your local repo to the new location without any conflicts.

At this point, we should clarify that git can work over the internet using HTTPS or SSH, both of which have upsides and downsides. We'll talk about these in the context of GitHub, but other services (and it's not uncommon to have credentials with multiple servers) would be approached in much the same fashion.
* HTTPS: If you set up a credential manager earlier, the first time you authenticate, you should be able to use your username, password, and 2FA device to sign in and have your credentials securely cached. If not, you'll have to enter your username and password for authentication each and every time you connect. This is frequently easier to work with in terms of access, as SSH ports can sometimes be blocked by firewalls.
* SSH: Much like authenticating with a server, it's possible to interface with GitHub via SSH. One advantage of using SSH keys to authenticate is that they do not provide access to your actual GitHub account, meaning there is some additional security in terms of recovery in case a key is compromised. It can also allow for finer-grained access control. On the other hand, generating and securing SSH keys (such as with ssh-agent) is technically demanding, and the authentication process at the command line may not be as frictionless as with HTTPS.

For example, let's make a new bare repo on GitHub and use it as a remote for our toy repo.

### `git fetch`, `git pull`

### `git clone`
This is the most common way to get a remote repository onto your local machine, but it's more-or-less the same thing as copying the content and adding the source repository as a remote so that you can continue to interact with the "upstream" (often shared) version. Therefore, it's also valid to download a zip of the whole repo (including the .git folder), decompress it where you want, and manually `git remote add` as before.

### What is this webpage?
I'm glad you asked! In fact, this page was automagically generated by GitHub Pages from a repo that you can find [here](https://github.com/1ceaham/dAlembertGitTutorial). Since this page is a regular repo, you can see its history, `git clone` it to your machine, modify it, add your own commits, and generally do whatever you need to with it! You are, of course, bound by the license of the content - for example, on GitHub, if a repo does not have a license, then it is copyrighted and you do not have the right to use, modify or redistribute it, only to read it (even then, only on GitHub).

(If you want a separate class on how to set up GitHub Pages for your free static hosting needs - an academic website, perhaps? - please let me know and we can discuss scheduling another seminar!)

## Collaboration
### Branches
Branches are basically a way to save and switch between different versions of the same repo. This can allow you to work on multiple ideas at the same time without having to worry about their dependencies. They are also often used as a way to distinguish the "doneness" of a given version of the repo.

For example, `master` (the default name of the branch you are on when initializing a repo) is often used as the "release" or "production-ready" branch, meaning it is always ready for end users to deploy. Conversely, `development` is often the "working" branch, where things are integrated and tested, and "feature branches" (which would be named after a given feature, like `add-pizza`) are where a singular idea is being worked on. The idea here is that it's painless to change between all of these (as long as everything is committed), so that you can context switch without having to think about file management.

Eventually, in this model, features that are finished (say you successfully `add-pizza`) get merged into `development`, and when all of the bugs have been ironed out and it's time to make a release, that is finally merged into `master` where most people will go to get the latest stable version of the codebase.

**This is perhaps the second most important reason to use a VCS.**

`git checkout -b`

`git switch <branchname>`

### GitHub "Fork and Pull" Model
Collaboration on GitHub often takes a branch-centered workflow as above. But what happens when you are not a collaborator on a repo you would like to contribute to? You can fork it (thus creating a copy under your own account), and assuming a permissive license, modify it however you like. Frequently, this part of the work is done just as above, with branches. Then, using the web interface, you can file a "Pull Request" to merge your forked, branched version back into the original repo. The owner can respond, perhaps offering comments or suggestions, and if they like the changes you've made, can bring them into the original repo.

### Other Methods
Of course, collaboration is not limited to these forms. Some groups use [mailing lists](https://en.wikipedia.org/wiki/Linux_kernel_mailing_list) to send [patches](https://git-send-email.io) via email, including comments, discussion, and logging. Some services introduce webhooks or enforce continuous integration (CI) rules that can automatically test, build, and deploy complex software projects, making it possible to fix a bug from your cell phone. 

## More resources
Want to know how git actually works under the hood? [Git from the inside out](https://codewords.recurse.com/issues/two/git-from-the-inside-out). Hint: it's hashes all the way down.

[How to undo (almost) anything in git.](https://github.blog/2015-06-08-how-to-undo-almost-anything-with-git/) You know, just in case you make a mistake.

For reference, a [GitHub-written cheat sheet](https://github.github.com/training-kit/downloads/github-git-cheat-sheet/) with some useful definitions and reminders.

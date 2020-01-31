# Interactive tutorial
## Getting started with git
### Make a project directory
* `pwd`
* `mkdir myTestRepo`
* `cd myTestRepo`.

### Initialize a repo
* `git init`
* `ls -a`: `.git`

### Add content
* `vi hello.sh` - Open a new file in vi (or vim) named `hello.sh`.
* `i` - Enter "Insert" mode.
* Type (or copy/paste, if your terminal supports that):
    ```
    #!/bin/bash
    echo "Hello world"
    ```
* `Esc` - Leave "Insert" mode.
* `:wq`, `Enter` - Issue the command "Write, then quit."
* `chmod +x hello.sh`
* `./hello.sh`

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

### Working on your own code
The knowledge you've gained up until this point is all you need to version your own code locally, which is *perfectly fine*. If there's no reason for you to put your code up on the internet, then don't do it! After all, you back up your work regularly, so there shouldn't be any need to duplicate it further, right? With these tools, you can use `git init` in a directory, `git add` the files you want to track (and probably put the rest in your `.gitignore`), `git commit` to take a snapshot, and keep on developing! No more need to keep versions in different folders. When you remember that nice idea you had a few months ago, you can look at `git log` to figure out which revision had the idea, `git checkout <commit>` or `git diff <commit> <file>` to see how it was different from what you've got currently, and then hop back to whatever you were working on with `git checkout master`. If you don't care about the internet, skip straight down to the [branches](#branches) section below for a few more tools that will help you to be even more productive.

## Working with remote repos
### Adding a remote and pushing
Whether on your network, another drive, or on a centralized server (like GitHub), you can add a link from your local repo to a remote repo with `git remote add`, a name, and a location. If you are beginning with a local repo, it's common to initialize the remote as a bare repo (meaning it doesn't have any items or any commits) so that you can directly `git push` (or, in other words, copy) the current state of your local repo to the new location without any conflicts.

At this point, we should clarify that git can work over the internet using HTTPS or SSH, both of which have upsides and downsides. We'll talk about these in the context of GitHub, but other services (and it's not uncommon to have credentials with multiple servers) would be approached in much the same fashion.
* HTTPS: If you set up a credential manager earlier, the first time you authenticate, you should be able to use your username, password, and 2FA device to sign in and have your credentials securely cached. If not, you'll have to enter your username and password for authentication each and every time you connect. This is frequently easier to work with in terms of access, as SSH ports can sometimes be blocked by firewalls.
* SSH: Much like authenticating with a server, it's possible to interface with GitHub via SSH. One advantage of using SSH keys to authenticate is that they do not provide access to your actual GitHub account, meaning there is some additional security in terms of recovery in case a key is compromised. It can also allow for finer-grained access control. On the other hand, generating and securing SSH keys (such as with ssh-agent) is technically demanding, and the authentication process at the command line may not be as frictionless as with HTTPS.

For example, let's make a new bare repo on GitHub and use it as a remote for our toy repo. On GitHub, click the little "plus" and select ["New repository."](https://github.com/new) For the name, put in `myTestRepo`. (It doesn't have to be the same as the directory you created, but is still convenient.) You can make it private if you like, and skip the rest as it suggests (since we're importing the repo from our machine). Hit the "Create repository" button.

Then, just as the second option on the next page states, simply replace `Username` with your actual username below. In this case, we're naming the GitHub remote `origin`, which is a super common name for this type of workflow, and pushing our (only) branch, `master`.
```
git remote add origin https://github.com/Username/myTestRepo.git
git push -u origin master
```
You'll probably have to enter your password, or maybe do some 2FA, but in the end, the GitHub repo will have the same content as your local version. You can check this by going to https://github.com/Username/myTestRepo.

Now, every time you want to do some work locally, you can simply make a commit and `git push origin master` to send all of your changes up to GitHub.

### Pulling from a remote
GitHub also allows you to edit files through their web interface, where "saving" is the same thing as creating a new commit. Try that out and re-add some text, create a new file, or do something else to your repo to get it out-of-sync with your local copy.

Afterward, you can check for changes in the remote repo with `git fetch`. If you see that there are changes (which would obviously be more common in the case where other collaborators have made changes and pushed them to GitHub), you can reflect those changes in your local repo with `git pull`. Note that if you have local changes or if you've made commits beyond or different from what is on the remote server, you'll have to merge them before git allows you to `pull`.

### Initializing a repo from a remote
`git clone` is the most common way to get a remote repository (that you don't already have) onto your local machine. It's more-or-less the same thing as copying the content and adding the source repository as a remote so that you can continue to interact with the "upstream" (often shared) version. Therefore, it's also valid to download a zip of the whole repo (including the .git folder), decompress it where you want, and manually `git remote add` as before.

### What is this webpage?
I'm glad you asked! In fact, this page was automagically generated by GitHub Pages from a repo that you can find [here](https://github.com/1ceaham/dAlembertGitTutorial). Since this page is a regular repo, you can see its history, `git clone` it to your machine, modify it, add your own commits, and hopefully, suggest how to make it better with a pull request! You are, of course, bound by the license of the content - for example, on GitHub, if a repo does not have a license, then it is copyrighted and you do not have the right to use, modify or redistribute it, only to read it (even then, only on GitHub). You can see the license for this page [below](#license).

(If you want a separate class on how to set up GitHub Pages for your free static hosting needs - an academic website, perhaps? - please let me know and we can discuss scheduling another seminar!)

## Collaboration
### Branches
Branches are basically a way to save and switch between different versions of the same repo. This can allow you to work on multiple ideas at the same time without having to worry about their dependencies. They are also often used as a way to distinguish the "doneness" of a given version of the repo.

For example, `master` (the default name of the branch you are on when initializing a repo) is often used as the "release" or "production-ready" branch, meaning it is always ready for end users to deploy. Conversely, `development` is often the "working" branch, where things are integrated and tested, and "feature branches" (which would be named after a given feature, like `add-pizza`) are where a singular idea is being worked on. The idea here is that it's painless to change between all of these (as long as everything is committed), so that you can context switch without having to think about file management.

Eventually, in this model, features that are finished (say you successfully `add-pizza`) get merged into `development`, and when all of the bugs have been ironed out and it's time to make a release, that is finally merged into `master` where most people will go to get the latest stable version of the codebase. The advantage of this workflow is that individual branches can progress at their own speed, and only have to be integrated at the point where they merge.

If you think back to earlier, when we went "back in time" by moving our `HEAD` reference to a previous commit, we had the option to begin a new series of commits on a branch. In one step, you can create and switch to that new branch with `git checkout -b <branchname>`. In this sense, you can think of branches as labels on particular commits in the history graph of your repository. When you go back to a commit and start a new branch (for a feature perhaps), you're essentially just adding a new label at that commit that you can refer to by name. When you then add a few new commits, the label comes along with you and stays at the tip of the branch. When that branch is eventually merged back in somewhere else, that label is concurrent with the label of the branch that it was merged into, and can therefore be deleted (though it will still be present in git's `reflog` history).

One final note is that people use branches for all kinds of things, and they don't even have to be related. Some [repos](https://en.wikipedia.org/wiki/Monorepo) will even contain multiple projects on different branches. At the end of the day, a branch is just a label for a particular commit, even if it has the "significance" of standing for all of the history between that commit and where it left a common ancestor. In fact, branches can even represent a snapshot without any ancestry: imagine a completely separate directory superimposed on the first, where all it takes to switch between the two is a `git switch <branchname>` (or `git checkout <branchname>` in older versions of git).

### GitHub "Fork and Pull" Model
Collaboration on GitHub often takes a branch-centered workflow as above. But what happens when you are not a collaborator on a repo you would like to contribute to? You can fork it (thus creating a copy under your own account), and assuming a permissive license, modify it however you like. Frequently, this part of the work is done just as above, with branches. Then, using the web interface, you can file a "Pull Request" to merge your forked, branched version back into the original repo. The owner can respond, perhaps offering comments or suggestions, and if they like the changes you've made, can bring them into the original repo.

### Other Methods
Of course, collaboration is not limited to these forms. Some groups use [mailing lists](https://en.wikipedia.org/wiki/Linux_kernel_mailing_list) to send [patches](https://git-send-email.io) via email, including comments, discussion, and logging. Some services introduce webhooks or enforce continuous integration (CI) rules that can automatically test, build, and deploy complex software projects, making it possible to fix a bug from your cell phone.

## More resources
Want to know how git actually works under the hood? [Git from the inside out](https://codewords.recurse.com/issues/two/git-from-the-inside-out). Hint: it's hashes all the way down.

[How to undo (almost) anything in git.](https://github.blog/2015-06-08-how-to-undo-almost-anything-with-git/) You know, just in case you make a mistake.

For reference, a [GitHub-written cheat sheet](https://github.github.com/training-kit/downloads/github-git-cheat-sheet/) with some useful definitions and reminders.

## License
[![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

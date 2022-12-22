---
title: 20 Git Commands You (Probably) Didn't Know About 🧙‍♂️
date: 2022-12-18 14:48:01
tags:
    - Git
---
![$cover](images/git.webp)

If you've ever browsed the [git manual](https://git-scm.com/docs) (or run `man git`), then you'll have noticed there's a whole lot more to git than what most of us use on a daily basis. A lot of these commands are incredibly powerful and can make your life a lot easier (others are a bit niche, but still good to know).

> This post outlines 20 of my favourite under-used git features, which you can use to level up your development process, impress your colleagues, help you answer git interview questions, and most importantly - have fun with!

### [](#contents-and-tldr)Contents (and TL;DR)

*   [Git Web](#git-web) - _Open builtin GUI_
*   [Git Notes](#git-notes) - _Attach extra info to commits_
*   [Git Bisect](#git-bisect) - _Debug like a pro_
*   [Git Grep](#git-grep) - _Search for anything_
*   [Git Archive](#git-archive) - _Compress project for sharing_
*   [Git Submodules](#git-submodules) - _Import other repos into yours_
*   [Git Bugreport](#git-bugreport) - _Compile bug report with system info_
*   [Git Fsck](#git-fsck) - _Verify and recover unreachable objects_
*   [Git Stripspace](#git-stripspace) - _Remove trailing whitespaces_
*   [Git Diff](#git-diff) - _Compare changes between two files_
*   [Git Hooks](#git-hooks) - _Execute script when a git command is run_
*   [Git Blame](#git-blame) - _Show who wrote a given line_
*   [Git Large File Storage](#git-lfs) - _Store big files in git_
*   [Git Garbage Collection](#git-gc) - _Optimize your git repo_
*   [Git Show](#git-show) - _Easily inspect any git object_
*   [Git Describe](#git-describe) - _Give readable name based on last tag_
*   [Git Tag](#git-tag) - _Create version tags at specific points_
*   [Git Reflog](#git-reflog) - _List all git actions made on a repo_
*   [Git Log](#git-log) - _View commit log, and branch diagrams_
*   [Git Cherry Pick](#git-cherry-pick) - _Pull a feature into your branch_
*   [Git Switch](#git-switch) - _Quickly jump between branches_
*   [Bonus](#bonus) - _Extend git with more commands!_

* * *

[](#git-web)Git Web
-------------------

> Run [`git instaweb`](https://git-scm.com/docs/git-instaweb) to instantly browse your working repository in gitweb

Git has a built-in [web-based visualiser](https://git-scm.com/docs/gitweb) for browsing local repositories, which lets you view and manage your repo via a GUI in the browser. It's packed with useful features, including:

*   Browsing and stepping through revisions and inspecting diffs, file contents and metadata
*   Viewing commit logs, branches, directories, file history, and attached data visually
*   Generating RSS or Atom feeds of commits and repository activity logs
*   Searching commits, files, changes and diffs

To open it, just run `git instaweb` from within your repo. Your browser should pop open, and load `http://localhost:1234`. If you don't have Lighttpd installed, you can specify an alternative web server with the `-d` flag. Other options can be configured either via flags (like `-p` for port, `-b` for the browser to open, etc), or under the `[instaweb]` block in your git config.

There's also the `git gui` command, which can open up a GUI-based git app

[![Screenshot of Git GUI](https://res.cloudinary.com/practicaldev/image/fetch/s--ATnCqGFm--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://i.ibb.co/0DrmcWG/Screenshot-from-2022-12-17-20-26-30.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--ATnCqGFm--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://i.ibb.co/0DrmcWG/Screenshot-from-2022-12-17-20-26-30.png)

* * *

[](#git-notes)Git Notes
-----------------------

> Use [`git notes`](https://git-scm.com/docs/git-notes) to add extra info to commits

Sometimes you need to attach additional data to a git commit (beyond just the changes, message, date-time and author info).

The notes are stored within `.git/refs/notes`, and since that's separate from the commit object data, you can modify the notes associated with a commit at anytime, without the SHA-1 hash changing.

You can view notes with `git log`, using most git GUI apps, or with the `git notes show` command. Some git hosts also show notes in the commit view (although [GH no longer displays notes](https://github.blog/2010-08-25-git-notes-display/)).

* * *

[](#git-bisect)Git Bisect
-------------------------

> With [`git bisect`](https://git-scm.com/docs/git-bisect) you can find the commit that introduced a bug using binary search

This is one of the most powerful, yet easy to use git commands- bisect is an absolute life saver when it comes to debugging. After starting a bisect, it checks out commits for you, and you tell it weather that commit is `good` (no bug), or `bad` (bug introduced), which lets you narrow down the the earliest commit where the bug is present.

To get started, run `git bisect start` then pass it a known good commit with `git bisect good <commit-hash>` and a known bad one (defaults to current) with `git bisect bad <optional-hash>`. It will then checkout the commit in-between the good and bad commits, then you specify weather the bug is present with either `git bisect good` or `git bisect bad`. It will then repeat the process, checking out a commit in the center of the bad and good, all the way until you've found the exact commit that introduced the bug. Cancel anytime with `git bisect reset`.

There's much more to the bisect command, including replays, viewing commits, skipping, so it's worth checking out the docs next time your debugging.

* * *

[](#git-grep)Git Grep
---------------------

> Use [`git grep`](https://git-scm.com/docs/git-grep) to search for code, files, commits or anything else across your repo

Ever find yourself needing to search for a string anywhere within a git project? With git grep you can easily search for any string, or RegEx throughout your project, and across branches (like a more powerful Ctrl + F!).

`git grep <regexp> <ref>`

It includes plenty of [options](https://git-scm.com/docs/git-grep#_options) to narrow down your search, or specify results format. For example, use `-l` to only return file names, `-c` to specify number of matches per file to return, `-e` to exclude results matching a condition, `--and` to specify multiple conditions, `-n` to search with line number.

Since git grep is regex-compatible, you can get much more advanced with the string your searching for.  
You can also use it to specify a file extension, like `git grep 'console.log' *.js` which will show all console.logs from within JavaScript files

The second parameter is a ref, and can be a branch name, commit, range of commits, or anything else. E.g. `git grep "foo" HEAD~1` will search the previous commit.

* * *

[](#git-archive)Git Archive
---------------------------

> Use [`git archive`](https://git-scm.com/docs/git-archive) to combine an entire repo into a single file

When sharing or backing up a repository, it's often preferred to store it as a single file. Using git archive will include all repo history, so it can be easily extracted back to it's original form. The command also includes a lot of additional options, so you can customise exactly what files are and aren't included in the archive.  

    git archive --format=tar --output=./my-archive HEAD
    

Enter fullscreen mode Exit fullscreen mode

* * *

[](#git-submodules)Git Submodules
---------------------------------

> Use [`git submodule`](https://git-scm.com/docs/git-submodule) to pull any other repository into yours

In git, [submodules](https://git-scm.com/docs/gitsubmodules) let you mount one repo into another, and is commonly used for core dependencies or splitting components into separate repositories. For more info, see [this post](https://notes.aliciasykes.com/17996/quick-tip-git-submodules).

Running the following command will pull a module into the specified location, and also create a `.gitmodules` file so that it's always downloaded when the repo is cloned. Use the `--recursive` flag to include sub-modules when cloning the repo.  

    git submodule add https://github.com/<user>/<repo> <path/to/save/at>
    

Enter fullscreen mode Exit fullscreen mode

There's also [`git subtree`](https://www.atlassian.com/git/tutorials/git-subtree), which does a similar thing, but without the need for metadata files.

* * *

[](#git-bug-report)Git Bug Report
---------------------------------

> Use [`git bugreport`](https://git-scm.com/docs/git-bugreport) to compose a bug ticket, including git and system info

This command will capture system info, and then open up a standard bug template (steps to reproduce, actual + expected output, etc). The completed file should be a very complete bug report, with all necessary info captured.

This is very handy if your a maintainer for an open source package and asking a user (developer) to raise a bug report, as it ensures all necessary data is included.

And if you are raising a bug report to the core git system, you can also run the [`git diagnose`](https://git-scm.com/docs/git-diagnose) command, and then raise your issue [here](https://github.com/git/git).

* * *

[](#git-fsck)Git Fsck
---------------------

> Use [`git fsck`](https://git-scm.com/docs/git-fsck) to check all objects, or recover unreachable ones

Although not often needed, sometimes you may have to verify the objects stored by git. This is where fsck (or File System ChecK) comes in, it tests the object database and verifies the SHA-1 ID of all objects and the connections they make.

It can also be used alongside the `--unreachable` flag to find objects that are no longer reachable from any named reference (since unlike other commands, it includes everything in `.git/objects`).

* * *

[](#git-stripspace)Git Stripspace
---------------------------------

> Use [`git stripspace`](https://git-scm.com/docs/git-stripspace) to format whitespaces within a given file

Best practice is to avoid trailing whitespaces at the end of lines, avoid having multiple consecutive blank lines, avoid empty lines from beginning and end of an input, and end each file with a new line. There's plenty of language-specific tools which do this automatically for you (like prettier), but Git also has this functionality builtin.

It's intended for metadata (commit messages, tags, branch descriptions, etc), but also works if you pipe a file to it, then pipe the response back to a file. E.g. `cat ./path-to-file.txt | git stripspace` or `git stripspace < dirty-file.txt > clean-file.txt`

You can also use this to remove comments (with `--strip-comments`), or even comment out lines (with `--comment-lines`).

* * *

[](#git-diff)Git Diff
---------------------

> With [`git diff`](https://git-scm.com/docs/git-diff) you can compare the difference between 2 sets of code

You're probably aware that you you can run `git diff` to show all changes since the last commit, or use `git diff <commit-sha>` to compare either 2 commits, or 1 commit to the HEAD. But there's much more you can do with the diff command.

You can also use it to compare any two arbitrary files, with `diff file-1.txt file-2.txt` (no more visiting [diffchecker.com](https://www.diffchecker.com/compare/)!)

Or compare 2 branches, or refs with each other, using `git diff branch1..branch2`

Note that a double dot (`..`) is the same as a space and indicates the diff input should be the tip of the branches, but you can also use a triple dot (`...`) to convert the first parameter into a ref of the shared common ancestor commit between the two diff inputs - very useful! If you want to only compare a single file across branches, just pass the files name in as the third argument.

You may want to see all changes made within a given date range, for this use `git diff HEAD@{7.day.ago} HEAD@{0}` (for the last week), again this can be paired up with a filename, branch names, specific commits or any other ref.

There's also the [`git range-diff`](https://www.git-scm.com/docs/git-range-diff) command, which provides a simple interface for comparing commit ranges.

There's much more to the git diff tool (as well as the option of using your own diff checker), so I recommend checking out [the docs](https://git-scm.com/docs/git-diff#_description).

* * *

[](#git-hooks)Git Hooks
-----------------------

> Use [`hooks`](https://git-scm.com/docs/githooks) to execute command or run scripts when a given get action occurs

Hooks let you automate pretty much anything. For example: ensuring standards are met (commit message, branch name, patch size), code quality (tests, lint), appending additional info to a commit (user, device, ticket ID), calling a webhook to record an event or run a pipeline, etc.

There's pre and post [hooks available](https://git-scm.com/docs/githooks) for most git events, like commit, rebase, merge, push, update, applypatch, etc.

Hooks are stored in `.git/hooks` (unless you configure them elsewhere with `git config core.hooksPath`), and can be tested with the [`git hook`](https://git-scm.com/docs/git-hook) command. Since they're just shell files, they can be used to run any command.

Hooks aren't pushed to the remote repository, so to share and manage them across your team, you'll need to use a [hook manager](https://github.com/aitemr/awesome-git-hooks#tools), like [lefthook](https://github.com/evilmartians/lefthook) or [husky](https://github.com/typicode/husky). There's also several [3rd-party tools](https://githooks.com/#projects), which make managing hooks easier, I recommend [overcommit](https://github.com/sds/overcommit).

Remember, hooks can always be skipped (with the `--no-verify` flag), so never rely purely on hooks, especially for anything security related.

* * *

[](#git-blame)Git Blame
-----------------------

> Use [`git blame`](https://git-scm.com/docs/git-blame) to show author info for a specific revision and line

A classic, quickly find out who wrote a specific line of code (aka which of your co-workers to blame for the bug!). But it's also useful to determine at which point in time something changed and inspect that commit and associated metadata.

For example, to view author and commit info for line 400 to 420 of index.rs, you'd run:  

    git blame -L 400,420 index.rs
    

Enter fullscreen mode Exit fullscreen mode

* * *

[](#git-lfs)Git LFS
-------------------

> Store large files using [`git lfs`](https://git-lfs.github.com/) to not bog down your repo

Often your project will contain larger files (such as databases, binary assets, archives or media files), which would slow down the git workflow and max out usage limits. That's where [Large File Storage](https://git-lfs.github.com/) comes in - it enables you to store these large assets elsewhere, while keeping them trackable with git and maintaining the same access controls/ permissions. LFS works by replacing these larger files with text pointers that are tracked within git.

To use it, just run `git lfs track <file glob>`, which will update your `.gitattributes` file. You can specify files by their extension (e.g. `*.psd`), directory, or individually. Running `git lfs ls-files` to view a list of tracked LFS files.

* * *

[](#git-gc)Git GC
-----------------

> Use [`git gc`](https://git-scm.com/docs/git-gc) to optimize your repository

Over time git repos accumulate various types of garbage, which take up disk space, and slow down actions. That's where the built-in garbage collector comes in. Running `git gc` will remove orphaned and inaccessible commits (with [`git prune`](https://git-scm.com/docs/git-prune)), compress file revisions and stored git objects, as well as some other general house keeping tasks like packing refs, pruning reflog, revere metadata or stale working trees and updating indexes.

Adding the `--aggressive` flag will [aggressively optimize](https://git-scm.com/docs/git-gc#_aggressive) the repository, throwing away any existing deltas and re-computing them, this takes much longer to run but may be needed if you've got a large repository.

* * *

[](#git-show)Git Show
---------------------

> Use [`git show`](https://git-scm.com/docs/git-show) to easily inspect any git object

Outputs objects (blobs, treees, tags or commits) in an easily readable form. To use, just run `git show <object>`. You'll likely also want to append the `--pretty` flag, for a clearer output, but there's many other options available to customize the output (with `--format`), so this command can be extremely powerful for displaying exactly what you need.

An instance that this is very useful for, is previewing a file in another branch, without switching branches. Just run `git show branch:file`

* * *

[](#git-describe)Git Describe
-----------------------------

> Use [`git describe`](https://git-scm.com/docs/git-describe) to find the latest tag reachable from a commit, and give it a human-readable name

Run `git describe` and you'll see a human-readable string made from combining the last tag name, with the current commit, to generate a string. You can also pass a specific tag to it,

Note that you must have created tags for this to work, unless you append the `--all` flag. Git describe will also only use annotated tags by default, so you must specify the `--tags` flag to make it use lightweight tags as well.

* * *

[](#git-tag)Git Tag
-------------------

> Tag a specific point in your repo's history using [`git tag`](https://git-scm.com/docs/git-tag)

It's often useful to be able to [tag](https://git-scm.com/book/en/v2/Git-Basics-Tagging) specific, important points in a repository’s history most commonly used to denote versions of releases. Creating a tag is as simple as `git tag <tagname>`, or you can tag a historical commit with `git tag -a v4.2.0 <commit sha>`. Like with commits, you can include a message alongside a tag, using `-m`.

Don't forget to push your tag to remote, with `git push origin <tagname>`.  
To list all tags, just run `git tag`, and optionally use `-l` for a wildcard search.  
You'll then be able to checkout a specific tag, with `git checkout <tagname>`

* * *

[](#git-reflog)Git Reflog
-------------------------

> List all updates made on your repo using [`git reflog`](https://git-scm.com/docs/git-reflog)

Git keeps track of updates to the tip of branches using a mechanism called reference logs, or "reflogs". Various events are tracked, including: clone, pull, push, commit, checkout and merge. It's often useful to be able to find an events reference, as many commands accept a ref as a parameter. Just run `git reflog` to view recent events on `HEAD`.

One thing that reflog is really useful for is recovering lost commits. Git never really loses anything, even when rewriting history (like rebasing or commit amending). Reflog allows you to go back to commits even though they are not referenced by any branch or tag.

By default reflog uses `HEAD` (your current branch), but you can run reflog on any ref. For example `git reflog show <branch name>`, or to see stashed changes with `git reflog stash`. Or show all references with `git reflog show --all`

* * *

[](#git-log)Git Log
-------------------

> Use [`git log`](https://git-scm.com/docs/git-log) to view a list of commits

You're likely already familiar with running `git log` to view a list of recent commits on your current branch. But there's a few things more you can do with git log.

Using `git log --graph --decorate --oneline` will show a nice neat commit graph along with ref pointers.

[![example git log output](https://res.cloudinary.com/practicaldev/image/fetch/s--EtfqYNeJ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://i.ibb.co/c1WByg8/Screenshot-from-2022-12-17-20-43-56.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--EtfqYNeJ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://i.ibb.co/c1WByg8/Screenshot-from-2022-12-17-20-43-56.png)

You also often need to be able to filter logs based on various parameters, the most useful of which are:

*   `git log --search="<anything>"` - Search logs for specific code changes
*   `git log --author="<pattern>"` - Show log only for specific author(s)
*   `git log --grep="<pattern>"` - Filter log using search term or regex
*   `git log <since>..<until>` - Show all commits between two references
*   `git log -- <file>` - Show all commits made only to a specific file

Or, just run `git shortlog` for a summerized list of commits.

* * *

[](#git-cherry-pick)Git Cherry Pick
-----------------------------------

> Use [`git cherry-pick`](https://git-scm.com/docs/git-cherry-pick) to pick specified commit(s) by reference and append them to the working HEAD

Sometimes you need to pull a specific commit from elsewhere, into your current branch. This can be very useful for applying hot fixes, undoing changes, restoring lost commits and in certain team collaboration settings. Note that often traditional merges are better practice, since cherry picking commits can cause duplicate commits in the log.

Usage is straightforward, just run `git cherry-pick <commit-hash>`. This will pull the specified commit into your current branch.

* * *

[](#git-switch)Git Switch
-------------------------

> Use [`git switch`](https://git-scm.com/docs/git-switch)

Moving between branches is something that we do often, the `switch` command is like a simplified version of `git checkout`, it can be used to create and navigate between branches, but unlike checkout does not copy modified files when you move between branches.

Similar to `checkout -b`, with the switch command you can append the `-c` flag to create a new branch, and jump strait into it, e.g. `git switch -c <new branch>`. And running `git switch -` will discard any experimental changes you've made, and return you to your previous branch.

* * *

[](#git-standup)Git Standup
---------------------------

> Use [`git standup`](https://github.com/kamranahmedse/git-standup) to recall what you did on the last working day , based on git commits

I've put this one at the end, as it's not included with most git clients, but you can [easily install it](https://github.com/kamranahmedse/git-standup#install) either with your systems package manager, using a 1-line curl script, or by building from source.

If your boss requires you do a daily standup, to give updates on yesterdays work, but you can never remember what you actually did - this one is for you! It'll show a nicely formatted list of everything done in a given time frame. Usage is simple, just run `git standup`, or use [these options](https://github.com/kamranahmedse/git-standup#options) to specify what data should be shown (author, timeframe, branches, etc.

* * *

[](#bonus)Bonus
---------------

Git can be easily extended with add-ons, to add extra commands that do useful tasks. One of the most complete extensions is [git-extras](https://github.com/tj/git-extras/blob/master/Commands.md) by [@tj](https://github.com/tj). It gives you 70+ [extra commands](https://github.com/tj/git-extras/blob/master/Commands.md) to automate common git jobs.

For a list of more useful git add-ons, see [stevemao/awesome-git-addons](https://github.com/stevemao/awesome-git-addons).

If you're working with GitHub repos, then the [GitHub CLI](https://cli.github.com/) let's you do common tasks (managing PRs, issues, code reviews, etc) from the command line.

* * *

If you like this kind of stuff,  
consider following for more :)

[![Follow Lissy93 on GitHub](https://res.cloudinary.com/practicaldev/image/fetch/s--8_HGggCT--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://img.shields.io/badge/-Lissy93-3a3a3a%3Fstyle%3Dflat%26logo%3DGitHub%26logoColor%3Dwhite)](https://github.com/Lissy93)[![Follow Lissy_Sykes on Twitter](https://res.cloudinary.com/practicaldev/image/fetch/s--BhCWBIgy--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://img.shields.io/badge/-%40Lissy_Sykes-00acee%3Fstyle%3Dflat%26logo%3DTwitter%26logoColor%3Dwhite)](https://twitter.com/Lissy_Sykes)
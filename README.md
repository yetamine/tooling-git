# Tooling for Git

This repository contains useful commands for common Git-related tasks.


## Description

All the commands follow the well-established Git philosophy to provide rather simpler, but versatile building blocks for more complex tasks. The commands are most useful when combined effectively (see some recipes below). Following commands are currently available:

* `git-ext-each`: lists all repositories in a directory tree and optionally executes an action
* `git-ext-exec`: executes a command for all repositories from the given list
* `git-ext-init`: sets up new repository according to Yetamine Flow
* `git-ext-test`: provides convenient tests for the most common cases

All tools provide `-h` or `--help` options to display usage details. Note that the commands are meant for humans and therefore may evolve and change, including their parameters.


## Installation

There is no actual installation, the commands can be run without additional actions, assuming that Git and Bash are available. However, it is convenient to adjust the `PATH` environment variable to include the location of the commands. Including the location in `PATH` triggers shell completion support, so that both `git-ext-` and `git ext-` works (just `git ext-` form does not recognize `-h` option, hence use `--help` instead).


## Recipes

Imagine having a workspace with a couple of repositories. Here are a few common tasks that you might want to perform:

```bash
# List all repositories with uncommitted changes
$ git ext-each | git ext-test --not is-clean

# Execute 'git pull' on all repositories
$ git ext-each | git ext-exec git pull
```

The nice thing of `git-ext-exec` is that it allows you providing the command to execute as a verbatim text, which makes composed commands with many special characters much easier:

```bash
$ git ext-each | git ext-test at-branch feature/XYZ | git ext-exec 'git flow feature finish && git push'
```

Conditional listing all repositories is a frequent operation and many bulk actions start with it. Therefore it may be useful to add to your `.bashrc` something like:

```bash
function git-list-if () {
    git ext-each | git ext-test "$@"
}
```

The previous example can be then simplified a bit:

```bash
$ git-list-if at-branch feature/XYZ | git ext-exec 'git flow feature finish && git push'
```


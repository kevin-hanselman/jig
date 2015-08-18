# jig
Stash Mercurial uncommitted changes. Simple, safe, and branch-friendly.

## Motivation
`hg shelve` can destroy your work, especially if you're working on multiple branches. `mq` is too much when all you need is a simple shelve tool.

The simpler and safer solution is to shelve using patch files. This script is a thin wrapper around built-in Mercurial commands for importing and exporting patch files.

## Usage

```
usage: jig [options] command [name]

Stash Mercurial working directory changes.
Simple, safe, and branch-friendly.

Commands:
  stash             export changes to a patch file
  unstash           import changes from a patch file
  list              list patch files
  show              show the contents of a patch file

Arguments:
  name              name of the patch file to use
                    (excluding file extension)
                    (defaults to the current branch name)

Options:
  -h, --help        print this text and exit
  -f, --force       overwrite an existing patch file
```

## Implementation

If you're worried that this script is going to destroy your repository and run `rm -rf /`, I recommend looking over the script. At about 100 LOC it makes for a quick read. Here's the gist:

### Stash
`jig stash` essentially runs:
```
hg diff > $patchfile
hg revert --all
```

### Unstash
`jig unstash` essentially runs:
```
hg import --no-commit $patchfile
```

## Installation
First, ensure the `jig` script is in your `$PATH`; either `mv`/`cp` the script or add your repo clone to your `$PATH`.

To use `jig` like a Mercurial sub-command, add aliases to your `hgrc`:

```
[alias]
stash    = !jig stash $@
unstash  = !jig unstash $@
jig-list = !jig list $@
jig-show = !jig show $@
```

## See Also

* [StackOverflow #1](http://stackoverflow.com/q/11520047/857893)
* [StackOverflow #2](http://stackoverflow.com/q/5145174/857893)
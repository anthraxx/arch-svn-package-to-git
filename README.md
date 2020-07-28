Arch Linux svn package to git converter
=======================================

Simple script that converts a git-svn checkout of a whole svn repository to
separated per package git repositories. Additionally it performs some garbage
collection and history/content rewrite.

### Requirements:
- Python 3.8
- git-filter-repo (this package is in `community`)

### Rewrites:
- Replace author name and e-mail via `AUTHORS` file
- Strip git-svn related git-svn-id from commit messages
- Strip svn specific $Id$ property

### GitSVN checkout:

The gitsvn checkout must be a bare clone that is setup as a mirror in order
to make single branch cloning work appropriately and be able to easily fetch
and update from the remote for all branch related refs.

git config:
```
[remote "origin"]
	url = https://github.com/archlinux/svntogit-community
	fetch = +refs/*:refs/*
	mirror = true
```


### Usage:

    gitsvn2git INPUT_GIT TARGET_DIR [PACKAGE]...

    gitsvn2git INPUT_GIT --list-packages


### Arguments:

- `INPUT_GIT [DIRECTORY]`
**synopsis:** Set the input directory of the git-svn bare repository checkout
**required:** Yes


- `TARGET_DIR [DIRECTORY]`
**synopsis:** Set the target directory where the repositories will be created
**required:** Yes


- `PACKAGE [STRING]...`
**synopsis:** Set the packages that should be converted to separate git repositories
**default:** All packages of the specified input repository


### Examples:
##### Whole repository:

    gitsvn2git repos/community community-converted

##### Specific packages:

    gitsvn2git repos/community community-converted capstone radare2

##### List bare clone packages:

    gitsvn2git repos/community --list-packages

##### Parallelize:

    parallel gitsvn2git repos/community community-converted ::: repos/community/*

    parallel gitsvn2git repos/community community-converted ::: $(gitsvn2git repos/community --list-packages)

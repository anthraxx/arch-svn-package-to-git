Arch Linux svn package to git converter
=======================================

Simple script that converts a git-svn checkout of a whole svn repository to
separated per package git repositories. Additionally it performs some garbage
collection and history/content rewrite.

### Requirements:
- Python 3.8
- git
- git-filter-repo
- bash
- findutils
- coreutils
- grep

### Rewrites:
- Replace author name and e-mail via `AUTHORS` file
- Strip git-svn related git-svn-id from commit messages
- Strip svn specific $Id$ property
- Tag all repository releases

### GitSVN checkout:

The gitsvn checkout must be a bare clone that is setup as a mirror in order
to make single branch cloning work appropriately and be able to easily fetch
all branch related refs from the remote.

git config:
```
[remote "origin"]
	url = https://github.com/archlinux/svntogit-community
	fetch = +refs/*:refs/*
	mirror = true
```

The gitsvn-repo-helper can aid in setting the require config:

```
gitsvn-repo-helper configure repos/community
```


### Usage:

    gitsvn2git INPUT_GIT TARGET_DIR [PACKAGE]...


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


### Environment variables:

- `GITSVN2GIT_BREAKPOINT_ON_ERROR`
**synopsis:** If set, invoke an interactive source code debugger for investigation

- `GITSVN2GIT_DEBUG`
**synopsis:** If set, print debug messages during execution

- `GITSVN2GIT_PEDANTIC`
**synopsis:** If set, treat some warnings like missing release tags as errors. Useful in combination with `GITSVN2GIT_BREAKPOINT_ON_ERROR` for debugging.

- `GITSVN2GIT_WERROR`
**synopsis:** If set, treat all warnings as errors. Useful in combination with `GITSVN2GIT_BREAKPOINT_ON_ERROR` for debugging.


### Examples:
##### Whole repository:

    gitsvn2git repos/community community-converted

##### Specific packages:

    gitsvn2git repos/community community-converted capstone radare2

##### Parallelize:

    parallel gitsvn2git repos/community community-converted ::: repos/community/*

    parallel gitsvn2git repos/community community-converted ::: $(gitsvn-repo-helper list repos/community)

##### List bare clone packages:

    gitsvn-repo-helper list repos/community

##### List bare clone package changes since last fetch:

    gitsvn-repo-helper since bec4565de repos/community

##### Fetch bare clone repo and print previous HEAD:

    gitsvn-repo-helper fetch repos/community

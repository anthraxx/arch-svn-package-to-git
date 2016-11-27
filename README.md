Arch Linux svn package to git converter
=======================================

Simple script that converts a git-svn checkout of a whole svn repository to separated per package git repositories.
Additionally it performs some garbage collection and history/content rewrite.

### Rewrites:

- Replace author name and e-mail via ```AUTHORS``` file
- Strip git-svn related git-svn-id from commit messages
- Strip svn specific $Id$ property


### Usage:

    gitsvn2git INPUT_GIT TARGET_DIR [PACKAGE]...


### Options:

- `INPUT_GIT [DIRECTORY]`  
**synopsis:** Set the input directory of the git-svn repository checkout  
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


##### Parallelize:

    cd repos/community
    parallel gitsvn2git . ../../community-converted ::: *



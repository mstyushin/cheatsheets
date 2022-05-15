Git cheatsheet
==============

-   Delete branch **branchName** locally&remotely:

        $ git push origin --delete branchName

-   Undo last commit (which hasn't been pushed yet):

        $ git reset --soft HEAD~1

-   List local branches:

        $ git branch -a

-   List remote branches:

        $ git branch -r

-   Pull submodules:

        $ git submodule foreach git pull origin master

-   Get current branchname:

        $ git rev-parse --abbrev-ref HEAD

-   Get LOC:

        $ git ls-files | xargs wc -l

-   Fetch all tags:

        $ git fetch --all --tags --prune

-   Checkout to specific tag:

        $ git checkout tags/TAGNAME

-   Checkout pullrequest: 

        $ git fetch origin pull/ID/head:BRANCHNAME
        $ git checkout BRANCHNAME


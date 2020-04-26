# PLClub Website

This iteration began in November 2019. The previous site had been maintained for at least 15 years and used OMake and Java. This version uses the Haskell library Hakyll.

## Dependencies

* The `stack` build tool for Haskell
* `bibtex2html` package
* `make`

## Build instructions

Because the site is deployed as static files, building is a 2-step process. First we build the executable (simply called `site`) and then we run `site` to build the website from the source files.

First set up a `stack.yaml` file for `stack`. We have one that you can copy:

	cp stack-default.yaml stack.yaml

The compiler _should_ then be able to build with just

	stack build

This will install all necessary Haskell libraries typically compiling them from scratch. It will also install a GHC binary. This process should only need to happen once. Be prepared to wait.

When the site is built we can run

	stack exec site -- build

to build the site, which outputs the static html in *_site/*. With

	stack exec site -- watch 

we can launch a live preview server at [http://localhost:8000](http://localhost:8000). The preview server will watch the source files and recompile on-the-fly. 

Sometimes it is necessary to recompile everything from scratch. In this case run

	stack exec site -- rebuild

## Deploy instructions

**After** building the site per the instructions above, you can deploy the site with

	stack exec site deploy
	
This presumes that you have access to the *plclub* account on Eniac (i.e., `ssh plclub@eniac` should work). This process will 

	1) Backup the existing website as `html-YYYY-MM-DD.tar`
	2) Synchronize your local folder `_site/` with the appropriate remote directory

It is conceivable that the remote file permissions can become incorrect when deploying. In this case, you will need to run the two commands

	find ~/html -type f -exec chmod a+r {} +
	find ~/html -type d -exec chmod a+rx {} +

on the remote machine (such as by saving them as "fix.sh" and running `ssh plclub@eniac ./fix.sh`)

## Adding information to the site

### Adding a new PL Club meeting

Look under _meetings_

### Adding a new person

Look under _people_

### Recent publications

The recent publications is built by running a Makefile in a temporary directory and then parsing the results with Pandoc. This is hacky but works surprisingly well.

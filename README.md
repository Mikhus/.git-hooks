# Semantic Git Hooks

This repository contains implementation of git hooks as pure bash scripts.
Goal of implemented hooks is to support idea of semantic releases. So it
restricts developer from doing meaningless, improper formatted commits. As a
result it would allow automate tracking changes just in git and provides
an ability to automate changelog creation on releases.

Currently it provides the following hooks:

  - `commit-msg` This hook checks for the correct message format and performs
    spell-checking of the commit message.
  - `pre-commit` Verifies that committed changes is correctly spelled

Spell checking is based on external tools. Currently it supports `hunspell` and
`aspell`. On some systems you may be required to install one of these tools
manually.

## Installing

You may install this repository as a submodule of your repo or simply clone it
to your repo and remove `.git` directory from the cloned repo:

~~~bash
cd ./my/project/workdir
git clone git@github.com:Mikhus/.git-hooks.git
cd .git-hooks
rm -rf .git
~~~

Now you may commit and `.git-hooks` to your project repo to share it between
teammates.

## Enabling/Disabling

As simple as:

~~~bash
cd ./my/project/workdir

# Enabling
./.git-hooks/scripts/init
# or, if you want to enable only specific hooks
./.git-hooks/scripts/init commit-msg

# Disabling
./.git-hooks/scripts/reset
~~~

You may want to automatically enable hooks for everyone in your team. There is
no other way to do it only by imploding the `init` command, for example in your
install or build tool, so whenever someone will run install or build the hooks
will be automatically initialized. For example in your `Makefile`:

~~~Makefile
SCRIPT_ROOT = .git-hooks/scripts

all: init
.PHONY: all

init:
	@$(SCRIPT_ROOT)/init
.PHONY: init
~~~

Or in your `package.json`

~~~json
{
  "name": "my-cool-project",
  "version": "1.0.0",
  "scripts": {
    "start": "./.git-hooks/scripts/init ; /usr/bin/env node ./index.js",
    "install": "./.git-hooks/scripts/init"
  }
}
~~~

I guess you've caught an idea :)...

Of course it does not give a 100% warranty that improper commits will be 
published, but that may be achieved only with installing server-side hooks and
that option is not always available, so having at least this kind of protection
is better than nothing.

## License

[ISC](https://rawgit.com/Mikhus/.git-hooks/master/LICENSE)

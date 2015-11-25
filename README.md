Contribute ~~anonymously~~ pseudonymously to Git repositories over Tor.

## tl;dr ##

	yourname@yourbox:~$ . gitnonymous somename
	somename⚔ yourname@yourbox:~$ git commit_

## What is this? ##

This is a tool for management of identities that you can use to obfuscate your true identity when making Git commits and pushing to public repositories.

## Setup ##

Before you start, you'll probably want to:

 * Set up an anonymous email account with some provider.
 * Sign up to the Git hosting service you use with that anonymous email address.

To configure a new ~~anonymous~~ pseudonymous identity on your machine:

	$ ./gitnonymous-setup KEYNAME

Where `KEYNAME` is some memorable string that you will use to identify your pseudonymous ID like `baby-protector` or `elite-freedom-defender`. You can create multiple pseudonymous identities.

Be aware that `KEYNAME` is stored in the SSH public key's comment field so don't make it personally identifiable.

Then you should edit the new file in `~/.gitnonymous-KEYNAME/config` to set the email address and name of your pseudonymous identity:

	export GIT_COMMITTER_NAME="Baby Protector"
	export GIT_COMMITTER_EMAIL="protect-all-babies@anonymous-mail-provider.com"
	export GIT_AUTHOR_NAME="Baby Protector"
	export GIT_AUTHOR_EMAIL="protect-all-babies@anonymous-mail-provider.com"

## Use ##

You can symlink the `gitnonymous` and `gitnonymous-setup` commands into your `~/bin` folder or somewhere else on your `PATH` to execute them without typing the full path.

Each time you want make pseudonymous commits in the current shell:

	$ . gitnonymous KEYNAME

After that when you commit and push you will do so with the pseudonymous identity you have created, over the tor network, using the new SSH key that was created.

This command:

 * Spawns an `ssh-agent` that is limited to the current shell.
 * Adds the pseudonymous SSH key to `ssh-agent`.
 * Sets the `GIT_COMMITTER` and `GIT_AUTHOR` environment variables.
 * Sets the `GIT_SSH` environment variable to point at a configured `git-ssh-wrap` script.
 * Sets the `GIT_PROXY_COMMAND` environment variable to proxy network requests through tor.

Your prompt will be updated to reflect the configured environment:

	KEYNAME⚔ yourname@yourbox:~$

To deactivate the gitnonymous environment run the same command again:

	KEYNAME⚔ yourname@yourbox:~$ . gitnonymous KEYNAME
	yourname@yourbox:~$ _

Or just exit the current shell.

## Information leaks ##

Whilst this will help you adopt a pseudonymous virtual identity, you should be aware of the following information leaks that may still be used to try to identify you, pointed out by @ryancdotorg on Hacker News:

 * Your timezone will appear in Git commits (narrows down location).
 * Commit times will be leaked (narrows down sleeping/working hours).
 * SSH client version will be leaked to servers you connect to (shows Linux distro version and patch level).

## Dependencies ##

 * `git`
 * `tor` (install `tor` package)
 * `nc` (install `netcat-openbsd` package)
 * `ssh`
 * `bash`

## Tested ##

 * With `git` 1.9.1 on Xubuntu 14.04.

Patches welcome!

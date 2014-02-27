# git multi

**THIS IS A THOUGHT EXPERIMENT. DO NOT USE IT. UNSAFE. YOU COULD DIE.**

`git multi` aims to provide a simple interface for interacting with multiple
repositories in a single directory.

## Why in heaven would you want to do that?
Because I am a sick, sick man. Get me help.

Also:
I maintain two repositories in my `~` for dotfiles. You can see one at
https://github.com/grahamc/dotfiles. **It is worth noting that `git multi`
will work in any directory, it isn't specific to your home directory.**

The other one is sup3r s3cr3t and contains things like pgp and ssh keys, IPs
of internal servers, and a list of my victims. That one is tucked away into a
private repository heavily safeguarded on my own server. The problem is that I
still want them to be easy to use, and I want them to automatically (WITHOUT
symlinks) update my home directory.

You may have other (dangerous) reasons. This plugin COULD BE dangerous. I
should not be held responsible for your `git fuckups`. For example, being in
your home directory and running `git clean -f -x` might delete all your files.

## How does this work
It's pretty simple, actually. Git supports setting a `work_tree` config setting
which tells it where the files are actually located. Add with that `git_dir`
and you can do dirty things like this.

This system stores the git repository in `.git-multi/$name`.

## Neat! How do I use it?
Also pretty simple.

### Adding a repository
Easy peasy!

```bash
git multi add grahamc git@github.com:grahamc/dotfiles.git
```

That intializes the structure, clones it to `.git-multi/grahamc/` and checks it
out into your current directory.

### Working with the repository
The idea here is  `git multi` doesn't have to create a new interface for all
the git commands.

```bash
git multi work grahamc
```

That command drops you in to a bash shell with **$GIT_WORK_TREE** and **$GIT_DIR**
set to the proper directory.

Running `git status` will show you everything you need to know. `git commit`
`push` `pull` and whatever you like to your hearts content.

#### Hey! Why doesn't $repoA show $repoB files as un-tracked?
Becauase when you do `git multi work` (or `git multi run`, which is like the
plumbing of this command) we automatically iterate over your other repositories
and fill out $repoA's `info/exclude` file with the files other repositories track.

Because we're nice.

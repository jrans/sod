# **S**trategy for **O**n Premise **D**evelopment

Sod's Law states: "If something can go wrong, it will.", but let it not be our development process.

## Why

Firstly ask the question: "Why am I having to develop on premise?"

**If you do not have a good answer to this question stop reading this and doing your work on premise at once.**

To have to make continual contributions to develop and maintain a project over a long period of time in a sub par environment is never going to be nice. Not only will you work live there and only there forever, it will never be shared with of improved by anyone who does not have access to the server.

But, if for whatever bad reason you must, never abandon your best coding practises. 

## What

This strategy is independent of your client deployment and relies solely on version control and our best friend [Git](https://git-scm.com/).

Just like we collaborate together on a remote repository with the abilty to branch off and merge back our work in the most beautiful way, we should expect to do no less even in a more restrictive setting.

## How 

Ensure we have `git` made available on the command line in your environment. If not possible, stop reading, you're on your own.

Now from the root of your project's working space: `/path/to/project`, we will kick things off by creating a folder named `base` and instantiating this `base` repository with a `.git` folder. Simply run:

```
git init base
```

You now have an empty git repository from which to do your work.

A next step here would be to transfer over your intial source code and environment dependencies and make this your initial commit.

Alternatively import a Git project developed elsewhere, it'll just be a clone, i.e. a folder with a `.git` file that reflects past history, and now will live it's own life.

You're now ready for git version controlled development however you like it.

**Note: it's of utmost importance that only one person ever touches the `base` repository at any point in time and there must be an effective way of communicating this act to team members when one does.**

## Who

Developing in the `base` folder could be enough for you if working alone. You can happily create branches and flip between new pieces of work, merging back to the `master` when ready.

However, multiple users working in the same base folder on a shared file system **will never work**.

We need versions of our `base` repository for each user's local development and we will treat the `base` repository as our effective remote single soure of truth.

Every user must therefore clone the base repository and create their working directory like so:

```shell
git clone base users/example_user
```

Now stepping into the users directory with `users/example_user` your normal local git flows apply. Simply push to and pull from `origin` whenever you want to make contributions to and get the latest code from the shared `base` repository's branches.

## When

When we're happy to make our first deployment for our project, having combined all of the team's latest hard work, it's time to make a release.

First in the `base` repository identify and `git checkout` to the specific branch or commit you'd like to release and perform:

```
git tag example_tag
```

Now, in the project directory, you can now create the release by making a shallow clone of the repository for this tag like so:

```shell
git clone --branch example_tag --single-branch base releases/example_release
```

The `releases/example_release` folder now has all code for making your first release.

When ready to update, simply repeat the process with a new tag and use the created folder to perform your next release.

Finally I would ensure that the previous release folder isn't cleaned up too quickly as by Sod's law you may need to revert back to it suddenly if things do indeed go wrong!

#

*This only a recipe and can be added to and improved upon for your individual deployments. You should however see how strikingly simple it is to recreate your normal development process, with commands you already know!*

#

Example folder/repo structure created through process:



```
base/.git
----/initial_files
users/user_1/.git
------------/initial_files
------------/user_1_change
-----/user_2/.git
------------/initial_files
------------/user_2_change
releases/release_1/.git
------------------/initial_files
------------------/user_1_change
--------/release_2/.git
------------------/initial_files
------------------/user_1_change
------------------/user_2_change
```

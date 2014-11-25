# `paths.d`: Executable file locations for [shellfire]

This module provides information to a [shellfire] application on the location of executable files needed by a particular [shellfire] shell function. This information is in a `${executableFile}.path` file. These are organised by package manager. They exist to allow [shellfire] programs to:-

* automatically download dependencies when run
* to enforce that the same origin of executable is used across OS distributions (eg GNU coreutils' `grep`)
* to ensure the correct `PATH` is set up

A missing `${executableFile}.path` file doesn't cause an error, although `INFO` messages are created if a [shellfire] application is run at `--verbose 2` or greater. Their use is entirely optional.

When an application is [fatten]ed, any required `${executableFile}.path` files are embedded inside it. Applications announce their binary dependencies by using statements in global scope like `core_dependency_requires PACKAGE_MANAGER rsync grep`. A special package manager of `*` is used to apply to all systems.

## What's a `${executableFile}.path` file

Very simply, it's a space separated one liner\* that maps an executable file name (eg `head`) to a a package name (eg `coreutils`) and an expected location in the file system (`/usr/local/opt/coreutils/libexec/gnubin/head`), eg

```
head coreutils /usr/local/opt/coreutils/libexec/gnubin/head
```

The file name, in this case `head`, should be the name of the `${executableFile}.path` file - in this case, `head.path`.

The package name (and its syntax) is the one used by the particular package manager the `.path` is for. Often, the package name and executable file name are the same (eg for `curl`). Sometimes it's more complex. For example, to use taps with the Homebrew package manager (for Mac OS X), the tap is prefixed to the package name, eg `homebrew/dupes/rsync` for `rsync.path`

\* Which must be terminated by a line feed.

## Importing

To use this module, simply add a git submodule `paths.d` in `etc/shellfire` in your git repository. For example:-

```bash
mkdir -p etc/shellfire
cd etc/shellfire
git submodule add "https://github.com/shellfire-dev/paths.d.git"
cd -
git submodule init --update
```

You are also free to use per-application `.path` files. For example, if your `_program_name` is `swaddle`:-

```bash
mkdir -p etc/shellfire/swaddle/{Homebrew,Debian,CentOS}
```

Underneath this folder are folders for each package manager, where one places `${executableFile}.path` files.

## Supported Package Managers

Currently, we support the package managers:-

* Homebrew
* Debian
* CentOS
* RedHat (by symlink to CentOS)
* Fedora (by symlink to CentOS)

If you can contribute `${executableFile}.path` files for another a package manager, please submit a pull request.

[shellfire]: https://github.com/shellfire-dev "shellfire homepage"
[fatten]: https://github.com/shellfire-dev/fatten "fatten homepage"

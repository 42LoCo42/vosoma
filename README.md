# vosoma - Void Sources Manager

This tool manages a local installation of [void-packages][]

It builds new versions of specified packages (`targets`)
when their templates are updated.
This can easily be automated using cron
and, by doing all of its operations
as an otherwise unprivileged system user,
is very secure.

Additionally, it supports ordered package overrides
in the style of [kisslinux][] using [mergerfs][].

## Installation
WIP

## Usage
Convention: `$HOME` signifies the home directory of the `vosoma` user.
This is usually `/var/cache/vosoma`

List all packages that should be built by `vosoma` in `$HOME/targets`.
Example:
```bash
$ cat $HOME/targets
linux5.11
wine
discord
```

Add package overrides (folders like srcpkgs containing packages) in `$HOME/repos`.
Example:
```bash
$ tree $HOME/repos
/var/cache/vosoma/repos
└── foo
    └── thing
        └── template

2 directories, 1 file
```
`foo` is a repository containing a single package, `thing`, which will be available as a target.

Specify the order of overrides in `$HOME/order`. When a package exists in multiple repositories,
the first one listed in this file will be used to build the package.
Example:
```bash
$ cat $HOME/order
repos/foo
void-packages/srcpkgs
```

To run the update script on each boot, install a cron daemon and do `sudo crontab -u vosoma -e`,
then add the entry `@reboot $HOME/update`

## Coming soon
- Build options for targets
- A handful of tools allowing easier management of targets, repos and their order
- A tool that allows a one-time build of a package from a repo, bypassing the override order
- Most likely some bugfixes
- Whatever else is suggested

[void-packages]: https://github.com/void-linux/void-packages
[mergerfs]: https://github.com/trapexit/mergerfs
[kisslinux]: https://github.com/kisslinux

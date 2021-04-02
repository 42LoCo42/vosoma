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
Convention: the shell prompt is `<username> <work directory> $`  
This means that you must only copy the commands beginning after the $ character.
1. Clone this repo
```bash
username ~ $ git clone git://github.com/42LoCo42/vosoma
username ~ $ cd vosoma
```
2. Install dependencies
```bash
username ~/vosoma $ sudo xbps-install git mergerfs
```
3. Create the `vosoma` user and set up its home directory
```bash
username ~/vosoma $ sudo useradd -d /var/cache/vosoma -m -r -s /bin/nologin -U vosoma
username ~/vosoma $ sudo chmod 755 /var/cache/vosoma
username ~/vosoma $ sudo cp update /var/cache/vosoma
```
4. Change to `vosoma` and initialize the local [void-packages][] installation
```bash
username ~/vosoma $ sudo su - vosoma -s /bin/sh
vosoma ~ $ ./update
```
5. (OPTIONAL) (**WIP - DON'T DO THIS YET!!!**) Enable automatic updates for `vosoma`
```bash
vosoma ~ $ git clone git://github.com/42LoCo42/vosoma repos/vosoma
vosoma ~ $ echo repos/vosoma >> order
vosoma ~ $ echo vosoma >> targets
```
6. For automatic updates, install a cron daemon (like [cronie][])
    and add `@reboot $HOME/vosoma` to `vosoma`'s crontab
```bash
vosoma ~ $ exit
username ~/vosoma $ sudo xbps-install cronie
username ~/vosoma $ sudo ln -s /etc/sv/crond /var/service
username ~/vosoma $ echo '@reboot $HOME/update' | sudo tee /var/spool/cron/vosoma
```

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

## Coming soon
- Build options for targets
- A handful of tools allowing easier management of targets, repos and their order
- A tool that allows a one-time build of a package from a repo, bypassing the override order
- Most likely some bugfixes
- Whatever else is suggested

[void-packages]: https://github.com/void-linux/void-packages
[mergerfs]: https://github.com/trapexit/mergerfs
[kisslinux]: https://github.com/kisslinux
[cronie]: https://github.com/cronie-crond/cronie

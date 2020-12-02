# Inspecting Augtool

While Augeas is a C library, the `augeas-tools` package provides the `augtool`
command line, allowing you to easily interact with Augeas from the command
line.


## Using a fake root


Since Augeas modifies configuration files, let's work on a copy of `/etc` in
order to avoid breaking the system:


```
mkdir fakeroot
cp -a /etc fakeroot/
```{{execute}}


Let's fire up `augtool`, pointing to the current directory as the fake root:

```
augtool -r fakeroot/
```{{execute}}


Augeas exposes a tree to manipulate its API. This tree has two nodes at the
root:

- `/augeas` to control the API itself
- `/files` to expose the content of the files Augeas parsed


## Loaded files and lenses


Inside `/augeas`, you'll find information about the Augeas API.
For example, `/augeas/load/` contains all the informations about the lenses
loaded by Augeas, and the files associated to them.

```
print /augeas/load/Hostname
```{{execute}}


The previous command displays information on the `Hostname` module. The lens is
preceded by an arobase (`@Hostname`), indicating that Augeas loaded it
automatically at start time.

There are two numbered `incl` nodes (respectively `incl[1]` and `incl[2]`), for
the two files that are automatically associated with the `Hostname.lns` lens.

You can find metadata on these files by looking into `/augeas/files`:


```
print /augeas/files/etc/hostname
```{{execute}}


This confirms that this file is known by Augeas, and was successfully
parsed by the `@Hostname` lens.

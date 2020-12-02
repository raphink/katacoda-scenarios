# Loaded files and lenses


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


# Inspecting files


Augeas parses configuration files on disk (also called the "concrete view")
and converts them to a tree representation in memory (called the "abstract
view").

This representation can be queried under the `/files` nodes, where each file
can be found according to its location on disk.

For example, you can inspect the representation for `/etc/hostname` in the
Augeas tree:

```
print /files/etc/hostname
```{{execute}}


which in our case maps to the `fakeroot/etc/hostname`{{open}} file.


You can also use a relative path and ommit `/files/`, because the
relative context (stored inside the `/augeas/context` node) defaults to
`/files`:


```
print etc/hostname
```{{execute}}


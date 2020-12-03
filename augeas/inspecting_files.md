Augeas parses configuration files on disk (also called the "concrete view")
and converts them to a tree representation in memory (called the "abstract
view").

This representation can be queried under the `/files` nodes, where each file
can be found according to its location on disk.

For example, you can inspect the representation for `fakeroot/etc/hosts` in the
Augeas tree:

```
print /files/etc/hosts
```{{execute T1}}


You can also use a relative path and ommit `/files/`, because the
relative context (stored inside the `/augeas/context` node) defaults to
`/files`:

```
print etc/hosts
```{{execute T1}}


Let's have a look at the `fakeroot/etc/hosts` file for comparison:

```
cat fakeroot/etc/hosts
```{{execute T2}}

You can see that:

- Each comment line is represented by a `#comment` node, with a value
- Each host line is represented by a sequentially numbered node
- The parameters for each host are labeled according to their column, so each
  host node contains:
  - 1 `ipaddr` node
  - 1 `canonical` node
  - 0, 1 or more `alias` nodes

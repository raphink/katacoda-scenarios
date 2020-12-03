Most GNU/Linux distributions provide Augeas as a standard package. On Debian
based operating systems, you can install Augeas with:

```
apt-get install -y augeas-lenses augeas-tools
```{{execute T1}}


The `augeas-lenses` package provides hundreds of premade lenses, which are
Augeas parsers for configuration file formats:

```
ls /usr/share/augeas/lenses/dist/
```{{execute T1}}


Each module provides one or more (usually one) Augeas lens. For example, the
`Fstab` module (from the `fstab.aug` file) provides the `Fstab.lns` lens.

Let's have a look at a simple module, `Hostname`:

```
cat /usr/share/augeas/lenses/dist/hostname.aug
```{{execute T1}}

You can see the name of the module at the top of the file, and the definition
of the lens, `Hostname.lns`.

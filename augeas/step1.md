# Installing and discovering Augeas


```
apt-get install -y augeas-lenses augeas-tools
```{{execute}}


The `augeas-lenses` package provides hundreds of premade lenses, which are
Augeas parsers for configuration file formats:

```
ls /usr/share/augeas/lenses/dist/
```{{execute}}


Each module provides one or more (usually one) Augeas lens. For example, the
`Fstab` module (from the `fstab.aug` file) provides the `Fstab.lns` lens.


While Augeas is a C library, the `augeas-tools` package provides the `augtool`
command line, allowing you to easily interact with Augeas from the command
line.

Let's fire up `augtool`:

```
augtool
```{{execute}}


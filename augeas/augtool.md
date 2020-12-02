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


```
ls /
```{{execute}}

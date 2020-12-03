Augeas works in a transactional way, so the tree is not automatically saved
when you modify it. Instead, you need to ask Augeas to save the tree back to
disk:


```
save
```{{execute T1}}


If all goes well, Augeas will tell you how many files were modified, and you
will see `fakeroot/etc/hosts` updated on disk:

```
cat fakeroot/etc/hosts
```{{execute T2}}

If errors occur (for example because Augeas was not able to validate a value),
you can see what went wrong by typing:

```
errors
```{{execute T1}}

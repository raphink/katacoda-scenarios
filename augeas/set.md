The end goal of Augeas is actually to modify configuration files.

For this purposed, there are write commands, the single of which is `set`.
You can see how to use it with `help set`{{execute}}.


For example, we can modify the second alias of the 3rd node with:

```
set etc/hosts/3/alias[2] "new-alias"
```{{execute}}

Let's check that the tree was modified:


```
print etc/hosts/3
```{{execute}}

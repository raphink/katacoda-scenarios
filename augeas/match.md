The `augtool` CLI provides various commands to query and modify the Augeas
tree.

You can see the list of available commands by typing `help`{{execute}}, and the
help for a specific command by appending the name of the command.

Let's have a look at the `match` command with `help match`{{execute}}.

This command allows to select one or more nodes in the tree, based on
a condition.

For example, to select all `alias` nodes in any node inside `etc/hosts`, we can
type:

```
match etc/hosts/*/alias
```{{execute}}


Where the star character stands for any subnode under the `etc/hosts` node.

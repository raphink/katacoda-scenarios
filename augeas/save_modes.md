If you don't want Augeas to replace existing files, you can make use of
alternate saving modes.

These modes are controlled by the `/augeas/save` node in the tree, which
defaults to `overwrite`:

```
get /augeas/save
```{{execute T1}}


## Backup mode

By using the `--backup` flag in `augtool`, Augeas will save the original files
with a `.augsave` suffix before replacing the files with their new versions.

You can also set this mode before saving the tree in Augeas by setting
`/augeas/save` to `backup`.


## New file mode

Alternatively, you can use `--newfile` flag in `augtool`, or set `/augeas/save`
to `newfile`, in order to save the new file with a `.augnew` extension.

Let's try that:

```
set etc/hosts/3/alias[1] some-new-alias
```{{execute T1}}

Then change the mode and save:


```
set /augeas/save newfile
save
```{{execute T1}}


We can now see the diff between both files:


```
diff -u --color fakeroot/etc/hosts fakeroot/etc/hosts.augnew
```{{execute T2}}

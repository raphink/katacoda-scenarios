In order to list pods, type `:pods`{{execute}}

This will list the pods in the default namespace only. Numeral keys let you
switch to other namespaces (see help in the top right corner), as well as list
the pods from all namespaces by typing `0`{{execute}}.


In every view, you can search for entries by using the `/` shortcut.
For example, search for the `kube-proxy` pods by typing `/kube-proxy`{{execute}}
then enter to validate the search.


You can display pod logs with the `l`{{execute}} key. By default, only the logs
from the last minute are displayed. Press `0`{{execute}} to display all the
logs (or another numeral keys to choose another time frame).

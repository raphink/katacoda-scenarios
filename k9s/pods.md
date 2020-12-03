In order to list pods, type `:pods`{{execute}}

This will list the pods in the default namespace only. Numeral keys let you
switch to other namespaces (see help in the top right corner), as well as list
the pods from all namespaces by typing `0`{{execute}}.


In every view, you can search for entries by using the `/` shortcut.
For example, search for the `kube-proxy` pods by typing `/kube-proxy`{{execute}}
then Enter to validate the search.

Pressing Enter on a pod line lets you inspect the containers in that pod.

Press Escape to get back to the list of pods. You can see at the bottom of the
screen the bread crumbs indicating the path you've followed in the K9s views.


You can display pod logs with the `l`{{execute}} key. By default, only the logs
from the last minute are displayed. Press `0`{{execute}} to display all the
logs (or another numeral key to choose another time frame).

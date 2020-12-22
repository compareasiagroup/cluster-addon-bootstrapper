# Cluster Addon Bootstrapper

Fork of https://github.com/kubernetes-sigs/cluster-addons - bootstrapper


cluster-addons-bootstrap manages two classes of addons with given template files in
`$ADDON_PATH` (default `/etc/kubernetes/addons/`).
- Addons with label `addonmanager.kubernetes.io/mode=Reconcile` will be periodically
reconciled. Direct manipulation to these addons through apiserver is discouraged because
addon-manager will bring them back to the original state. In particular:
	- Addon will be re-created if it is deleted.
	- Addon will be reconfigured to the state given by the supplied fields in the template
	file periodically.
	- Addon will be deleted when its manifest file is deleted from the `$ADDON_PATH`.
- Addons with label `addonmanager.kubernetes.io/mode=EnsureExists` will be checked for
existence only. Users can edit these addons as they want. In particular:
	- Addon will only be created/re-created with the given template file when there is no
	instance of the resource with that name.
	- Addon will not be deleted when the manifest file is deleted from the `$ADDON_PATH`.

Notes:
- Label `kubernetes.io/cluster-service=true` is deprecated (for this utility).
In a future release,cluster-addons-bootstrap may not respect it anymore. Addons
have this label but without `addonmanager.kubernetes.io/mode=EnsureExists` will be
treated as "reconcile class addons" for now.
- Resources under `$ADDON_PATH` need to have either one of these two labels.
Otherwise it will be omitted.

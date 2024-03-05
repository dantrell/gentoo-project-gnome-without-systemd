Profiles
========

* [Differences Between GNOME Profiles](#differences-between-gnome-profiles)

Differences Between GNOME Profiles
----------------------------------

Previously, the most notable USE flag changes between Gentoo's and ours was:

1. dropping `qt3support` and `qt4`

    The former isn't something that should be enabled upstream, especially since things have moved to `qt4` and/or `qt5` by default. Users should decide for themselves if they want `qt3support`. As for the latter, it shouldn't be enabled globally at all in a GNOME environment, especially if `gtkstyle` is not enforced (and `gtkstyle` should be enforced regardless).

2. enabling `gtkstyle`

3. dropping `gtk`

    This is handled by default and on packages where it's not enabled it likely refers to optional GTK+-related support that does not enable a GUI.

The vast majority of the other USE flags dropped were there to obtain clean emerge output prior to `GNOME 3.14`.

Nowadays, these differences are of little relevance as Gentoo's and our profiles are arguably functionally the same.

Plumbing
========

* [Overview](#overview)
* [Classification](#classification)
* [Classification (`ConsoleKit`)](#classification-consolekit)
* [Classification (`elogind`)](#classification-elogind)
* [Implementation (`ConsoleKit`)](#implementation-consolekit)
* [Implementation (`elogind`)](#implementation-elogind)

Overview
--------

> **Note**: Ebuilds are interchangeable with any closely-related `Gentoo Linux` derivative.

[dantrell-gnome](https://github.com/dantrell/gentoo-overlay-dantrell-gnome):

    ├── app-admin
    │   ├── gnome-system-log
    │   └── system-config-printer
    ├── dev-scheme
    │   └── guile
    ├── net-im
    │   └── telepathy-mission-control
    └── sys-power
        ├── acpid
        └── upower

[dantrell-gnome-3-14](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-14), [3-16](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-16), [3-18](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-18), [3-20](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-20), [3-22](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-22), [3-24](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-24), [3-26](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-26), [3-28](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-28), [3-30](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-30), [3-32](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-32), [3-34](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-34), [3-36](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-36), [3-38](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-38), [40](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-40), [41](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-41) and [42](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-42):

    ├── games-board
    │   └── aisleriot
    ├── gnome-base
    │   ├── gdm
    │   ├── gnome-control-center
    │   ├── gnome-session
    │   ├── gnome-settings-daemon
    │   ├── gnome-shell
    │   └── nautilus
    ├── media
    │   └── clutter
    ├── x11-terms
    │   └── gnome-terminal
    ├── x11-themes
    │   └── gnome-backgrounds
    └── x11-wm
        └── mutter

Classification
--------------

`Aesthetic` and `Performant`:

| Package                               | USE flag                  | Description
|---------------------------------------|---------------------------|------------
| app-admin/gnome-system-log            | N/A                       | The newer `gnome-extra/gnome-logs` depends on `sys-apps/systemd`
| app-admin/system-config-printer       | N/A                       | Regularly outdated
| dev-scheme/guile                      | N/A                       | Different implementation
| games-board/aisleriot                 | N/A                       | Different implementation
| gnome-base/gnome-control-center       | -vanilla-datetime         | Disables automatic datetime and timezone options
| gnome-base/gnome-control-center       | -vanilla-hostname         | Disables changing hostname
| gnome-base/gnome-settings-daemon      | -vanilla-inactivity       | Restores old timeout for user inactivity
| gnome-base/gnome-shell                | +deprecated-background    | Restores old background code (avoids wallpaper corruption when resuming from suspend)
| gnome-base/gnome-shell                | -vanilla-async            | Disables multiple asynchronous allocations (prevents a never-ending asynchronous loop)¹
| gnome-base/gnome-shell                | -vanilla-gc               | Forces more frequent garbage collection
| gnome-base/gnome-shell                | -vanilla-motd             | Improves handling of motd when logging in (displays text in monospace and decreases the display time)
| gnome-base/gnome-shell                | -vanilla-screen           | Improves handling of screen blanking (uses the "Blank Screen" time as the idle delay)
| gnome-base/gsettings-desktop-schemas  | -vanilla-fonts            | Restores old Document and Monospace font defaults
| gnome-base/nautilus                   | -vanilla-icon             | Improves icons (adds another zoom level)
| gnome-base/nautilus                   | -vanilla-icon-grid        | Use old icon grid and text width proportions (makes text labels in icon view warp at ~16 characters)
| gnome-base/nautilus                   | -vanilla-menu             | Reorder context menu (primarily places "Open In Terminal" below "Open in New Tab" and other extensions below "Open With")
| gnome-base/nautilus                   | -vanilla-menu-compress    | Use old compress extension from File Roller instead of the native one
| gnome-base/nautilus                   | -vanilla-rename           | Support slow double click to rename (triggers rename mode by clicking an icon twice with a pause in between)
| gnome-base/nautilus                   | -vanilla-search           | Support alternative search (find without recursion by pressing `CTRL` + `SHIFT` + `F`)
| gnome-base/nautilus                   | -vanilla-thumbnailer      | Use FFmpeg as the video thumbnailer instead of Totem
| media-video/totem                     | -vanilla-thumbnailer      | Use FFmpeg as the video thumbnailer instead of Totem
| x11-libs/gtk+                         | +vanilla-sidebar          | Simplifies GTK sidebars (e.g. Nautilus "Places")
| x11-libs/vte                          | -vanilla-notify           | Support desktop notifications from OSC 777²
| x11-terms/gnome-terminal              | +deprecated-transparency  | Restores background transparency²
| x11-terms/gnome-terminal              | -vanilla-hotkeys          | Disables function keys (avoids clashes with command line utilities)
| x11-terms/gnome-terminal              | +vanilla-icon             | Restores old desktop icon name (fixes missing icon for some use cases)
| x11-terms/gnome-terminal              | -vanilla-notify           | Support desktop notifications from OSC 777²
| x11-terms/gnome-terminal              | -vanilla-open-terminal    | Separate "New Window/Tab" menu entries by default
| x11-themes/gnome-backgrounds          | -vanilla-live             | Replaces Adwaita live backgrounds (800x600) with the set from GNOME 3.10 (2560x1440)
| x11-wm/mutter                         | +deprecated-background    | Restores old background code (avoids wallpaper corruption when resuming from suspend)
| x11-wm/mutter                         | +vanilla-mipmapping       | Disables mipmapping (trades scaled and smoothed window previews for a measurable performance increase)

`Functional`:

| Package                               | USE flag                  | Description
|---------------------------------------|---------------------------|------------
| sys-apps/openrc                       | +vanilla-loopback         | Don't wait for a valid network connection before continuing with startup
| sys-apps/openrc                       | -vanilla-shutdown         | Don't complicate the shutdown process
| sys-apps/openrc                       | +vanilla-warnings         | Silence deprecation warnings for runscript

> **¹** For details, see: [gtk-embed: Don't allow multiple async allocations](https://github.com/endlessm/gnome-shell/commit/6d675c2c7df6652291f45f08293eadddc1ef65bc).

> **²** This patchset is sourced from [Fedora](https://src.fedoraproject.org/rpms/gnome-terminal/tree/master). ↩

Classification (`ConsoleKit`)
-----------------------------

`Critical`:

| Package                             | USE flag                  | Description
|-------------------------------------|---------------------------|------------
| gnome-base/gdm                      | +ck                       | Restores support for `sys-auth/consolekit`
| gnome-base/gnome-control-center     | +ck                       | Restores support for `sys-power/upower`
| gnome-base/gnome-session            | +ck                       | Removes dependency on an older version of `sys-power/upower`
| gnome-base/gnome-settings-daemon    | +ck                       | Restores support for `sys-power/pm-utils`
| gnome-base/gnome-shell              | +ck                       | Restores support for `sys-power/pm-utils`
| media-libs/clutter                  | N/A                       | Reorganizes backends (prioritizes X11)
| net-im/telepathy-mission-control    | +ck                       | Remove dependency on an older version of `sys-power/upower`
| sys-power/acpid                     | N/A                       | Restores support for GNOME's Power Management System
| sys-power/upower                    | +ck                       | Restores support for `sys-power/pm-utils`

Classification (`elogind`)
--------------------------

`Critical`:

| Package                             | USE flag                  | Description
|-------------------------------------|---------------------------|------------
| gnome-base/gdm                      | +elogind                  | Supports `sys-auth/elogind`
| gnome-base/gnome-control-center     | +elogind                  | Supports `sys-auth/elogind`
| gnome-base/gnome-session            | +elogind                  | Supports `sys-auth/elogind`
| net-misc/networkmanager             | +elogind                  | Supports `sys-auth/elogind`
| sys-apps/accountsservice            | +elogind                  | Supports `sys-auth/elogind`
| sys-apps/dbus                       | +elogind                  | Supports `sys-auth/elogind`
| sys-auth/pambase                    | +elogind                  | Supports `sys-auth/elogind`
| sys-auth/polkit                     | +elogind                  | Supports `sys-auth/elogind`
| sys-fs/udisks                       | +elogind                  | Supports `sys-auth/elogind`
| x11-wm/mutter                       | +elogind                  | Supports `sys-auth/elogind`

Implementation (`ConsoleKit`)
-----------------------------

`UPower`:

* Use `sys-power/upower` instead of `sys-power/upower-pm-utils`
* Reintegrate Power Management Utilities (`pm-utils`) support
* Have relevant packages depend on `>=sys-power/upower-0.99:=[ck]`

`GNOME`:

* Globally disable the `systemd` USE flag
* Replace the masked `openrc-force` USE flag with unmasked `ck` and `systemd` USE flags.
* Replace `sys-apps/systemd` dependency with this:

```sh
	ck? ( <sys-auth/consolekit-0.9 )
	systemd? ( >=sys-apps/systemd-186:0= )
```

or this (or equivalent):

```sh
	ck? ( >=sys-power/upower-0.99:=[ck] )
	systemd? ( >=sys-apps/systemd-186:0= )
```

`GNOME Control Center`, `GNOME Session`, `GNOME Settings Daemon`, `GNOME Shell` and `Telepathy Mission Control`:

* Reintegrate `UPower` support

`GDM`:

* Reintegrate `ConsoleKit` support

`acpid`:

* Account for power button functionality moving to `GNOME Settings Daemon`

`Guile`:

* Remove incomplete SLOT support (alternatively, fix incomplete SLOT support and provide `eselect guile`)

Implementation (`elogind`)
-----------------------------

99.99% Autotools Configure and/or Meson Build Goo™.

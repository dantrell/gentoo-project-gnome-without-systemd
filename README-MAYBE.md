Alternative Documentation
=========================

* [Updating Old Layman Installations](#updating-old-layman-installations)
* ["Basic" OpenRC Mode](#basic-openrc-mode)
* [GNOME With Systemd (`Wait... What?`)](#gnome-with-systemd)

Updating Old Layman Installations
---------------------------------

1. Rebuild the config files:

```sh
cp -a /etc/layman/layman.cfg /etc/layman/layman.cfg.bak
layman-updater --rebuild
```

2. Delete the old Layman make.conf file (if it exists):

```sh
rm /var/lib/layman/make.conf
```

3. Remove the reference to the old Layman make.conf file (if it exists) with `nano -w /etc/portage/make.conf`:

   > ~~source /var/lib/layman/make.conf~~

"Basic" OpenRC Mode
-------------------

> This mode is no longer unofficially supported by `Gentoo Linux` since it now forces you to pick between `elogind` and `systemd`.

This is GNOME without systemd and without basic functionality. This route is best suited if you:

* exclusively use startx
* don't need basic functionality
* want the GNOME ecosystem present while using another Desktop Environment¹

The primary benefit is that it does not require the project overlays (which contains ~200 ebuilds per GNOME release version) but instead requires you to create and occasionally maintain a local overlay (which will contain one or two ebuilds depending on circumstances). This allows you to follow whichever GNOME release is in the main portage tree without relying on a third party.

1. Globally mask the systemd USE flag with `nano -w /etc/portage/profile/use.mask`:

```sh
systemd
```

2. Mask GDM >3.17.2 with `nano -w /etc/portage/package.mask/gdm` (or use a patched GDM)¹:

    > **Note**: Since only the two most recent GNOME releases are kept in the main portage tree you'll have to fork an appropriate version of GDM. To expedite matters, simply choose from the relevant project overlays (since they all work): [3.14](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-14/tree/master/gnome-base/gdm), [3.16](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-16/tree/master/gnome-base/gdm), [3.18](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-18/tree/master/gnome-base/gdm), [3.20](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-20/tree/master/gnome-base/gdm), [3.22](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-22/tree/master/gnome-base/gdm), [3.24](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-24/tree/master/gnome-base/gdm), [3.26](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-26/tree/master/gnome-base/gdm), [3.28](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-28/tree/master/gnome-base/gdm), [3.30](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-30/tree/master/gnome-base/gdm), [3.32](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-32/tree/master/gnome-base/gdm), [3.34](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-34/tree/master/gnome-base/gdm) or [3.36](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-36/tree/master/gnome-base/gdm).

```sh
>gnome-base/gdm-3.17.2
```

3. Furthermore, if you intend to call GNOME Session as root (or use a patched GDM), mask ConsoleKit2 with `nano -w /etc/portage/package.mask/consolekit`:

    > **Note**: For now it is sufficient to simply mask ConsoleKit2 but if in the future ConsoleKit is dropped from the main tree and its functionality is still needed, you'll have fork it too.

```sh
>sys-auth/consolekit-0.4.6
```

> **¹** It is sufficient to [fake the presence](https://wiki.gentoo.org/wiki//etc/portage/profile/package.provided) of GDM instead if you don't intend to use it at all. ↩

GNOME With Systemd
------------------

As noted elsewhere¹, "this project's secondary purpose is to allow switching between GNOME release versions".

This means that for those seeking a specific GNOME release version, a lot of time can be saved by just using the project overlays. For OpenRC, no extra work is needed. For systemd, some profile changes need to be reversed:

1. Disable the `Critical`² USE flags with `nano -w /etc/portage/make.conf`:

```sh
# Critical
USE="${USE} -ck -elogind"
```

Disable the `Aesthetic`² and `Performant`² USE flags (and enable the `Functional` USE flags) with `nano -w /etc/portage/package.use/gnome`:

```
# Aesthetic and Performant
app-arch/file-roller -nautilus
gnome-base/gnome-control-center vanila-datetime
gnome-base/gnome-control-center vanila-hostname
gnome-base/gnome-shell -deprecated-background
gnome-base/gnome-shell vanilla-motd
gnome-base/gnome-shell vanilla-screen
gnome-base/nautilus vanilla-icon
gnome-base/nautilus vanilla-icon-grid
gnome-base/nautilus vanilla-menu
gnome-base/nautilus vanilla-menu-compress
gnome-base/nautilus vanilla-rename
gnome-base/nautilus vanilla-search
x11-terms/gnome-terminal -deprecated-transparency
x11-terms/gnome-terminal vanilla-hotkeys
x11-themes/gnome-backgrounds vanilla-live
x11-wm/mutter -deprecated-background

# Functional
gnome-base/gnome-session user-session
net-wireless/bluez user-session
sys-apps/dbus user-session
```

2. Globally reverse the `systemd` USE flag mask with `nano -w /etc/portage/profile/use.mask`:

```sh
-systemd
```

3. Enable the systemd USE flag with `nano -w /etc/portage/make.conf`:

```sh
USE="${USE} systemd"
```

> **¹** For details, see: [Long-term Solutions](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/POLITICS.md#long-term-solutions).

> **²** For details, see: [Classification](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/HACKING.md#classification).

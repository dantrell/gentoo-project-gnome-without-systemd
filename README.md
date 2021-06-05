GNOME Without Systemd (for `Gentoo Linux` et al.)¹
==================================================

> **Note**: Preface aside, feel free to skip the introduction and go straight to [Getting Started](#dependencies) but **if you intend to contact me, please read everything** to make sure that your question hasn't already been answered.

* Preface
  * [`Security Advisories`](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/SECURITY.md#security-advisories)
* Introduction
  * [Description](#description)
  * [Overview](#overview)
  * [Rationale](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/POLITICS.md#rationale)
  * [Long-term Solutions](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/POLITICS.md#long-term-solutions)
  * [Long-term Support](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/GOVERNANCE.md#long-term-support)
  * [Updates](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/GOVERNANCE.md#updates)
  * [Bugs and Other Issues](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/GOVERNANCE.md#bugs-and-other-issues)
  * [Contributing](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/CONTRIBUTING.md)
* Getting Started
  * [Dependencies](#dependencies)
  * [Incompatibilities](#incompatibilities)
  * [Incompatibilities (`ConsoleKit`)](#incompatibilities-consolekit)
  * [Incompatibilities (`elogind`)](#incompatibilities-elogind)
  * [Derivatives](#derivatives)
  * [Known Issues](#known-issues)
  * [Preparing Overlays](#preparing-overlays)
  * [Fetching Overlays](#fetching-overlays)
  * [Profile Selection](#profile-selection)
  * [Profile Selection (`Gentoo Linux`)](#profile-selection-gentoo-linux)
  * [Configuration (Optional GNOME Applications)](#configuration-optional-gnome-applications)
  * [Configuration (Optional Quality of Life Improvements)](#configuration-optional-quality-of-life-improvements)
  * [Configuration (Wayland)](#configuration-wayland)
  * [Upgrading](#upgrading)
  * [Installation](#installation)
  * [Finishing Touches](#finishing-touches)
  * [Finishing Touches (`ConsoleKit`)](#finishing-touches-consolekit)
  * [Finishing Touches (`elogind`)](#finishing-touches-elogind)
  * [Finishing Touches (text-based login)](#finishing-touches-text-based-login)
  * [Finishing Touches (text-based login with `ConsoleKit`)](#finishing-touches-text-based-login-with-consolekit)
  * [Finishing Touches (text-based login with `elogind`)](#finishing-touches-text-based-login-with-elogind)
  * [Switching](#switching)
  * [Uninstallation](#uninstallation)
  * [Tips](#tips)
  * [External Resources](#external-resources)
* Internals
  * [Profiles](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/HACKING.md#profiles)
  * [Plumbing](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/HACKING.md#plumbing)

> **¹** For details, see: [Distributions based on Gentoo](https://wiki.gentoo.org/wiki/Distributions_based_on_Gentoo). ↩

Description
-----------

For those not familiar with the matter, the state of GNOME is such that:

* systemd is not a hard compile time dependency
* systemd is a hard run time dependency for basic functionality

Basic functionality is what the GNOME team has decided includes:

* power management
* session tracking
* lid close handling
* log/event viewing
* Wayland (in GNOME, not in general)

This means that right now, without applying any patches, you can run GNOME in two official conditions:

1. GNOME with systemd and with basic functionality
2. GNOME without systemd and without basic functionality¹

This project delivers:

* GNOME without systemd and with basic functionality

For our purposes this means a dependency on `OpenRC`, `ConsoleKit` (or `elogind`) and `UPower` with Power Management Utilities (`pm-utils`) reintegrated (or `elogind` support).

Searching for information on this subject in places other than my official contributions on `Gentoo Linux` (e.g. forum topic #[1022050](https://forums.gentoo.org/viewtopic-t-1022050.html)) and `Funtoo Linux` (e.g. issues #[1329](https://bugs.funtoo.org/browse/FL-1329), #[1637](https://bugs.funtoo.org/browse/FL-1637) & #[2485](https://bugs.funtoo.org/browse/FL-2485) and forum topic #[263](http://forums.funtoo.org/topic/263-systemd/#entry1338)) will turn up information that is in many cases almost entirely [dead wrong](https://forums.gentoo.org/viewtopic-p-7587904.html#7587904). So let me emphasize that this is indeed the full GNOME experience without systemd and with minimal divergence from the vanilla default.

The only feature which does not work is `Wayland` (unless you are using an `elogind` implementation).

> **¹** For details, see: ["Basic" OpenRC Mode](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/README-MAYBE.md#basic-openrc-mode). ↩

Overview
--------
| Timeline    | Description
|-------------|------------
| 25 Jun 2015 | Initial commits¹
| 16 Jul 2015 | [Project announcement](https://forums.gentoo.org/viewtopic-t-1022050.html) (and support thread)
| 05 Jun 2018 | [Support thread](https://forums.gentoo.org/viewtopic-t-1082226.html) (part 2)
| 27 Aug 2018 | `Funtoo Linux` stage 3 tarballs no longer Portage-compatible with `Gentoo Linux`
| 03 Dec 2018 | `Funtoo Linux` announces [GNOME 3.30 without systemd](https://twitter.com/funtoo/status/1069695288544292868)
| 26 Mar 2019 | `Gentoo Linux` announces [GNOME 3.30 for all init systems](https://forums.gentoo.org/viewtopic-t-1094796.html)
| 08 Apr 2019 | [Support thread](https://forums.gentoo.org/viewtopic-t-1095396.html) (part 3)
| 05 Mar 2021 | [Support thread](https://forums.gentoo.org/viewtopic-t-1131384.html) (part 4)
| 14 May 2021 | `Gentoo Linux` stage 3 tarballs (20210509 & older) confirmed to be compatible with the project
| 15 May 2021 | GNOME 3.14 through GNOME 3.38 confirmed to be build passing with [GCC](https://gcc.gnu.org/releases.html) 8 & 9
| 05 Jun 2021 | Documentation revision #326²

> **¹** See overlay commit history for GNOME [3.14](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-14/commits/master) & [3.16](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-16/commits/master). ↩

> **²** Commit history is not displayed because updates mirror the effect of a git shallow clone with "--depth 1". ↩

|            | GNOME Series | Basic Functionality³ | Quality of Life Improvements⁴ | Grading⁵ | Availability⁶
|------------|--------------|---------------------:|-------------------------------|----------|--------------
| sunsetting | 3.14         | ConsoleKit & elogind | 14 of 14 (100%)               | Gold     | 3.14.2
| sunsetting | 3.16         | ConsoleKit & elogind | 15 of 16 (~94%)               | Silver   | 3.16.2
| sunsetting | 3.18         | ConsoleKit & elogind | 15 of 16 (~94%)               | Silver   | 3.18.2
| sunsetting | 3.20         | ConsoleKit & elogind | 15 of 15 (100%)               | Gold     | 3.20.2
| sunsetting | 3.22         | ConsoleKit & elogind | 16 of 16 (100%)               | Silver   | 3.22.2
| sunsetting | 3.24         |              elogind | 14 of 16 (~88%)               | Silver   | 3.24.2
| longterm   | 3.26         |              elogind | 14 of 16 (~88%)               | Bronze   | 3.26.2 (🗘)
| longterm   | 3.28         |              elogind | 14 of 16 (~88%)               | Bronze   | 3.28.2 (🗘)
| longterm   | 3.30         |              elogind | 16 of 18 (~89%)               | Bronze   | 3.30.2 (🗘)
| longterm   | 3.32         |              elogind | 15 of 17 (~88%)               | Silver   | 3.32.2
| longterm   | 3.34         |              elogind | 15 of 17 (~88%)               | Silver   | 3.34.8 (bugfixes)
| stable     | 3.36         |              elogind | 15 of 17 (~88%)               | Bronze   | 3.36.9 (bugfixes)
| mainline   | 3.38         |              elogind | 15 of 17 (~88%)               | Bronze   | 3.38.6 (bugfixes)
| next       | 3.40 (40.0)  |              elogind |                               | Paper™   | wip
| maybe      | 3.42 (41.0)  |                      |                               |          |

> **³** Power management, session tracking and log/event viewing. ↩

> **⁴** These are [optional patches](#configuration-optional-quality-of-life-improvements) which improve the GNOME experience. ↩

> **⁵** There are three classes of grading: "gold", superb; "silver", satisfactory; and "bronze", acceptable. Loosely speaking, three aspects are graded: accessibility, usability and stability. Each qualifying aspect gives a grade increase. If a GNOME release version is not "gold" and it is not considered "alpha" or "beta", it is lacking in one or multiple aspects.↩

> **⁶** Full GNOME release versions are in the form of MAJOR.MINOR.PATCH. For the purposes of this project, PATCH at: 0 is considered "alpha", 1 is considered "beta" and 2 or higher is considered "stable". A `🗘` symbol denotes that the project [ebuilds](https://wiki.gentoo.org/wiki/Ebuild) were not synced against Gentoo. ↩

Dependencies
------------

> **Note**: `Gentoo Linux` users can choose to follow Sakaki's more detailed instructions ([GNOME/GNOME Without systemd/Dantrell](https://wiki.gentoo.org/wiki/GNOME/GNOME_Without_systemd/Dantrell)) for this project instead of continuing below. Likewise, there is also an excellent resource ([Sakaki's EFI Install Guide](https://wiki.gentoo.org/wiki/Sakaki's_EFI_Install_Guide)) for those wanting to dual-boot with Windows 8+.

It is assumed you followed the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) (or what's mentioned in relevant [Derivatives](#derivatives) section) and have a machine running with the default SysV-style [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) init system with an [appropriately configured](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation#User_administration) user account for daily use.

It is further assumed that you set `VIDEO_CARDS` and `INPUT_DEVICES` in `/etc/portage/make.conf` and configured the kernel accordingly for the following dependencies:

* [X Window server](https://wiki.gentoo.org/wiki/Xorg/Guide)
* [evdev](https://wiki.gentoo.org/wiki/Evdev) or [udev](https://wiki.gentoo.org/wiki/Udev)
* [synaptics](https://wiki.gentoo.org/wiki/Synaptics)
* [PulseAudio](https://wiki.gentoo.org/wiki/ALSA)
* [ACPI](https://wiki.gentoo.org/wiki/ACPI)

The remaining dependencies require no kernel configuration:

* [ConsoleKit](https://wiki.gentoo.org/wiki/ConsoleKit) (not `ConsoleKit2`) or `elogind`
* [D-Bus](https://wiki.gentoo.org/wiki/D-Bus)
* `UPower` with Power Management Utilities (`pm-utils`) reintegrated or `elogind` support

All of the above packages are pulled in automatically. Relevant services will be activated in the [Finishing Touches](#finishing-touches) section.

Incompatibilities
-----------------

1. NVIDIA Drivers with Userspace VESA Framebuffer (`uvesafb`)

   Starting from version 358, the NVIDIA drivers are incompatible with the Userspace VESA Framebuffer (`uvesafb`) and will crash your system if you attempt to: (1) change between VTT and X, or (2) suspend or hibernate.

   You can work around the issue by switching from `uvesafb` to the EFI Framebuffer (`efifb`) with or without the Simple Framebuffer (`simplefb`). However, this also requires you to switch from a BIOS to a UEFI bootloader and depending on your hardware (especially your monitor) it may disable your high-resolution console.

   If you want to continue using `uvesafb` feel free to remain on the last working long-lived branch release of the NVIDIA drivers, 352.79 (25 Jan 2016). However, it is only compatible with Linux Kernel versions older than 4.5.

   Reference:

   * Gentoo Bug #[564096](https://bugs.gentoo.org/show_bug.cgi?id=564096)
   * NVIDIA Topic #[912455](https://devtalk.nvidia.com/default/topic/912455)

Incompatibilities (ConsoleKit)
------------------------------

*  ConsoleKit2

   ConsoleKit2 is supposed to be backwards compatible with ConsoleKit (which this project is based around) but this is not the case.

Incompatibilities (elogind)
---------------------------

*  NVIDIA Drivers with Wayland

   The nouveau drivers have better support.

Derivatives
-----------

> **Note**: If you are using a `Gentoo Linux` derivative that is *build passing*, now is a good time to check if there are any instructions related to [getting started](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/README-DERIVATIVES.md).

| Last Release Tested | Name                            | Status¹
|---------------------|---------------------------------|--------
| 27 Aug 2018         | `Funtoo Linux` stage 3 tarballs | build failing

> **¹** Derivatives that are *build failing* are considered to have broken compatibility with `Gentoo Linux` and are not supported. ↩

Known Issues
------------

*  Python & SQLite Circular Dependencies

   Can occur during new installs using the combined (gnome+plasma) profiles.
   Might require a USE flag to be temporarily disabled on one of two packages:

        dev-lang/python (Change USE: -sqlite)
        dev-db/sqlite (Change USE: -icu)

   We could handle this seamlessly on our end but we want users to be aware of this particular circular dependency.

*  Rust (on x86 or multilib systems)

   Set abi_x86_32 on dev-lang/rust or gnome-base/librsvg will fail to build.

*  librsvg

   Might require dev-lang/vala to be manually installed first.

*  GTK+

   Might require app-i18n/ibus² (if present) to be rebuilt first otherwise it will build but fail to install.

> **¹** For details, see: Gentoo bug #[550446](https://bugs.gentoo.org/550446).

Preparing Overlays
------------------

Install Layman¹ (checking that the `git` USE flag is enabled or `dev-vcs/git` is separately installed):

    emerge --ask layman

Add our `repositories.xml` file to Layman with `nano -w /etc/layman/layman.cfg`:

    #  original unsigned lists and definitions
    #  one url per line, indented

    overlays  :
        https://api.gentoo.org/overlays/repositories.xml
        https://raw.githubusercontent.com/dantrell/gentoo-overlay-dantrell-gnome/master/repositories.xml

Update the cached remote list(s):

    layman --fetch

If it went right it will say something similar to:

```sh
Fetching remote list,...
Remote list already up to date: http://www.gentoo.org/proj/en/overlays/repositories.xml
Last-modified: Sat, 01 Oct 2016 21:40:18 GMT
Fetching new list... https://raw.githubusercontent.com/dantrell/gentoo-overlay-dantrell-gnome/master/repositories.xml
Last-modified: Mon, 03 Oct 2016 00:31:03 GMT
Fetch Ok
```

Warnings like "an installed db file was not found at" are okay if Layman previously had no cached remote list.

> **¹** If updating old (<=2.0) layman installations, see: [Updating Old Layman Installations](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/README-MAYBE.md#updating-old-layman-installations). ↩

Fetching Overlays
-----------------

> **Note**: You can add as many GNOME release overlays as you want (even all of them). Your profile selection will determine which one takes precedence.

Add the generic GNOME overlay:

    layman --add dantrell-gnome

Add your preferred GNOME release overlay such as `GNOME 3.38`:

    layman --add dantrell-gnome-3-38

In the future, to update all overlays, use:

    layman --sync-all

Profile Selection
-----------------

> **Note**: If you do not select a profile, you are on your own.

There are two sets of bundled profiles:

1. The "defaults" which globally turns on [GNOME related](https://github.com/dantrell/gentoo-overlay-dantrell-gnome/blob/master/profiles/targets/desktop/gnome/make.defaults) USE flags.

2. The "extended versions" which closely mirrors Gentoo's GNOME profile¹ and globally turns on USE flags which [indirectly affect GNOME](https://github.com/dantrell/gentoo-overlay-dantrell-gnome/blob/master/profiles/targets/desktop/gnome/extended/make.defaults) in addition to the defaults.

> **¹** For details, see: [Differences Between GNOME Profiles](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/HACKING.md#differences-between-gnome-profiles). ↩

For immediate access to [hidden profiles](https://forums.gentoo.org/viewtopic-p-8438940.html#8438940) do:

    emerge --ask --oneshot --verbose app-admin/eselect

To list the bundled profiles use:

    eselect profile list

The relevant output will look similar to:

    [35]  dantrell-gnome:default/linux/amd64/17.1/desktop/gnome+plasma (stable)
    [36]  dantrell-gnome:default/linux/amd64/17.1/desktop/gnome+plasma/mainline (stable)
    [37]  dantrell-gnome-3-38:default/linux/amd64/17.1/desktop/gnome/3.38 (stable)
    [38]  dantrell-gnome-3-38:default/linux/amd64/17.1/desktop/gnome/3.38/extended (stable)

Profile Selection (`Gentoo Linux`)
----------------------------------

To expedite matters for a default install, use the bundled profile for either `amd64`:

    eselect profile set dantrell-gnome-3-38:default/linux/amd64/17.0/desktop/gnome/3.38

or `x86`:

    eselect profile set dantrell-gnome-3-38:default/linux/x86/17.0/desktop/gnome/3.38

> **Note**: If you are using a `Gentoo Linux` derivative, now is a good time to check if there are any instructions related to [profile selection](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/README-DERIVATIVES.md).

Configuration (Optional GNOME Applications)
-------------------------------------------

Even if you choose `gnome` instead of `gnome-light` in the [Installation](#installation) section, there are still some GNOME applications that won't be pulled in automatically. You can manage those through the USE flags on `gnome-base/gnome-extra-apps` with `nano -w /etc/portage/package.use/gnome-extra-apps`:

| USE flag    | Default | Release Version Supported¹ | Description
|-------------|---------|----------------------------|------------
| anjuta      | ✘ No    |                            | Install dev-util/anjuta (IDE)
| bijiben     | ✔ Yes   |                            | Install app-misc/bijiben (note editor)
| boxes       | ✘ No    |                            | Install gnome-extra/gnome-boxes (remote and virtual system manager)
| builder     | ✘ No    | `3.16`+                    | Install dev-util/gnome-builder (IDE)
| california  | ✘ No    |                            | Install gnome-extra/california (calendar)
| celluloid   | ✔ Yes   | `3.36`+                    | Install media-video/celluloid (media player)
| dino        | ✔ Yes   | `3.32`+                    | Install net-im/dino (chat client)
| empathy     | ✘ No    |                            | Install net-im/empathy (chat client)
| epiphany    | ✘ No    |                            | Install www-client/epiphany (web browser)
| evolution   | ✔ Yes   |                            | Install mail-client/evolution (mail client)
| flashback   | ✘ No    | (`WIP`)                    | Install gnome-base/gnome-flashback (session and helper application)
| foliate     | ✔ Yes   | `3.36`+                    | Install app-text/foliate (ebook viewer)
| fonts       | ✔ Yes   |                            | Install media-fonts/{noto,symbola,unifont} to complement media-fonts/cantarell
| games       | ✔ Yes   |                            | Install GNOME Games (a collection of small five-minute games in a variety of styles and genres)
| geary       | ✘ No    |                            | Install mail-client/geary (mail client)
| gnote       | ✘ No    |                            | Install app-misc/gnote (note editor)
| latexila    | ✘ No    |                            | Install app-editors/latexila (integrated LaTeX environment)
| multiwriter | ✘ No    | `3.16`+                    | Install gnome-extra/gnome-multi-writer (USB device writer)
| recipes     | ✔ Yes   | `3.22`+                    | Install gnome-extra/gnome-recipes (live cookbook)
| share       | ✔ Yes   |                            | Install gnome-extra/gnome-user-share (personal file sharing tool)
| shotwell    | ✔ Yes   |                            | Install media-gfx/shotwell (photo manager)
| simple-scan | ✘ No    |                            | Install media-gfx/simple-scan (document scanning utility)
| todo        | ✔ Yes   | `3.18`+                    | Install gnome-extra/gnome-todo (task manager)
| tracker     | ✔ Yes   |                            | Install app-misc/tracker (search engine, search tool and metadata storage system)
| usage       | ✔ Yes   | `3.28`+                    | Install gnome-extra/gnome-usage (system resources)

> **¹** If no release version is specified then all available versions are supported. ↩

Configuration (Optional Noto Fonts Styles, Glyphs & Format)
-----------------------------------------------------------

Google's Noto Fonts are (arguably) the most expansive typographic families ever made, supporting over 800 languages (even dead ones) and over 110,000 characters. As a result, however, it is enormous and subsequently can (in some cases) have a noticeable and reproducible impact on performance (directly related to the number of fonts processed).

You can manage this through the USE flags on `media-fonts/noto` with `nano -w /etc/portage/package.use/noto`:

| USE flag | Default | Release Version Supported¹ | Description
|----------|---------|----------------------------|------------
| emoji    | ✘ No    |                            | Support Emojis²
| extra    | ✘ No    |                            | Support Condensed, Extra or SemiBold styles²
| minimal  | ✘ No    |                            | Only support Latin, Greek and Cyrillic glyphs (~600 language)²
| ttf      | ✔ Yes   |                            | Use the TTF format (instead of the OTF format)²

By default, we mirror the result of the main tree but you may want to change this.²

> **¹** If no release version is specified then all available versions are supported. ↩

> **²** For details, see: [Alternatives to media-fonts/noto{,-cjk,-emoji}?](https://forums.gentoo.org/viewtopic-p-8279788.html). ↩

Configuration (Optional Quality of Life Improvements)
-----------------------------------------------------

These are optional patches which aim to improve the GNOME experience and are mostly aesthetic and performant in nature. They can be managed through the `deprecated*` and `vanilla*` USE flags (to determine what flag does what check their [classification](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/HACKING.md#classification)) on the following packages:

| Package                                | Default | Release Version Supported¹  | Description
|----------------------------------------|---------|-----------------------------|------------
| `gnome-base/gnome-control-center`      | ✔ Yes   | `3.14` - `3.30`             | Disables automatic datetime and timezone options
|                                        | ✔ Yes   | `3.14` - `3.30`             | Disables changing hostname
| `gnome-base/gnome-settings-daemon`     | ✔ Yes   | `3.28`+                     | Restores old timeout for user inactivity
| `gnome-base/gnome-shell`               | ✔ Yes   |                             | Restores old background code (avoids wallpaper corruption when resuming from suspend)
|                                        | ✔ Yes   |                             | Disables multiple asynchronous allocations (prevents a never-ending asynchronous loop)²
|                                        | ✔ Yes   |                             | Forces more frequent garbage collection
|                                        | ✔ Yes   |                             | Improves handling of motd when logging in (displays text in monospace and decreases the display time)
|                                        | ✔ Yes   |                             | Improves handling of screen blanking (uses the "Blank Screen" time as the idle delay)
| `gnome-base/gsettings-desktop-schemas` | ✔ Yes   | `3.32`+                     | Restores old Document and Monospace font defaults
| `gnome-base/nautilus`                  | ✔ Yes   | `3.16` & `3.18`             | Improves icons (adds another zoom level)
|                                        | ✘ No    | `3.16` & `3.18` (`Shelved`) | Use old icon grid and text width proportions (makes text labels in icon view warp at ~16 characters)
|                                        | ✔ Yes   |                             | Reorder context menu (primarily places "Open In Terminal" below "Open in New Tab" and other extensions below "Open With")
|                                        | ✔ Yes   |                             | Support slow double click to rename (triggers rename mode by clicking an icon twice with a pause in between)
|                                        | ✔ Yes   | `3.20`+                     | Support alternative search (find without recursion by pressing `CTRL` + `SHIFT` + `F`)
|                                        | ✔ Yes   | `3.22`+                     | Use old compress extension from File Roller instead of the native one
|                                        | ✔ Yes   |                             | Use FFmpeg as the video thumbnailer instead of Totem
| `media-video/totem`                    | ✔ Yes   |                             | Use FFmpeg as the video thumbnailer instead of Totem
| `x11-libs/vte`                         | ✔ Yes   | `3.32`+                     | Support desktop notifications from OSC 777
| `x11-terms/gnome-terminal`             | ✔ Yes   |                             | Restores background transparency
|                                        | ✔ Yes   |                             | Disables function keys (avoids clashes with command line utilities)
|                                        | ✔ Yes   | `3.30`+                     | Separate "New Window/Tab" menu entries by default
|                                        | ✔ Yes   | `3.32`+                     | Support desktop notifications from OSC 777
| `x11-themes/gnome-backgrounds`         | ✔ Yes   |                             | Replaces Adwaita live backgrounds (800x600) with the set from GNOME 3.10 (2560x1440)
| `x11-wm/mutter`                        | ✔ Yes   |                             | Restores old background code (avoids wallpaper corruption when resuming from suspend)
|                                        | ✘ No    |                             | Disables mipmapping (trades scaled and smoothed window previews for a measurable performance increase)

> **¹** If no release version is specified then all available versions are supported. ↩

> **²** For details, see: [gtk-embed: Don't allow multiple async allocations](https://github.com/endlessm/gnome-shell/commit/6d675c2c7df6652291f45f08293eadddc1ef65bc).

Configuration (Wayland)
-----------------------

> **Note**: This step is necessary since `X` is the default and `Wayland` requires the `elogind` implementation.

Assuming all other conditions are met, you can enable `Wayland` with `nano -w /etc/portage/make.conf`:

```sh
USE="${USE} -ck elogind"
USE="${USE} egl wayland"
```

Upgrading
---------

Regardless of whether GNOME is already installed or not, upgrade all installed packages, dependencies, and deep dependencies that are outdated or have USE flag changes (avoiding unnecessary rebuilds when USE changes have no impact):

    emerge --ask --update --deep --changed-use --with-bdeps=y @world

This step is necessary to switch to my forks of certain packages. As an added bonus, if you already have GNOME installed, it should just Do The Right Thing™.

Installation
------------

> **Note**: Build failures are possible if you have parallelism enabled. In such cases, re-issue the previous command.

Proceed with either:

    emerge --ask --keep-going gnome

or:

    emerge --ask --keep-going gnome-light

Finishing Touches
-----------------

> **Note**: The following commands assume you want a GUI-based login and NetworkManager support.

Configure `xdm` with `nano -w /etc/conf.d/xdm`:

```sh
# We always try and start X on a static VT. The various DMs normally default
# to using VT7. If you wish to use the xdm init script, then you should ensure
# that the VT checked is the same VT your DM wants to use. We do this check to
# ensure that you haven't accidentally configured something to run on the VT
# in your /etc/inittab file so that you don't get a dead keyboard.
CHECKVT=7

# What display manager do you use ?  [ xdm | gdm | kdm | gpe | entrance ]
# NOTE: If this is set in /etc/rc.conf, that setting will override this one.
DISPLAYMANAGER="gdm"
```

Allow users to use [direct rendering](https://wiki.gentoo.org/wiki/Xorg/Hardware_3D_acceleration_guide#Add_appropriate_user.28s.29_to_the_video_group):

    getent group video && gpasswd --add <username> video

Allow users to manage devices:

    getent group plugdev && gpasswd --add <username> plugdev

Allow users to run games which reside in `/usr/games/bin/`:

    getent group games && gpasswd --add <username> games

Configure services:

    rc-update del dhcpcd default

    rc-update add NetworkManager default
    rc-update add dbus default
    rc-update add xdm default

Finishing Touches (`ConsoleKit`)
--------------------------------

Configure services:

    rc-update add acpid default
    rc-update add consolekit default

    rc && exit

Finishing Touches (`elogind`)
-----------------------------

Configure services:

    rc-update add elogind boot

    rc && exit

Finishing Touches (text-based login)
------------------------------------

Configure `autostart` (startup applications) with `nano -w ~/.xprofile`:

```sh
gnome-terminal
```

Finishing Touches (text-based login with `ConsoleKit`)
------------------------------------------------------

Configure `startx` (or anything that calls xinit) with `nano -w ~/.xinitrc`:

```sh
# Fix Missing Applications in GNOME
export XDG_MENU_PREFIX=gnome-

# Properly Launch the Desired X Session
exec ck-launch-session gnome-session
```

Finishing Touches (text-based login with `elogind`)
---------------------------------------------------

Configure `startx` (or anything that calls xinit) with `nano -w ~/.xinitrc`:

```sh
# Fix Missing Applications in GNOME
export XDG_MENU_PREFIX=gnome-

# Properly Launch the Desired X Session
exec gnome-session
```

Switching
---------

> **Note**: If downgrading, use of binary packages will complicate the process.

Update all overlays (and fetch new ones):

    layman --sync-all

Add your preferred GNOME release overlay:

    layman --add dantrell-gnome-3-38

Set your profile to your preferred GNOME release:

    eselect profile set dantrell-gnome-3-38:default/linux/amd64/17.0/desktop/gnome/3.38

Upgrade all installed packages, dependencies, and deep dependencies that are outdated or have USE flag changes (avoiding unnecessary rebuilds when USE changes have no impact):

    emerge --ask --update --deep --changed-use --with-bdeps=y @world

Clean dependencies:

    emerge --ask --depclean

Uninstallation
--------------

> **Note**: The overlays do some of their work even if you don't use one of the bundled profiles so if you don't want to delete them, you must dereference them.

Start with either:

    emerge --ask --unmerge gnome

or:

    emerge --ask --unmerge gnome-light

Remove all of the project overlays:

    layman --delete dantrell-gnome
    layman --delete dantrell-gnome-3-38

Switch profiles (after reviewing any relevant¹ `Gentoo Linux` [news items](https://www.gentoo.org/support/news-items/)):

    eselect profile list
    eselect profile set <target>

Update `@world`:

    emerge --ask --update --deep --changed-use --with-bdeps=y @world

Clean dependencies:

    emerge --ask --depclean

> **²** For details, see: [New 17.0 profiles in the Gentoo repository](https://www.gentoo.org/support/news-items/2017-11-30-new-17-profiles.html) & [amd64 17.1 profiles are now stable](https://www.gentoo.org/support/news-items/2019-06-05-amd64-17-1-profiles-are-now-stable.html).

Tips
----

*  13.0, 17.0 & 17.1 Portage Profiles

   | Version | Description                                                                                                                         | Status
   |---------|-------------------------------------------------------------------------------------------------------------------------------------|-------
   | 13.0    | Optional PIE                                                                                                                        | Hidden¹
   | 17.0    | [Enforces PIE](https://wiki.gentoo.org/wiki/Hardened/Toolchain#Automatic_generation_of_Position_Independent_Executables_.28PIEs.29) | Hidden¹ for amd64; Visible for x86
   | 17.1    | Enforces PIE & [Removes /lib32 Directory](https://wiki.gentoo.org/wiki/Project:AMD64/Multilib_layout)                               | Visible

*  Sleep and Shutdown Hooks:

   | Implementation | Sleep                         | Shutdown
   |----------------|-------------------------------|---------
   | ConsoleKit     | /etc/pm/sleep.d               | /etc/pm/sleep.d
   | elogind-227.*  | /lib/elogind/system-sleep     | /lib/elogind/system-shutdown
   | elogind-234.0+ | /usr/lib/elogind/system-sleep | /usr/lib/elogind/system-shutdown

> **¹** For details, see: the latter half of Gentoo post #[8438940](https://forums.gentoo.org/viewtopic-p-8438940.html#8438940).

External Resources
------------------

* [GNOME/GNOME Without systemd](https://wiki.gentoo.org/wiki/GNOME/GNOME_Without_systemd)
* [GNOME/GNOME Without systemd/Dantrell](https://wiki.gentoo.org/wiki/GNOME/GNOME_Without_systemd/Dantrell)

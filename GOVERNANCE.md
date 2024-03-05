Long-term Support
-----------------

> **Note**: Now that Gentoo has a systemdless GNOME, this project's primary purpose can be said to have been met however the secondary purpose remains.

I previously assisted `Funtoo Linux` in officially implementing my patchset for GNOME 3.12 through 3.16. Now, through these overlays, users of `Gentoo Linux` can enjoy the fruits of my labor as well. On that note, I would like to thank [Sakaki](https://github.com/sakaki-/funtoo-2-gentoo) for her work facilitating matters in between.

That said, there is often a concern about how long a project will be supported. On this, I can say that:

* This project's primary purpose¹ is to buy time to come up with and implement a proper answer for GNOME without systemd. This is of particular interest to `Funtoo Linux` so even if I stop supporting this project entirely, it may be that they will continue to carry the torch for a time (albeit, with their own implementation).

The good news is that support for the majority (if not entirety) of the GNOME 3.20 series is likely since there really isn't much more to account for on top of what I have already done (i.e. updates will probably continue on schedule until early 2019). So, in short, I will support it for as long as I continue to use GNOME and their code changes do not make integrating my patchsets [too difficult](https://forums.gentoo.org/viewtopic-p-7588016.html#7588016).

* This project's secondary purpose is to allow switching between GNOME release versions. As such, even if a proper answer¹ for GNOME without systemd is found and implemented, updates will probably continue on schedule (again, provided that I still use GNOME).

> **¹** For details, see: [Long-term Solutions](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/POLITICS.md#long-term-solutions).

Updates
-------

If at all possible, release updates (such as 42 to 43) will occur at least twice per year.

Normal package updates will occur at least once per month.

Vulnerable package updates will occur ASAP.

This somewhat slow cycle is intended for stability reasons but `Funtoo Linux`, for instance, might opt to update things faster on their end. If they do, understand that they may not incorporate everything you find here and that they may incorporate everything differently.

Bugs and Other Issues
---------------------

Please remember that this project is not officially supported by GNOME or `Gentoo Linux` and that `Funtoo Linux` maintains their own implementation. As such, any emerge failure related to packages in the project overlays (especially packages listed in the [Plumbing](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/HACKING.md#plumbing) section) should be reported accordingly.

If you encounter an issue while using `Gentoo Linux`, contact me (according to preference):

* via the [support thread](https://forums.gentoo.org/viewtopic-t-1148720.html) where others can chime in as well; or
* [directly](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/CONTRIBUTING.md)

If you encounter an issue while using `Funtoo Linux` and their implementation, contact them:

* via their [bug tracker](https://bugs.funtoo.org)

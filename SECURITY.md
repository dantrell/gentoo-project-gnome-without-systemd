Security Advisories
-------------------

> **Note**: The older the GNOME release version the less you should use it (if at all) in a multi-user and/or web-facing manner.

I do keep my eyes open for various security advisories regarding GNOME-related packages and I do my best to apply fixes across all provided GNOME release versions.

Still, you may notice that some packages provided by the project will fail a GLSA (
Gentoo Linux Security Advisory)¹ check, however, in most (if not all) cases, I have already patched the related CVEs (and if you believe I missed some, feel free to contact me [directly](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/CONTRIBUTING.md)).

This is because Gentoo's preference towards resolving vulnerabilities is to upgrade the package and drop any vulnerable versions (which will result in a clean GLSA check) because (probably) it's easier. I backport (which will often fail a GLSA check) because I support multiple GNOME release versions.

However, there is a limit to what I can backport and to what GLSA covers. In particular when it comes to long-term support for considerably old GNOME release versions.

As such, please **use a recent GNOME release version unless you have taken the necessary precautions** (i.e. only interact with potentially vulnerable packages in a single-user and/or non-web-facing manner). In particular, watch out for the following packages:

* Evolution
* GNOME Online Accounts
* GVfs
* Python
  * 2.7
* WebKitGTK+
  * GNOME Web (codename: Epiphany)
  * Sushi

> **¹** For details, see: [Keeping Gentoo Secure](https://www.gentoo.org/support/security/).

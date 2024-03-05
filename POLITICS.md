Rationale
---------

When people discovered this project (long before it was ever on GitHub) and my role in initially bringing it to `Funtoo Linux`, they invariably asked me one of two questions (sometimes both):

1. But why?

2. What do you have against systemd?

My reply is twofold:

1. I wanted to test out some power management features in GNOME only to find out that support for it was removed. I promptly looked at the codebase and reverted the changes. It really was that simple and took me a day, tops. In fact, because I found it so trivial to do so I'm of opinion that upstream should have left the relevant code in and marked it as deprecated (but that's another matter).

   You see, almost the entire reason that I use `Gentoo Linux` is so that I can run my system the way I want. So basically, I did it because I could.

2. I don't have anything against systemd. What I dislike is how systemd is being forcibly implemented. If systemd was that beneficial to me, I would be voluntarily embracing it as I have done so many other changes, but it is not that beneficial and so I will not embrace it, especially not when it is forced on me.

   Now, I don't think that change is inherently a bad thing. Sometimes it is sorely needed but there is a process to these things. Take the case of Windows 8 and Metro. Microsoft would have saved themselves so much grief if they had made Metro the default as they did but gave the users who weren't ready to be forcibly transitioned to it the option of reverting to the prior long-time default. Especially when to do so would have cost them nothing.

   It is poor thinking to make a choice for your users based on what you think is right for them and rationalize it as correct in the face of strong evidence to the contrary because you believe that they just need time "to see the light". Sometimes, when you are wrong, you are just wrong. But it is terrible thinking to trap your users into that choice without giving them a way out because then they have no option but to fight back. This logic applies to many facets of life.

Long-term Solutions
-------------------

* [elogind](https://github.com/elogind/elogind)¹

> **¹** For details, see: [GNOME in GuixSD](https://savannah.gnu.org/forum/forum.php?forum_id=8491), [GNOME in Slackware](https://github.com/w41l/gnome) and [GNOME in Void Linux](https://www.voidlinux.eu/news/2017/03/Gnome-3.22.html). ↩

wdoekes build of (Xonotic) NetRadiant for Ubuntu/Debian
=======================================================

Radiant is the open source cross platform level editor for idTech games
like Quake III Arena.

There are at least three competing versions out there:

- `GtkRadiant <https://github.com/wdoekes/gtkradiant-deb>`_ 1.6 (a
  continuation of the original 1.4, maintained by TTimo);
- `NetRadiant <https://github.com/wdoekes/netradiant-deb>`_ 1.5 (a
  continuation of the original 1.5, maintained by Xonotic);
- NetRadiant-custom (TBD, maintained by Garux).

This repository contains build tools to build Debian/Ubuntu packages for
`NetRadiant <https://gitlab.com/xonotic/netradiant>`_ along with the
*q3map2* compiler and *bspc* bot file builder.

In the `releases section <../../releases>`_, you might find some
precompiled debian packages... if you're lucky. But if there aren't any,
building for your specific Debian derivative should be a breeze.


Building NetRadiant for your distro
-----------------------------------

Using Docker::

    ./Dockerfile.build [ubuntu/focal]

If the build succeeds, the built Debian packages are placed inside (a
subdirectory of) ``Dockerfile.out/``.

The files may look similar to these::

    -     16,411  netradiant_1.5.0+20220215+3-0wjd1+ubu20.04_amd64.buildinfo
    -      3,026  netradiant_1.5.0+20220215+3-0wjd1+ubu20.04_amd64.changes
    -  2,225,780  netradiant_1.5.0+20220215+3-0wjd1+ubu20.04_amd64.deb
    -     11,584  netradiant_1.5.0+20220215+3-0wjd1+ubu20.04.debian.tar.xz
    -      1,333  netradiant_1.5.0+20220215+3-0wjd1+ubu20.04.dsc
    -  9,997,240  netradiant_1.5.0+20220215+3.orig.tar.gz
    - 26,300,636  netradiant-dbgsym_1.5.0+20220215+3-0wjd1+ubu20.04_amd64.ddeb
    -    616,240  netradiant-game-quake3_1.5.0+20220215+3-0wjd1+ubu20.04_all.deb

The ``netradiant*.orig.tar.gz`` contains source files from multiple
repositories. With ``SOURCE_VERSION`` files in the directories,
recording the exact versions. (NOTE: For the gamepacks, exact versions
are not recorded.)

The ``netradiant*.deb`` holds files in::

    - /usr/bin (netradiant, bspc, mbspc, daemonmap, q2map, q3map2, ...)
    - /usr/lib/x86_64-linux-gnu/netradiant (plugins/modules)
    - /usr/share/netradiant (arch independent data files, images)

The ``netradiant-game-quake3*.deb`` holds files in::

    - /usr/share/netradiant/gamepacks/games/q3.game
    - /usr/share/netradiant/gamepacks/q3.game (game data)


Running NetRadiant
------------------

Starting should be a matter of running ``netradiant``::

    $ dpkg -L netradiant | grep ^/usr/bin/
    /usr/bin/bspc
    /usr/bin/bspc.ttimo
    /usr/bin/daemonmap
    /usr/bin/h2data
    /usr/bin/mbspc
    /usr/bin/netradiant
    /usr/bin/q2map
    /usr/bin/q3data
    /usr/bin/q3map2
    /usr/bin/qdata3

Game configuration will be stored in ``~/.config/netradiant``::

    $ find ~/.config/netradiant -type f | sort
    ~/.config/netradiant/global.pref
    ~/.config/netradiant/prtview.ini
    ~/.config/netradiant/q3.game/local.pref
    ~/.config/netradiant/q3.game/shortcuts.ini
    ~/.config/netradiant/radiant.log
    ~/.config/netradiant/XMLDump.xml

Shader config is scanned in this order (continues even when found)::

    ~/Documents/q3maps/baseq3/scripts/shaderlist.txt
    ~/.q3a/baseq3/scripts/shaderlist.txt
    /usr/share/netradiant/base/scripts/shaderlist.txt

Shader image locations are scanned in this order (stops when found)::

    /usr/share/netradiant/base/textures/common/watercaulk.jpg
    ~/.q3a/baseq3/textures/common/watercaulk.jpg
    ~/Documents/q3maps/baseq3/textures/common/watercaulk.jpg
    /usr/share/netradiant/base/textures/common/watercaulk.tga
    ~/.q3a/baseq3/textures/common/watercaulk.tga
    ~/Documents/q3maps/baseq3/textures/common/watercaulk.tga


Other
-----

See `<README-quake3.rst>`_ for Quake3 specific setup.


BUGS/TODO
---------

* See if we can find an appropriate version better than
  1.5.0+20220215+3.

* Check if netradiant-custom is better suited for map Quake3 editing, see:
  https://github.com/Garux/netradiant-custom/issues/7

* Document/decide on handling the gamepacks:

  - do we want to record source versions, we don't right now;

  - use quake3 instead of q3 for naming, because of better findability;

  - only put one game in a gamepack, we may want to manually create
    gamepacks: the gtkradiant versions contain more contents (example
    maps).

* Check if we need gnome-themes-extra, gtk2-engines-murrine,
  libcanberra-gtk-module, which are listed in the control file.

* Right now there is only a tiny index.html in
  /usr/share/netradiant/docs. We *could* move that to
  /usr/share/doc/netradiant.

* The netradiant-game-quake3 has plenty of docs in
  /usr/share/netradiant/gamepacks/q3.game/docs. Do we want to move that
  to /usr/share/doc/netradiant?

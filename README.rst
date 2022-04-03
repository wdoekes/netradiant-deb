wdoekes build of (Xonotic) NetRadiant for Ubuntu/Debian
=======================================================

Using Docker::

    ./Dockerfile.build

If the build succeeds, the built Debian packages are placed inside (a
subdirectory of) ``Dockerfile.out/``.

The files may look similar to these::

    -     16,387  netradiant_1.5.0+20220215+3-0wjd0+ubu20.04_amd64.buildinfo
    -      3,023  netradiant_1.5.0+20220215+3-0wjd0+ubu20.04_amd64.changes
    -  2,084,940  netradiant_1.5.0+20220215+3-0wjd0+ubu20.04_amd64.deb
    -      9,992  netradiant_1.5.0+20220215+3-0wjd0+ubu20.04.debian.tar.xz
    -      1,330  netradiant_1.5.0+20220215+3-0wjd0+ubu20.04.dsc
    -  9,589,026  netradiant_1.5.0+20220215+3.orig.tar.gz
    - 25,811,996  netradiant-dbgsym_1.5.0+20220215+3-0wjd0+ubu20.04_amd64.ddeb
    -    616,072  netradiant-game-quake3_1.5.0+20220215+3-0wjd0+ubu20.04_all.deb

The ``netradiant*.orig.tar.gz`` contains source files from multiple
repositories. With ``SOURCE_VERSION`` files in the directories,
recording the exact versions. (NOTE: For the gamepacks, exact versions
are not recorded.)

The ``netradiant*.deb`` holds files in::

    - /usr/bin (netradiant, bspc, daemonmap, q2map, q3map2, ...)
    - /usr/lib/x86_64-linux-gnu/netradiant (plugins/modules)
    - /usr/share/netradiant (arch independent data files, images)

The ``netradiant-game-quake3*.deb`` holds files in::

    - /usr/share/netradiant/gamepacks/games
    - /usr/share/netradiant/gamepacks/q3.game


Running NetRadiant
------------------

Starting should be a matter of running ``netradiant``::

    $ dpkg -L netradiant | grep ^/usr/bin/
    /usr/bin/bspc
    /usr/bin/daemonmap
    /usr/bin/h2data
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


BUGS/TODO
---------

* See if we can find an appropriate version better than
  1.5.0+20220215+3.

* Document/decide on handling the gamepacks:
  - use quake3 instead of q3 for naming, because of better findability;
  - only put one game in a gamepack, we may want to manually create
    gamepacks: the gtkradiant versions contain more contents (example
    maps).

* Check if source tgz is okay (no large blobs, and complete).

* We should check that we can replace this with GtkRadiant and vice
  versa without additional configuration hell.

* Check if we need any patches from
  https://github.com/wdoekes/gtkradiant-deb/tree/main/patches

* Check if we need to do any additional README from
  https://github.com/wdoekes/gtkradiant-deb/blob/main/README.rst

* Do we want/need to move docs to /usr/share/doc/netradiant? Right now
  some/all docs are in /usr/share/netradiant.

* Not sure if we want DAEMONMAP and CRUNCH enabled. Do we?

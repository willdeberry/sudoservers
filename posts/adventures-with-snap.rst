.. title: Adventures with Snap
.. slug: adventures-with-snap
.. date: 2017-08-07 20:52:36 UTC-04:00
.. tags: ubuntu, snap, snapcraft
.. category: snap
.. link: 
.. description: 
.. type: text

And here comes another package format :) .

So as I was just getting used to docker and building debian/ubuntu packages (.deb), there is now a new format trying to become standard. This is snap packages. I know, I know...there is also flatpacks, but while I have investigated, seems like a bit higher barrier to entry than what snaps offer up front.

While I won't go through the entire process of building and setting things up, I just wanted to hit on a couple points I learned recently. For a full break down, feel free to head over to `snapcraft.io`_ to get all the details. The gist of things is that with a simple yaml file, you can create a snap package to distribute your code. For example, here is what I used for packaging up my bluetooth utility Bjarkan_:

.. code:: yaml

    name: bjarkan
    architectures: [amd64]
    version: '1.2.0'
    summary: Command line bluetooth utility
    description: |
      A command line bluetooth utility that allows you to completely manage all
      things bluetooth. Scanning, pairing, listing of devices.

    grade: stable # must be 'stable' to release into candidate/stable channels
    confinement: strict # use 'strict' once you have the right plugs and slots

    apps:
      bjarkan:
        command: env GI_TYPELIB_PATH=$SNAP/usr/lib/girepository-1.0:$SNAP/usr/lib/x86_64-linux-gnu/girepository-1.0 bjarkan

    parts:
      bjarkan:
        plugin: python
        stage-packages:
          - gir1.2-gtk-3.0
          - python3-dbus
          - python3-gi

.. _snapcraft.io: https://snapcraft.io/docs/build-snaps/your-first-snap

.. _Bjarkan: https://github.com/willdeberry/bjarkan

.. _exa: https://github.com/ogham/exa

.. _`classic confinement`: https://insights.ubuntu.com/2017/01/09/how-to-snap-introducing-classic-confinement/

The point and learning curve I wanted to cover here is the confinement section to the yaml file and the comment that comes along with the file after you initialize the directory, I believe, doesn't help with things. The issue is when to use ``strict`` versus other options. When you are developing and testing your snap locally, you are recoomended to use ``confinement: devmode``. Then just like the comment in the file states, you are recommeneded to use ``strict`` once you are ready to release. The big difference here is access and interaction with the rest of the operating system. This became really apparent when I went to snap up a package that I came across via reddit (exa_).

So the problem here is that ``exa`` sole purpose is to be a simple replacement for ``ls`` which inherently interacts with the file system to give the user some feedback and details about their files. Well with a ``strict`` confinement, this completely falls apart. You are presented with OS and permission denied errors.

.. code:: console

    [wdeberry-linux ~]$ /snap/bin/exa /
    /: Permission denied (os error 13)

This is because what is in a snap package, literally cannot interact with anything outside of it's own installation directory. However, there is an answer! Since ``snapd`` version 2.20, Canonical has introduced `classic confinement`_. This allows you to still build a completely bundled and isolated package for installation purposes, but be able to interact with the rest of the system.

Such a simple change but something that can have huge impact on how you design your snap package going forward.

    .. note::

        I just have to take moment and highly recommend integration with https://snapcraft.io/. This simple service allows you to tie changes pushed to your project on github. Thus creating a new snap package ready for deployment within the snap store all for free :).
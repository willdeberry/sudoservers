.. title: A Crazy Attempt at KDE Plasma on macOS
.. slug: a-crazy-attempt-at-kde-plasma-on-macos
.. date: 2017-06-03 20:48:00 UTC-04:00
.. tags: tech, kde, docker, macOS
.. category: docker
.. link: 
.. description: 
.. type: text

So I started an adventure to see what it would take and see if even possible to get some kind of linux desktop environment running successfully on macOS. Let it be known while this was fun, this definitely did not work nor was practical.

The first step was to download and get docker setup on my mac. This all started because I heard about a simple program called kitematic on a linux podcast last week. Kitematic is a simple GUI wrapper around docker hub and docker in general and allows you to manage things without being intimidated. You can download this great tool via https://kitematic.com/.

Once you have that installed, I downloaded the kdeneon sponsored ``all`` image. This quickly led to the snowball that would eventually takeover this side project. I found out real quick that it was having a hard time finding the :code:`DISPLAY` variable 

.. image:: /images/display.png

This was resolved via installing xquartz for macOS. You can get this via https://www.xquartz.org/. The second step was to run the command :code:`socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\"` in a separate terminal. Once you have this running in a terminal, you need to set the :code:`DISPLAY` variable to following in Kitematic: :code:`ipaddress:0` (replacing ipaddress with you address). At this point the docker image should start for you, and this is where the layers of crazy start coming out.

You will notice that once you fix the :code:`DISPLAY` variable issue, you now run into openGL issues. You need to edit the file :code:`/opt/X11/bin/startx` and look for the options :code:`defaultserverargs`. Add :code:`+iglx` to those options using you favorite editor and save. Log out and back in to restart xquartz and give it another try.

At this point, you start seeing some real craziness, red screens and all.

.. image:: /images/red.png

This is where I got stuck. Apparently xqaurtz is stuck to openGL 2.1 where KDE and more modern X11 programs expect you to be able to produce openGL 3+.

At this point, I wouldn't say I have given up, but defiitely was a fun experiement and provided great insight. We shall see where this leads me.

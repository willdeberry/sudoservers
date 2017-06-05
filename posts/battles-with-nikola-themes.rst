.. title: Battles with Nikola Themes
.. slug: battles-with-nikola-themes
.. date: 2017-06-04 20:12:43 UTC-04:00
.. tags: nikola theme
.. category: nikola
.. link: 
.. description: 
.. type: text

So after spending all day, I finally got my blog themed the way I wanted.

The goal was to get the blog running using the Zen_ theme. However this for one reason or another was not as straight forward as I intended it to be. Maybe this was due to my ignorance with Nikola or just reading too much into things.

.. _Zen: https://themes.getnikola.com/v7/zen/

The process started by reading the WARNINGS on the Zen site about the need for the ``less`` plugin. At this point I installed the ``less`` plugin via :code:`nikola plugin -i less`. I believe this was the start of the errors I faced. From this point on, every time I tried and build my blog, I got the following errors:

.. code-block::

   Scanning posts......done!
   ERROR: Two different tasks can't have a common target.'output/assets/css/main.css' is a target for copy_assets:output/assets/css/main.css and build _less:output/assets/css/main.css.

So long story short, my PSA at this point and time is if you are getting this error when building, remove the ``less`` plugin and continue on. I dealt with this for about 2 hours and thus wanted to provide a heads up to other fellow bloggers.
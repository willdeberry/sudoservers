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

So I reached out to the all knowing Google and was led around in circles and basically found nothing that could help me actually build with the theme I wanted. Frustration and giving up started to creep in. So I went back to the WARNINGS that led me down this path in the beginning.

  WARNING: The zen themes are LESS-powered (not less... because it is more-powered ;-)) If you use webassests (USE_BUNDLES = True in your conf.py), the theme will use compiled css files, so don't worry at all... But, if you want to build the css files from the zen LESS files, you have to use USE_BUNDLES = False, and have installed the lessc (official LESS compiler). Additionaly, you have a LESS plugin available in the Nikola plugins repo to automatically build the LESS files inside nikola build command. You can easily install it just doing: nikola plugin -i less.

I then dug through :code:`conf.py` and realized that :code:`USE_BUNDLES` is set to ``True`` by default. This finally caused a light bulb to go off and thus I attempted to run things without ``less``. So I uninstalled the ``less`` plugin :code:`nikola plugin -r less`. This was finally the tipping point and got things working for me. All is good now but that was definitely a frustrating battle I was not ready for.

TL;DR: If you are getting this error when building, remove the ``less`` plugin and continue on. I dealt with this for about 2-3 hours and thus wanted to provide a heads up to other fellow bloggers.
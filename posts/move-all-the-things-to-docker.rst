.. title: Move all the things to Docker
.. slug: move-all-the-things-to-docker
.. date: 2017-07-11 22:02:12 UTC-04:00
.. tags: docker esxi vm
.. category: docker, virtualization
.. link: 
.. description: 
.. type: text

So what happens when you have a local esxi server and you realize that your resource usage is higher than what you are actually using....you blow it all away and migrate to docker! :)

My setup consisted of 3 VMs total. A VM for a web server ``(nginx)``, a database VM which was both ``mysql`` and ``mongodb``, and lastly, a DNS server (``bind``) that also doubled as a ``salt`` server. Obviously, the first thing done is back up all your files. In my case this involved nginx configurations, dns server configurations and a couple of databases I had running. I have also backed up my salt configurations but being that I am down sizing on the amount of servers to manage, not sure yet that salt actually fits or is even needed with the new setup.

So outside of having to learn docker and docker-compose in general, there were 2 hurdles that really dragged this process out beyond a couple of hours to a couple of days. The first hurdle I ran into was getting letsencrpyt working properly. Because who wants to run a web server on http anymore, especially when a fully qualified cert is free and super easy to get. The second hurdle was getting nextcloud installed using their fpm image.

The first hurdle was definitely the least worries of the two, but was definitely frustrating since I hit this wall 10 minutes into the transition. For letsencrpyt, I mainly used this guide_, and their source code to piece things together. The thing that killed me was copying and pasting was ``line 18`` from their docker-compose.yml_ file. :code:`--standalone-supported-challenges http-01` is a depracated option to the certbot command now. So when I saw this, I ended up removing that argument alltogether rather than understanding what it was trying to accomplish. This led me down a 2 hour whole of trying to figure out why ``letsencrypt`` was trying to use port 443 for generating a cert when 443 wasn't listening yet in the webserver, since I didn't have a cert. The answer, once I read the man pages, was to use :code:`--preferred-challenges http`. This forced things to be done over port 80 and generated the certs properly.

.. _guide: https://bitbucket.org/automationlogic/le-docker-compose.git

.. _docker-compose.yml: https://bitbucket.org/automationlogic/le-docker-compose/src/2f1b37b842e3ed9aaa6aef645f7e0f6782308c1d/docker-compose.yml?at=master&fileviewer=file-view-default#docker-compose.yml-18
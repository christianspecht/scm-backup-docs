FAQ
===

.. _why-no-local-backup:

Why doesn't SCM Backup support backing up local installations of [hoster X]?
----------------------------------------------------------------------------

SCM Backup only supports backing up from the hosters' cloud offerings. Most hosters also support some kind of "install on your own server" version of their product, **but SCM Backup doesn't support any of them, and this is very unlikely to change.**

The main reason is: Supporting local installations would make automated testing much more complicated.

SCM Backup has a `load of integration tests <https://github.com/christianspecht/scm-backup/tree/master/src/ScmBackup.Tests.Integration/Hosters>`_ which are executed for each supported hoster, calling the hoster's API and actually backing up test repositories - all by just using the hoster's existing cloud infrastructure.

For "install on your own server" versions, setting this up would be much more complicated: For each hoster, we'd need a local installation running somewhere...and reachable from the internet, so our CI provider (currently `AppVeyor <https://www.appveyor.com>`_) can access it.

And not all hosters even offer a free-as-in-beer version of their local product - for example, the only version of Bitbucket Server that's freely available `seems to be a 30 day test version <https://www.atlassian.com/software/bitbucket/download>`_.

.. note::

    If you are using a local installation of a hoster supported by SCM Backup *(or any other Git/Mercurial/whatever server that can be installed on a machine you control)*, you probably don't need a tool like SCM Backup!

    You (or at least someone in your organization) should have enough access to the machine, so you can directly backup the folder from that machine which contains all the repositories.
    
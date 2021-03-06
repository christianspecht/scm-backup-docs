Installation
============

.. _install-requirements:

System Requirements
-------------------

.NET Core
+++++++++

SCM Backup is written in `.NET Core <https://dotnet.github.io/>`_, the cross-platform version of .NET.

The available releases are `framework-dependent deployments <https://docs.microsoft.com/en-us/dotnet/core/deploying/>`_, which means that the same download should work on any Windows, Linux and MacOS machine, as long as .NET Core is installed on it.

If it's not on your machine, you can get it from the `official download page <https://www.microsoft.com/net/download>`_. The exact version you need is **version 3.1** of the .NET Core **runtime**.

.. note::

    So far, SCM Backup has been written and tested on Windows only. Technically, it should run on Linux and MacOS as well, but this has not been tested yet.


Source control software
+++++++++++++++++++++++

SCM Backup doesn't come with its own versions of `Git <https://git-scm.com/>`_ and/or `Mercurial <https://www.mercurial-scm.org/>`_, so the respective SCM needs to be installed on your machine if you have at least one repository of the given type.

By default, SCM Backup expects all source control software to be in your path, so it just needs to execute ``git``, ``hg`` etc. without a complete path, although it's possible to :ref:`specify the path to the executable in the config <config-scms>`.

Note that at runtime, SCM Backup checks the presence of all required SCMs on your system. It will stop if you have repositories needing a SCM which is not present on your system.


Download
--------

At the moment, there are only .zip downloads.

Download the .zip file from `the latest release <https://github.com/christianspecht/scm-backup/releases/latest>`_ and unzip it into a folder of your choice.


How to run
----------

.. warning::

    You should edit the configuration file before running SCM Backup for the first time!
    
    :doc:`Read the guide </config>` for more information.

Run the actual application by executing ``ScmBackup.exe`` 



Windows Task Scheduler
++++++++++++++++++++++

To run SCM Backup via Windows Task Scheduler, you need to specify the path to the exe **and** the directory (in "Start in") in the "Edit Action" screen:

.. image:: install-scheduler.png

Omitting the directory `can cause problems <https://github.com/christianspecht/scm-backup/issues/30>`_.



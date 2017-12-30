Installation
============

System Requirements
-------------------

SCM Backup is written in `.NET Core <https://dotnet.github.io/>`_, the cross-platform version of .NET.

The available releases are `framework-dependent deployments <https://docs.microsoft.com/en-us/dotnet/core/deploying/>`_, which means that the same download should work on any Windows, Linux and MacOS machine, as long as .NET Core is installed on it.

If it's not on your machine, you can get it from the `official download page <https://www.microsoft.com/net/download>`_. You need at least **version 2.0** of the .NET Core **runtime**.

.. note::

    So far, SCM Backup has been written and tested on Windows only. Technically, it should run on Linux and MacOS as well, but this has not been tested yet.


Download
--------

At the moment, there are only .zip downloads.

Download the .zip file from `the latest release <https://github.com/christianspecht/scm-backup/releases/latest>`_ and unzip it into a folder of your choice.


How to run
----------

.. warning::

    You should edit the configuration file before running SCM Backup for the first time!
    
    :doc:`Read the guide </config>` for more information.

The actual application is in the ``ScmBackup.dll`` library. You can execute it with the ``dotnet`` command::

    dotnet ScmBackup.dll

For Windows, there's a batch file named ``ScmBackup.bat`` which does exactly that.

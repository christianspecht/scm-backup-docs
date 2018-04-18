Implementing a new hoster
=========================

Steps how to implement support for backing up a new source code hoster, using the implementation for Bitbucket as an example.



Basics
------

In the ``ScmBackup`` project, create a new subfolder in the `"Hosters" folder <https://github.com/christianspecht/scm-backup/tree/master/src/ScmBackup/Hosters>`_ and name it like the hoster you are implementing, e.g. ``Bitbucket``.

Inside the folder, create the classes listed below.

.. note::
    SCM Backup uses naming conventions to put everything together, so make sure that:
    
    - all classes have exactly the same prefix
    - the part after the prefix is exactly like in the examples below
    
To see examples, take a look at the respective classes of the existing hosters (e.g. `GitHub <https://github.com/christianspecht/scm-backup/tree/master/src/ScmBackup/Hosters/Github>`_, `Bitbucket <https://github.com/christianspecht/scm-backup/tree/master/src/ScmBackup/Hosters/Bitbucket>`_).



Hoster
------

- Example class name: ``BitbucketHoster``
- Must implement ``IHoster``



ConfigSourceValidator
---------------------

Validates all config sources for that hoster.

- Example class name: ``BitbucketConfigSourceValidator``
- Must inherit from ``ConfigSourceValidatorBase``, which implements ``IConfigSourceValidator`` and contains "default" rules which apply to all hosters



Api
---

- Example class name: ``BitbucketApi``
- Must implement ``IHosterApi``
- Should call the hoster's API and return a list of repository metadata for the current user or organization


Backup
------

- Example class name: ``BitbucketBackup``
- Must inherit from ``BackupBase``, which implements ``IBackup`` and creates the actual backups by cloning the repositories.


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

.. note::

    To see examples, take a look at:

    - the respective classes of the existing hosters
        - Examples: `GitHub <https://github.com/christianspecht/scm-backup/tree/master/src/ScmBackup/Hosters/Github>`_, `Bitbucket <https://github.com/christianspecht/scm-backup/tree/master/src/ScmBackup/Hosters/Bitbucket>`_
    - their tests:
        - ``...ConfigSourceValidatorTests`` in the `unit tests <https://github.com/christianspecht/scm-backup/tree/master/src/ScmBackup.Tests/Hosters>`_
        - ``...ApiTests``/``...BackupTests`` in the `integration tests <https://github.com/christianspecht/scm-backup/tree/master/src/ScmBackup.Tests.Integration/Hosters>`_


Hoster
------

- Example class name: ``BitbucketHoster``
- Must implement ``IHoster``



ConfigSourceValidator
---------------------

Validates all config sources for that hoster.

- Example class name: ``BitbucketConfigSourceValidator``
- Must inherit from ``ConfigSourceValidatorBase``, which implements ``IConfigSourceValidator`` and contains "default" rules which apply to all hosters
- Tests: Create a new class in ``ScmBackup.Tests.Hosters`` which inherits from ``IConfigSourceValidatorTests``



Api
---

- Example class name: ``BitbucketApi``
- Must implement ``IHosterApi``
- Should call the hoster's API and return a list of repository metadata for the current user or organization
- Tests: Create a new class in ``ScmBackup.Tests.Integration.Hosters`` which inherits from ``IHosterApiTests``


Backup
------

- Example class name: ``BitbucketBackup``
- Must inherit from ``BackupBase``, which implements ``IBackup`` and creates the actual backups by cloning the repositories.
- Tests: Create a new class in ``ScmBackup.Tests.Integration.Hosters`` which inherits from ``IBackupTests``

.. note::

    When a hoster supports multiple SCMs, you want to test backups with all of them, so you should create a separate test class for each SCM.
    
    An example for this is Bitbucket, which supports Git **and** Mercurial, so there are `BitbucketBackupGitTests <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup.Tests.Integration/Hosters/BitbucketBackupGitTests.cs>`_ and `BitbucketBackupMercurialTests <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup.Tests.Integration/Hosters/BitbucketBackupMercurialTests.cs>`_.



More about the tests
--------------------

The base classes for the tests (``IConfigSourceValidatorTests``, ``IHosterApiTests``, ``IBackupTests``) contain all the tests and a few properties, some of them abstract or virtual.

The child classes just need to inherit from the respective base class and fill the properties *(for repo URLs, commit IDs etc.)*.

So the same tests are executed for each ``IConfigSourceValidator``, ``IHosterApi`` and ``IBackup`` implementation (please see also :doc:`contribute-app-tests`).


.. note::

    For special cases, which only apply to a certain implementation, you can create additional tests directly in the child class instead of the base classes.
    
    One example for this is the Github API. There's `a special quirk <https://github.com/christianspecht/scm-backup/issues/13>`_ which only occurs in the Github API.
    
    Because of this, we have a special integration test for this, directly in the `GithubApiTests <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup.Tests.Integration/Hosters/GithubApiTests.cs>`_ class, so it's only executed there, and not for all ``IHosterApi`` implementations.


Documentation
-------------

Add the hoster to the lists on `the website's front page <https://github.com/christianspecht/scm-backup-site/blob/master/index.md>`_, and on the `Introduction <https://github.com/christianspecht/scm-backup-docs/blob/master/source/intro.rst>`_ page in this documentation.


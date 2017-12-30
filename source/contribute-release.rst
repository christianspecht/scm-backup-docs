Making a new release
====================

To make a new release version, the following steps must be followed:

1. Determine the new version number
-----------------------------------

SCM Backup uses `Semantic Versioning <https://semver.org/>`_.

The new version number **must** be in "three-digit" ``MAJOR.MINOR.PATCH`` format, for example ``1.0.0`` !!


2. Release the application
--------------------------

Each push to ``master`` creates a new CI build on `AppVeyor <https://ci.appveyor.com/project/ChristianSpecht/scm-backup>`_ anyway.

Create a new release by creating a Git tag with the new version number.

The CI build will recognize this and automatically use this version number to create a new `GitHub release <https://github.com/christianspecht/scm-backup/releases>`_.

.. note::

    Don't forget to actually push the tag! Git doesn't do this automatically.
    
    - From the command line, it's ``git push origin 1.0.0``.
    
    - In `Git GUI <https://git-scm.com/docs/git-gui>`_, you need to set this checkbox when pushing:
    
        .. image:: contribute-git-push-tags.png


3. Release the docs
-------------------

- Set the `version number <http://www.sphinx-doc.org/en/stable/config.html#confval-version>`_ in the `Sphinx configuration file <https://github.com/christianspecht/scm-backup-docs/blob/master/source/conf.py>`_ ``conf.py`` to the new version number, but in short ``X.Y`` format.

    Apparently Read the Docs uses this number at least in the automatically created PDF.

- Create the same Git tag with the same version number in the documentation repository as well.

    This will create a version of the documentation for this release, making use of `Read the Docs' versioning capabilities <http://docs.readthedocs.io/en/latest/versions.html>`_.

Restoring your backups
======================

Folder structure
----------------

For an example of the folder structure SCM Backup creates, we'll use the `default config file that comes with SCM Backup <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup/settings.yml>`_. It looks like this (only the parts that are relevant here)::

    # all backups go here
    localFolder: 'c:\scm-backup'

    sources:

      - title:  github_singleuser
        hoster: github
        type: user
        name: scm-backup-testuser

- Inside the ``localFolder``, SCM Backup creates one subfolder for each source defined in the config, named like the source's ``title``
- Inside that, it creates one subfolder for each repository
- Inside *that*, it creates subfolders:

  - ``repo`` for the actual repository
  - ``wiki`` if the repository has a wiki
  
At the time of writing, the `GitHub user "scm-backup-testuser" <https://github.com/scm-backup-testuser>`_ has two visible repositories (used for integration tests):

- `scm-backup-testuser/scm-backup <https://github.com/scm-backup-testuser/scm-backup>`_ *(has a wiki)*
- `scm-backup-testuser/wiki-doesnt-exist <https://github.com/scm-backup-testuser/wiki-doesnt-exist>`_ *(doesn't have a wiki)*

So SCM Backup would create this folder structure (``C:`` is the main drive on Windows)::

    C:
    └───scm-backup
        └───github_singleuser
            ├───scm-backup-testuser#scm-backup
            │   ├───repo
            │   └───wiki
            └───scm-backup-testuser#wiki-doesnt-exist
                └───repo
    
.. note:: The ``repo`` and ``wiki`` subfolders are the bare repositories that the next section refers to.


How to restore
--------------

Generally, SCM Backup creates local repositories and pulls from the remote repositories into the local ones.

Those local repositories are **bare repositories**, i.e. they don't contain a working directory.

When you look inside the repository directories, you'll see some directories and files, depending on whether it's a Git/Mercurial/etc. repository.

**Your complete history and your source code are in there - you just don't see the actual files!**

The repository is backed up without a working directory, because it's not necessary.

All the data already exists inside the repository, a second copy of everything in the working directory would just be a waste of space.

The easiest way to restore your working directory is to clone the bare repository that SCM Backup created *(called* ``bare-repo`` *in the examples)*, which will create a clone **with** a working directory *(called* ``working-repo`` *in the examples)*.

For more details, please see the sub-page for the respective SCM:

.. toctree::
   :maxdepth: 1
   
   restore-git
   restore-hg

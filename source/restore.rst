Restoring your backups
======================

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

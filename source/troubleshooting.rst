Troubleshooting
===============

Simulate SCM Backup's Git usage
-------------------------------

If something goes wrong in the moment when SCM Backup is actually trying to backup your repos, you can manually do the same steps to verify if they work directly in Git.

*(if they don't, the problem may be caused by an issue with your local Git installation)*

SCM Backup does this:

#. If the local repo doesn't exist yet, `create an empty one <https://github.com/christianspecht/scm-backup/blob/1.5.0/src/ScmBackup/Scm/GitScm.cs#L75>`_::

    git init --bare "/local-backup-dir/"

#. Fetch everything `from the remote repo into the local repo <https://github.com/christianspecht/scm-backup/blob/1.5.0/src/ScmBackup/Scm/GitScm.cs#L115>`_ that is not there yet::

    git -C "/local-backup-dir/" fetch --force --prune https://example.com/repo-clone-url/ refs/heads/*:refs/heads/* refs/tags/*:refs/tags/*

Of course, ``/local-backup-dir/`` must be a valid path for your operating system (e.g. ``c:\example\`` for Windows).


Troubleshooting
===============

.. _trouble-gitusage:

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



SCM Backup fails with strange errors
------------------------------------

There were already multiple issues that I *(SCM Backup maintainer)* couldn't reproduce so far: `#33 <https://github.com/christianspecht/scm-backup/issues/33>`_, `#57 <https://github.com/christianspecht/scm-backup/issues/57>`_, `#58 <https://github.com/christianspecht/scm-backup/issues/58>`_, `#66 <https://github.com/christianspecht/scm-backup/issues/66>`_


Things that almost all issues have in common:

- **Always:** the hoster is Bitbucket
- **Often:** Git fails with this error::

    fatal: protocol error: unexpected 'Error running git: fork/exec /usr/bin/git-upload-pack: no such file or directory'

- **Often:** SCM Backup is executed on Windows
- **Sometimes:** people tried to :ref:`ignore <config-ignorerepos>` the repo that caused the error, only to have the same error occur for another repo, and another...

Other things worth mentioning:

- one of the failing repos had `about 500 branches and tags <https://github.com/christianspecht/scm-backup/issues/33#issuecomment-479670392>`_, some of the branches had very long names
- one of the failing repos `was completely empty <https://github.com/christianspecht/scm-backup/issues/66#issuecomment-1098562774>`_
- many different Git versions were used, so apparently it's not a bug in a specific Git version or something like that

**If you have a similar problem, please try these steps:**

#. Enable :ref:`config-logrepofinished` to be 100% sure which repo is the culprit

#. Execute :ref:`the steps SCM Backup does directly in Git <trouble-gitusage>`, with the repo where the problem occured

    .. note:: Until now, executing the commands in Git directly *did* work for all users who had the problem with SCM Backup. But still, it's interesting to know.

#. Try to :ref:`ignore <config-ignorerepos>` the repo.

    This helped for some people...for others, the same problem occured with multiple other repos.

**I don't have a working solution for this problem yet, because I wasn't able to reproduce it myself.**

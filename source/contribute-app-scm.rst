Implementing a new SCM
======================

.. note::

    To see example code, take a look at the existing SCM implementations and their tests:
    
    - `GitScm <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup/Scm/GitScm.cs>`_ / `GitScmTests <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup.Tests.Integration/Scm/GitScmTests.cs>`_
    - `MercurialScm <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup/Scm/MercurialScm.cs>`_ / `MercurialScmTests <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup.Tests.Integration/Scm/MercurialScmTests.cs>`_


Add ScmType
-----------

Add the new SCM to the `ScmType <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup/ScmType.cs>`_ enum.


IScm implementation
-------------------

In the ``ScmBackup`` project, create a new class in the `"Scm" folder <https://github.com/christianspecht/scm-backup/tree/master/src/ScmBackup/Scm>`_. Name it like the SCM you are implementing, e.g. ``GitScm``

The class must implement the interface `IScm <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup/Scm/IScm.cs>`_.

When the respective SCM has a command-line tool *(like most current SCMs do)*, the easiest way to implement the class is by inheriting from the abstract `CommandLineScm <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup/Scm/CommandLineScm.cs>`_ class.

(``CommandLineScm`` handles the plumbing to actually execute the command line tool, including looking for the executable at :ref:`the path specified in the config <config-scms>`)


ScmAttribute
++++++++++++

All SCM implementations need to have an attribute, so SCM Backup is able to properly recognize them.

Apply the `ScmAttribute <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup/Scm/ScmAttribute.cs>`_ to the class and set the ``Type`` parameter to the ``ScmType`` you added in the first step.

Example for Git::

    namespace ScmBackup.Scm
    {
        [Scm(Type = ScmType.Git)]
        internal class GitScm : CommandLineScm, IScm
        {
        
        }
    }

Integration tests
-----------------

In the ``ScmBackup.Tests.Integration`` project, create a new test class in the `Scm <https://github.com/christianspecht/scm-backup/tree/master/src/ScmBackup.Tests.Integration/Scm>`_ folder which inherits from ``IScmTests``. Name it accordingly, e.g. ``GitScmTests``.

``IScmTests`` contains all the tests and a few abstract properties for repo URLs, commit IDs etc.

The child classes just need to set these, so the same tests are executed for all ``IScm`` implementations.

Please see also :doc:`contribute-app-tests`.


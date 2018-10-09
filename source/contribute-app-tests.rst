How to run the integration tests
================================

For each supported hoster, SCM Backup needs to:

- make API calls to get a list of repositories
- use Git/Mercurial etc. to clone repositories

So there are integration tests for each hoster which do these things as well, some of them with authentication.

We created test users especially for these tests (for example: `user <https://github.com/scm-backup-testuser/>`_ and `organization <https://github.com/scm-backup-testorg>`_ used for GitHub integration tests ), but of course we can't publish their passwords.

So in order to run any of these integration tests, you need to setup your *own* test users and test repositories.

SCM Backup's integration tests read the users, password etc. from a file named ``environment-variables.ps1`` in the main project directory, which is not in the repository.

You need to create your own by copying/renaming `environment-variables.ps1.sample <https://github.com/christianspecht/scm-backup/blob/master/environment-variables.ps1.sample>`_, and changing the values.


xUnit.Net Cheat Sheet
---------------------

Running a single test
+++++++++++++++++++++

Edit ``run-all-tests.ps1``, look for the **INTEGRATION TESTS** section and add the ``--filter`` parameter to the ``dotnet test`` call, like this::

    --filter "FullyQualifiedName=ScmBackup.Tests.Integration.Hosters.BitbucketBackupMercurialTests.MakesBackup"

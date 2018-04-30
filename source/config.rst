Configuration
=============

SCM Backup is configured in `YAML <https://en.wikipedia.org/wiki/YAML>`_, by editing the config file ``settings.yml``.

General Options
---------------

.. _config-folder:

localFolder
+++++++++++

The folder (on the machine where SCM Backup runs) where all the backups will be stored.

The folder must already exist, SCM Backup won't create it.

Example::

    localFolder: 'c:\scm-backup'

    
waitSecondsOnError
++++++++++++++++++

When an error occurs, SCM Backup will wait that many seconds before exiting the application.

Example::
    
    waitSecondsOnError: 5


.. _config-scms:

scms
++++

SCM Backup uses the source control software already installed on your system. By default, it assumes that the required SCMs are installed in your path.

If this isn't the case, or if you have multiple versions of the same SCM on your system and want SCM Backup to use a specific one, you can specify the complete path to the executable in the config file.

Example::

    scms:
      - name: git
        path: 'c:\git\git.exe'

    
.. _config-sources:

Sources
-------

SCM Backup will be able to backup from multiple source code hosters, and multiple accounts per hoster.

For example, your GitHub user may be a member of an `organization <https://help.github.com/articles/about-organizations/>`_, and you may want to backup all repositories of your user, **and** all repositories of that organization.

In SCM Backup terms, these would be two different **sources**: your GitHub user would be one source, and the organization would be a second one.

You can define as many sources as you want in the config file, in this format::

    sources:

      - title: some_title
        hoster: github
        type: user
        name: your_user_name

      - title: another_title
        hoster: github
        type: org
        name: your_org_name

Each source must have at least those four properties:
        
``title``

    Must be unique in the whole config file.

    For each source, SCM Backup will create a sub-folder named like the source's title in the :ref:`main backup folder <config-folder>`.
    
``hoster``

    The source code hoster from which you want to backup. See the sub-pages for valid values for each hoster.

``type``

    Either ``user`` or ``org``, depending if you want to backup an user or a organization.

``name``

    The name of the user/organization you want to backup.


There are more possible options (for authentication, for example), but these can vary depending on the source code hoster.

See the respective sub-page for detailed documentation per hoster:

.. toctree::
   :maxdepth: 2
   
   config-github
   config-bitbucket


ignoreRepos
+++++++++++

Optional: For each source, you can specify a list of repositories you do **not** want to be backed up.

Example::

    sources:

      - title: some_title
        hoster: github
        type: user
        name: your_user_name
        ignoreRepos:
            - repo1
            - Some-Other-Repo

.. note::

    - The repository names are case-sensitive!
    - For hosters where the repositories are "sub-items" of the users (like GitHub), you just need to specify the repository name, not the user name (i.e. ``repo`` instead of ``user/repo``).

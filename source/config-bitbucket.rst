Bitbucket
=========

Configuration settings for backing up repositories from Bitbucket.

.. warning::

    **Known limitations:**
    
    - Issues are not backed up
    
    - SCM Backup supports Bitbucket Cloud (see `Hosting Options <https://bitbucket.org/product/guides/getting-started/overview#bitbucket-software-hosting-options>`_) only, the self-hosted options are **not** supported!
       ⇒ see :ref:`why-no-local-backup` 


Sources
-------

For the basics, please read the :ref:`config-sources` section first.

For Bitbucket, the ``hoster`` entry in the config file needs to look like this::

    hoster: bitbucket


Authentication
--------------

Without authentication, SCM Backup can only backup your public repositories.

In this case, it shows a warning:

.. image:: config-auth-warning.png

To backup your private repositories as well, you need to authenticate:

- To backup a user's repositories, you need to authenticate with that user.
- To backup a team's repositories, you need to authenticate with a user who has sufficient permissions to that team's repositories.

Create an `app password <https://confluence.atlassian.com/bitbucket/app-passwords-828781300.html>`_ for SCM Backup for that user:

#. In the user's settings on Bitbucket, go to the **App passwords** area (``https://bitbucket.org/account/user/YOUR-USERNAME/app-passwords``) and create a new app password. Give it at least the following permissions:
    
    .. image:: config-bitbucket-pw-permissions.png
    
    - Account: Read
    - Repositories: Read
    - Issues: Read
    - Wikis: Read and write *(SCM Backup only needs to read, but there's no separate "just read" permission)*
    
#. Put the username and the app password into the ``authName`` and ``password`` properties of the source in the config file.

    Example::
        
        sources:

          - title: some_title
            hoster: bitbucket
            type: org
            name: your_team_name
            authName: your_user_name
            password: your_app_password
            
    This will backup the repositories of the team ``your_team_name``, but authenticate with the user ``your_user_name`` and the app password.

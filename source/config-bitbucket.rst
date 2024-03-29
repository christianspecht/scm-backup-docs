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

SCM Backup will always backup a Bitbucket `Workspace <https://support.atlassian.com/bitbucket-cloud/docs/what-is-a-workspace/>`_.

| Bitbucket repository URLs look like this: ``https://bitbucket.org/WORKSPACEID/REPO/``...so the URL part directly behind ``https://bitbucket.org`` is the workspace ID.
| *(The distinction between "users" and "organizations" - which most other hosters have, and which Bitbucket also had before they introduced workspaces - doesn't really exist anymore)*

- To backup the repos of any workspace, the ``name`` entry in the config needs to be set to the workspace ID
- If the workspace is the user's "personal" workspace *(workspace name is equal to username)*, set the ``type`` in the config to ``user``. For any other workspace, set the ``type`` to ``org``. [#type]_




Authentication
--------------

Without authentication, SCM Backup can only backup your public repositories.

In this case, it shows a warning:

.. image:: config-auth-warning.png

To backup your private repositories as well, you need to authenticate with a user who has sufficient permissions to the workspace's repositories.

Create an `app password <https://confluence.atlassian.com/bitbucket/app-passwords-828781300.html>`_ for SCM Backup for that user:

#. In the user's settings on Bitbucket, go to the **App passwords** area (``https://bitbucket.org/account/user/YOUR-USERNAME/app-passwords``) and create a new app password. Give it at least the following permissions:
    
    .. image:: config-bitbucket-pw-permissions.png
    
    - Account: Read
    - Repositories: Read
    - Issues: Read
    - Wikis: Read and write *(SCM Backup only needs to read, but there's no separate "just read" permission)*
    
#. Put the username and the app password into the ``authName`` and ``password`` properties of the source in the config file.

    .. note::
    
        By default, a user's workspace ID is equal to the user name, but it's possible to `change the workspace ID <https://confluence.atlassian.com/bitbucket/change-a-workspace-id-966686598.html>`_, so workspace ID and user name don't match anymore.
        
        In this case, you need to set ``name`` to the workspace ID and ``authName`` to the user name.
        
    Example::
        
        sources:

          - title: some_title
            hoster: bitbucket
            type: org
            name: your_workspace
            authName: your_user_name
            password: your_app_password
            
    This will backup the repositories of the workspace ``your_workspace``, but authenticate with the user ``your_user_name`` and the app password.
    

.. rubric:: Footnotes

.. [#type] For what SCM Backup does, there's no difference between users and workspaces. The actual backup will work with both ``types``.
   
   However, not all validations are done for both types, and if you set the wrong ``type``, you may see misleading warning messages (`example <https://github.com/christianspecht/scm-backup/issues/68>`_).

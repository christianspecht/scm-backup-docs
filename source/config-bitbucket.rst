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

.. warning:: 

    In the past, Bitbucket used app passwords for this. App passwords are now replaced by API tokens and all existing app passwords will be disabled June 9, 2026 (`1 <https://www.atlassian.com/blog/bitbucket/bitbucket-cloud-transitions-to-api-tokens-enhancing-security-with-app-password-deprecation>`__, `2 <https://www.atlassian.com/blog/bitbucket/bitbucket-cloud-enters-phase-2-of-app-password-deprecation>`__).
    
    If you're currently using app passwords to authenticate SCM Backup, you must replace them by API tokens before June 9, 2026.


Create an `API token <https://support.atlassian.com/bitbucket-cloud/docs/api-tokens/>`_ for SCM Backup for that user:

#. Login with your Atlassian ID, go to the `API tokens page <https://id.atlassian.com/manage-profile/security/api-tokens>`__ and `follow these steps <https://support.atlassian.com/bitbucket-cloud/docs/create-an-api-token/>`__ to create a new API token with scopes.

    (important: there are two options to create a token either with or without scopes, and you need one **with** scopes!)


#.  Select Bitbucket as the app, and give the token at least the following permissions:

    - ``read:repository:bitbucket``
    - ``read:wiki:bitbucket``
    
    
#. Put your Bitbucket email address and the API token into the ``authName`` and ``password`` properties of the source in the config file.


    .. warning::

        **TODO: this won't work, because after creating the token, Bitbucket shows this:**

                To authenticate with Bitbucket Cloud using an API token:

                You will need the API token and your Bitbucket email address for Bitbucket APIs.
                You will need the API token and your Bitbucket user name for Git commands.


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

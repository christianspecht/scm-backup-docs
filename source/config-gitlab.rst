GitLab
======

Configuration settings for backing up repositories from GitLab.

.. warning::

    **Known limitations:**
    
    - Issues are not backed up
    - SCM Backup supports GitLab.com (the SaaS option, see `Pricing <https://about.gitlab.com/pricing/>`_) only, the self-managed options are **not** supported!
       ⇒ see :ref:`why-no-local-backup` 
    - if you backup private GitLab repos, you need to create a new access token once a year (because they expire after 365 days, see :ref:`gl-auth` below)


Sources
-------

For the basics, please read the :ref:`config-sources` section first.

For GitLab, the ``hoster`` entry in the config file needs to look like this::

    hoster: gitlab


.. _gl-auth:

Authentication
--------------

Without authentication, SCM Backup can only backup your public repositories.

In this case, it shows a warning:

.. image:: config-auth-warning.png

To backup your private repositories as well, you need to authenticate:

- To backup a user's repositories, you need to authenticate with that user.
- To backup a group's repositories, you need to authenticate with a user who has sufficient permissions to that group's repositories.

Create a `personal access token <https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html#create-a-personal-access-token>`_ for SCM Backup for that user:

#. Ensure that you're logged into GitLab with the correct user, then `click this link to go to the "Personal Access Tokens" page <https://gitlab.com/-/user_settings/personal_access_tokens?name=SCM+Backup&scopes=read_api,read_repository>`__.

    Click on "Add new token" ⇒ the token's name and the correct scopes should be pre-filled.
    
    .. warning::
    
        | Note that **the token has an expiration date, and the maximum you can set is 365 days.**
        | (this was `changed in GitLab 16.0 <https://gitlab.com/gitlab-org/gitlab/-/issues/392855>`__, previously it was possible to have tokens that never expire)
        
        The default value is much less than that *(at the time of writing this, it was a month)*, so be sure to change the default value.

    
#. Put the username and the token into the ``authName`` and ``password`` properties of the source in the config file.

    Example::
        
        sources:

          - title: some_title
            hoster: gitlab
            type: org
            name: your_group_name
            authName: your_user_name
            password: your_token
            
    This will backup the repositories of the group ``your_group_name``, but authenticate with the user ``your_user_name``.


Wikis and rate limits
---------------------

Unfortunately, the "main" API call doesn't return whether a project has a wiki.

To determine if there's a wiki which needs to be backed up, SCM Backup has to do the following **for each project that the API call returns**:

#. check if the "wiki" feature is activated for the current project (Repo settings ⇒ Features, default value for new repos: yes)
#. if yes, make a separate call to the `Wikis API <https://docs.gitlab.com/ee/api/wikis.html#list-wiki-pages>`_ to check if this wiki has at least one page

.. note:: This means one additional API call per project with enabled wiki...even if the wiki doesn't have a single page.

When you have lots of repos, this has two effects concerning `GitLab's rate limits <https://docs.gitlab.com/ee/user/gitlab_com/index.html#gitlabcom-specific-rate-limits>`_:

- SCM Backup pauses after each wiki API call to avoid hitting the limit of 10 requests per second per IP address

    ⇒ the more repos you have, the more time will the whole API calling take

- There's another limit of 600 API calls per minute altogether.

    ⇒ you may hit that limit when you have hundreds of repos and a fast Internet connection.

Both issues can be avoided by disabling the wiki feature in all projects that don't actually use the wiki.

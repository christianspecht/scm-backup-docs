Info for Bitbucket Backup users
===============================

`Bitbucket Backup <https://christianspecht.de/bitbucket-backup/>`_ is a previous application written by the author of SCM Backup. It's similar to SCM Backup, but limited to Bitbucket and one user or team only.

**Here is some useful information for Bitbucket Backup users switching to SCM Backup:**



Setup
-----

Bitbucket Backup is written in .NET 4, SCM Backup is written in .NET Core; see the different :ref:`install-requirements`.

Plus, there's no MSI setup anymore, just a zip file with the binaries.



Configuration
-------------

When you run Bitbucket Backup for the first time, it asks for all configuration values and stores them in the user's settings.

SCM Backup is able to backup multiple accounts from multiple hosters, so asking for all the config values at runtime isn't practical anymore.

Instead, you save them all into a :doc:`configuration file <config>`.

A minimal working configuration file to backup your Bitbucket user would look like this::

    localFolder: 'c:\your-backup-folder' # your backups are stored here

    sources:

      - title: some_title  # must be unique in the whole config file, will be used as subfolder name
        hoster: bitbucket
        type: user
        name: your_user_name
        authName: your_user_name
        password: your_app_password

...or like this for a team::

    localFolder: 'c:\your-backup-folder'

    sources:

      - title: some_other_title
        hoster: bitbucket
        type: org
        name: your_team_name
        authName: your_user_name
        password: your_app_password


...or like this to backup both the user **and** the team *(which Bitbucket Backup can't do)*::

    localFolder: 'c:\your-backup-folder'

    sources:
    
      - title: some_title
        hoster: bitbucket
        type: user
        name: your_user_name
        authName: your_user_name
        password: your_app_password

      - title: some_other_title
        hoster: bitbucket
        type: org
        name: your_team_name
        authName: your_user_name
        password: your_app_password

Read more about possible settings for :ref:`sources <config-sources>` and :doc:`Bitbucket <config-bitbucket>`.


Emailing output
---------------

Like Bitbucket Backup, SCM Backup is able to send an email with log information, but the configuration is different. See :doc:`how it's done in SCM Backup <output-email>`.

Bitbucket Backup takes advantage of ``SmtpClient``'s ability to `read configuration file settings <https://docs.microsoft.com/en-us/dotnet/api/system.net.mail.smtpclient.-ctor?view=netframework-4.7.2#System_Net_Mail_SmtpClient__ctor>`_ by itself.

So `all possible options <https://docs.microsoft.com/de-de/dotnet/framework/configure-apps/file-schema/network/mailsettings-element-network-settings>`_ for ``<mailSettings>`` were available, and Bitbucket Backup didn't need to bother to support or even know about them all, because ``SmtpClient`` directly read them from the app's config file.

Apparently `this is not possible in .NET Core <https://github.com/dotnet/corefx/issues/12537>`_ and maybe ``SmtpClient`` `is kind of deprecated anyway <https://github.com/dotnet/docs/issues/1876>`_, so SCM Backup is using `MailKit <https://github.com/jstedfast/MailKit>`_ instead, which `doesn't read values from the config and never will <https://github.com/jstedfast/MailKit/issues/630#issuecomment-357670414>`_.

So SCM Backup has to know about every possible config value, and time will tell whether `those available now <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup/Configuration/ConfigEmail.cs>`_ will work for everyone.


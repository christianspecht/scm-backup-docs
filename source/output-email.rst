Emailing output
===============

The same information which is logged to the console and to the log files, can be sent via email as well.

You need to provide the SMTP settings, as well as the ``From`` and ``To`` email adresses, in the :ref:`email section in the config file <config-email>`.

.. note::
    To specify multiple recipients, separate them with a semicolon::
        
        email:
          from: sender@example.com
          to: foo@example.com;bar@example.com
          [...]

If this is set, SCM Backup will send a mail to the specified adress after each finished backup.

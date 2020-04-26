Logging
=======

SCM Backup uses `NLog <http://nlog-project.org/>`_ for logging.

All console outputs are also generated via logging (with a `CompositeLogger <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup/Loggers/CompositeLogger.cs>`_ which logs `to the console <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup/Loggers/ConsoleLogger.cs>`_ **and** `to NLog <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup/Loggers/NLogLogger.cs>`_).

So all console outputs are in the log files as well.


Log levels
----------

To keep it simple, SCM Backup only has `four log levels <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup/ErrorLevel.cs>`_.

The `ConsoleLogger <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup/Loggers/ConsoleLogger.cs>`_ outputs all levels except ``Debug``.

The `NLogLogger <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup/Loggers/NLogLogger.cs>`_ maps SCM Backup's log levels to a subset of NLog's log levels.

NLog is configured via NLog's regular `NLog.config <https://github.com/christianspecht/scm-backup/blob/master/src/ScmBackup/NLog.config>`_ file, so all possible `NLog configuration settings <https://github.com/nlog/NLog/wiki/Configuration-file>`_ apply.

For example, you can change the minimal log level to ``Debug`` (default: ``Info``), to log additional information::

    <rules>
      <logger name="*" minlevel="Debug" writeTo="f" />    
    </rules>


Log files
---------

The log files are in a subfolder named ``logs`` in SCM Backup's application folder.

On each application start, a new log file (``scm-backup.log``) is generated.

Old files are available in the ``archive`` subfolder.


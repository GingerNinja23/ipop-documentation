If you run into errors/crashes, please proceed as follows:

1. Try to recreate the problem and obtain log files:

    Providing a log file is very useful. The logging level is determined in the JSON configuration file by adding the following parameters:

    ```bash
    "controller_logging" : "DEBUG",
    "tincan_logging": 2
    ```
    * **logging_level** - sets the Python logging module for the controller with possible values
      *DEBUG*, *INFO*, *WARNING*, *ERROR*, *CRITICAL*
    * **logging** - set the logging level for IPOP-Tincan, _0_ = no logging, _1_ = error logging,
      _2_ = info logging (recommended), _3_ = verbose logging   

1. Report the issue

    Use Github's issue tracker: https://github.com/ipop-project/ipop-tincan/issues

    If you don't have a Github account, you may also report the issue on our mailing list: https://lists.ufl.edu/cgi-bin/wa?SUBED1=IPOP-USERS-L&A=1

1. Provide us with the log files, if available

    Don't post the log files with your issue - the logs will generally be too big for this. Instead, if you can make the log files available through a public URL from which we can download, this is the most convenient for us. If you cannot post the logs on a public server for download, let us know and we will provide an email address for sending your log files.
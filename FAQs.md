1.  How do I check the IP address of peers in my SocialVPN?

    Currently, this can be done by issuing a get_state [[controller API|Controller API]] call to the running tincan process. In Linux this can be done with the command line:

    ```bash
    echo '{"m":"get_state"}' | netcat -u 127.0.0.1 5800
    ```

    A feature for future releases is to provide a Web-based interface to browse the status of peers.

2.  How do I enable logging for debugging?

    Generally you don't need to enable logging unless you are a developer, but if you run into errors/crashes, providing a log file is very useful. The logging level is determined in the JSON configuration file by adding the following parameters:

    ```bash
    "logging_level" : "logging.DEBUG",
    "logging": 1
    ```

    * **logging_level** - sets the Python logging module for the controller with possible values
      *logging.DEBUG*, *logging.INFO*, *logging.WARNING*, *logging.ERROR*, *logging.CRITICAL*
    * **logging** - set the logging level for IPOP-Tincan, 0 = no logging, 1 = error logging,
      2 = info logging (recommended), 3 = verbose logging

3.  How do you configure IPOP to restart on its own if there is a failure?

    Currently, this is supported for the GroupVPN controller in Linux with the use of a watchdog. Check the documentation on [[running GroupVPN on Linux|Running GroupVPN on Linux]] for details.

4.  What XMPP services are known to work with IPOP?

    It is know to work with Google XMPP service (with exceptions; see [[known issues|Known issues]]), and with ejabberd-based servers (e.g. www.jabber.org).
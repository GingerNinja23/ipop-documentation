It is common practice to use a watchdog process to monitor and respawn
look running processes (especially those written in unmanaged languages
such is C/C++). We therefore designed a simple watchdog process that
spawns both ipop-tincan and the controller.

Our watchdog process automatically starts the ipop-tincan, so that you 
don't have to run it separately. (the path of the binary should be specified
in configuration file). If the ipop-tincan crashes or is not responding, 
the watchdog process terminates the ipop-tincan process and starts it as a 
new process.

Go to [[this page|Running SocialVPN or GroupVPN on Linux]] and follow the
first section:
* Download and configure SocialVPN

## Run watchdog process

```
sudo ./watchdog.py -c config.json
```

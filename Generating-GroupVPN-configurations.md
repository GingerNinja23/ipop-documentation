You can use [`groupvpn-config`](https://github.com/ipop-project/groupvpn-config) to generate configuration files and (optionally) create the required users and relationships in ejabberd.

***

Start by viewing the help message:

    $ groupvpn-config --help

A very basic invocation, which just prints `ejabberdctl` commands and
configuration data in a readable format, is:

    $ groupvpn-config testgroup localhost 5

Passwords are randomly generated, so if you need to generate the same
passwords on multiple runs of the tool, you can pass a string to be used
as a random seed using the `--seed` option:

    $ groupvpn-config testgroup localhost 5 --seed asdfghjkl

If you want configuration files in a zip file:

    $ groupvpn-config testgroup localhost 5 --zip >configs.zip

By default, `ejabberdctl` commands are printed but not run. Use the
`--configure` flag to actually run them:

    $ groupvpn-config testgroup localhost 5 --configure --zip >configs.zip

Or maybe you want to save the commands to a file to run later (the
commands are printed to `stderr`):

    $ groupvpn-config testgroup localhost 5 --zip >configs.zip 2>commands.sh

If you want to see only `ejabberdctl` commands or only configuration
data printed, you can redirect one of the output streams to /dev/null:

    $ groupvpn-config testgroup localhost 5 2>/dev/null
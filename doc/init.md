Sample init scripts and service configuration for elleriumd
=======================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/elleriumd.service:    systemd service unit configuration
    contrib/init/elleriumd.openrc:     OpenRC compatible SysV style init script
    contrib/init/elleriumd.openrcconf: OpenRC conf.d file
    contrib/init/elleriumd.conf:       Upstart service configuration file
    contrib/init/elleriumd.init:       CentOS compatible SysV style init script

1. Service User
---------------

All three startup configurations assume the existence of a "ellerium" user
and group.  They must be created before attempting to use these scripts.

2. Configuration
----------------

At a bare minimum, elleriumd requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, elleriumd will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that elleriumd and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If elleriumd is run with "-daemon" flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'

Once you have a password in hand, set rpcpassword= in /etc/ellerium/ellerium.conf

For an example configuration file that describes the configuration settings,
see contrib/debian/examples/ellerium.conf.

3. Paths
--------

All three configurations assume several paths that might need to be adjusted.

Binary:              /usr/bin/elleriumd
Configuration file:  /etc/ellerium/ellerium.conf
Data directory:      /var/lib/elleriumd
PID file:            /var/run/elleriumd/elleriumd.pid (OpenRC and Upstart)
                     /var/lib/elleriumd/elleriumd.pid (systemd)

The configuration file, PID directory (if applicable) and data directory
should all be owned by the ellerium user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
ellerium user and group.  Access to ellerium-cli and other elleriumd rpc clients
can then be controlled by group membership.

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists on just copying it to
/usr/lib/systemd/system directory, followed by the command
"systemctl daemon-reload" in order to update running systemd configuration.

To test, run "systemctl start elleriumd" and to enable for system startup run
"systemctl enable elleriumd"

4b) OpenRC

Rename elleriumd.openrc to elleriumd and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
"/etc/init.d/elleriumd start" and configure it to run on startup with
"rc-update add elleriumd"

4c) Upstart (for Debian/Ubuntu based distributions)

Drop elleriumd.conf in /etc/init.  Test by running "service elleriumd start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

4d) CentOS

Copy elleriumd.init to /etc/init.d/elleriumd. Test by running "service elleriumd start".

Using this script, you can adjust the path and flags to the elleriumd program by
setting the ELPD and FLAGS environment variables in the file
/etc/sysconfig/elleriumd. You can also use the DAEMONOPTS environment variable here.

5. Auto-respawn
---------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
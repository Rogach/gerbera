# Load Gerbera as a System Daemon

This readme outlines how to add the Gerbera runtime
as a system daemon using the `systemd`.

The gerbera installation uses cmake to configure and install
the systemd daemon for gerbera

Default install path is `/etc/systemd/system/gerbera.service`


### Create a Gerbera System User

You should run Gerbera as a separate user to avoid vulnerabilities in
exposing root access.

Here is a way to create a system user in linux command line

```
$ sudo useradd --system gerbera
```

Verify that the `gerbera` user was created

```
$ id -u gerbera
```
> Returns the user id of the user


### Create a Gerbera Configuration File

Create a valid Gerbera `config.xml` in the destination `/etc/gerbera/config.xml`
This file must be accessible to the gerbera user.

```
$ sudo mkdir /etc/gerbera
$ sudo touch /etc/gerbera/config.xml
$ sudo chown -Rv gerbera:gerbera /etc/gerbera
```

### Enable Systemd Daemon

The cmake installation adds *gerbera.service* to `/etc/systemd/system` by default.  

> The installation does not *enable* the service daemon.

1. Notify `systemd` that a new gerbera.service file exists by executing the following command:

     ```
     $ sudo systemctl daemon-reload
     ```
2. Start up the daemon

    ```
    $ sudo systemctl start gerbera
    ```

### Success

Check the status of gerbera.  You should see success similar to below

```
$ sudo systemctl status gerbera

● gerbera.service - Gerbera Media Server
   Loaded: loaded (/etc/systemd/system/gerbera.service; disabled; vendor preset: disabled)
   Active: active (running) since Wed 2017-09-20 19:48:44 EDT; 47s ago
 Main PID: 4818 (gerbera)
    Tasks: 12 (limit: 4915)
   CGroup: /system.slice/gerbera.service
           └─4818 /usr/local/bin/gerbera -c /etc/gerbera/config.xml
```


### Troubleshooting

If for some reason the service fails to start.  You can troubleshoot the behavior
by starting gerbera from the shell

```
$ su gerbera
Password:
bash-4.4$  /usr/local/bin/gerbera -c /etc/gerbera/config.xml

2017-09-20 19:54:47    INFO: Gerbera UPnP Server version 1.1.0_alpha - http://gerbera.io/
2017-09-20 19:54:47    INFO: ===============================================================================
2017-09-20 19:54:47    INFO: Gerbera is free software, covered by the GNU General Public License version 2
2017-09-20 19:54:47    INFO: Copyright 2016-2017 Gerbera Contributors.
2017-09-20 19:54:47    INFO: Gerbera is based on MediaTomb: Copyright 2005-2010 Gena Batsyan, Sergey Bostandzhyan, Leonhard Wimmer.
2017-09-20 19:54:47    INFO: ===============================================================================
2017-09-20 19:54:47    INFO: Loading configuration from: /etc/gerbera/config.xml
2017-09-20 19:54:47    INFO: Checking configuration...

```

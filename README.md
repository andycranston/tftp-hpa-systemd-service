# tftp-hpa-systemd-service

A systemd service template for H. Peter Anvin's TFTP server daemon.

## Introduction

The `tftpd-hpa.service` file is a systemd service template for H. Peter Anvin's TFTP server daemon.

The source for H. Peter Anvin's TFTP server daemon can be found here:

https://mirrors.edge.kernel.org/pub/software/network/tftp/tftp-hpa/

The version I used is `tftp-hpa-5.2` but other versions may also work.

Edit the file `tftpd-hpa.service` and change the line which is similar to:

```
ExecStart=/usr/local/bin/tftpd --foreground --secure --user nobody --permissive --create --umask 022 /var/tftpboot
```

Edits you may need to make:

+ The path and name of the tftpd executable
+ The username for the --user option
+ The directory to serve files from (for GETs)  and to (for PUTs)

For example:

```
ExecStart=/home/andyc/bin/tftpd --foreground --secure --user andyc --permissive --create --umask 022 /home/andyc/tftpboot
```

Copy `tftpd-hpa.service` to the directory:

```
/etc/systemd/system
```

Make sure the mode, owner and group of:

```
/etc/systemd/system/tftpd-hpa.service
```

is:

```
-rwxr-xr-x   root   root
```

Reload the systemd daemons:

```
sudo systemctl dameon-reload
```

Enable the `tftpd-hpa` service:

```
sudo systemctl enable tftpd-hpa.service
```

Start the `tftpd-hpa` service:

```
sudo systemctl start tftpd-hpa.service
```

From another host use the `tftp` command to transfer files to and from the system.

## Firewall

You may need to add a rule to your firewall. UDP port 69 must be opened on the host running the `tftpd`
server dameon.

## Notes on compiling tftpd

To get the tftpd excecutable uncompress and untar the source distrubution and change into the
directory that gets created when untarring.

Run configure as follows:

```
./configure --prefix=/var/tmp/foo
```

Then run make:

```
make
```

At this point DO NOT run `make install` - it is not necessary.

The `tftpd` executable is located here:

```
tftpd/tftpd
```

Also the manual page can be generated/viewed with:

```
nroff -man tftpd/tftpd.8 | more -s
```

--------------------------------------------------------

End of README.md

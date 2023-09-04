# Backup Tools
Backup Tools is a system providing
encrypted backups using [restic](https://restic.net/).

## Install restic
Install `restic` on your system. Instructions
for select systems are provided below. For 
other systems follow the instructions of the 
official documentation linked 
[here](https://restic.readthedocs.io/en/stable/020_installation.html).

### Arch Linux
```sh
pacman -S restic
```

### Debian
```sh
apt-get install restic
```

### Fedora
```sh
dnf install restic
```

## Create a proxy script
Create a file called `/usr/local/bin/restic-offsite` and 
put the following into it:
```sh
#!/bin/sh

export RESTIC_REPOSITORY="<repository>"
export RESTIC_PASSWORD="<password>"

exec restic "$@"
```
Change `<repository>` and `<password>` accordingly.

Change the permissions of the file:
```sh
chmod 700 /usr/local/bin/restic-offsite
```

Change the ownership of the file:
```sh
chown root /usr/local/bin/restic-offsite
```

## Create a backup service
Put the following into `/etc/systemd/user/restic-backup.service`:
```
[Unit]
Description=Restic Backup Service

[Service]
Type=oneshot
ExecStart=restic-offsite backup "/home/<user>" --exclude "/home/<user>/"'/.*' --verbose
ExecStartPost=restic-offsite forget --keep-within-daily 7d --keep-within-weekly 1m --keep-within-monthly 1y --keep-within-yearly 10y --verbose
```
Change `<user>` accordingly.

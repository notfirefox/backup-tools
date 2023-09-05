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
Now we want to create a systemd system service
to automate the backup process. Documentation
on writing unit files can be found
[here](https://wiki.archlinux.org/title/Systemd#Writing_unit_files).

### Service
Put the following into `/etc/systemd/user/restic-backup.service`:
```
[Unit]
Description=Restic Backup Service

[Service]
Type=oneshot
ExecStart=restic-offsite backup "/home/<user>" --exclude '/home/<user>/.*' --verbose
ExecStartPost=restic-offsite forget --keep-within-daily 7d --keep-within-weekly 1m --keep-within-monthly 1y --keep-within-yearly 10y --verbose
```
Change `<user>` accordingly.

### Timer
Put the following into `/etc/systemd/system/restic-backup.timer`:
```
[Unit]
Description=Daily Restic Backups

[Timer]
OnCalendar=daily
Persistent=true

[Install]
WantedBy=timers.target
```

## Create a prune service
In order to clean up ununsed data in the restic repository
we need a separate system service that runs less regularly.

### Service
Put the following into `/etc/systemd/user/restic-prune.service`:
```
[Unit]
Description=Restic Prune service
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
ExecStart=restic-offsite prune
```

### Timer
Put the following into `/etc/systemd/system/restic-prune.timer`:
```
[Unit]
Description=Weekly Restic Cleanup
After=network-online.target
Wants=network-online.target

[Timer]
OnCalendar=weekly
Persistent=true

[Install]
WantedBy=timers.target
```

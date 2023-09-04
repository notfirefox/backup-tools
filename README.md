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
Change the repository and the password accordingly.

Change the permissions of the file:
```sh
chmod 700 /usr/local/bin/restic-offsite
```

Change the ownership of the file:
```sh
chown root /usr/local/bin/restic-offsite
```

## Create a backup service
Download the systemd unit file:
```sh
curl "https://raw.githubusercontent.com/notfirefox/backup-tools/rewrite/etc/systemd/user/restic-backup.service" -o /etc/systemd/user/restic-backup.service
```

Download the systemd timer file:
```sh
curl "https://github.com/notfirefox/backup-tools/raw/rewrite/etc/systemd/user/restic-backup.timer" -o /etc/systemd/user/restic-backup.timer
```

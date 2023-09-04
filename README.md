# Backup Tools

## Install restic
Install `restic` on your system. Instructions
for Arch Linux, Debian and Fedora are provided
below. For other systems follow the official 
[documentation](https://restic.readthedocs.io/en/stable/020_installation.html).

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

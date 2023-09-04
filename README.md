# Backup Tools

## Install restic

## Arch Linux
```sh
sudo pacman -S restic
```

### Fedora
```sh
sudo dnf install restic
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
sudo chmod 700 /usr/local/bin/restic-offsite
```

Change the ownership of the file:
```sh
sudo chown root /usr/local/bin/restic-offsite
```

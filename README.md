# Offsite Backup
Offsite Backup is a system providing
encrypted backups using [restic](https://restic.net/).

## Install restic
Instructions on installing restic can be found
[here](https://restic.readthedocs.io/en/stable/020_installation.html).
The command for Fedora is provided below.
```sh
dnf install restic
```

## Create restic user
Create a dedicated restic user.
```sh
useradd --system --create-home --shell /sbin/nologin restic
```

## Create a proxy script
Create a file called `/usr/local/bin/restic-offsite` and 
put the following into it:
```sh
#!/bin/sh

export RESTIC_REPOSITORY="<repository>"
export RESTIC_PASSWORD="<password>"

until host 'example.com'
do
    sleep 1
done

exec restic "$@"
```
Change `<repository>` and `<password>` accordingly.
If your offsite backup depends on a specific host
change `example.com` as well to that specific host.

Change the permissions of the file:
```sh
chmod 750 /usr/local/bin/restic-offsite
```

Change the ownership of the file:
```sh
chown root:restic /usr/local/bin/restic-offsite
```

## Create a backup service
Now we want to create a systemd system service
to automate the backup process. Documentation
on writing unit files can be found
[here](https://wiki.archlinux.org/title/Systemd#Writing_unit_files).

### Service
Put the following into `/etc/systemd/system/restic-backup.service`:
```
[Unit]
Description=Restic Backup Service
After=network-online.target nss-lookup.target
Wants=network-online.target

[Service]
Type=oneshot
User=restic
ExecStart=/usr/local/bin/restic-offsite backup '/home/<user>' --exclude '/home/*/.*' --verbose
ExecStartPost=/usr/local/bin/restic-offsite forget --keep-within-daily 7d --keep-within-weekly 1m --keep-within-monthly 1y --keep-within-yearly 10y --verbose
AmbientCapabilities=CAP_DAC_READ_SEARCH
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

Enable the timer:
```sh
systemctl enable --now restic-backup.timer
```

## Create a prune service
In order to clean up ununsed data in the restic repository
we need a separate system service that runs less regularly.

### Service
Put the following into `/etc/systemd/system/restic-prune.service`:
```
[Unit]
Description=Restic Prune service
After=network-online.target nss-lookup.target
Wants=network-online.target

[Service]
Type=oneshot
User=restic
ExecStart=/usr/local/bin/restic-offsite prune
```

### Timer
Put the following into `/etc/systemd/system/restic-prune.timer`:
```
[Unit]
Description=Weekly Restic Cleanup

[Timer]
OnCalendar=weekly
Persistent=true

[Install]
WantedBy=timers.target
```

Enable the timer:
```sh
systemctl enable --now restic-prune.timer
```

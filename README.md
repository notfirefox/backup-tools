# Backup Tools
Simple backup tools

## Usage

### Creating Backups
To create a backup run the following command.
```sh
create-backup
```
This will create a file `XXXX-YY-ZZ.tar`, where
- `XXXX` is the current year
- `YY` the current month
- and `ZZ` the current day.
Move this file to a safe location.

### Restoring Backups
To restore a backup run the following command.
```sh
restore-backup XXXX-YY-ZZ.tar
```
This will do a full restore.

# Backup Tools
Simple backup tools

## Usage

### Backing Up
To create a backup run the following command.
```sh
backup
```
This will create a file `XXXX-YY-ZZ.tar`, where
- `XXXX` is the current year
- `YY` the current month
- and `ZZ` the current day.

Move this file to a safe location.

### Restoring
To restore a backup run the following command.
```sh
restore XXXX-YY-ZZ.tar
```
This will do a full restore.

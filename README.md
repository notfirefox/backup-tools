# Backup Tools
Backup Tools consists of two scripts: `backup` and `restore`.
Both are designed to handle backups of the `$HOME` directory.
`backup` will dump everything into a single tar file that can
then be stored somewhere else. `restore` takes an existing
tar file and restores everything into the `$HOME` folder.
Partial restoration is planned.

## Usage

### Backup
To create a backup run the following command.
```sh
backup
```
This will create a file `XXXX-YY-ZZ.tar`, where
- `XXXX` is the current year
- `YY` the current month
- and `ZZ` the current day.

Move this file to a safe location.

### Restore
To restore a backup run the following command.
```sh
restore XXXX-YY-ZZ.tar
```
This will do a full restore.

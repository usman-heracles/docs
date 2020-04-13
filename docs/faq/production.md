Below is a series of questions and answers around production setups when running Moov services. The hosted Moov service will have different answers than self-hosted options.

## Operating System

Moov offers two foramts for running services in production: Docker images or compiled binaries for Linux, macOS, and Windows. The supported versions of each OS are what is supported by Docker or Go and Moov makes no attempts to support older operating systems.

## Database Backups

Backup and restore of database contents is a critical component of production deployments. This is how business operations continue after major system failure.

There are several secure and production-grade backup solutions such as [Restic](https://restic.net/) or [Tarsnap](https://www.tarsnap.com/) if you are going to manage your own backups.

#### SQLite

SQLite is a file-based database and by default Moov services don't require auth to access the file. Instead we rely on machine-level restrictions to limit access to the database file and write-ahead log.

Typically backing up a database file would be a shell command followed by copying/encrypting that file to an external data store:

```
$ sqlite3 paygate.db .backup paygate_backup.sql
```

More Details: https://stackoverflow.com/questions/25675314/how-to-backup-sqlite-database/43398520#43398520

#### MySQL

MySQL is a network-based database which requires username/password or certificate authentication to connect. The backup process for this database involves a `mysqldump` command followed by copying that file to an external data store.

- [MySQL Backup and Recovery](https://dev.mysql.com/doc/refman/8.0/en/backup-and-recovery.html)
- [Web Cheat Sheet: MySQL Backup and Restore](http://webcheatsheet.com/SQL/mysql_backup_restore.php)
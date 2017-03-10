# usp_MS_AdvancedRestore
All-In-One MSSQL Database Restore Script


Example Usages:

Generate scripts to restore MyDB from full, differential, and transaction log backups in G:\backups to 9am October 17, 2016
Leave the database in NORECOVERY
```sql
EXEC [dbo].[usp_MS_AdvancedRestore]
    @RestoreDirectory = 'G:\backups'
    ,@OriginalDatabaseName = 'MyDB'
    ,@RestoreToDatabase = 'MyDB'
    ,@movedatafileTo = 'D:\Data\MyDB'
    ,@movelogfileTo = 'L:\Logs\MyDB'
    ,@backupdate = '2016-10-17'
    ,@StopAt = '09:00'
    ,@Stats = 100
    ,@State = 'NORECOVERY'
    ,@OutputOnly = 1
```


Perform restore of MyDB from full backups in G:\backups and transaction log backups in G:\tlbackups to 9am October 17, 2016
```sql
EXEC [dbo].[usp_MS_AdvancedRestore]
    @RestoreDirectory = 'G:\backups'
    ,@RestoreLogsDirectory = 'G:\tlbackups'
    ,@OriginalDatabaseName = 'MyDB'
    ,@RestoreToDatabase = 'MyDB'
    ,@movedatafileTo = 'D:\Data\MyDB'
    ,@movelogfileTo = 'L:\Logs\MyDB'
    ,@backupdate = '2016-10-17'
    ,@StopAt = '09:00'
    ,@Stats = 100
    ,@OutputOnly = 0
```

Generate scripts to restore MyDB from full and transaction log backups in G:\backups to 9am October 17, 2016. 
Filter on file names to reduce time using defaults.
```sql
EXEC [dbo].[usp_MS_AdvancedRestore]
    @RestoreDirectory = 'G:\backups'
    ,@UseFilesFilter = 1
    ,@OriginalDatabaseName = 'MyDB'
    ,@RestoreToDatabase = 'MyDB'
    ,@movedatafileTo = 'D:\Data\MyDB'
    ,@movelogfileTo = 'L:\Logs\MyDB'
    ,@backupdate = '2016-10-17'
    ,@StopAt = '09:00'
    ,@Stats = 100
    ,@OutputOnly = 1
```


Generate scripts to restore MyDB from full, differential, and transaction log backups in G:\backups to 9am October 17, 2016. 
Filter on file names to reduce time using custom pattern.
Custom patten matching has some shortcuts you can use:

{dbname} - The original database name

{qi} - \[0-9]\[0-9]\[0-9]\[0-9]

{di} - \[0-9]\[0-9]


```sql
EXEC [dbo].[usp_MS_AdvancedRestore]
    @RestoreDirectory = 'G:\backups'
    ,@UseFilesFilter = 1
    ,@FullBackupFilePattern = '{dbname}_{qi}_{di}_{di}'
    ,@FullBackupExt = 'full'
    ,@DiffBackupFilePattern = '{dbname}_{qi}_{di}_{di}_{qi}'
    ,@DiffBackupExt = 'diff'
    ,@LogBackupFilePattern = '{dbname}_{qi}{qi}_{qi}{qi}'
    ,@LogBackupExt = 'tlog'
    ,@OriginalDatabaseName = 'MyDB'
    ,@RestoreToDatabase = 'MyDB'
    ,@movedatafileTo = 'D:\Data\MyDB'
    ,@movelogfileTo = 'L:\Logs\MyDB'
    ,@backupdate = '2016-10-17'
    ,@StopAt = '09:00'
    ,@Stats = 100
    ,@OutputOnly = 1
```


Restore MyDB using the full backup only, even if transaction logs are found, to October 17, 2016.
Filter on file names to reduce time using custom pattern, including today's date.
```sql
EXEC [dbo].[usp_MS_AdvancedRestore]
    @RestoreDirectory = 'G:\backups'
    ,@UseFilesFilter = 1
    ,@FullBackupFilePattern = '{dbname}_20161017'
    ,@LogBackupFilePattern = '{dbname}_20161017_{qi}'
    ,@OriginalDatabaseName = 'MyDB'
    ,@RestoreToDatabase = 'MyDB'
    ,@movedatafileTo = 'D:\Data\MyDB'
    ,@movelogfileTo = 'L:\Logs\MyDB'
    ,@backupdate = '2016-10-17'
    ,@StopAt = '09:00'
    ,@RestoreFullOnly = 1
    ,@Stats = 100
    ,@OutputOnly = 0
```


Restored MyDB from full and transaction log backups to 9am October 17, 2016.
Filter on file names to reduce time using custom pattern including today's date.
Leave in STANDBY to allow reads.
```sql
EXEC [dbo].[usp_MS_AdvancedRestore]
    @RestoreDirectory = 'G:\backups'
    ,@UseFilesFilter = 1
    ,@FullBackupFilePattern = '{dbname}_20161017'
    ,@LogBackupFilePattern = '{dbname}_20161017_{qi}'
    ,@OriginalDatabaseName = 'MyDB'
    ,@RestoreToDatabase = 'MyDB'
    ,@movedatafileTo = 'D:\Data\MyDB'
    ,@movelogfileTo = 'L:\Logs\MyDB'
    ,@backupdate = '2016-10-17'
    ,@StopAt = '09:00'
    ,@State = 'STANDBY'
    ,@StandbyDir = 'S:\Standby Rollback Files'
    ,@Stats = 100
    ,@OutputOnly = 0
```


Restore additional logs for MyDB to roll the STANDBY database to 5pm October 17, 2016.
Filter on file names to reduce time using custom pattern including today's date.
Leave in STANDBY to allow reads.
```sql
EXEC [dbo].[usp_MS_AdvancedRestore]
    @RestoreDirectory = 'G:\backups'
    ,@UseFilesFilter = 1
    ,@FullBackupFilePattern = '{dbname}_20161017'
    ,@LogBackupFilePattern = '{dbname}_20161017_{qi}'
    ,@OriginalDatabaseName = 'MyDB'
    ,@RestoreToDatabase = 'MyDB'
    ,@movedatafileTo = 'D:\Data\MyDB'
    ,@movelogfileTo = 'L:\Logs\MyDB'
    ,@backupdate = '2016-10-17'
    ,@StopAt = '17:00'
    ,@State = 'STANDBY'
    ,@StandbyDir = 'S:\Standby Rollback Files'
    ,@RestoreFromLastLSN = 1
    ,@Stats = 100
    ,@OutputOnly = 0
```

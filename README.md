# How-to-backup-a-folder-every-day-and-keep-3-recent-backup-file
How to backup a folder every day and keep 3 recent backup file



## Step 1: Make a backup.sh with the content below
```
#!/bin/bash

# Set folder to backup
TARGET_BACKUP_DIR="/root/name"
# Where to save backup file
BACKUP_DIR="/root/backup_name"

# Set backup file name
BACKUP_FILE="backup-$(date +%Y%m%d%H%M%S).7z"

# Create backup archive
7z a "${BACKUP_DIR}/${BACKUP_FILE}" "${TARGET_BACKUP_DIR}"

# Remove all the files modified older than 15 mins ago
find /root/backup_name -type f -mmin +15 -delete
```



### NOTE: To Remove all the files modified older than 15 mins ago
```
find /root/backup_name -type f -mmin +15 -delete
```

### NOTE: Remove all the files modified older than 3 days ago
```
find /root/backup_name -type f ! -newermt "$(date -d '10 days ago' +'%Y-%m-%d')" -delete
```

## Step 2: Add it to cron tab
```
crontab -e
```
add in the end of line, it will run 2AM every day
```
0 2 * * * /bin/bash /path/to/backup.sh
```


```
The first field, '0', specifies the minute when the job should run. In this case, '0' means the job will run at the start of the hour (i.e., at 2:00 am).
The second field, '2', specifies the hour when the job should run. In this case, '2' means the job will run at 2:00 am.
The third field, '*', specifies the day of the month when the job should run. In this case, the asterisk means "every day of the month".
The fourth field, '*', specifies the month when the job should run. In this case, the asterisk means "every month of the year".
The fifth field, '*', specifies the day of the week when the job should run. In this case, the asterisk means "every day of the week".
```

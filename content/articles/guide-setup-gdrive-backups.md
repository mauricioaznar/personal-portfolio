---
title: GDrive backups in ubuntu 18
---

## Intro


<br />

### GDrive commmand line installment

<br />

tutorials:
* [gdrive installment in linux 1](https://medium.com/geekculture/how-to-upload-file-to-google-drive-from-linux-command-line-69668fbe4937)
* [gdrive installment in linux 2](https://andypalmer.me/blog/linux/mysql-database-backups-google-drive/)
* [gdrive installment in linux 3](https://allaboutdjango.com/auto-backup-mysql-database-to-google-drive/)
* [crontabs](https://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-linux-or-unix-oses/)

<br />

1. Copy the latest release into folder
```shell
wget https://github.com/prasmussen/gdrive/releases/download/2.1.1/gdrive_2.1.1_linux_386.tar.gz
```

<br />

2. Unzip the Archive
```shell
tar -xvf gdrive_2.1.1_linux_386.tar.gz
```

<br />


3. Perform authentication
```shell
./gdrive about
```

<br />


4. Move utility to /usr/local/bin/gdrive so we can call it globally


```shell
mv gdrive-linux-x64 /usr/local/bin/gdrive
```

<br />



### Write backup .sh script

```shell
#!/bin/sh

# Bail out if there are any errors
set -e

# The name of the database we're going to backup.
DB_NAME="inopack"

# The Google Drive Folder ID where database exports will be uploaded to
GDRIVE_FOLDER_ID="1waC1ESInU6SURm6WvbNDeocJppGhH0hZ"

# Date format for dadtes appended to database export files
DATE_FORMAT="%Y-%m-%d_%H:%M:%S"

# Options to pass to the mysqldump command
MYSQLDUMP_FILE_NAME="$DB_NAME-$(date +$DATE_FORMAT).sql"

# Perform mysqldump
/usr/bin/mysqldump -u root --password='root' "$DB_NAME" > "$MYSQLDUMP_FILE_NAME"

# Upload the newly created file to Google Drive
/usr/local/bin/gdrive upload --parent "$GDRIVE_FOLDER_ID" "$MYSQLDUMP_FILE_NAME"

# Delete mysqldump file
rm "$MYSQLDUMP_FILE_NAME"
```

<br />



### Crontab 

```shell
crontab -e
```


```
0 3 * * *  /bin/sh /home/mau/generate-inopack-backup.sh >> /home/mau/log 2>&1
```









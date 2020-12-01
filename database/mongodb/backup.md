Mongodb Database dump restore
===

### 导出为 csv 格式

```text
mongoexport --db myinfo --collection userdetails --csv --fields user_id,education,interest --out /opt/backups/userdetails.csv
```

### 按条件导出

```text
mongoexport --db myinfo --collection userdetails --query "{'interest' : 'MUSIC'}"
```

### 从csv 导入

```text
mongoimport --db myinfo --collection userdetails --type csv --headerline --file /backup_data/backup/userdetails.csv
```


### 单数据库备份

```bash
#!/usr/bin/zsh

BACKUP_DIR=/data/mongobackup
DAILY_DIR=$BACKUP_DIR/DAILY
WEEKLY_DIR=$BACKUP_DIR/WEEKLY
MONTHLY_DIR=$BACKUP_DIR/MONTHLY

DB_NAME=lwqbg_production
DAILYFILE=$(date +%u)_${DB_NAME}.tar
WEEKLYFILE=$(date +%U)_${DB_NAME}.tar
MONTHLYFILE=$(date +%m)_${DB_NAME}.tar

function make_dir(){
  ary=($DAILY_DIR $WEEKLY_DIR $MONTHLY_DIR)
  for dir in ${ary[@]}; do
    mkdir -p $dir
  done
}

function onedb_backup(){
  tmpfile=/tmp/${DB_NAME}_backup.tar
  mongodump -d $DB_NAME --quiet --gzip --archive=$tmpfile
  cp $tmpfile $DAILY_DIR/$DAILYFILE

  if [ ! -e $WEEKLY_DIR/$WEEKLYFILE ];then
    echo "Creating Weekly File" 
    cp $tmpfile $WEEKLY_DIR/$WEEKLYFILE
  fi  

  if [ ! -e $MONTHLY_DIR/$MONTHLYFILE ];then
    echo "Creating Monthly File" 
    cp $tmpfile $MONTHLY_DIR/$MONTHLYFILE
  fi  
  rm $tmpfile
  echo "tmpfile removed"
}

make_dir
onedb_backup

```

### 数据库恢复

```text
mongorestore --quiet --gzip --archive=backupedfile

mongorestore -d different database --quiet --gzip --archive=backedfile 
```

### 重命名数据库

```text
mongodump --archive="mongodump-test-db" --db=test

mongorestore --archive="mongodump-test-db" --nsFrom='test.*' --nsTo='examples.*'
```
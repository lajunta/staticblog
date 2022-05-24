# 数据库备份与恢复

## 备份

```bash
#!/usr/bin/zsh

BACKUP_DIR=/data/mariadbak
DAILY_DIR=$BACKUP_DIR/DAILY
WEEKLY_DIR=$BACKUP_DIR/WEEKLY
MONTHLY_DIR=$BACKUP_DIR/MONTHLY

DB_NAME=lwqzx
DAILYFILE=$(date +%u)_${DB_NAME}.gz
WEEKLYFILE=$(date +%U)_${DB_NAME}.gz
MONTHLYFILE=$(date +%m)_${DB_NAME}.gz

function make_dir(){
  ary=($DAILY_DIR $WEEKLY_DIR $MONTHLY_DIR)
  for dir in ${ary[@]}; do
    mkdir -p $dir
  done
}

function onedb_backup(){
  tmpfile=/tmp/${DB_NAME}_backup
  mysqldump ${DB_NAME} | gzip -9 > $tmpfile
  sleep 1
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

## 恢复

```bash
gzip -d sqldump.gz
mysqldump -u user -h 127.0.0.1 -D dbname -p < sqldump
```

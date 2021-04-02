#!/bin/bash

if [[ $1 == "" || $2 == "" ]] ; then
echo "格式为: addgroup.sh 名称 编号"
exit 0
fi
gname=$1$2
gnum=$2
userdel -r $gname
rm -rf /home/$gname
sed -i "/$gname/d" /etc/vsftpd.chroot_list
sed -i "/$gname/d" /etc/vsftpd.userlist

sitepath="/etc/apache2/sites-available/$gname.conf"
rm $sitepath
a2dissite $gname
systemctl reload apache2
systemctl reload vsftpd

mysql -e "drop database if exists $gname;"
mysql -e "revoke ALL PRIVILEGES on $gname.* from '$gname'@'localhost';"
mysql -e "drop user if exists '$gname'@'localhost';"

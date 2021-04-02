#!/bin/bash

if [[ $1 == "" || $2 == "" ]] ; then
echo "格式为: addgroup.sh 名称 编号"
exit 0
fi
gname=$1$2
gnum=$2
cms=${gnum}_CMS
layout=${gnum}_Layout
design=${gnum}_Design
userdel -r $gname
rm -rf /home/$gname/
useradd -m -p "$(openssl passwd -1 $gname)" -s /bin/sh $gname
#sudo -i -u $gname sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
#sudo -i -u $gname sed -i "s/robbyrussell/frisk/g" /home/$gname/.zshrc
sudo -u $gname mkdir -p /home/$gname/$cms
sudo -u $gname mkdir -p /home/$gname/$layout
sudo -u $gname mkdir -p /home/$gname/$design
grep -qxF $gname /etc/vsftpd.chroot_list || echo $gname >> /etc/vsftpd.chroot_list
grep -qxF $gname /etc/vsftpd.userlist || echo $gname >> /etc/vsftpd.userlist

content=$(cat <<- DOC
Alias /$cms /home/$gname/$cms
Alias /$layout /home/$gname/$layout
Alias /$design /home/$gname/$design
<Directory /home/$gname/$cms>
    Options FollowSymLinks
    AllowOverride Limit Options FileInfo
    DirectoryIndex index.php  index.html index.htm
    Order allow,deny
    Require all granted
    Allow from all
</Directory>
<Directory /home/$gname/$cms/wp-content>
    Options FollowSymLinks
    Require all granted
    Order allow,deny
    Allow from all
</Directory>
<Directory /home/$gname/$layout>
    AllowOverride Limit Options FileInfo
    Order allow,deny
    Require all granted
    Allow from all
</Directory>
<Directory /home/$gname/$design>
    AllowOverride Limit Options FileInfo
    Order allow,deny
    Require all granted
    Allow from all
</Directory>
DOC
)

sitepath="/etc/apache2/sites-available/$gname.conf"
echo "$content" > $sitepath 
a2ensite $gname
systemctl reload apache2
systemctl reload vsftpd

mysql -e "create database if not exists $gname;"
mysql -e "CREATE USER if not exists '$gname'@'localhost' IDENTIFIED BY '$gname';"
mysql -e "GRANT ALL PRIVILEGES ON $gname.* TO '$gname'@'localhost';"

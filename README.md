# docker

## Install docker, docker toolbox
```
# mac
download and install  dmg (docker) & pkg (docker toolbox)
# windows
# linux
```


## Get Mysql 8.0 image
MySql Server image tags : 

https://hub.docker.com/r/mysql/mysql-server/tags

```
docker pull mysql/mysql-server:8.0
docker images
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root --name mysql8 mysql/mysql-server
docker ps -a
```

### Unable to connect to 127.0.0.1:3306

http://github.com/docker-library/mysql/issues/275

```
docker exec -it [container_id] bash
mysql -u root -p
  Password: root

mysql> SELECT host, user FROM mysql.user;
mysql> CREATE USER 'username'@'%' IDENTIFIED BY 'password';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'username'@'%';
mysql> exit
```

```
docker exec -it [container_id] bash
vim /etc/my.cnf
  [mysqld]
  ...
  secure_file_priv=""
```

### Move files to docker container

https://stackoverflow.com/questions/22907231/copying-files-from-host-to-docker-container
```
docker cp file.csv [container_id]:/var/lib/mysql/SCHEMA_NAME/
```

```sql
CREATE TABLE `ANIMAL_INS` (
  col1 type1,
  ...
  colN typeN
);

LOAD DATA INFILE 'aac_intakes.csv' 
-- LOAD DATA LOCAL INFILE '/Users/agnt/Downloads/20616_26627_bundle_archive/aac_intakes.csv' 
INTO TABLE `ANIMAL_INS`
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
```


# Volume control

* Volume up/down
```commandline
amixer -D pulse sset Master 3277+
amixer -D pulse sset Master 3277-
```

* Mute/unmute toggle
```commandline
amixer -D pulse sset Master toggle
```

* check volume percentage
```commandline
amixer get Master
```

* play/pause/next/previous
```commandline
rhythmbox-client --play-pause
rhythmbox-client --previous
rhythmbox-client --next
```

# Spring Tool Suite 4 plugins
```
http://vrapper.sourceforge.net/update-site/stable
http://eclipse-color-theme.github.com/update
http://dl.bintray.com/harawata/eclipse
http://propedit.sourceforge.jp/eclipse/updates/
```

# Applying .jar to application
download tar.gz and do the following
```commandline
sudo mkdir /opt/APP_NAME
sudo tar -xzvf ide.tar.gz -C /opt/APP_NAME
```

[How to move everything to parent directory](https://superuser.com/questions/88202/how-do-i-move-files-and-directories-to-the-parent-folder-in-linux/542214)
```commandline
cd /opt/APP_NAME/APP_NAME_DUP
sudo find . -maxdepth 1 -exec mv {} .. \;
cd ..
sudo rm -rf APP_NAME_DUP
```

```commandline
sudo vim /opt/APP_NAME/bin/APP_NAME64.vmoptions
sudo vim /opt/APP_NAME/bin/APP_NAME.vmoptions

-javaagent:/full/path/to/bin/myFile.jar
```

```commandline
cp ~/myFile.jar /opt/APP_NAME/bin/
sudo chown root.root myFile.jar
sudo chmod 755 myFile.jar
sudo java -jar /opt/APP_NAME/bin/myFile.jar
cd /opt/APP_NAME/bin/ && ./APP_NAME.sh
```

# APP_NAME create shortcut
```commandline
sudo vim /usr/share/applications/APP_NAME.desktop
```
```
[Desktop Entry]
Version=1.0
Name=ide
Exec=/opt/APP_NAME/bin/APP_NAME.sh
Path=/opt/APP_NAME/bin
Icon=/opt/APP_NAME/bin/APP_NAME.png
Terminal=false
Type=Application
Categories=Application;Development;IDE
Keywords=APP_NAME;
StartupWMClass=ide
```

[Duplicating or missing launcher icon](https://askubuntu.com/questions/403766/duplicate-icons-for-manually-created-gnome-launcher-items):
```
$ xprop WM_CLASS
# click application to print StartupWMClass
# insert 'StartupWMClass=app_name' line in .desktop
```

# create symlink
```commandline
sudo ln -s /full/path/to/script.sh /usr/local/bin/name_of_new_command
vim /full/path/to/script.sh 
    # --snip--
    java -jar path/to/jar/jarName.jar "$*"
    # "$*" takes all arguements given to the bash script and send them directly
    # to your java program.
    name_of_new_command arg1 arg2 arg3 arg4
```


# Useful Commands
```commandline
lshw -short
lshw -class network

dmesg
```


# tlp
improve power usage and battery life in laptop


# powertop


# Java
```commandline
java -version
javac -version # openjdk not installed
# openjdk
sudo apt-get install default-jdk

# oracle jdk
sudo yum localinstall jdk-8u172-linux-x64.rpm
```

set default java command
```commandline
sudo alternatives --config java
1 ...
2 ...
```


# Samba
- setting on Ubuntu
```commandline
sudo apt-get update
sudo apt-get install samba
sudo gedit /etc/samba/smb.conf &
```

```
[share]
    comment = Ubuntu File Server Share
    path = /srv/samba/share
    browsable = yes
    guest ok = yes
    read only = no
    create mask = 0755
```

```commandline
sudo mkdir -p /srv/samba/share
sudo chown nobody.nogroup /srv/samba/share
sudo gedit /etc/init/nmbd.conf

sudo service smbd restart
sudo service nmbd restart

sudo touch share/test.txt
```
- setting on Windows
check network folder

http://192.168.10.25:9580/process/web/login.do?client_id=1hbaxuob0r60g&secret_key=c2b094c581d0425abb595d04e55b5b5f&os_type=3&stat_token=test_stat_token&redirect_url=https%3A%2F%2Fglobal.rowem.com%2F


https://partner-auth-test.passikey.com/process/web/login.do?client_id=1hbaxuob0r60g&secret_key=c2b094c581d0425abb595d04e55b5b5f&os_type=3&stat_token=test_stat_token&redirect_url=https%3A%2F%2Fglobal.rowem.com%2F

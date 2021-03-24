# Apache aufsetzen und konfigurieren


## Verzeichnis erstellen vmls3
```bash
root@vmLS3:~# mkdir -p /www/www.smartlearn.dmz/err
```

## Verzeichnis erstellen vmls5
```bash 
root@vmLS5:~# mkdir -p /www/www.smartlearn.ch/err
root@vmLS5:~# mkdir -p /www/ku1.smartlearn.ch/err
root@vmLS5:~# mkdir -p /www/ku2.smartlearn.ch/err
``` 

## Installieren
```bash
root@vmLS3:~# sudo apt install apache2 -y
``` 

## Directory zu Apache config hinzufügen
```bash
root@vmLS3:~# vim /etc/apache2/apache2.conf
```

```bash
<Directory /www/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
```

## Namebased Virtualhost erstellen

```bash
root@vmLS3:~# cd /etc/apache2/sites-enabled/
root@vmls3:/etc/apache2/sites-enabled# vim www.smartlearn.dmz.conf
```
```bash
<VirtualHost *:80>
ServerName www.smartlearn.dmz
ServerAlias smartlearn.dmz
DocumentRoot /www/www.smartlearn.dmz
</VirtualHost>
```
## Bind Anpassen
```Bash
root@vmLS3:~# vim /etc/bind/db.smartlearn.dmz
``` 
```Bash
www     IN      CNAME   vmls3.smartlearn.dmz.
``` 

# Apache aufsetzen und konfigurieren


## Verzeichnis erstellen vmls3
<pre>
root@vmLS3:~# mkdir -p /www/www.smartlearn.dmz/err
</pre>

## Verzeichnis erstellen vmls5
<pre> 
root@vmLS5:~# mkdir -p /www/www.smartlearn.ch/err
root@vmLS5:~# mkdir -p /www/ku1.smartlearn.ch/err
root@vmLS5:~# mkdir -p /www/ku2.smartlearn.ch/err
</pre>

## Installieren
<pre>
root@vmLS3:~# sudo apt install apache2 -y
</pre>

## Directory zu Apache config hinzufügen
<pre>
root@vmLS3:~# vim /etc/apache2/apache2.conf
</pre>

<pre>
<Directory /www/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
</pre>

## Namebased Virtualhost erstellen
```bash
root@vmLS3:~# cd /etc/apache2/sites-enabled/
root@vmls3:/etc/apache2/sites-enabled# vim www.smartlearn.dmz.conf
```
<pre>
<VirtualHost *:80>
ServerName www.smartlearn.dmz
ServerAlias smartlearn.dmz
DocumentRoot /www/www.smartlearn.dmz
</VirtualHost>
</pre>

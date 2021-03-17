# Modul300-Dokumentation

## Install Bind
<pre>
sudo apt install bind9
</pre>

## create files
<pre>
sudo cp db.empty db.smartlearn.dmz
sudo cp db.empty db.smartlearn.lan
sudo cp db.empty db.192.168.220
sudo cp db.empty db.192.168.210
</pre>

## named.conf
<pre>
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

//
// DMZ
//

zone "smartlearn.dmz" {
        type master;
        notify no;
        file "/etc/bind/db.smartlearn.dmz";
};

zone "220.168.192.in-addr.arpa" {
        type master;
        notify no;
        file "etc/bind/db.192.168.220";
};

//
// LAN
//

zone "smartlearn.lan" {
        type master;
        notify no;
        file "/etc/bind/db.smartlearn.lan";
};

zone "210.168.192.in-addr.arpa" {
        type master;
        notify no;
        file "etc/bind/db.192.168.210";
};
</pre>

## named.conf.options
<pre>
options {
        // IPv4
        listen-on port 53 {
                any;
        };
        // IPv6 ist innerhalb von Smartlearn nicht in Verwendung
        listen-on-v6 {
                none;
        };
        // DNS-Anfragen sollen an Quad9 weitergleitet werden
        forwarders {
                1.1.1.1;
                9.9.9.9;
        };
        // Anfragen sollen von ueberall her moeglich sein (inkl. Internet)
        allow-query {
                any;
        };
        // Rekursive Anfragen nur fuer Clients aus dem smartlearn-Netz
        allow-recursion {
                localhost;
                127.0.0.1;
                192.168.220.0/24;
                192.168.210.0/24;
        };

};
</pre>

## db.smartlearn.dmz
<pre>
;
; Zonendatei fuer lan.smartlearn
; /etc/bind/db.smartlearn.dmz
;
$TTL    3600
@       IN      SOA     vmls3.dmz.smartlearn.      root.localhost. (
                        1
                        1H
                        2H
                        1D
                        1H )
@       IN      NS      vmls1.dmz.smartlearn.
vmlf1   IN      A       192.168.220.1
vmls3	IN 	A 	192.168.220.12
</pre>

## db.192.168.220
<pre>
;
; Zonendatei fuer 192.168.220
; /etc/bind/db.192.168.220
;
$TTL    3600
@       IN      SOA     vmls3.smartlearn.dmz.   root.localhost. (
                        1
                        1H
                        2H
                        1D
                        1H )
@       IN      NS      vmls3.smartlearn.dmz.
1       IN      PTR     vmlf1.smartlearn.dmz.
12      IN      PTR     vmls3.smartlearn.dmz.
</pre>

## db.smartlearn.lan
<pre>
;
; Zonendatei fuer lan
; /etc/bind9/db.smartlearn.lan
;
$TTL    3600
@       IN      SOA     vmls3.smartlearn.dmz.      root.localhost. (
                        1
                        1H
                        2H
                        1D
                        1H )
@       IN      NS      vmls3.smartlearn.dmz.
vmlf1   IN      A       192.168.210.1
vmwp1   IN      A       192.168.210.10
vmlp1   IN      A       192.168.210.30
vmls4   IN      A       192.168.210.60
vmls5   IN      A       192.168.210.61
test    IN      A       192.168.210.99
</pre>

## db.192.168.210
<pre>
;
; Zonendatei fuer 192.168.210
; /etc/bind/db.192.168.210
;
$TTL    3600
@       IN      SOA     vmls3.smartlearn.dmz.   root.localhost. (
                        1
                        1H
                        2H
                        1D
                        1H )
@       IN      NS      vmls3.smartlearn.dmz.
1       IN      PTR     vmlf1.smartlearn.lan.
10      IN      PTR     vmwp1.smartlearn.lan.
30      IN      PTR     vmlp1.smartlearn.lan.
60      IN      PTR     vmls4.smartlearn.lan.
61      IN      PTR     vmls5.smartlearn.lan.
99      IN      PTR     test.smartlearn.lan.
</pre>

### Bind config auf Host anpassen
Anpassen der Bind Config der Hosts

## DNS eintrag ändern
<pre>
sudo vim /etc/netplan/filename
sudo netplan apply
</pre>

## Searchdomain on vmLP1
wird nicht zweingend gebraucht
<pre>
nm-connection-editor
</pre>
1. Anschliessend auf das Interface
2. IPv4-Einstellungen auswählen
3. Unter Suchdomäne <pre>smartlearn.lan smartlearn.dmz</pre> eintragen

# Debug
<pre>
systemd-resolve --status
systemd-resolve --flush
</pre>

## Searchdomain on other 
<pre>
sudo vim /etc/netplan/...
</pre>

<pre>
nameservers:
        search: [ smartlearn.lan smartlearn.dmz ]
</pre>

#Testen von DNS
<pre>
dig Suche DNS
dig A  vmls4.smartlearn.lan 192.168.220.12

nslookup Suche
nslookup vmls4.smartlearn.lan
</pre>

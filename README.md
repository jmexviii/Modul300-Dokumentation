# Modul300-Dokumentation

1. sudo apt install bind9
2. create files
<pre>
-  sudo cp db.empty db.smartlearn.dmz
-  sudo cp db.empty db.smartlearn.lan
-  sudo cp db.empty db.192.168.220
-  sudo cp db.empty db.192.168.210
</pre>

4. named.conf

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
        notifiy no;
        file "/etc/bind/db.smartlearn.dmz";
};

zone "220.168.192.in-addr.arpa" {
        type master;
        notify no;
        file "etc/bind/db.220.168.192";
};

//
// LAN
//

zone "smartlearn.lan" {
        type master;
        notifiy no;
        file "/etc/bind/db.smartlearn.lan";
};

zone "210.168.192.in-addr.arpa" {
        type master;
        notify no;
        file "etc/bind/db.210.168.192";
};
</pre>


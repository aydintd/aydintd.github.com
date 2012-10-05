---
layout: post
title: LTSP-DHCP Konfigurasyonu
---

##  LTSP ve DHCP Server Kurulumu 

```bash
$ sudo apt-get install ltsp-server-standalone
```
 
komutu LTSP ve DHCP sunucularının kurulumunu sağlar.
Sunucuya daha önce DHCP server kurulmamışsa bu komut sayesinde
her iki server da aynı anda kurulacaktır.

### LTSP Server Yapılandırılması

```bash
$ sudo vim /etc/ltsp/ltsp-build-client.conf
```

komutu LTSP sunucunun konfigürasyon ayarlarını saklar.
ltsp-build-client.conf dosyası içerisine aşağıdaki metin
bloğunu girin :

```conf
    # The chroot architecture.
    ARCH=i386

    # ubuntu-desktop and edubuntu-desktop are tested.
    # If you test with [k|x]ubuntu-desktop, edit this page and mention if it worked OK.
    # kubuntu lucid (10.10) working okay.
    FAT_CLIENT_DESKTOPS="ubuntu-desktop"

    # Space separated list of programs to install.
    # The java plugin installation contained in ubuntu-restricted-extras
    # needs some special care, so let's use it as an example.
    LATE_PACKAGES="
        ubuntu-restricted-extras
        gimp
        nfs-client"

    # This is needed to answer "yes" to the Java EULA.
    # We'll create that file in the next step.
    DEBCONF_SEEDS="/etc/ltsp/debconf.seeds"

    # This uses the server apt cache to speed up downloading.
    # This locks the servers dpkg, so you can't use apt on
    # the server while building the chroot.
    MOUNT_PACKAGE_DIR="/var/cache/apt/archives/"
```

### DHCP Server Yapılandırılması;


`$ sudo vim /etc/ltsp/dhcpd.conf`

komutunu verip, aşağıdaki metni dhcpd.conf dosyası içine copy-paste edin :

    authoritative;
    allow bootp;

    subnet 10.0.2.0 netmask 255.255.255.0 {
        range 10.0.2.20 10.0.2.250;

        option domain-name "example.com";
        option domain-name-servers 192.168.111.5;
        option broadcast-address 10.0.2.255;
        option routers 10.0.2.2;
    #    next-server 192.168.0.1;

    #    get-lease-hostnames true;
        option subnet-mask 255.255.255.0;
        option root-path "/opt/ltsp/i386";
        if substring( option vendor-class-identifier, 0, 9 ) = "PXEClient" {
            filename "/ltsp/i386/pxelinux.0";

        } else {
            filename "/ltsp/i386/nbi.img";
        }
    }

+ (!) option domain-name-servers karşısına yazılan adres, 
internetinizin o anki size verdiği DNS adresi olmalı. Aksi takdirde;
Daha sonradan kurulacak olan client'ın internete bağlı gözüktüğünü 
ancak herhangi bir websitesi açamadığını göreceksiniz. 

`$ sudo vim /etc/default/isc-dhcp-server`

yazıp, isc-dhcp-server dosyası içinde yorum satırı olarak bulunan
INTERFACES'i yorum satırından çıkarıp;

`INTERFACES = "eth1"`

olarak değiştirip dosyayı kaydedin.

### Server'ları Yeniden Başlatmak ;

`$ /etc/init.d/networking restart`
`$ /etc/init.d/dhcp3-server restart`

komutlarını verin ve server'ları yeniden başlatın.

***
## FAT-Client Kurulumu

Aşağıdaki kodu yazın :

`$ ltsp-build-client --fat-client --fat-client-desktop ubuntu.desktop --arch i386 --skipimage`

* Bu kod FAT-Clientları sunucu içerisinde oluşturur. (`/opt/ltsp/i386` dizini altında.)
(Client'a hangi işletim sistemini kurmak istiyorsanız, kodda o işletim sistemini 
yazmalısınız. Örneğin ubuntu için kod içersinde ubuntu.desktop denmiş.)

### FAT-Client Güncellemesi

`$ ltsp-update-sshkeys`
`$ ltsp-update-image --arch i386`

* FAT-Client'lar içersinde yaptığımız her değişiklikten sonra,client'ların değişikliği
(örneğin bir driver yüklenmesi ya da bir program kurulması) algılaması için image oluşturmak 
gerekiyor.


## Ağ Ayarları
 Arayüzleri tanımlamak için;

`$ sudo vim /etc/network/interfaces`

komutunu yazıp, interfaces dosyasına aşağıdaki metni girin :

    auto lo

    iface lo inet loopback

    auto eth1

    iface eth1 inet static

           address 10.0.0.2
           netmask 255.255.255.254
           broadcast 10.0.2.255

    auto eth2

    iface eth2 inet dhcp


* Tüm bunlardan sonra, sunucunun internet bağlantısının olduğunu ancak,
client'ınızın internet bağlantısının olmadığını göreceksiniz.

* Bunun sebebinin client'ın serverdan internete bağlanma isteği göndermesine karşın
server'ın buna izin vermemesidir. Yani sunucu, kendi üzerinden client'lara internete
çıkma hakkı tanımıyor. 

### NAT Maskeleme

* İç ağdaki makinaların Internet üzerindeki makinalarla haberleşmesini sağlayabilmek için NAT Maskelemesi (Network Access Translation Masquerade) yapılmalıdır.

* Yukarıdaki NAT Maskeleme ve İzinler için rc.local içine aşağıdaki komutları girin :

`$ sudo vim /etc/init.d/rc.local`

`$ iptables -t nat -A POSTROUTING -o eth2 -j MASQUERADE`

`$ echo "1" > /proc/sys/net/ipv4/ip_forward`

* Gerekli izinler ve maskeleme yapıldı.
* Artık client'lar da sunucu üzerinden internete ulaşabilirler.



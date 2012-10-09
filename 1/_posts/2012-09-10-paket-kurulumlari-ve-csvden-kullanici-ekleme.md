---
layout: post
title: Paket Kurulumları ve .csv Uzantılı Dosyalardan Kullanıcı/Grup Ekleme
---

## Paket Kurulumu

- `sudo ./lab-paket-gem-yükle gem_listesi paket_listesi` komutunu çalıştır

        #!/bin/bash
        for gem in `cat $1`; do
        gemler="$gemler $gem"
        done

        gem install $gemler

        for paket in `cat $2`; do
        paketler="$paketlerler $paket"
        done

        gem install $paketler

   UYARI: Bu listeler her bir gem ve paket adını alt alta olacak şekilde eklemelidir.  
   Örnek dosya: src/gemler.txt ve src/paketler.txt

## Kullanıcı/Grupların Eklenmesi

- `$ sudo ./lab-kullanıcıları-ekle Dosya_ADI` komutunu çalıştır

   Öntanımlı kabuğu değiştir

        #!/bin/bash
        useradd -Ds /bin/bash

        for isim in `cat $1`; do
        useradd -d /home/$isim -p `mkpasswd 123456` $isim
                mkdir /home/$isim
                chown -R $isim:$isim /home/$isim
        done

   Betiğe argüman olarak verilecek dosya formatı şu şekilde olmalıdır:  
   Dosya:  
   cankurnaz  
   emineker  

   UYARI: Bu dosyada türkçe karakter ve noktalama işaretleri olmamalıdır; bu  
   işlemin öncesinde dosyayı belirtilen formata uygun hale getirin.

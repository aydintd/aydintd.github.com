---
layout: post
title: Betik ile Paket/Gem Kurulumu
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


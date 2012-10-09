---
layout: post

title: .csvden Kullanıcı/Grup Ekleme
---

## Kullanıcı/Grupların Eklenmesi
 
- `$ sudo ./lab-kullanıcıları-ekle Dosya_ADI` komutunu çalıştır

  Öntanımlı kabuğu değiştir
  
        ```sh
        #!/bin/bash
        useradd -Ds /bin/bash
  
        for isim in `cat $1`; do
        useradd -d /home/$isim -p `mkpasswd 123456` $isim
          mkdir /home/$isim
          chown -R $isim:$isim /home/$isim
        done
        ```

  Betiğe argüman olarak verilecek dosya formatı şu şekilde olmalıdır:  

        ```vim
        cankurnaz  
        emineker
        ```  
  
  UYARI: Bu dosyada türkçe karakter ve noktalama işaretleri olmamalıdır; bu  
  işlemin öncesinde dosyayı belirtilen formata uygun hale getirin.

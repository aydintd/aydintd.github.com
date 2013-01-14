---
title: Nginx-mysql-php5 cgi-supervisor Kurulumu
tag:  
layout: post
category: 
---
  Fatfreeframeworks (F3), Yii veya Php denemelerimiz için  
  kullanacağımız ortamı sırasıyla kuralım :  

### Kurulum İçerikleri  

  * mysql Kurulumu  
  * nginx kurulumu ve konfigurasyonu  
  * php5-cgi kurulumu  
  * supervisor kurulumu  

#### Mysql Kurulumu  

  Paketi aşağıdaki şekilde kuruyoruz :  

  `$ sudo apt-get install mysql-server mysql-client`  

  Kurulumda bizden mysql root password istiyor. Uygun bir  
  password girip kurulumun tamamlanmasını bekliyoruz.  

  Kurulum bittikten sonra mysql doğru çalışıyor mu diye  
  kontrol etmemiz gerek. Mysql prosesi var mı kontrol edelim :    

  `$ sudo ps ax | grep mysqld`  
	  
	  6234 ?            Ssl  O:OO /usr/sbin/mysqld
	  6546 pts/0        S+   0:00  grep --color=auto mysqld

  Yukarıdakine benzer bir çıktı veriyorsa mysql doğru kurulmuş demektir.  
  Ancak dilerseniz mysql e bir giriş yapalım ve emin olalım.  

  `$ mysql -u root -p` 

  Kurulum yaparken ki root şifrenizi girip mysql konsolunuzu görüntüleyin.  
  `exit` diyerek çıkış yapalım.  

#### Php5 &  php-cgi Kurulumu

  Aşağıdaki komutla paketi kuralım :

  `$ sudo apt-get install php5 php5-cgi`

  Kurulum bittikten sonra php-cgi processi çalışıyor mu kontrol edelim.  

  `$ sudo ps ax | grep cgi`
         
	 4580 pts/0    S+     0:00 grep --color=auto cgi
  
  Kuracağımız nginx sunucusunda php5 çalıştırabilmek için FastCGI kullanacağız.  
  Bu yüzden gerekli modülleri aktivesi için aşağıdaki komutu çalıştırıyoruz :  

  ```
  $ sudo apt-get install php5-mysql php5-curl php5-gd php5-idn php-pear \ 
  php5-imagick php5-imap php5-common php5-mcrypt php5-memcache   \
  php5-mhash php5-ming php5-ps php5-pspell php5-recode php5-snmp \ 
  php5-sqlite php5-tidy php5-xmlrpc php5-xsl
  ```
  Bu modülleri kurduktan sonra sitelerimizi oluşturabilmek adına  
  /var/www dizinine gerekli izni veriyoruz (eğer /www dizini yoksa kendiniz  
  oluşturun[1]) :  
  
  `$ sudo mkdir /var/www` [1]
  `$ sudo chmod 777 -R /var/www`

  Denemek için konsolunuzda :

        cat >/var/www/test.php
	<?php
	phpinfo();
	?>
	ctrl + d

  Dedikten sonra web tarayıcınıza `http://localhost/test.php`
  yazdığınızda sizi php nin test sayfasının karşılamasını umuyoruz.  
  Eğer bir terslik varsa şu ana kadar ki kurulumlarınızı gözden geçirin.  


##TODO

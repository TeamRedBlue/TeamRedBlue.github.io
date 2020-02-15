---
title: Apache Nifi
layout: post
---

# Ney bu Nifi ?
---
# Temel anlamda, elinizdeki kaynakları yönetebileceğiniz, kullanımı kolay bir araç diyebiliriz. Yazının devamında örneklerle daha da açıklamaya çalışacağım.
---
# Öngereksinimler
---
# Kullandığınız makinede min java8 yüklü olması gerekmekte başka da bişey lazım değil
---
# Kurulum
# <a href="https://nifi.apache.org/download.html">Buradan</a> bin.tar.gz olanı indirebilirsiniz. Ben son sürüm olanı kullanıyorum.
---
# tar ile dosyaları çıkardıktan sonra çıkarılan dosyanın içerisine gidip bin klasörü altında nifi yi çalıştıralım.
---
```zsh
~/Desktop/NIFI/nifi-1.11.1/bin 
:: ./nifi.sh start 
nifi.sh: JAVA_HOME not set; results may vary

Java home: 
NiFi home: /home/sesh/Desktop/NIFI/nifi-1.11.1

Bootstrap Config File: /home/sesh/Desktop/NIFI/nifi-1.11.1/conf/bootstrap.conf

WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by org.apache.nifi.bootstrap.util.OSUtils (file:/home/sesh/Desktop/NIFI/nifi-1.11.1/lib/bootstrap/nifi-bootstrap-1.11.1.jar) to method java.lang.ProcessImpl.pid()
WARNING: Please consider reporting this to the maintainers of org.apache.nifi.bootstrap.util.OSUtils
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release

~/Desktop/NIFI/nifi-1.11.1/bin 
:: sudo netstat -tulpan | grep 8080
[sudo] password for sesh: 
tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN      374615/java         
```
---
# localhost:8080/nifi adresi nifi servisinin arayüzü olarak gelmektedir.
# Şimdi bu arayüzde yapılabilecek çok şey var ancak kimseyi darlamamak veya rüyalarına girmemek için 1-2 örnek yapıp yazıyı bitireceğim.
# Sol üstte Processor iconuna basıp içerisinde bir çok element görebiliriz. Bu örnekte GenerateFlowFile ve PutFile elementlerini kullanacağız.
---
# GenerateFlowFile adından da anlaşılacağı gibi rastgele data üretir. Ama istersek rastgele değil 'ŞU' datayı üret te diyebiliriz. 
# Neyse basalım buna ekrana sürükleyip konfigüre edelim.
---

![Processor](https://teamredblue.github.io/processor.png)

---

![Processor](https://teamredblue.github.io/flow.png)

---
# Üzerine çift tıklayıp Scheduling sekmesine tıklayalım.

![Processor](https://teamredblue.github.io/sch2.png)

---
# Burada run Schecule değerini 0 değil de 1 olarak ayarlayalım.
---

# Properties sekmesini açalım.
![Processor](https://teamredblue.github.io/text.png)

# Custom Text alanını kafamıza göre doldurabiliriz.
---

# Tekrar Processor simgesine tıklayarak gelen menüden PutFile elementini seçelim ve dashboarda yerleştirelim.

![Processor](https://teamredblue.github.io/put.png)

---

![Processor](https://teamredblue.github.io/nifi_falan.png)

# Directory i değiştirelim

# PutFile elementinin Settings sekmesine giderek success ve failure alanlarını işaretleyelim. Bu alanlar herhangi bir problem oluşunca veya flow başarı ile çalışınca ne olacağıyla alakalı bize yönlendirme yapmamızı istemektedir. Ancak bizim success veya failure senaryolarında yapacak başka bir şeyimiz olmadığından dümdüz işaretleyip geçelim.

![Processor](https://teamredblue.github.io/failure.png)

---

# Apply diyip GenerateFlowFile ile PutFile ı bağlayalım.
# Bağlarken gelen ekrandan success i işaretliyip ok diyelim

![Processor](https://teamredblue.github.io/success.png)

---

# Şu an ekranın aşağıdaki gibi olması gerekir. 
![Processor](https://teamredblue.github.io/gogo.png)

---

# Shifte basılı tutup tüm elementleri seçip Sol menüden Start tuşuna basalım ve flowu başlatalım.
![Processor](https://teamredblue.github.io/start.png)

---
```zsh

/tmp/nifi_falan 
:: cat 8095ff70-e303-42a3-916a-98d59bb23d67 
───────┬─────────────────────────────────────────────────────────────────────────────
       │ File: 8095ff70-e303-42a3-916a-98d59bb23d67
───────┼─────────────────────────────────────────────────────────────────────────────
   1   │ NIFI MIFI BİŞELER
───────┴─────────────────────────────────────────────────────────────────────────────
```

---

# Şimdi bu örnekte biraz midesizlik yapıp GenerateFlowFile ile sürekli web servis logu üretelim . Logun içerisindeki ip adresini ayrıştırıp, ayrıştırılan ip adresini json a çevirip dosyaya yazdıralım.

---

# Kullanılacak Processorler;
 1 - GenerateFlowFile - Sürekli bize log üretmesini sağlayalım.

 2 - ExtractText - Burası en leş kısım regex falan yazmak gerekiyo, yazılan regex i text içerisinde aratıp eşleşmesini sağlıyor.

   3 - RouteOnAttribute - Regex ile taratılan dosyada bulunan kısım buraya aktarılıyor ve değişkene burada vereceğimiz değişkene sahip oluyor. Kısacası regexte buldugumuzu bi değişkene atıyoruz diyebiliriz.
 
   4 - AttributesToJson - Aldığımız veriyi jsona çeviren dalgamotor.
 
   5 - PutFile - Dosyaya basmayak mı dayı ?

---

# GenerateFlowFile içeriğindeki custom text i aşağıdaki gibi değşitirelim. Örnek Log;
127.0.0.1 - peter [9/Feb/2017:10:34:12 -0700] "GET /sample-image.png HTTP/2" 200 1479

![Processor](https://teamredblue.github.io/customtext.png)

---

# ExtractText -> Properties altındaki Menüden + ya tıklayarak ip_bul isminde bir alan açalım ve karşısında ip yi bulacak regexi yazalım. Regex; (^\d+.\d+.\d+.\d+) anlaşılacağı üzere dümenden regex yazıyoruz arkadaşlar. 
![Processor](https://teamredblue.github.io/ipbul.png)

---

# RouteOnAttribute diyip + ya basalım ve aşağıdaki alanı ekleyelim.

![Processor](https://teamredblue.github.io/route.png)

---

# AttributesToJSON diyip + ya basalım. Aşağıdaki alanı ekleyelim.

![Processor](https://teamredblue.github.io/json.png)

# Destination alanını da flowfile-content olarak değiştirelim.

![Processor](https://teamredblue.github.io/json2.png)

---

# PutFile ile bir directory verip başlatalım.
![Processor](https://teamredblue.github.io/dumen.png)


---

![Processor](https://teamredblue.github.io/asdasd.png)

---

# Hepsini sıradan birbirine bağlayıp success failure ip_bul gibi elementlerin hepsini işaretleyelim. Son olarak PutFile elementine tıklayarak success ve failure u da işaretledikten sonra tüm flowu başlatalım.

---

![Processor](https://teamredblue.github.io/result.png)

--

# Author <a href=" https://twitter.com/SectionText">Section Text</a>
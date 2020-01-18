---
title: Binary Anatomisi
layout: post
---

# C Derleme İşlemi
 ---
### Derleme işlemine; bir kodun human-readable formattan, işlemcinin okuyabilececği makine diline çevrilmesine denir. Bir C programının derlenmesi 4 aşamadan oluşur. Preprocessing, compilation, assembly ve linking. Bu yazıda ELF Dosyaları için derlemenin tüm aşamalarını detaylıca açıklayacağım. 
 
 ---
 
![alt text](http://localhost:4000/a.png)

---
# Preprocessing
---
### Derleme işleminin ilk aşamasıdır. Preprocessing için C kaynak kodu dosyaları (.c) gereklidir. #include ile direktif edilen dosyalara header dosyaları denir. bu dosyalar .h uzantılı olmakla birlikte içerisinde gerekli fonksiyonlar bulundurmaktadır.
### Basit bir C programı yazıp başına gelenlere bakalım.
---
```c++
#include <stdio.h>
#define STRING  "%s"
#define  MSG  "Vay basima gelenler\n"
int main(int argc, char *argv[]) {
  printf(STRING,MSG);
  return 0; }
```
```zsh
$ gcc -E -P compilation.c 
```
```c++
typedef long unsigned int size_t;
typedef __builtin_va_list __gnuc_va_list;
typedef unsigned char __u_char;
typedef unsigned short int __u_short;
typedef unsigned int __u_int;
typedef unsigned long int __u_long;
typedef signed char __int8_t;
typedef unsigned char __uint8_t;
typedef signed short int __int16_t;
typedef unsigned short int __uint16_t;
typedef signed int __int32_t;
typedef unsigned int __uint32_t;
typedef signed long int __int64_t;
typedef unsigned long int __uint64_t;
typedef __int8_t __int_least8_t;
typedef __uint8_t __uint_least8_t;
typedef __int16_t __int_least16_t;
typedef __uint16_t __uint_least16_t;
typedef __int32_t __int_least32_t;
typedef __uint32_t __uint_least32_t;
typedef __int64_t __int_least64_t;
typedef __uint64_t __uint_least64_t;
typedef long int __quad_t;
typedef unsigned long int __u_quad_t;
typedef long int __intmax_t;
typedef unsigned long int __uintmax_t;
typedef unsigned long int __dev_t;
typedef unsigned int __uid_t;
typedef unsigned int __gid_t;
typedef unsigned long int __ino_t;
typedef unsigned long int __ino64_t;
typedef unsigned int __mode_t;
typedef unsigned long int __nlink_t;
typedef long int __off_t;
typedef long int __off64_t;
typedef int __pid_t;
typedef struct { int __val[2]; } __fsid_t;
typedef long int __clock_t;
typedef unsigned long int __rlim_t;
typedef unsigned long int __rlim64_t;
typedef unsigned int __id_t;
typedef long int __time_t;
typedef unsigned int __useconds_t;
typedef long int __suseconds_t;
typedef int __daddr_t;
typedef int __key_t;
typedef int __clockid_t;
typedef void * __timer_t;
typedef long int __blksize_t;
typedef long int __blkcnt_t;
typedef long int __blkcnt64_t;
typedef unsigned long int __fsblkcnt_t;
typedef unsigned long int __fsblkcnt64_t;
typedef unsigned long int __fsfilcnt_t;
typedef unsigned long int __fsfilcnt64_t;
typedef long int __fsword_t;
typedef long int __ssize_t;
typedef long int __syscall_slong_t;
typedef unsigned long int __syscall_ulong_t;
typedef __off64_t __loff_t;
typedef char *__caddr_t;
typedef long int __intptr_t;
typedef unsigned int __socklen_t;
typedef int __sig_atomic_t;
typedef struct
{
  int __count;
  union
  {
    unsigned int __wch;
    char __wchb[4];
  } __value;
} __mbstate_t;
typedef struct _G_fpos_t
{
  __off_t __pos;
  __mbstate_t __state;
} __fpos_t;
typedef struct _G_fpos64_t
{
  __off64_t __pos;
  __mbstate_t __state;
} __fpos64_t;
struct _IO_FILE;
typedef struct _IO_FILE __FILE;
struct _IO_FILE;
typedef struct _IO_FILE FILE;
struct _IO_FILE;
struct _IO_marker;
struct _IO_codecvt;
struct _IO_wide_data;
typedef void _IO_lock_t;
struct _IO_FILE
{
  int _flags;
  char *_IO_read_ptr;
  char *_IO_read_end;
  char *_IO_read_base;
  char *_IO_write_base;
  char *_IO_write_ptr;
  char *_IO_write_end;
  char *_IO_buf_base;
  char *_IO_buf_end;
  char *_IO_save_base;
  char *_IO_backup_base;
  char *_IO_save_end;
  struct _IO_marker *_markers;
  struct _IO_FILE *_chain;
  int _fileno;
  int _flags2;
  __off_t _old_offset;
  unsigned short _cur_column;
  signed char _vtable_offset;
  char _shortbuf[1];
  _IO_lock_t *_lock;
  __off64_t _offset;
  struct _IO_codecvt *_codecvt;

  struct _IO_wide_data *_wide_data;
  struct _IO_FILE *_freeres_list;
  void *_freeres_buf;
  size_t __pad5;
  int _mode;
  char _unused2[15 * sizeof (int) - 4 * sizeof (void *) - sizeof (size_t)];
};
typedef __gnuc_va_list va_list;
typedef __off_t off_t;
typedef __ssize_t ssize_t;
typedef __fpos_t fpos_t;
extern FILE *stdin;
extern FILE *stdout;
extern FILE *stderr;
extern int remove (const char *__filename) __attribute__ ((__nothrow__ , __leaf__));
extern int rename (const char *__old, const char *__new) __attribute__ ((__nothrow__ , __leaf__));
extern int renameat (int __oldfd, const char *__old, int __newfd,
       const char *__new) __attribute__ ((__nothrow__ , __leaf__));
extern FILE *tmpfile (void) ;
extern char *tmpnam (char *__s) __attribute__ ((__nothrow__ , __leaf__)) ;
extern char *tmpnam_r (char *__s) __attribute__ ((__nothrow__ , __leaf__)) ;
extern char *tempnam (const char *__dir, const char *__pfx)
     __attribute__ ((__nothrow__ , __leaf__)) __attribute__ ((__malloc__)) ;
extern int fclose (FILE *__stream);
extern int fflush (FILE *__stream);
extern int fflush_unlocked (FILE *__stream);
extern FILE *fopen (const char *__restrict __filename,
      const char *__restrict __modes) ;
extern FILE *freopen (const char *__restrict __filename,
        const char *__restrict __modes,
        FILE *__restrict __stream) ;
extern FILE *fdopen (int __fd, const char *__modes) __attribute__ ((__nothrow__ , __leaf__)) ;
extern FILE *fmemopen (void *__s, size_t __len, const char *__modes)
  __attribute__ ((__nothrow__ , __leaf__)) ;
extern FILE *open_memstream (char **__bufloc, size_t *__sizeloc) __attribute__ ((__nothrow__ , __leaf__)) ;
extern void setbuf (FILE *__restrict __stream, char *__restrict __buf) __attribute__ ((__nothrow__ , __leaf__));
extern int setvbuf (FILE *__restrict __stream, char *__restrict __buf,
      int __modes, size_t __n) __attribute__ ((__nothrow__ , __leaf__));
extern void setbuffer (FILE *__restrict __stream, char *__restrict __buf,
         size_t __size) __attribute__ ((__nothrow__ , __leaf__));
extern void setlinebuf (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__));
extern int fprintf (FILE *__restrict __stream,
      const char *__restrict __format, ...);
extern int printf (const char *__restrict __format, ...);
extern int sprintf (char *__restrict __s,
      const char *__restrict __format, ...) __attribute__ ((__nothrow__));
extern int vfprintf (FILE *__restrict __s, const char *__restrict __format,
       __gnuc_va_list __arg);
extern int vprintf (const char *__restrict __format, __gnuc_va_list __arg);
extern int vsprintf (char *__restrict __s, const char *__restrict __format,
       __gnuc_va_list __arg) __attribute__ ((__nothrow__));
extern int snprintf (char *__restrict __s, size_t __maxlen,
       const char *__restrict __format, ...)
     __attribute__ ((__nothrow__)) __attribute__ ((__format__ (__printf__, 3, 4)));
extern int vsnprintf (char *__restrict __s, size_t __maxlen,
        const char *__restrict __format, __gnuc_va_list __arg)
     __attribute__ ((__nothrow__)) __attribute__ ((__format__ (__printf__, 3, 0)));
extern int vdprintf (int __fd, const char *__restrict __fmt,
       __gnuc_va_list __arg)
     __attribute__ ((__format__ (__printf__, 2, 0)));
extern int dprintf (int __fd, const char *__restrict __fmt, ...)
     __attribute__ ((__format__ (__printf__, 2, 3)));
extern int fscanf (FILE *__restrict __stream,
     const char *__restrict __format, ...) ;
extern int scanf (const char *__restrict __format, ...) ;
extern int sscanf (const char *__restrict __s,
     const char *__restrict __format, ...) __attribute__ ((__nothrow__ , __leaf__));
extern int fscanf (FILE *__restrict __stream, const char *__restrict __format, ...) __asm__ ("" "__isoc99_fscanf") ;
extern int scanf (const char *__restrict __format, ...) __asm__ ("" "__isoc99_scanf") ;
extern int sscanf (const char *__restrict __s, const char *__restrict __format, ...) __asm__ ("" "__isoc99_sscanf") __attribute__ ((__nothrow__ , __leaf__));

extern int vfscanf (FILE *__restrict __s, const char *__restrict __format,
      __gnuc_va_list __arg)
     __attribute__ ((__format__ (__scanf__, 2, 0))) ;
extern int vscanf (const char *__restrict __format, __gnuc_va_list __arg)
     __attribute__ ((__format__ (__scanf__, 1, 0))) ;
extern int vsscanf (const char *__restrict __s,
      const char *__restrict __format, __gnuc_va_list __arg)
     __attribute__ ((__nothrow__ , __leaf__)) __attribute__ ((__format__ (__scanf__, 2, 0)));
extern int vfscanf (FILE *__restrict __s, const char *__restrict __format, __gnuc_va_list __arg) __asm__ ("" "__isoc99_vfscanf")
     __attribute__ ((__format__ (__scanf__, 2, 0))) ;
extern int vscanf (const char *__restrict __format, __gnuc_va_list __arg) __asm__ ("" "__isoc99_vscanf")
     __attribute__ ((__format__ (__scanf__, 1, 0))) ;
extern int vsscanf (const char *__restrict __s, const char *__restrict __format, __gnuc_va_list __arg) __asm__ ("" "__isoc99_vsscanf") __attribute__ ((__nothrow__ , __leaf__))
     __attribute__ ((__format__ (__scanf__, 2, 0)));
extern int fgetc (FILE *__stream);
extern int getc (FILE *__stream);
extern int getchar (void);
extern int getc_unlocked (FILE *__stream);
extern int getchar_unlocked (void);
extern int fgetc_unlocked (FILE *__stream);
extern int fputc (int __c, FILE *__stream);
extern int putc (int __c, FILE *__stream);
extern int putchar (int __c);
extern int fputc_unlocked (int __c, FILE *__stream);
extern int putc_unlocked (int __c, FILE *__stream);
extern int putchar_unlocked (int __c);
extern int getw (FILE *__stream);
extern int putw (int __w, FILE *__stream);
extern char *fgets (char *__restrict __s, int __n, FILE *__restrict __stream)
     ;
extern __ssize_t __getdelim (char **__restrict __lineptr,
                             size_t *__restrict __n, int __delimiter,
                             FILE *__restrict __stream) ;
extern __ssize_t getdelim (char **__restrict __lineptr,
                           size_t *__restrict __n, int __delimiter,
                           FILE *__restrict __stream) ;
extern __ssize_t getline (char **__restrict __lineptr,
                          size_t *__restrict __n,
                          FILE *__restrict __stream) ;
extern int fputs (const char *__restrict __s, FILE *__restrict __stream);
extern int puts (const char *__s);
extern int ungetc (int __c, FILE *__stream);
extern size_t fread (void *__restrict __ptr, size_t __size,
       size_t __n, FILE *__restrict __stream) ;
extern size_t fwrite (const void *__restrict __ptr, size_t __size,
        size_t __n, FILE *__restrict __s);
extern size_t fread_unlocked (void *__restrict __ptr, size_t __size,
         size_t __n, FILE *__restrict __stream) ;
extern size_t fwrite_unlocked (const void *__restrict __ptr, size_t __size,
          size_t __n, FILE *__restrict __stream);
extern int fseek (FILE *__stream, long int __off, int __whence);
extern long int ftell (FILE *__stream) ;
extern void rewind (FILE *__stream);
extern int fseeko (FILE *__stream, __off_t __off, int __whence);
extern __off_t ftello (FILE *__stream) ;
extern int fgetpos (FILE *__restrict __stream, fpos_t *__restrict __pos);
extern int fsetpos (FILE *__stream, const fpos_t *__pos);
extern void clearerr (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__));
extern int feof (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__)) ;
extern int ferror (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__)) ;
extern void clearerr_unlocked (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__));
extern int feof_unlocked (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__)) ;
extern int ferror_unlocked (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__)) ;
extern void perror (const char *__s);
extern int sys_nerr;
extern const char *const sys_errlist[];
extern int fileno (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__)) ;
extern int fileno_unlocked (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__)) ;
extern FILE *popen (const char *__command, const char *__modes) ;
extern int pclose (FILE *__stream);

extern char *ctermid (char *__s) __attribute__ ((__nothrow__ , __leaf__));
extern void flockfile (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__));
extern int ftrylockfile (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__)) ;
extern void funlockfile (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__));
extern int __uflow (FILE *);
extern int __overflow (FILE *, int);

int main(int argc, char *argv[]) {
 printf("%s","Vay basima gelenler\n");
 return 0; }
 ```
 ---
# stdio.h headerının bütün girdilerini, global variable ve function prototypelarının kaynak koda kopyalandığını görebiliriz. 
# Compilation 
---
# Preprocessing işlemi tamamlandıktan sonra kaynak kod derlenmeye hazır hale getirilir. Bu işlem preprocessing işleminenden geçen kaynak kodun, assembly diline çevrilmesi ile sonuçlanır. Bilinen derleyiciler optimization level ile bu işlemin derinliği değiştirebilir. Bu işlem, disassemble sonucunu etkiler.
---
```zsh
$ gcc -S -masm=intel compilation.c 
$ file compilation.s 
compilation.s: assembler source, ASCII text
$ cat compilation.s
```
```nasm
  .file  "compilation.c"
  .intel_syntax noprefix
  .text
  .section  .rodata
.LC0:
  .string  "Vay basima gelenler"
  .text
  .globl  main
  .type  main, @function
main:
.LFB0:
  .cfi_startproc
  endbr64
  push  rbp
  .cfi_def_cfa_offset 16
  .cfi_offset 6, -16
  mov  rbp, rsp
  .cfi_def_cfa_register 6
  sub  rsp, 16
  mov  DWORD PTR -4[rbp], edi
  mov  QWORD PTR -16[rbp], rsi
  lea  rdi, .LC0[rip]
  call  puts@PLT
  mov  eax, 0
  leave
  .cfi_def_cfa 7, 8
  ret
  .cfi_endproc
.LFE0:
  .size  main, .-main
  .ident  "GCC: (Ubuntu 9.2.1-9ubuntu2) 9.2.1 20191008"
  .section  .note.GNU-stack,"",@progbits
  .section  .note.gnu.property,"a"
  .align 8
  .long   1f - 0f
  .long   4f - 1f
  .long   5
0:
  .string   "GNU"
1:
  .align 8
  .long   0xc0000002
  .long   3f - 2f
2:
  .long   0x3
3:
  .align 8
4:
```
---
# Burda gördüğümüz şey bizim c kaynak kodunun içeriğinin, assembly koduna çevrilmiş halidir. Bunu gösteriyorum çünkü dikkat edilmesi gereken nokta; Assembly kodunu okuması kolay. Çünkü semboller ve fonksiyonlar korunmuştur. Örneğin; değişkenler ve constantlar adreslerden ziyade bir isme de sahiptirler. Çıktıda görüldüğü gibi LC0 dan bahsediyorum. LC0 gibi rastgele oluşturulan bir isim de buna dahildir.
# Buradaki ayrı bir dava ise, kaynak kodda printf yazıyordu ancak burada puts ile karşılaşıyoruz. Bunun sebebi derleyicinin (gcc) printf yerine puts u kullanmasıdır.
---
# Assembly 
---
# Bu işlemde assembly e çevrilmiş olan program, makine koduna çevrilip işlemci tarafından okunacak hale getirilecektir. Bu işlem object dosyaları ile sonuçlanır. (.o)
---
```zsh
$ gcc -c compilation.c
$ file compilation.o 
compilation.o: ELF 64-bit LSB relocatable, x86-64, version 1 (SYSV), not stripped
```
---
# LSB; Belleğe en az anlamlı byte dan itibaren yazılacağını belirler.
# relocatable; Program içerisindeki herhangi bir yere yerleşebilirler. Object dosyalar çalıştırılamayan dosya türüdür. Ayrıca bu dosya  türü içerisinde kod ve veri barındırır. Bu dosyalar paylaşılan veya paylaşılmayan nesneler olarak da bilinir. Bir ELF çalıştırılabilir dosyasının ihtiyaç duyduğu kütüphaneleri barındırır.
## Ancak relocatable çalıştırılabilir dosyalar da mevcuttur. Ama bunlar relocatable dosyalar yerine shared dosyalar olarak gösterilir. Bunlar sıradan kütüphane dosyalarından farklıdır. Çünkü bu dosyaların bir entry point adresi vardır.
---
# Linking
---
# Linking aşaması son aşamadır. Dosya işlemci tarafından okunup çalıştırılabilecek formata getirilir. İhtiyaç duyulan materyaller object (.o) ve library (.a) dosyalarıdır.
# Tüm bu dosyalar executable dosyanın içerisinde birleştirilir. Modern sistemlerde Link-Time-Optimization ile ek bir optimizasyona ihtiyaç duyulabilir. 
# Linking aşamasından önce kod veya datanın yerleşeceği adres belli değildir, bu nedenle object dosyalar içerisinde bulundurduğu datanın (örneğin bir fonksiyon) nasıl çözüleceğini belirten semboller barındırır.  Bunlara symbolic references denir. Linkerin işi programa ait tüm object dosyalarını almak ve bunları tipik olarak belirli bir hafıza adresinde (entry point) yüklenmesi amaçlanan executable dosyası haline getirmektir. 
# Bir diğer konu ise kütüphanelere yapılan referanslar (call) kütüphanenin türüne bağlı olarak tamamen çözülebilir veya çözülmeyebilir. Yani Statik kütüphaneler (.a)  executable dosyada birleştirilir. Bu sayede kütüphaneye yapılan herhangi bir referansın tamamen çözülmesini sağlar. Bir sistemde çalışan tüm prgramlar arasında bellekte paylaşılan dinamik kütüphaneler vardır. Yani kütüphaneyi onu kullanan her bir uygulamanın (executable) içine kopyalamak yerine, kütüphane belleğe bir defa yüklenir. Kütüphaneyi kullanacak tüm uygulamalar bu bellekteki kopyayı kullanmak zorundadır. Linking aşamasında dinamik kütüphanelerin bulunduğu adresler bilinmemektedir. Bu nedenle yapılan referanslar çözümlenememektedir. Bunun yerine Linker, executable dosyada bu symbolic referenceları bırakır ve bu executable dosya belleğe yüklenene kadar çözümlenmez.
---
```zsh
$ gcc compilation.c -o compilation

$ file compilation
compilation: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=220b48247f049c5946337c8d1282b2104c7c91b0, for GNU/Linux 3.2.0, not stripped
```
# Dynamically Linked: Yani executable dosya içerisinde dinamik kütüphane barındırıyor ve çalıştırılana kadar kütüphanelerin adresleri çözümlenemiyor.
# interpreter /lib64/ld-linux-x76-64.so.2: Linux için interpreter ld-linux*.so.* olarak gösterilir.  Kısacası Linker bu.
# Not Stripped: Dosya Strip edilmemiş. Yani içerisindeki sectionlar dümdüz okunabilir halde. İlerde anlatcam.
---
# Symbolic Bilgi
---
```zsh
$ readelf --syms compilation
```
```nasm
Symbol table '.dynsym' contains 7 entries:
   Num:    Value          Size Type    Bind   Vis      Ndx Name
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND 
     1: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_deregisterTMCloneTab
     2: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND puts@GLIBC_2.2.5 (2)
     3: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND __libc_start_main@GLIBC_2.2.5 (2)
     4: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__
     5: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_registerTMCloneTable
     6: 0000000000000000     0 FUNC    WEAK   DEFAULT  UND __cxa_finalize@GLIBC_2.2.5 (2)

Symbol table '.symtab' contains 65 entries:
   Num:    Value          Size Type    Bind   Vis      Ndx Name
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND 
     1: 0000000000000318     0 SECTION LOCAL  DEFAULT    1 
     2: 0000000000000338     0 SECTION LOCAL  DEFAULT    2 
     3: 0000000000000358     0 SECTION LOCAL  DEFAULT    3 
     4: 000000000000037c     0 SECTION LOCAL  DEFAULT    4 
     5: 00000000000003a0     0 SECTION LOCAL  DEFAULT    5 
     6: 00000000000003c8     0 SECTION LOCAL  DEFAULT    6 
     7: 0000000000000470     0 SECTION LOCAL  DEFAULT    7 
     8: 00000000000004f2     0 SECTION LOCAL  DEFAULT    8 
     9: 0000000000000500     0 SECTION LOCAL  DEFAULT    9 
    10: 0000000000000520     0 SECTION LOCAL  DEFAULT   10 
    11: 00000000000005e0     0 SECTION LOCAL  DEFAULT   11 
    12: 0000000000001000     0 SECTION LOCAL  DEFAULT   12 
    13: 0000000000001020     0 SECTION LOCAL  DEFAULT   13 
    14: 0000000000001040     0 SECTION LOCAL  DEFAULT   14 
    15: 0000000000001050     0 SECTION LOCAL  DEFAULT   15 
    16: 0000000000001060     0 SECTION LOCAL  DEFAULT   16 
    17: 00000000000011e8     0 SECTION LOCAL  DEFAULT   17 
    18: 0000000000002000     0 SECTION LOCAL  DEFAULT   18 
    19: 0000000000002018     0 SECTION LOCAL  DEFAULT   19 
    20: 0000000000002060     0 SECTION LOCAL  DEFAULT   20 
    21: 0000000000003db8     0 SECTION LOCAL  DEFAULT   21 
    22: 0000000000003dc0     0 SECTION LOCAL  DEFAULT   22 
    23: 0000000000003dc8     0 SECTION LOCAL  DEFAULT   23 
    24: 0000000000003fb8     0 SECTION LOCAL  DEFAULT   24 
    25: 0000000000004000     0 SECTION LOCAL  DEFAULT   25 
    26: 0000000000004010     0 SECTION LOCAL  DEFAULT   26 
    27: 0000000000000000     0 SECTION LOCAL  DEFAULT   27 
    28: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS crtstuff.c
    29: 0000000000001090     0 FUNC    LOCAL  DEFAULT   16 deregister_tm_clones
    30: 00000000000010c0     0 FUNC    LOCAL  DEFAULT   16 register_tm_clones
    31: 0000000000001100     0 FUNC    LOCAL  DEFAULT   16 __do_global_dtors_aux
    32: 0000000000004010     1 OBJECT  LOCAL  DEFAULT   26 completed.8055
    33: 0000000000003dc0     0 OBJECT  LOCAL  DEFAULT   22 __do_global_dtors_aux_fin
    34: 0000000000001140     0 FUNC    LOCAL  DEFAULT   16 frame_dummy
    35: 0000000000003db8     0 OBJECT  LOCAL  DEFAULT   21 __frame_dummy_init_array_
    36: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS compilation.c
    37: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS crtstuff.c
    38: 0000000000002164     0 OBJECT  LOCAL  DEFAULT   20 __FRAME_END__
    39: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS 
    40: 0000000000003dc0     0 NOTYPE  LOCAL  DEFAULT   21 __init_array_end
    41: 0000000000003dc8     0 OBJECT  LOCAL  DEFAULT   23 _DYNAMIC
    42: 0000000000003db8     0 NOTYPE  LOCAL  DEFAULT   21 __init_array_start
    43: 0000000000002018     0 NOTYPE  LOCAL  DEFAULT   19 __GNU_EH_FRAME_HDR
    44: 0000000000003fb8     0 OBJECT  LOCAL  DEFAULT   24 _GLOBAL_OFFSET_TABLE_

    45: 0000000000001000     0 FUNC    LOCAL  DEFAULT   12 _init
    46: 00000000000011e0     5 FUNC    GLOBAL DEFAULT   16 __libc_csu_fini
    47: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_deregisterTMCloneTab
    48: 0000000000004000     0 NOTYPE  WEAK   DEFAULT   25 data_start
    49: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND puts@@GLIBC_2.2.5
    50: 0000000000004010     0 NOTYPE  GLOBAL DEFAULT   25 _edata
    51: 00000000000011e8     0 FUNC    GLOBAL HIDDEN    17 _fini
    52: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND __libc_start_main@@GLIBC_
    53: 0000000000004000     0 NOTYPE  GLOBAL DEFAULT   25 __data_start
    54: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__
    55: 0000000000004008     0 OBJECT  GLOBAL HIDDEN    25 __dso_handle
    56: 0000000000002000     4 OBJECT  GLOBAL DEFAULT   18 _IO_stdin_used
    57: 0000000000001170   101 FUNC    GLOBAL DEFAULT   16 __libc_csu_init
    58: 0000000000004018     0 NOTYPE  GLOBAL DEFAULT   26 _end
    59: 0000000000001060    47 FUNC    GLOBAL DEFAULT   16 _start
    60: 0000000000004010     0 NOTYPE  GLOBAL DEFAULT   26 __bss_start
    61: 0000000000001149    38 FUNC    GLOBAL DEFAULT   16 main
    62: 0000000000004010     0 OBJECT  GLOBAL HIDDEN    25 __TMC_END__
    63: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_registerTMCloneTable
    64: 0000000000000000     0 FUNC    WEAK   DEFAULT  UND __cxa_finalize@@GLIBC_2.2
```
# Mainin adresini çözümlemeye çalışmış, ancak bu adres henüz doğru değil.   
# 61: 0000000000001149    38 FUNC    GLOBAL DEFAULT   16 main

# Mainin Sizeını 38 byte olarak çözümlemiş ve bir FUNC olduğunu tespit etmiş.
# Sembolik bilgiler executable dosyanın bir parçasıdır. Linker yalnızca temel sembollere ihtiyacı vardır ancak debugging için diğer sembolleri de kullanarak detaylı bilgi verir. ELF Dosyaları için debugging semboller tipik olarak DWARF biçiminde oluşturulur. PE dosyaları için ise (windows) PDB biçimi kullanılır. Farkları ise DWARF bilgileri executable dosyanın içine gömerken, PDB ayrı bir sembol dosyası şeklinde gelir.
---
# Stripped Binary
---
```zsh
$ strip --strip-all compilation
$ file compilation
compilation: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=220b48247f049c5946337c8d1282b2104c7c91b0, for GNU/Linux 3.2.0, stripped
$ readelf --syms compilation
Symbol table '.dynsym' contains 7 entries:
```
```nasm
   Num:    Value          Size Type    Bind   Vis      Ndx Name
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND 
     1: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_deregisterTMCloneTab
     2: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND puts@GLIBC_2.2.5 (2)
     3: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND __libc_start_main@GLIBC_2.2.5 (2)
     4: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__
     5: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_registerTMCloneTable
     6: 0000000000000000     0 FUNC    WEAK   DEFAULT  UND __cxa_finalize@GLIBC_2.2.5 (2)
```
		
---
# Binarynin stripped olduğunu gördük. Tüm semboller gitti geriye yalnızca .dynsym sembolü kaldı. 
# Aslında hiç bir şeyin hiç bir yere gittiği yok. Hepsi runtime esnasında linker tarafından çözülerek işleme alınacak. İlerde anlatcam.
---
# Disassemble Object File
---
```zsh
$ objdump -sj .rodata compilation.o

compilation.o:     file format elf64-x86-64

Contents of section .rodata:
```
```nasm
 0000 56617920 62617369 6d612067 656c656e  Vay basima gelen
 0010 6c657200                             ler.            
 ```
 ```zsh
$ objdump -d compilation.o -M intel

compilation.o:     file format elf64-x86-64


Disassembly of section .text:
```

```nasm
0000000000000000 <main>:
   0:   55                      push   rbp
   1:   48 89 e5                mov    rbp,rsp
   4:   48 83 ec 10             sub    rsp,0x10
   8:   89 7d fc                mov    DWORD PTR [rbp-0x4],edi
   b:   48 89 75 f0             mov    QWORD PTR [rbp-0x10],rsi
   f:   48 8d 3d 00 00 00 00    lea    rdi,[rip+0x0]        # 16 <main+0x16>
  16:   e8 00 00 00 00          call   1b <main+0x1b>
  1b:   b8 00 00 00 00          mov    eax,0x0
  20:   c9                      leave  
  21:   c3                      ret 
```
---
# İlk objdump çıktısında rodata; yani elf dosyasının içerisindeki read only data sectionuna baktık. C kodunda tanımladığımız string değeri ASCII encoding ile görüntülenmiş ve okunması gayet kolay.  

# İkinci objdump çıktısında tüm main fonksiyonunun içeriğini intel syntaxınde  disassemble ettik. İlginç olan şey, "Vay basima gelenler" stringi 0 a set edilmiş. Bir sonraki çağrı "Call" ekrana puts ile stringi bastıracaktır.
---
# Disassemble Executable 
---
```zsh
$ objdump -d compilation -M intel
```
```nasm
compilation:     file format elf64-x86-64


Disassembly of section .init:

0000000000001000 <_init>:
    1000:	48 83 ec 08          	sub    rsp,0x8
    1004:	48 8b 05 dd 2f 00 00 	mov    rax,QWORD PTR [rip+0x2fdd]        # 3fe8 <__gmon_start__>
    100b:	48 85 c0             	test   rax,rax
    100e:	74 02                	je     1012 <_init+0x12>
    1010:	ff d0                	call   rax
    1012:	48 83 c4 08          	add    rsp,0x8
    1016:	c3                   	ret    

Disassembly of section .plt:

0000000000001020 <.plt>:
    1020:	ff 35 e2 2f 00 00    	push   QWORD PTR [rip+0x2fe2]        # 4008 <_GLOBAL_OFFSET_TABLE_+0x8>
    1026:	ff 25 e4 2f 00 00    	jmp    QWORD PTR [rip+0x2fe4]        # 4010 <_GLOBAL_OFFSET_TABLE_+0x10>
    102c:	0f 1f 40 00          	nop    DWORD PTR [rax+0x0]

0000000000001030 <puts@plt>:
    1030:	ff 25 e2 2f 00 00    	jmp    QWORD PTR [rip+0x2fe2]        # 4018 <puts@GLIBC_2.2.5>
    1036:	68 00 00 00 00       	push   0x0
    103b:	e9 e0 ff ff ff       	jmp    1020 <.plt>

Disassembly of section .plt.got:

0000000000001040 <__cxa_finalize@plt>:
    1040:	ff 25 b2 2f 00 00    	jmp    QWORD PTR [rip+0x2fb2]        # 3ff8 <__cxa_finalize@GLIBC_2.2.5>
    1046:	66 90                	xchg   ax,ax

Disassembly of section .text:

0000000000001050 <_start>:
    1050:	31 ed                	xor    ebp,ebp
    1052:	49 89 d1             	mov    r9,rdx
    1055:	5e                   	pop    rsi
    1056:	48 89 e2             	mov    rdx,rsp
    1059:	48 83 e4 f0          	and    rsp,0xfffffffffffffff0
    105d:	50                   	push   rax
    105e:	54                   	push   rsp
    105f:	4c 8d 05 5a 01 00 00 	lea    r8,[rip+0x15a]        # 11c0 <__libc_csu_fini>
    1066:	48 8d 0d f3 00 00 00 	lea    rcx,[rip+0xf3]        # 1160 <__libc_csu_init>
    106d:	48 8d 3d c1 00 00 00 	lea    rdi,[rip+0xc1]        # 1135 <main>
    1074:	ff 15 66 2f 00 00    	call   QWORD PTR [rip+0x2f66]        # 3fe0 <__libc_start_main@GLIBC_2.2.5>
    107a:	f4                   	hlt    
    107b:	0f 1f 44 00 00       	nop    DWORD PTR [rax+rax*1+0x0]

0000000000001080 <deregister_tm_clones>:
    1080:	48 8d 3d a9 2f 00 00 	lea    rdi,[rip+0x2fa9]        # 4030 <__TMC_END__>
    1087:	48 8d 05 a2 2f 00 00 	lea    rax,[rip+0x2fa2]        # 4030 <__TMC_END__>
    108e:	48 39 f8             	cmp    rax,rdi
    1091:	74 15                	je     10a8 <deregister_tm_clones+0x28>
    1093:	48 8b 05 3e 2f 00 00 	mov    rax,QWORD PTR [rip+0x2f3e]        # 3fd8 <_ITM_deregisterTMCloneTable>
    109a:	48 85 c0             	test   rax,rax
    109d:	74 09                	je     10a8 <deregister_tm_clones+0x28>
    109f:	ff e0                	jmp    rax
    10a1:	0f 1f 80 00 00 00 00 	nop    DWORD PTR [rax+0x0]
    10a8:	c3                   	ret    
    10a9:	0f 1f 80 00 00 00 00 	nop    DWORD PTR [rax+0x0]

00000000000010b0 <register_tm_clones>:
    10b0:	48 8d 3d 79 2f 00 00 	lea    rdi,[rip+0x2f79]        # 4030 <__TMC_END__>
    10b7:	48 8d 35 72 2f 00 00 	lea    rsi,[rip+0x2f72]        # 4030 <__TMC_END__>
    10be:	48 29 fe             	sub    rsi,rdi
    10c1:	48 89 f0             	mov    rax,rsi
    10c4:	48 c1 ee 3f          	shr    rsi,0x3f
    10c8:	48 c1 f8 03          	sar    rax,0x3
    10cc:	48 01 c6             	add    rsi,rax
    10cf:	48 d1 fe             	sar    rsi,1
    10d2:	74 14                	je     10e8 <register_tm_clones+0x38>
    10d4:	48 8b 05 15 2f 00 00 	mov    rax,QWORD PTR [rip+0x2f15]        # 3ff0 <_ITM_registerTMCloneTable>
    10db:	48 85 c0             	test   rax,rax
    10de:	74 08                	je     10e8 <register_tm_clones+0x38>
    10e0:	ff e0                	jmp    rax
    10e2:	66 0f 1f 44 00 00    	nop    WORD PTR [rax+rax*1+0x0]
    10e8:	c3                   	ret    
    10e9:	0f 1f 80 00 00 00 00 	nop    DWORD PTR [rax+0x0]

00000000000010f0 <__do_global_dtors_aux>:
    10f0:	80 3d 39 2f 00 00 00 	cmp    BYTE PTR [rip+0x2f39],0x0        # 4030 <__TMC_END__>
    10f7:	75 2f                	jne    1128 <__do_global_dtors_aux+0x38>
    10f9:	55                   	push   rbp
    10fa:	48 83 3d f6 2e 00 00 	cmp    QWORD PTR [rip+0x2ef6],0x0        # 3ff8 <__cxa_finalize@GLIBC_2.2.5>
    1101:	00 
    1102:	48 89 e5             	mov    rbp,rsp
    1105:	74 0c                	je     1113 <__do_global_dtors_aux+0x23>
    1107:	48 8b 3d 1a 2f 00 00 	mov    rdi,QWORD PTR [rip+0x2f1a]        # 4028 <__dso_handle>
    110e:	e8 2d ff ff ff       	call   1040 <__cxa_finalize@plt>
    1113:	e8 68 ff ff ff       	call   1080 <deregister_tm_clones>
    1118:	c6 05 11 2f 00 00 01 	mov    BYTE PTR [rip+0x2f11],0x1        # 4030 <__TMC_END__>
    111f:	5d                   	pop    rbp
    1120:	c3                   	ret    
    1121:	0f 1f 80 00 00 00 00 	nop    DWORD PTR [rax+0x0]
    1128:	c3                   	ret    
    1129:	0f 1f 80 00 00 00 00 	nop    DWORD PTR [rax+0x0]

0000000000001130 <frame_dummy>:
    1130:	e9 7b ff ff ff       	jmp    10b0 <register_tm_clones>

0000000000001135 <main>:
    1135:	55                   	push   rbp
    1136:	48 89 e5             	mov    rbp,rsp
    1139:	48 83 ec 10          	sub    rsp,0x10
    113d:	89 7d fc             	mov    DWORD PTR [rbp-0x4],edi
    1140:	48 89 75 f0          	mov    QWORD PTR [rbp-0x10],rsi
    1144:	48 8d 3d b9 0e 00 00 	lea    rdi,[rip+0xeb9]        # 2004 <_IO_stdin_used+0x4>
    114b:	e8 e0 fe ff ff       	call   1030 <puts@plt>
    1150:	b8 00 00 00 00       	mov    eax,0x0
    1155:	c9                   	leave  
    1156:	c3                   	ret    
    1157:	66 0f 1f 84 00 00 00 	nop    WORD PTR [rax+rax*1+0x0]
    115e:	00 00 

0000000000001160 <__libc_csu_init>:
    1160:	41 57                	push   r15
    1162:	4c 8d 3d 7f 2c 00 00 	lea    r15,[rip+0x2c7f]        # 3de8 <__frame_dummy_init_array_entry>
    1169:	41 56                	push   r14
    116b:	49 89 d6             	mov    r14,rdx
    116e:	41 55                	push   r13
    1170:	49 89 f5             	mov    r13,rsi
    1173:	41 54                	push   r12
    1175:	41 89 fc             	mov    r12d,edi
    1178:	55                   	push   rbp
    1179:	48 8d 2d 70 2c 00 00 	lea    rbp,[rip+0x2c70]        # 3df0 <__init_array_end>
    1180:	53                   	push   rbx
    1181:	4c 29 fd             	sub    rbp,r15
    1184:	48 83 ec 08          	sub    rsp,0x8
    1188:	e8 73 fe ff ff       	call   1000 <_init>
    118d:	48 c1 fd 03          	sar    rbp,0x3
    1191:	74 1b                	je     11ae <__libc_csu_init+0x4e>
    1193:	31 db                	xor    ebx,ebx
    1195:	0f 1f 00             	nop    DWORD PTR [rax]
    1198:	4c 89 f2             	mov    rdx,r14
    119b:	4c 89 ee             	mov    rsi,r13
    119e:	44 89 e7             	mov    edi,r12d
    11a1:	41 ff 14 df          	call   QWORD PTR [r15+rbx*8]
    11a5:	48 83 c3 01          	add    rbx,0x1
    11a9:	48 39 dd             	cmp    rbp,rbx
    11ac:	75 ea                	jne    1198 <__libc_csu_init+0x38>
    11ae:	48 83 c4 08          	add    rsp,0x8
    11b2:	5b                   	pop    rbx
    11b3:	5d                   	pop    rbp
    11b4:	41 5c                	pop    r12
    11b6:	41 5d                	pop    r13
    11b8:	41 5e                	pop    r14
    11ba:	41 5f                	pop    r15
    11bc:	c3                   	ret    
    11bd:	0f 1f 00             	nop    DWORD PTR [rax]

00000000000011c0 <__libc_csu_fini>:
    11c0:	c3                   	ret    

Disassembly of section .fini:

00000000000011c4 <_fini>:
    11c4:	48 83 ec 08          	sub    rsp,0x8
    11c8:	48 83 c4 08          	add    rsp,0x8
    11cc:	c3                   	ret    
```
---
# Yukarıdaki çıktı strip edilmemiş binary nin disassebmle sonucudur. Binary i strip halinden çıkartmak için tekrar gcc ile derlenebilir.
---
# Disassemble Stripped Binary
---
```zsh
$ strip --strip-all compilation-stripped 
$ file compilation-stripped 
compilation-stripped: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=0e2d48d6d32fad3e312d96eadcf199145a3417cc, for GNU/Linux 3.2.0, stripped
```
---
```zsh
$ objdump -d compilation-stripped -M intel 
```
```nasm
compilation-stripped:     file format elf64-x86-64


Disassembly of section .init:

0000000000001000 <.init>:
    1000:	48 83 ec 08          	sub    rsp,0x8
    1004:	48 8b 05 dd 2f 00 00 	mov    rax,QWORD PTR [rip+0x2fdd]        # 3fe8 <__cxa_finalize@plt+0x2fa8>
    100b:	48 85 c0             	test   rax,rax
    100e:	74 02                	je     1012 <puts@plt-0x1e>
    1010:	ff d0                	call   rax
    1012:	48 83 c4 08          	add    rsp,0x8
    1016:	c3                   	ret    

Disassembly of section .plt:

0000000000001020 <puts@plt-0x10>:
    1020:	ff 35 e2 2f 00 00    	push   QWORD PTR [rip+0x2fe2]        # 4008 <__cxa_finalize@plt+0x2fc8>
    1026:	ff 25 e4 2f 00 00    	jmp    QWORD PTR [rip+0x2fe4]        # 4010 <__cxa_finalize@plt+0x2fd0>
    102c:	0f 1f 40 00          	nop    DWORD PTR [rax+0x0]

0000000000001030 <puts@plt>:
    1030:	ff 25 e2 2f 00 00    	jmp    QWORD PTR [rip+0x2fe2]        # 4018 <__cxa_finalize@plt+0x2fd8>
    1036:	68 00 00 00 00       	push   0x0
    103b:	e9 e0 ff ff ff       	jmp    1020 <puts@plt-0x10>

Disassembly of section .plt.got:

0000000000001040 <__cxa_finalize@plt>:
    1040:	ff 25 b2 2f 00 00    	jmp    QWORD PTR [rip+0x2fb2]        # 3ff8 <__cxa_finalize@plt+0x2fb8>
    1046:	66 90                	xchg   ax,ax

Disassembly of section .text:

0000000000001050 <.text>:
    1050:	31 ed                	xor    ebp,ebp
    1052:	49 89 d1             	mov    r9,rdx
    1055:	5e                   	pop    rsi
    1056:	48 89 e2             	mov    rdx,rsp
    1059:	48 83 e4 f0          	and    rsp,0xfffffffffffffff0
    105d:	50                   	push   rax
    105e:	54                   	push   rsp
    105f:	4c 8d 05 5a 01 00 00 	lea    r8,[rip+0x15a]        # 11c0 <__cxa_finalize@plt+0x180>
    1066:	48 8d 0d f3 00 00 00 	lea    rcx,[rip+0xf3]        # 1160 <__cxa_finalize@plt+0x120>
    106d:	48 8d 3d c1 00 00 00 	lea    rdi,[rip+0xc1]        # 1135 <__cxa_finalize@plt+0xf5>
    1074:	ff 15 66 2f 00 00    	call   QWORD PTR [rip+0x2f66]        # 3fe0 <__cxa_finalize@plt+0x2fa0>
    107a:	f4                   	hlt    
    107b:	0f 1f 44 00 00       	nop    DWORD PTR [rax+rax*1+0x0]
    1080:	48 8d 3d a9 2f 00 00 	lea    rdi,[rip+0x2fa9]        # 4030 <__cxa_finalize@plt+0x2ff0>
    1087:	48 8d 05 a2 2f 00 00 	lea    rax,[rip+0x2fa2]        # 4030 <__cxa_finalize@plt+0x2ff0>
    108e:	48 39 f8             	cmp    rax,rdi
    1091:	74 15                	je     10a8 <__cxa_finalize@plt+0x68>
    1093:	48 8b 05 3e 2f 00 00 	mov    rax,QWORD PTR [rip+0x2f3e]        # 3fd8 <__cxa_finalize@plt+0x2f98>
    109a:	48 85 c0             	test   rax,rax
    109d:	74 09                	je     10a8 <__cxa_finalize@plt+0x68>
    109f:	ff e0                	jmp    rax
    10a1:	0f 1f 80 00 00 00 00 	nop    DWORD PTR [rax+0x0]
    10a8:	c3                   	ret    
    10a9:	0f 1f 80 00 00 00 00 	nop    DWORD PTR [rax+0x0]
    10b0:	48 8d 3d 79 2f 00 00 	lea    rdi,[rip+0x2f79]        # 4030 <__cxa_finalize@plt+0x2ff0>
    10b7:	48 8d 35 72 2f 00 00 	lea    rsi,[rip+0x2f72]        # 4030 <__cxa_finalize@plt+0x2ff0>
    10be:	48 29 fe             	sub    rsi,rdi
    10c1:	48 89 f0             	mov    rax,rsi
    10c4:	48 c1 ee 3f          	shr    rsi,0x3f
    10c8:	48 c1 f8 03          	sar    rax,0x3
    10cc:	48 01 c6             	add    rsi,rax
    10cf:	48 d1 fe             	sar    rsi,1
    10d2:	74 14                	je     10e8 <__cxa_finalize@plt+0xa8>
    10d4:	48 8b 05 15 2f 00 00 	mov    rax,QWORD PTR [rip+0x2f15]        # 3ff0 <__cxa_finalize@plt+0x2fb0>
    10db:	48 85 c0             	test   rax,rax
    10de:	74 08                	je     10e8 <__cxa_finalize@plt+0xa8>
    10e0:	ff e0                	jmp    rax
    10e2:	66 0f 1f 44 00 00    	nop    WORD PTR [rax+rax*1+0x0]
    10e8:	c3                   	ret    
    10e9:	0f 1f 80 00 00 00 00 	nop    DWORD PTR [rax+0x0]
    10f0:	80 3d 39 2f 00 00 00 	cmp    BYTE PTR [rip+0x2f39],0x0        # 4030 <__cxa_finalize@plt+0x2ff0>
    10f7:	75 2f                	jne    1128 <__cxa_finalize@plt+0xe8>
    10f9:	55                   	push   rbp
    10fa:	48 83 3d f6 2e 00 00 	cmp    QWORD PTR [rip+0x2ef6],0x0        # 3ff8 <__cxa_finalize@plt+0x2fb8>
    1101:	00 
    1102:	48 89 e5             	mov    rbp,rsp
    1105:	74 0c                	je     1113 <__cxa_finalize@plt+0xd3>
    1107:	48 8b 3d 1a 2f 00 00 	mov    rdi,QWORD PTR [rip+0x2f1a]        # 4028 <__cxa_finalize@plt+0x2fe8>
    110e:	e8 2d ff ff ff       	call   1040 <__cxa_finalize@plt>
    1113:	e8 68 ff ff ff       	call   1080 <__cxa_finalize@plt+0x40>
    1118:	c6 05 11 2f 00 00 01 	mov    BYTE PTR [rip+0x2f11],0x1        # 4030 <__cxa_finalize@plt+0x2ff0>
    111f:	5d                   	pop    rbp
    1120:	c3                   	ret    
    1121:	0f 1f 80 00 00 00 00 	nop    DWORD PTR [rax+0x0]
    1128:	c3                   	ret    
    1129:	0f 1f 80 00 00 00 00 	nop    DWORD PTR [rax+0x0]
    1130:	e9 7b ff ff ff       	jmp    10b0 <__cxa_finalize@plt+0x70>
    1135:	55                   	push   rbp
    1136:	48 89 e5             	mov    rbp,rsp
    1139:	48 83 ec 10          	sub    rsp,0x10
    113d:	89 7d fc             	mov    DWORD PTR [rbp-0x4],edi
    1140:	48 89 75 f0          	mov    QWORD PTR [rbp-0x10],rsi
    1144:	48 8d 3d b9 0e 00 00 	lea    rdi,[rip+0xeb9]        # 2004 <__cxa_finalize@plt+0xfc4>
    114b:	e8 e0 fe ff ff       	call   1030 <puts@plt>
    1150:	b8 00 00 00 00       	mov    eax,0x0
    1155:	c9                   	leave  
    1156:	c3                   	ret    
    1157:	66 0f 1f 84 00 00 00 	nop    WORD PTR [rax+rax*1+0x0]
    115e:	00 00 
    1160:	41 57                	push   r15
    1162:	4c 8d 3d 7f 2c 00 00 	lea    r15,[rip+0x2c7f]        # 3de8 <__cxa_finalize@plt+0x2da8>
    1169:	41 56                	push   r14
    116b:	49 89 d6             	mov    r14,rdx
    116e:	41 55                	push   r13
    1170:	49 89 f5             	mov    r13,rsi
    1173:	41 54                	push   r12
    1175:	41 89 fc             	mov    r12d,edi
    1178:	55                   	push   rbp
    1179:	48 8d 2d 70 2c 00 00 	lea    rbp,[rip+0x2c70]        # 3df0 <__cxa_finalize@plt+0x2db0>
    1180:	53                   	push   rbx
    1181:	4c 29 fd             	sub    rbp,r15
    1184:	48 83 ec 08          	sub    rsp,0x8
    1188:	e8 73 fe ff ff       	call   1000 <puts@plt-0x30>
    118d:	48 c1 fd 03          	sar    rbp,0x3
    1191:	74 1b                	je     11ae <__cxa_finalize@plt+0x16e>
    1193:	31 db                	xor    ebx,ebx
    1195:	0f 1f 00             	nop    DWORD PTR [rax]
    1198:	4c 89 f2             	mov    rdx,r14
    119b:	4c 89 ee             	mov    rsi,r13
    119e:	44 89 e7             	mov    edi,r12d
    11a1:	41 ff 14 df          	call   QWORD PTR [r15+rbx*8]
    11a5:	48 83 c3 01          	add    rbx,0x1
    11a9:	48 39 dd             	cmp    rbp,rbx
    11ac:	75 ea                	jne    1198 <__cxa_finalize@plt+0x158>
    11ae:	48 83 c4 08          	add    rsp,0x8
    11b2:	5b                   	pop    rbx
    11b3:	5d                   	pop    rbp
    11b4:	41 5c                	pop    r12
    11b6:	41 5d                	pop    r13
    11b8:	41 5e                	pop    r14
    11ba:	41 5f                	pop    r15
    11bc:	c3                   	ret    
    11bd:	0f 1f 00             	nop    DWORD PTR [rax]
    11c0:	c3                   	ret    

Disassembly of section .fini:

00000000000011c4 <.fini>:
    11c4:	48 83 ec 08          	sub    rsp,0x8
    11c8:	48 83 c4 08          	add    rsp,0x8
    11cc:	c3                   	ret    
```
---
# Aslında ilk bakışta fark bariz belli gibi, main fonksiyonu ortalarda yok.
# Main fonksiyonu .text sectionu altındadır. Yani şurada;
```nasm
    1135:	55                   	push   rbp
    1136:	48 89 e5             	mov    rbp,rsp
    1139:	48 83 ec 10          	sub    rsp,0x10
    113d:	89 7d fc             	mov    DWORD PTR [rbp-0x4],edi
    1140:	48 89 75 f0          	mov    QWORD PTR [rbp-0x10],rsi
    1144:	48 8d 3d b9 0e 00 00 	lea    rdi,[rip+0xeb9]        # 2004 <__cxa_finalize@plt+0xfc4>
    114b:	e8 e0 fe ff ff       	call   1030 <puts@plt>
    1150:	b8 00 00 00 00       	mov    eax,0x0
    1155:	c9                   	leave  
    1156:	c3                   	ret 
```
---
# Bir sonraki yazıda executable dosyanın memory durumları, elf ve pe dosyaları incelenecektir.

# Author <a href=" https://twitter.com/SectionText">Section Text</a> 
---
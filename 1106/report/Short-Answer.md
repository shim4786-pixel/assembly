# Short Answer 문제 풀이  

## 문제 1  
**Q : What will be the value of BX after the following instructions execute?**  
```asm
mov  bx,0FFFFh
and  bx,6Bh
```
**A :** BX = 006Bh  

---
## 문제 2  
**Q : What will be the value of BX after the following instructions execute?**  
```asm
mov  bx,91BAh
and  bx,92h
```
**A :** BX = 0092h   

---
## 문제 3  
**Q : What will be the value of BX after the following instructions execute?**  
```asm
mov  bx,0649Bh
or   bx,3Ah
```
**A :** BX = 64BBh  

---
## 문제 4  
**Q : What will be the value of BX after the following instructions execute?**  
```asm
mov  bx,029D6h
xor  bx,8181h
```
**A :** BX = A857h  

---
## 문제 5  
**Q : What will be the value of EBX after the following instructions execute?**  
```asm
mov  ebx,0AFAF649Bh
or   ebx,3A219604h
```
**A :** EBX = 0BFAFF69Fh  

---
## 문제 6  
**Q : What will be the value of RBX after the following instructions execute?**  
```asm
mov  rbx,0AFAF649Bh
xor  rbx,0FFFFFFFFh
```
**A :** RBX = 0000000050509B64h  

---
## 문제 7  
**Q : In the following instruction sequence, show the resulting value of AL where indicated, in binary:**  
```asm
mov  al,01101111b
and  al,00101101b        ; a.
mov  al,6Dh
and  al,4Ah              ; b.
mov  al,00001111b
or   al,61h              ; c.
mov  al,94h
xor  al,37h              ; d.
```
**A :**  
a. 00101101b  
b. 01001000b  
c. 01101111b  
d. 10100011b  

---
## 문제 8  
**Q : In the following instruction sequence, show the resulting value of AL where indicated, in hexadecimal:**  
```asm
mov  al,7Ah
not  al               ; a.
mov  al,3Dh
and  al,74h           ; b.
mov  al,9Bh
or   al,35h           ; c.
mov  al,72h
xor  al,0DCh          ; d.
```
**A :**  
a. 85h  
b. 34h  
c. 0BFh  
d. 0AEh  

---
## 문제 9  
**Q : In the following instruction sequence, show the values of the Carry, Zero, and Sign flags where indicated:**  
```asm
mov  al,00001111b
test al,00000010b       ; a. CF=   ZF=   SF=
mov  al,00000110b
cmp  al,00000101b       ; b. CF=   ZF=   SF=
mov  al,00000101b
cmp  al,00000111b       ; c. CF=   ZF=   SF=
```
**A :**  
a. CF = 0, ZF = 0, SF = 0  
b. CF = 0, ZF = 0, SF = 0  
c. CF = 1, ZF =0 , SF = 1  

---
## 문제 10  
**Q : Which conditional jump instruction executes a branch based on the contents of ECX?**  
**A :** LOOP, LOOPE, LOOPNE, LOOPZ, LOOPNZ  

---
## 문제 11  
**Q : How are JA and JNBE affected by the Zero and Carry flags?**  
**A :**

---
## 문제 12  
**Q : What will be the final value in EDX after this code executes?**  
```asm
    mov  edx,1
    mov  eax,7FFFh
    cmp  eax,8000h
    jl   L1
    mov  edx,0
L1:
```
**A :**

---
## 문제 13  
**Q : What will be the final value in EDX after this code executes?**  
```asm
    mov  edx,1
    mov  eax,7FFFh
    cmp  eax,8000h
    jb   L1
    mov  edx,0
L1:
```
**A :**

---
## 문제 14  
**Q : What will be the final value in EDX after this code executes?**  
```asm
    mov  edx,1
    mov  eax,7FFFh
    cmp  eax,0FFFF8000h
    jl   L2
    mov  edx,0
 L2:
```
**A :**

---
## 문제 15  
**Q : (True/False): The following code will jump to the label named Target.**  
```asm
mov  eax,-30
cmp  eax,-50
jg   Target
```
**A :**

---
## 문제 16  
**Q : (True/False): The following code will jump to the label named Target.**  
```asm
mov  eax,-42
cmp  eax,26
ja   Target
```
**A :**

---
## 문제 17  
**Q : What will be the value of RBX after the following instructions execute?**  
```asm
mov  rbx,0FFFFFFFFFFFFFFFFh
and  rbx,80h
```
**A :**

---
## 문제 18  
**Q : What will be the value of RBX after the following instructions execute?**  
```asm
mov  rbx,0FFFFFFFFFFFFFFFFh
and  rbx,808080h
```
**A :**

---
## 문제 19  
**Q : What will be the value of RBX after the following instructions execute?**  
```asm
mov  rbx,0FFFFFFFFFFFFFFFFh
and  rbx,80808080h
```
**A :**

---

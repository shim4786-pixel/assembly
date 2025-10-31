# Short Answer 문제 풀이

## 문제 1
**Q : Which instruction pushes all of the 32-bit general-purpose registers on the stack?**  
**A :**  

---
## 문제 2
**Q : Which instruction pushes the 32-bit EFLAGS register on the stack?**  
**A :**  

---
## 문제 3
**Q : Which instruction pops the stack into the EFLAGS register?**  
**A :**  

---
## 문제 4
**Q : Challenge: Another assembler (called NASM) permits the PUSH instruction to list multiple specific registers. Why might this approach be better than the PUSHAD instruction in MASM? Here is a NASM example:  
 PUSH EAX EBX ECX**  
**A :**  

---
## 문제 5
**Q : Challenge: Suppose there were no PUSH instruction. Write a sequence of two other instructions that would accomplish the same as push eax.**  
**A :**  

---
## 문제 6
**Q : (True/False): The RET instruction pops the top of the stack into the instruction pointer.**  
**A :**  

---
## 문제 7
**Q : (True/False): Nested procedure calls are not permitted by the Microsoft assembler unless the NESTED operator is used in the procedure definition.**  
**A :**  

---
## 문제 8
**Q : (True/False): In protected mode, each procedure call uses a minimum of 4 bytes of stack space.**  
**A :**  

---
## 문제 9
**Q : (True/False): The ESI and EDI registers cannot be used when passing 32-bit parameters to procedures.**  
**A :**  

---
## 문제 10
**Q : (True/False): The ArraySum procedure (Section 5.2.5) receives a pointer to any array of doublewords.**  
**A :**  

---
## 문제 11
**Q : (True/False): The USES operator lets you name all registers that are modified within a procedure.**  
**A :**  

---
## 문제 12
**Q : (True/False): The USES operator only generates PUSH instructions, so you must code POP instructions yourself.**  
**A :**  

---
## 문제 13
**Q : (True/False): The register list in the USES directive must use commas to separate the register names.**  
**A :**  

---
## 문제 14
**Q : Which statement(s) in the ArraySum procedure (Section 5.2.5) would have to be modified so it could accumulate an array of 16-bit words? Create such a version of ArraySum and test it.**  
**A :**  

---
## 문제 15
**Q : What will be the final value in EAX after these instructions execute?**  
```asm
push 5
push 6
pop  eax
pop  eax
```
**A :**  

---
## 문제 16
**Q : Which statement is true about what will happen when the example code runs?**  
```asm
 1: main PROC
 2:   push 10
 3:   push 20
 4:   call Ex2Sub
 5:   pop  eax
 6:   INVOKE ExitProcess,0
 7: main ENDP
 8:
 9: Ex2Sub PROC
10:   pop eax
11:   ret
12: Ex2Sub ENDP
```
a. EAX will equal 10 on line 6  
b. The program will halt with a runtime error on Line 10  
c. EAX will equal 20 on line 6  
d. The program will halt with a runtime error on Line 11   
**A :**  

---
## 문제 17
**Q : Which statement is true about what will happen when the example code runs?**  
```asm
 1: main PROC
 2:   mov  eax,30
 3:   push eax
 4:   push 40
 5:   call Ex3Sub
 6:   INVOKE ExitProcess,0
 7: main ENDP
 8:
 9: Ex3Sub PROC
10:   pusha
11:   mov eax,80
12:   popa
13:   ret
14: Ex3Sub ENDP
```
a. EAX will equal 40 on line 6  
b. The program will halt with a runtime error on Line 6  
c. EAX will equal 30 on line 6   
d. The program will halt with a runtime error on Line 13  
**A :**  

---
## 문제 18
**Q :Which statement is true about what will happen when the example code runs?**  
```asm
 1: main PROC
 2:   mov eax,40
 3:   push offset Here
 4:   jmp  Ex4Sub
 5: Here:
 6:   mov eax,30
 7:   INVOKE ExitProcess,0
 8: main ENDP
 9: 
10: Ex4Sub PROC
11:   ret
12: Ex4Sub ENDP
```
a. EAX will equal 30 on line 7  
b. The program will halt with a runtime error on Line 4  
c. EAX will equal 30 on line 6   
d. The program will halt with a runtime error on Line 11  
**A :**  

---
## 문제 19
**Q : Which statement is true about what will happen when the example code runs?**  
```asm
 1: main PROC
 2:   mov edx,0
 3:   mov eax,40
 4:   push eax
 5:   call Ex5Sub
 6:   INVOKE ExitProcess,0
 7: main ENDP
 8:
 9: Ex5Sub PROC
10:   pop  eax
11:   pop  edx
12:   push eax
13:   ret
14: Ex5Sub ENDP
```
a. EDX will equal 40 on line 6   
b. The program will halt with a runtime error on Line 13  
c. EDX will equal 0 on line 6   
d. The program will halt with a runtime error on Line 11  
**A :**  

---
## 문제 20
**Q : What values will be written to the array when the following code executes?**  
```asm
.data
array DWORD 4 DUP(0)
.code
main PROC
   mov eax,10
   mov  esi,0
   call proc_1
   add  esi,4
   add  eax,10
   mov  array[esi],eax
   INVOKE ExitProcess,0
main ENDP

proc_1 PROC
   call proc_2
   add  esi,4
   add  eax,10
   mov  array[esi],eax
   ret
proc_1 ENDP

proc_2 PROC
   call proc_3
   add  esi,4
   add  eax,10
   mov  array[esi],eax
   ret
proc_2 ENDP

proc_3 PROC
   mov  array[esi],eax
   ret
proc_3 ENDP
```
**A :**  

---

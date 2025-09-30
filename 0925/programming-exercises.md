# Programming Exercise 문제 풀이

## 문제 1 Integer Expression Calculation  
**Q: Using the AddTwo program from Section 3.2 as a reference, write a program that calculates the following expression, using registers: A = (A + B) − (C + D). Assign integer values to the EAX, EBX, ECX, and EDX registers.**    
**A:** AddTwo.asm 을 참고하여 A=(A+B)-(C-D) 코드를 작성한다.  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.code
main PROC
	mov eax, 20
	mov ebx, 10
	mov ecx, 5
	mov edx, 2

	add eax, ebx      ; eax = eax + ebx
	add ecx, edx      ; ecx = ecx + edx
	sub eax, ecx      ; eax = eax - ecx
	call DumpRegs
	exit
main ENDP

END main
```
출력값  
```asm
EAX=00000017  EBX=0000000A  ECX=00000007  EDX=00000002
  ESI=00DE10AA  EDI=00DE10AA  EBP=006FF960  ESP=006FF954
  EIP=00DE367B  EFL=00000206  CF=0  SF=0  ZF=0  OF=0  AF=0  PF=1
```
EAX에 정상적으로 23이 출력되었다.  

---
## 문제 2 Symbolic Integer Constants    
**Q: Write a program that defines symbolic constants for all seven days of the week. Create an array variable that uses the symbols as initializers.**  
**A:**

---
## 문제  
**Q:**  
**A:**

---
## 문제 1 Integer Expression Calculation  
**Q:**  
**A:**

---
## 문제 1 Integer Expression Calculation  
**Q:**  
**A:**

---
## 문제 1 Integer Expression Calculation  
**Q:**  
**A:**

---

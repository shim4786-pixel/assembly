# Algorithm Workbench 문제 풀이  

## 문제 1  
**Q : Write a sequence of shift instructions that cause AX to be sign-extended into EAX. In other words, the sign bit of AX is copied into the upper 16 bits of EAX. Do not use the CWD instruction.**  
**A :**

---
## 문제 2  
**Q : Suppose the instruction set contained no rotate instructions. Show how you would use SHR and a conditional jump instruction to rotate the contents of the AL register 1 bit to the right.**  
**A :**

---
## 문제 3  
**Q : Write a logical shift instruction that multiplies the contents of EAX by 16.**  
**A :**

---
## 문제 4  
**Q : Write a logical shift instruction that divides EBX by 4.**  
**A :**

---
## 문제 5  
**Q : Write a single rotate instruction that exchanges the high and low halves of the DL register.**  
**A :**

---
## 문제 6  
**Q : Write a single SHLD instruction that shifts the highest bit of the AX register into the lowest bit position of DX and shifts DX one bit to the left.**  
**A :**

---
## 문제 7  
**Q : Write a sequence of instructions that shift three memory bytes to the right by 1 bit position. Use the following test data:**  
```asm
byteArray BYTE 81h,20h,33h
```
**A :**

---
## 문제 8  
**Q : Write a sequence of instructions that shift three memory words to the left by 1 bit position.
 Use the following test data:**  
```asm
wordArray WORD 810Dh, 0C064h,93ABh
```
**A :**

---
## 문제 9  
**Q : Write instructions that multiply 5 by 3 and store the result in a 16-bit variable val1.**  
**A :**

---
## 문제 10  
**Q : Write instructions that divide 276 by 10 and store the result in a 16-bit variable val1.**  
**A :**

---
## 문제 11  
**Q : Implement the following C++ expression in assembly language, using 32-bit unsigned operands:**  
```asm
val1 = (val2 * val3) / (val4 - 3)
```
**A :**

---
## 문제 12  
**Q : Implement the following C++ expression in assembly language, using 32-bit signed operands:**  
```asm
val1 = (val2 / val3) * (val1 + val2)
```
**A :**

---
## 문제 13  
**Q : Write a procedure that displays an unsigned 8-bit binary value in decimal format. Pass the binary value in AL. The input range is limited to 0 to 99, decimal. The only procedure you can call from the book’s link library is WriteChar. The procedure should contain approximately eight instructions. Here is a sample call:**  
```asm
mov  al,65            ; range limit: 0 to 99
call showDecimal8
```
**A :**

---
## 문제 14  
**Q : Challenge: Suppose AX contains 0072h and the Auxiliary Carry flag is set as a result of adding two unknown ASCII decimal digits. Use the Intel 64 and IA-32 Instruction Set Reference to determine what output the AAA instruction would produce. Explain your answer.**  
**A :**

---
## 문제 15  
**Q : Challenge: Using only SUB, MOV, and AND instructions, show how to calculate x = n mod y, assuming that you are given the values of n and y. You can assume that n is any 32-bit unsigned integer, and y is a power of 2.**  
**A :**

---
## 문제 16  
**Q : Challenge: Using only SAR, ADD, and XOR instructions (but no conditional jumps), write code that calculates the absolute value of the signed integer in the EAX register. Hints: A number can be negated by adding 1 to it and then forming its one’s complement. Also, if you XOR an integer with all 1s, its 1s are reversed. On the other hand, if you XOR an integer with all zeros, the integer is unchanged.**  
**A :**

---


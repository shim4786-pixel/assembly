# Algorithm Workbench 문제 풀이  

## 문제 1  
**Q : Write a single instruction that converts an ASCII digit in AL to its corresponding binary value. If AL already contains a binary value (00h to 09h), leave it unchanged.**  
**A :**

---
## 문제 2  
**Q : Write instructions that calculate the parity of a 32-bit memory operand. Hint: Use the formula presented earlier in this section: B0 XOR B1 XOR B2 XOR B3.**  
**A :**

---
## 문제 3  
**Q : Given two bit-mapped sets named SetX and SetY, write a sequence of instructions that generate a bit string in EAX that represents members in SetX that are not members of SetY.**  
**A :**

---
## 문제 4  
**Q : Write instructions that jump to label L1 when the unsigned integer in DX is less than or equal to the integer in CX.**  
**A :**

---
## 문제 5  
**Q : Write instructions that jump to label L2 when the signed integer in AX is greater than the integer in CX.**  
**A :**

---
## 문제 6  
**Q : Write instructions that first clear bits 0 and 1 in AL. Then, if the destination operand is equal to zero, the code should jump to label L3. Otherwise, it should jump to label L4.**  
**A :**

---
## 문제 7  
**Q : Implement the following pseudocode in assembly language. Use short-circuit evaluation and assume that val1 and X are 32-bit variables.**  
```asm
if( val1 > ecx ) AND ( ecx > edx ) 
   X = 1
else
   X = 2;
```
**A :**

---
## 문제 8  
**Q : Implement the following pseudocode in assembly language. Use short-circuit evaluation and assume that X is a 32-bit variable.**  
```asm
if( ebx > ecx ) OR ( ebx > val1 ) 
   X = 1
else
   X = 2
```
**A :**

---
## 문제 9  
**Q : Implement the following pseudocode in assembly language. Use short-circuit evaluation and assume that X is a 32-bit variable.**  
```asm
if( ebx > ecx AND ebx > edx) OR ( edx > eax ) 
   X = 1
else
   X = 2
```
**A :**

---
## 문제 10  
**Q : Implement the following pseudocode in assembly language. Use short-circuit evaluation and assume that A, B, and N are 32-bit signed integers. **  
```asm
while N > 0
  if N != 3 AND (N < A OR N > B)
     N = N – 2
  else
     N = N – 1
end whle
```
**A :**

---


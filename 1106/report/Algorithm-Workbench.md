# Algorithm Workbench 문제 풀이  

## 문제 1  
**Q : Write a single instruction that converts an ASCII digit in AL to its corresponding binary value. If AL already contains a binary value (00h to 09h), leave it unchanged.**  
**A :** ASCII 코드에서 '0'은 0x30, '9'는 0x39이다. 0~9의 값을 가지면 그대로 두고 ASCII코드의 '0' ~ '9'를 0 ~ 9 로 만들어야 한다.  
0x30은 `0011 0000b` 이다. 즉,  
`and AL, 0Fh`  

---
## 문제 2  
**Q : Write instructions that calculate the parity of a 32-bit memory operand. Hint: Use the formula presented earlier in this section: B0 XOR B1 XOR B2 XOR B3.**  
**A :**  
```asm
.data
   Value DWORD ?
.code
   mov al, BYTE PTR Value
   xor al, BYTE PTR Value+1
   xor al, BYTE PTR Value+2
   xor al, BYTE PTR Value+3
```

---
## 문제 3  
**Q : Given two bit-mapped sets named SetX and SetY, write a sequence of instructions that generate a bit string in EAX that represents members in SetX that are not members of SetY.**  
**A :**  
```asm
.data
   SetX DWORD ?
   SetY DWORD ?
.code
   mov eax, SetX
   xor eax, SetY
   and eax, SetX
```

---
## 문제 4  
**Q : Write instructions that jump to label L1 when the unsigned integer in DX is less than or equal to the integer in CX.**  
**A :**
```asm
cmp dx, cx
jbe L1
```

---
## 문제 5  
**Q : Write instructions that jump to label L2 when the signed integer in AX is greater than the integer in CX.**  
**A :**
```asm
cmp ax, cx
jg L2
```

---
## 문제 6  
**Q : Write instructions that first clear bits 0 and 1 in AL. Then, if the destination operand is equal to zero, the code should jump to label L3. Otherwise, it should jump to label L4.**  
**A :**
```asm
and al, 11111100h
jz L3
jmp L4
```

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
```asm
cmp val1, ecx
ja L1
jmp next
L1:
   cmp ecx, edx
   ja L2
   jmp next
L2:
   mov X, 1
   jmp END
next:
   mov X, 2
END:
```

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
```asm
cmp ebx, ecx
jbe L1
jmp next
L1:
   cmp ebx, val1
   jbe L2
   jmp next
L2:
   mov X, 2
   jmp END
next:
   mov X, 1
End:
```

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
```asm
cmp ebx, ecx
jbe L1
cmp ebx, edx
ja next
L1:
   cmp edx, eax
   ja next
   jmp L2
L2:
   mov X, 2
   jmp END
next:
   mov X, 1
END:
```

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
```asm
begin:
   cmp N, 0
   jng ENDwhile
   cmp N, 3
   jnz L1
   jmp ELSE
L1:
   cmp N, A
   jl YAP
   cmp N, B
   jg YAP
   jmp ELSE
YAP:
   sub N, 2
   jmp begin
ELSE:
   sub N, 1
   jmp begin  
ENDwhile:
```

---


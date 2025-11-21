# Algorithm Workbench 문제 풀이  
## 문제 1  
**Q : Here is a calling sequence for a procedure named AddThree that adds three doublewords (assume that the STDCALL calling convention is used):**  
```asm
push 10h
push 20h
push 30h
call AddThree
```
**Draw a picture of the procedure’s stack frame immediately after EBP has been pushed on the runtime stack.**  
**A :**

---
## 문제 2  
**Q : Create a procedure named AddThree that receives three integer parameters and calculates and returns their sum in the EAX register.**  
**A :**
```asm

```

---
## 문제 3  
**Q : Declare a local variable named pArray that is a pointer to an array of doublewords.**  
**A :**
```asm

```

---
## 문제 4  
**Q : Declare a local variable named buffer that is an array of 20 bytes.**  
**A :**
```asm

```

---
## 문제 5  
**Q : Declare a local variable named pwArray that points to a 16-bit unsigned integer.**  
**A :**
```asm

```

---
## 문제 6  
**Q : Declare a local variable named myByte that holds an 8-bit signed integer.**  
**A :**
```asm

```

---
## 문제 7  
**Q : Declare a local variable named myArray that is an array of 20 doublewords.**  
**A :**
```asm

```

---
## 문제 8  
**Q : Create a procedure named SetColor that receives two stack parameters: forecolor and backcolor, and calls the SetTextColor procedure from the Irvine32 library. **  
**A :**
```asm

```

---
## 문제 9  
**Q : Create a procedure named WriteColorChar that receives three stack parameters: char, forecolor, and backcolor. It displays a single character, using the color attributes specified in forecolor and backcolor.**  
**A :**
```asm

```

---
## 문제 10  
**Q : Write a procedure named DumpMemory that encapsulates the DumpMem procedure in the Irvine32 library. Use declared parameters and the USES directive. The following is an example of how it should be called: INVOKE DumpMemory, OFFSET array, LENGTHOF array, TYPE array.**  
**A :**
```asm

```

---
## 문제 11  
**Q : Declare a procedure named MultArray that receives two pointers to arrays of doublewords, and a third parameter indicating the number of array elements.  Also, create a PROTO declaration for this procedure.**  
**A :**
```asm

```

---

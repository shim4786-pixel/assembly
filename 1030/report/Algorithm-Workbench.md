# Algorithm Workbench 문제 풀이

## 문제 1  
**Q : Write a sequence of statements that use only PUSH and POP instructions to exchange the values in the EAX and EBX registers (or RAX and RBX in 64-bit mode). **  
**A :** PUSH한 순서대로 POP하면 위치를 바꿀 수 있다.    
```asm
push eax
push ebx
pop eax
pop ebx
```

---
## 문제 2
**Q : Suppose you wanted a subroutine to return to an address that was 3 bytes higher in memory than the return address currently on the stack. Write a sequence of instructions that would be inserted just before the subroutine’s RET instruction that accomplish this task.**  
**A :** 스택에 저장된 값을 꺼내 3을 더한 뒤 다시 스택에 넣는다.    
```asm
pop esi
add esi, 3
push esi
ret
```

---
## 문제 3
**Q : Functions in high-level languages often declare local variables just below the return address on the stack. Write an instruction that you could put at the beginning of an assembly language subroutine that would reserve space for two integer doubleword variables. Then, assign the values 1000h and 2000h to the two local variables.**  
**A :** 스택 포인터를 줄여 공간을 확보하면 고급 언어의 함수들이 지역 변수를 선언하는 것 처럼 만들 수 있다.    
```asm
AW3Sub PROC
  sub esp, 8
  mov DWORD PTR [esp], 1000h
  mov DWORD PTR [esp +4], 2000h
  .
  .
  .
  add esp, 8
  ret
AW3Sub ENDP
```

---
## 문제 4
**Q : Write a sequence of statements using indexed addressing that copies an element in a doubleword array to the previous position in the same array.**  
**A :** ecx로 배열의 원소를 지정하고 그보다 주소가 4만큼 낮은(이전 원소) 위치에 복사한다.  
```asm
; esi = OFFSET Array
; ecx = 현재 인덱스 (0이면 배열의 첫 번째 원소)
mov eax, [esi + ecx*4]
mov [esi + ecx*4 - 4], eax
```

---
## 문제 5
**Q : Write a sequence of statements that display a subroutine’s return address. Be sure that whatever modifications you make to the stack do not prevent the subroutine from returning to its caller.**  
**A :** 스택에서 꺼내어 값을 읽기만 하고 바로 되돌려주는 서브루틴을 작성한다.    
```asm
AW5Sub PROC
  pop eax
  call WriteHex
  push eax
  ret
AW5Sub ENDP
```

---

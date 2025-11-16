# Programming Exercises 문제 풀이

## ★ 1. Display ASCII Decimal    
**Q : Write a procedure named WriteScaled that outputs a decimal ASCII number with an implied decimal point. Suppose the following number were defined as follows, where DECIMAL_OFFSET indicates that the decimal point must be inserted five positions from the right side of the number:**  
```asm
DECIMAL_OFFSET = 5
.data
decimal_one BYTE "100123456789765"
```
**WriteScaled would display the number like this:**  
```
1001234567.89765
```
** When calling WriteScaled, pass the number’s offset in EDX, the number length in ECX, and the decimal offset in EBX. Write a test program that passes three numbers of different sizes to the WriteScaled procedure.**  
**A :** ASCII 숫자로 된 문자열을 받아 소수점을 갖는 10진수로 출력하는 WriteScaled 프로시저 작성  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc
DECIMAL_OFFSET = 5     ; 소수점이 오른쪽으로부터 5번째 위치에 삽입
.data
    dec1 BYTE "100123456789765", 0
    dec2 BYTE "12345678", 0
    dec3 BYTE "999999999999", 0
.code
main PROC
    ;첫 번째 숫자
    mov edx, OFFSET dec1            
    mov ecx, LENGTHOF dec1 - 1  ; 개행문자 0 제외
    mov ebx, DECIMAL_OFFSET
    call WriteScaled

    ;두 번째 숫자
    mov edx, OFFSET dec2            
    mov ecx, LENGTHOF dec2 - 1
    mov ebx, 4
    call WriteScaled

    ;세 번째 숫자
    mov edx, OFFSET dec3            
    mov ecx, LENGTHOF dec3 - 1
    mov ebx, 6
    call WriteScaled

    exit 
main ENDP
;---------------------------------------------------
; WriteScaled
; 문자열을 n.m의 형태로 출력
; 입력 : EDX = 숫자 문자열 주소
;        ECX = 숫자 길이
;        EBX = 소수점 위치
; 출력 : 소수점을 갖는 10진수 문자열
;---------------------------------------------------
WriteScaled PROC
    push eax
    push esi

    mov esi, edx
    mov eax, ecx
    sub eax, ebx    ; 소수점 위치 계산
                    ; ecx - ebx = LENGTHOF left
left:
    cmp eax, 0      ; 왼쪽 부분이 끝났다면
    je dot          ; 소수점 삽입

    push eax
    mov al, BYTE PTR[esi]   ; 현재 문자를 al에 로드하여 출력
    call WriteChar
    pop eax
    inc esi         ; 다음 문자로 이동하고
    dec eax         ; 남은 문자 수는 1 감소
    jmp left
dot:
    mov al, '.'
    call WriteChar
right:
    cmp ebx, 0      ; 오른쪽 부분이 끝났다면
    je done         ; 종료
    push eax
    mov al, BYTE PTR[esi]
    call WriteChar
    pop eax
    inc esi
    dec ebx
    jmp right
done:
    call Crlf

    pop esi
    pop eax
    ret
WriteScaled ENDP

END main
```

---
## ★ 2. Extended Subtraction Procedure   
**Q : Create a procedure named Extended_Sub that subtracts two binary integers of arbitrary size. The storage size of the two integers must be the same, and their size must be a multiple of 32 bits. Write a test program that passes several pairs of integers, each at least 10 bytes long.**  
**A :** 32비트의 배수 길이인 숫자쌍들을 받아 뺄셈을 계산하는 프로시저 작성  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
    val1 DWORD 12345678h
    val2 DWORD 11111111h

    val3 DWORD 12345678h, 9ABCDEF0h 
    val4 DWORD 11111111h, 22222222h 

    val5 DWORD 12345678h, 9ABCDEF0h, 00001234h 
    val6 DWORD 11111111h, 22222222h, 00000001h 
.code
main PROC
    ; 32비트 뺄셈
    mov edx, OFFSET val1
    mov esi, OFFSET val2
    mov ecx, SIZEOF val1
    call Extended_Sub

    mov eax, [edx]
    call WriteHex
    call Crlf

    ; 64비트 뺄셈
    mov edx, OFFSET val3
    mov esi, OFFSET val4
    mov ecx, SIZEOF val3
    call Extended_Sub

    mov ecx, 2
    mov edx, OFFSET val3
print64:
    mov eax, [edx]
    call WriteHex
    add edx, 4
    loop print64
    call Crlf

    ;96비트 뺄셈
    mov edx, OFFSET val5
    mov esi, OFFSET val6
    mov ecx, SIZEOF val5
    call Extended_Sub

    mov ecx, 3
    mov edx, OFFSET val5
print96:
    mov eax, [edx]
    call WriteHex
    add edx, 4
    loop print96
    call Crlf

    exit 
main ENDP
;--------------------------------------------------------
; Extended_Sub
; 32비트 단위 뺄셈 수행(A - B)
; 입력 : EDX = A(피감수) 주소
;        ESI = B(피수) 주소
;        ECX = 길이(바이트 단위)
; 출력 : 결과를 피감수 주소에 저장
;--------------------------------------------------------
Extended_Sub PROC
    push edx
Subloop:
    cmp ecx, 0
    je done

    mov eax, [edx]  ; 현재 워드의 피감수
    sbb eax, [esi]  ; 현재 워드의 피수 + CF
    mov [edx], eax  ; 결과를 피감수 주소에 저장

    add edx, 4
    add esi, 4
    sub ecx, 4  ; 다음 워드로 이동
    jmp Subloop
done:
    pop edx
    ret
Extended_Sub ENDP

END main
```

---
## ★★ 3. Packed Decimal Conversion   
**Q : Write a procedure named PackedToAsc that converts a 4-byte packed decimal integer to a string of ASCII decimal digits. Pass the packed integer and the address of a buffer holding the ASCII digits to the procedure. Write a short test program that passes at least 5 packed decimal integers to your procedure.**  
**A :** 4바이트 압축 10진 정수를 받아 ASCII 문자열로 변환하는 프로시저를 작성  
압축 10진 정수는 한 바이트에 10진수 정수 2개를 넣은 형태의 수이다.  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
    pack1 BYTE 12h, 34h, 56h, 78h
    pack2 BYTE 01h, 23h, 45h, 67h
    pack3 BYTE 18h, 27h, 36h, 45h
    pack4 BYTE 11h, 22h, 33h, 44h
    pack5 BYTE 87h, 65h, 43h, 21h

    buffer BYTE 16 DUP(0)
.code
main PROC
    mov edx, OFFSET pack1
    mov edi, OFFSET buffer
    call PackedToAsc
    call WriteString
    call Crlf

    mov edx, OFFSET pack2
    mov edi, OFFSET buffer
    call PackedToAsc
    call WriteString
    call Crlf

    mov edx, OFFSET pack3
    mov edi, OFFSET buffer
    call PackedToAsc
    call WriteString
    call Crlf

    mov edx, OFFSET pack4
    mov edi, OFFSET buffer
    call PackedToAsc
    call WriteString
    call Crlf

    mov edx, OFFSET pack5
    mov edi, OFFSET buffer
    call PackedToAsc
    call WriteString
    call Crlf

    exit 
main ENDP
;----------------------------------------------
; PackedToAsc
; 4바이트 압축 10진 정수를 ASCII 문자열로 변환
; 입력 : EDX = 패킹된 정수 주소
;        EDI = 변환된 ASCII를 저장할 버퍼 주소
; 출력 : 버퍼에 저장된 ASCII 문자열을 반환
;----------------------------------------------
PackedToAsc PROC
    push eax
    push esi
    push ecx

    mov esi, edx        ; 패킹된 정수 주소
    mov ecx, 4          ; 패킹된 바이트 수
    push edi
next_byte:
    mov al, [esi]       ; 현재 바이트 읽어 들이기
    mov ah, al
    shr al, 4           ; AL은 상위 4비트 추출
    and ah, 0Fh         ; AH는 하위 4비트 추출

    add al, '0'         ; ASCII 변환
    add ah, '0'

    mov [edi], al       ; 상위 4비트 문자 저장
    inc edi
    mov [edi], ah       ; 하위 4비트 문자 저장
    inc edi

    inc esi
    loop next_byte

    mov BYTE PTR[edi], 0    ; WriteString 위해 null-terminated 문자열 작성
    pop edi
    mov edx, edi            ; WriteString은 edx를 보고 출력함
    pop ecx
    pop esi
    pop eax
    ret
PackedToAsc ENDP

END main
```

---
## ★★ 4. Encryption Using Rotate Operations   
**Q : Write a procedure that performs simple encryption by rotating each plaintext byte a varying number of positions in different directions. For example, in the following array that represents the encryption key, a negative value indicates a rotation to the left and a positive value indicates a rotation to the right. The integer in each position indicates the magnitude of the rotation:**  
```asm
key BYTE -2, 4, 1, 0, -3, 5, 2, -4, -4, 6
```
**Your procedure should loop through a plaintext message and align the key to the first 10 bytes of the message. Rotate each plaintext byte by the amount indicated by its matching key array value. Then, align the key to the next 10 bytes of the message and repeat the process. Write a program that tests your encryption procedure by calling it twice, with different data sets.**  
**A :**
```asm

```

---
## ★★★ 5. Prime Numbers   
**Q :Write a program that generates all prime numbers between 2 and 1000, using the Sieve of Eratosthenes method. You can find many articles that describe the method for finding primes in this manner on the Internet. Display all the prime values.**  
**A :**
```asm

```

---
## ★★★ 6. Greatest Common Divisor (GCD)   
**Q : The greatest common divisor (GCD) of two integers is the largest integer that will evenly divide both integers. The GCD algorithm involves integer division in a loop, described by the following pseudocode:**  
```pseudocode
int GCD(int x, int y)
    {
       x = abs(x)          // absolute value
       y = abs(y)
       do {
         int n = x % y
         x = y
         y = n
       } while (y > 0)
       return x
   }
```
** Implement this function in assembly language and write a test program that calls the function
 several times, passing it different values. Display all results on the screen.**  
**A :**
```asm

```

---
## ★★★ 7. Bitwise Multiplication   
**Q : Write a procedure named BitwiseMultiply that multiplies any unsigned 32-bit integer by EAX, using only shifting and addition. Pass the integer to the procedure in the EBX register, and return the product in the EAX register. Write a short test program that calls the procedure and displays the product. (We will assume that the product is never larger than 32 bits.) This is a fairly challenging program to write. One possible approach is to use a loop to shift the multiplier to the right, keeping track of the number of shifts that occur before the Carry flag is set. The resulting shift count can then be applied to the SHL instruction, using the multiplicand as the destination operand. Then, the same process must be repeated until you find the last 1 bit in the multiplier.**  
**A :**
```asm

```

---
## ★★★ 8. Add Packed Integers   
**Q : Extend the AddPacked procedure from Section 7.6.1 so that it adds two packed decimal integers of arbitrary size (both lengths must be the same). Write a test program that passes AddPacked several pairs of integers: 4-byte, 8-byte, and 16-byte. We suggest that you use the following registers to pass information to the procedure:**  
```
ESI - pointer to the first number
EDI - pointer to the second number
EDX - pointer to the sum
ECX - number of bytes to add
```
**A :**
```asm

```

---

# Programming Exercises 문제 풀이
## ★ 1. Improved Str_copy Procedure  
**Q : The Str_copy procedure shown in this chapter does not limit the number of characters to be copied. Create a new version (named Str_copyN) that receives an additional input parameter indicating the maximum number of characters to be copied.**  
**A :** 복사되는 문자 개수를 파라미터로 받는 Str_copyN 만들기(Str_copy를 따라)  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

Str_copyN PROTO, source:PTR BYTE, target:PTR BYTE, maxCount:DWORD

.data
    sourceStr1 BYTE "Hello Assembly World", 0
    sourceStr2 BYTE "Short", 0

    targetStr1 BYTE 50 DUP(?)
    targetStr2 BYTE 50 DUP(?)

    maxD DWORD 10

.code
main PROC
    INVOKE Str_copyN, ADDR sourceStr1, ADDR targetStr1, maxD
    mov edx, OFFSET targetStr1
    call WriteString
    call Crlf

    INVOKE Str_copyN, ADDR sourceStr2, ADDR targetStr2, maxD
    mov edx, OFFSET targetStr2
    call WriteString
    call Crlf
    exit 
main ENDP
;--------------------------------------------------------
Str_copyN PROC USES eax ecx esi edi,
    source:PTR BYTE,
    target:PTR BYTE,
    maxCount:DWORD
; 받은 파라미터 :
;    source : 원본 문자열
;    target : 복사할 목적지 문자열
;    maxCount : 복사할 최대 문자 개수
; source로부터 최대 maxCount만큼 문자를 복사하는 프로시저
;--------------------------------------------------------
    INVOKE Str_length, source
    mov ecx, eax
    mov edx, maxCount

    cmp ecx, edx    ; ecx <= maxCount ?
    jbe SizeOK      ; 문자열의 길이가 더 짧거나 같으면 그대로 사용
    mov ecx, edx    ; 더 길다면 maxCount 까지만 복사
SizeOK:
    mov esi, source
    mov edi, target
    cld
    rep movsb
    mov BYTE PTR[edi], 0
    ret 12
Str_copyN ENDP
END main
```

---
## ★ 2. Str_concat Procedure  
**Q : Write a procedure named Str_concat that concatenates a source string to the end of a target string. Sufficient space must exist in the target string to accommodate the new characters. Pass pointers to the source and target strings. Here is a sample call:**  
```asm
.data
targetStr BYTE "ABCDE",10 DUP(0)
sourceStr BYTE "FGH",0
.code
INVOKE Str_concat, ADDR targetStr, ADDR sourceStr
```
**A :** 충분한 크기의 target 문자열 뒤에 source 문자열을 이어붙이는 프로시저 Str_concat  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

Str_concat PROTO, pTarget:PTR BYTE, pSource:PTR BYTE

.data
    targetStr BYTE "ABCDE", 10 DUP(0)
    sourceStr BYTE "FGH", 0

.code
main PROC
    INVOKE Str_concat, ADDR targetStr, ADDR sourceStr
    mov edx, OFFSET targetStr
    call WriteString
    call Crlf
    exit 
main ENDP
;---------------------------------------------------------
Str_concat PROC USES eax ecx esi edi,
    pTarget:PTR BYTE,
    pSource:PTR BYTE
; target 문자열 끝에 source 문자열을 이어 붙이는 프로시저
; 받은 파라미터 :
;   pTarget = 충분한 공간을 가진 문자열
;   pSource = pTarget 뒤에 이어붙일 문자열
;---------------------------------------------------------
    INVOKE Str_length, pTarget
    mov edi, pTarget
    add edi, eax    ; target 끝위치로 이동

    INVOKE Str_length, pSource
    mov ecx, eax
    inc ecx         ; 널 문자 포함하여 source 길이만큼 복사준비

    mov esi, pSource
    cld
    rep movsb
    ret 8
Str_concat ENDP
END main
```

---
## ★ 3. Str_remove Procedure  
**Q : Write a procedure named Str_remove that removes n characters from a string. Pass a pointer to the position in the string where the characters are to be removed. Pass an integer specifying the number of characters to remove. The following code, for example, shows how to remove “xxxx” from target:**  
```asm
.data
target BYTE "abcxxxxdefghijklmop",0
.code
INVOKE Str_remove, ADDR [target+3], 4
```
**A :** 문자열에서 n개의 문자를 제거하는 Str_remove 프로시저  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

Str_remove PROTO, pStart:PTR BYTE, count:DWORD

.data
    target BYTE "abcxxxxdefghijklmop", 0
    msg BYTE "변경 후 => ", 0
.code
main PROC
    mov edx, OFFSET target
    call WriteString
    call Crlf
    INVOKE Str_remove, ADDR target+3, 4
    mov edx, OFFSET msg
    call WriteString
    mov edx, OFFSET target
    call WriteString
    call Crlf
    exit 
main ENDP
;-------------------------------------------------------
Str_remove PROC USES eax ecx esi edi,
    pStart:PTR BYTE,
    count:DWORD
; target에서 주어진 위치의 문자를 count개 삭제하는 프로시저
; 받은 파라미터 :
;   pStart = 삭제할 문자의 첫 위치
;   count = 삭제할 문자의 개수
; 반환값 : 없음 (삭제 후 이어붙인 문자열 target)
;-------------------------------------------------------
    mov esi, pStart
    mov eax, count
    add esi, eax        ; ESI = pStart + count

    INVOKE Str_length, esi
    mov ecx, eax
    inc ecx             ; null 문자를 포함한 남은 문자열
    
    mov edi, pStart
    cld
    rep movsb           ; count만큼 끌어당겨서 작성

    ret 8
Str_remove ENDP
END main
```

---
## ★ 4. Str_find Procedure  
**Q : Write a procedure named Str_find that searches for the first matching occurrence of a source string inside a target string and returns the matching position. The input parameters should be a pointer to the source string and a pointer to the target string. If a match is found, the procedure sets the Zero flag and EAX points to the matching position in the target string. Otherwise, the Zero flag is clear and EAX is undefined. The following code, for example, searches for “ABC” and returns with EAX pointing to the “A” in the target string:**  
```asm
.data
target BYTE "123ABC342432",0
source BYTE "ABC",0
pos    DWORD ?
.code
INVOKE Str_find, ADDR source, ADDR target
jnz notFound
mov pos,eax                      ; store the position value
```
**A :** target안에서 source 문자열이 처음으로 나타나는 곳을 찾아 그 위치를 반환하는 프로시저  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

Str_find PROTO, pSource:PTR BYTE, pTarget:PTR BYTE

.data
    target BYTE "123ABC342432", 0
    source BYTE "ABC", 0
    pos DWORD ?
    msgF BYTE "찾았습니다. 주소는 : ", 0
    msgNF BYTE "찾지 못했습니다.", 0
.code
main PROC
    INVOKE Str_find, ADDR source, ADDR target
    jnz NFound
FoundS:
    mov pos, eax
    mov edx, OFFSET msgF
    call WriteString
    call WriteHex
    jmp done
NFound:
    mov edx, OFFSET msgNF
    call WriteString   
done:
    call Crlf
    exit
main ENDP
;-------------------------------------------------------
Str_find PROC USES esi edi ecx,
    pSource:PTR BYTE,
    pTarget:PTR BYTE
; 받은 파라미터 :
;   pTarget = 탐색할 문자열
;   pSource = 검색어가 담긴 문자열
; 반환값 :
;   찾으면 ZF = 1, EAX = 찾은 문자열의 주소
;   못찾으면 ZF = 0, EAX = 정의되지 않음
;-------------------------------------------------------
    mov esi, pSource
    mov edi, pTarget
SearchLoop:
    mov al, [edi]
    cmp al, 0
    je NotFound     ; target이 끝났으면 탐색 실패

    mov al, [edi]
    mov dl, [esi]
    cmp al, dl
    jne NextChar    ; 첫 글자가 불일치하면 다음 문자로

    push edi
    push esi        ; 일치하면 일치테스트 시작
    INVOKE Str_length, esi
    mov ecx, eax

    pop esi
    mov ebx, esi    ; 불일치를 대비해 source 시작값 저장
    pop edi
    repe cmpsb      ; source 와 target 비교
    jecxz Found     ; ecx == 0 이라면 전체 일치

    mov esi, ebx
NextChar:
    inc edi         ; 다음 문자로 이동
    jmp SearchLoop
Found:
    INVOKE Str_length, pSource
    sub edi, eax    ; edi는 비교 후 +1, 시작 위치는 edi - pSource length임
    mov eax, edi    ; eax에는 검색어의 시작 주소값
    cmp eax, eax    ; ZF = 1
    ret 8
NotFound:
    cmp eax, 0      ; ZF = 0
    ret 8   
Str_find ENDP
END main
```

---
## ★ 5. Str_nextWord Procedure  
**Q : Write a procedure called Str_nextWord that scans a string for the first occurrence of a certain delimiter character and replaces the delimiter with a null byte. There are two input parameters: a pointer to the string and the delimiter character. After the call, if the delimiter was found, the Zero flag is set and EAX contains the offset of the next character beyond the delimiter. Otherwise, the Zero flag is clear and EAX is undefined. The following example code passes the address of target and a comma as the delimiter:**  
```asm
.data
target BYTE "Johnson,Calvin",0
.code
INVOKE Str_nextWord, ADDR target, ','
jnz notFound
```
**In Figure 9-5, after calling Str_nextWord, EAX points to the character following the position where the comma was found (and replaced). **  
<img width="446" height="144" alt="image" src="https://github.com/user-attachments/assets/a03a1245-6dd8-4ab8-be12-91f91d75f1cf" />  
**A :** 문자열 내 분리 문자를 찾아 널 문자로 바꾸고 뒤를 반환하는 프로시저  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

Str_nextWord PROTO, pStr:PTR BYTE, delim:BYTE

.data
    target BYTE "Johnson,Calvin", 0
    msgF BYTE "분리 문자 발견", 0
    msgNF BYTE " 분리 문자 미발견", 0
.code
main PROC
    INVOKE Str_nextWord, ADDR target, ','
    jnz NFound
FoundS:
    mov edx, OFFSET msgF
    call WriteString
    call Crlf
    mov edx, eax
    call WriteString
    jmp done
NFound:
    mov edx, OFFSET msgNF
    call WriteString
done:
    call Crlf
    exit
main ENDP
;-------------------------------------------------------
Str_nextWord PROC USES esi edi,
    pStr:PTR BYTE,
    delim:BYTE
; 받은 파라미터 :
;   pStr = 탐색할 문자열
;   delim = 분리 확인 문자
; 반환값 :
;   찾으면 ZF = 1, EAX = 분리 문자의 다음 오프셋
;   못찾으면 ZF = 0, EAX = 정의되지 않음
;-------------------------------------------------------
    mov esi, pStr
    mov al, delim
    mov ecx, 0FFFFFFFFh         ; 충분히 큰 값
                                ; null 값을 만나면 멈추니 Str_length대신 사용가능

    mov edi, esi
    cld
    repne scasb                 ; al(분리 문자)과 문자열 비교, 일치할 때까지
    jz Found                    ; 찾았다면 Found로
    cmp eax, 0                  ; 못 찾았으면 ZF = 0
    ret 8
Found:
    lea eax, [edi-1+1]          ; 찾았다면 eax에 분리 문자 다음 주소 저장
    mov BYTE PTR[edi-1], 0      ; 분리 문자 위치에는 널 문자 대입
    cmp eax, eax                ; ZF = 1
    ret 8
Str_nextWord ENDP
END main
```

---
## ★ 6. Constructing a Frequency Table  
**Q :  Write a procedure named Get_frequencies that constructs a character frequency table. Input to the procedure should be a pointer to a string and a pointer to an array of 256 doublewords initialized to all zeros. Each array position is indexed by its corresponding ASCII code. When the procedure returns, each entry in the array contains a count of how many times the corresponding character occurred in the string. For example,**  
```asm
.data
target BYTE "AAEBDCFBBC",0
freqTable DWORD 256 DUP(0)
.code
INVOKE Get_frequencies, ADDR target, ADDR freqTable
```
**Figure 9-6 shows a picture of the string and entries 41 (hexadecimal) through 4B in the frequency table. Position 41 contains the value 2 because the letter A (ASCII code 41h) occurred twice in the string. Similar counts are shown for other characters. Frequency tables are useful in data compression and other applications involving character processing. The Huffman encoding algorithm, for example, stores the most frequently occurring characters in fewer bits than other characters that occur less often.**  
<img width="464" height="128" alt="image" src="https://github.com/user-attachments/assets/c6a043e9-b07e-4324-93de-8c21f64bc419" />  
**A :**
```asm

```

---
## ★ 7. Sieve of Eratosthenes  
**Q : The Sieve of Eratosthenes, invented by the Greek mathematician of the same name, provides a quick way to find all prime numbers within a given range. The algorithm involves creating an array of bytes in which positions are “marked” by inserting 1s in the following manner: Beginning with position 2 (which is a prime number), insert a 1 in each array position that is a multiple of 2. Then do the same thing for multiples of 3, the next prime number. Find the next prime number after 3, which is 5, and mark all positions that are multiples of 5. Proceed in this manner until all multiples of primes have been found. The remaining positions of the array that are unmarked indicate which numbers are prime. For this program, create a 65,000-element array and display all primes between 2 and 65,000. Declare the array in an uninitialized data segment (see Section 3.4.11) and use STOSB to fill it with zeros. **  
**A :**
```asm

```

---
## ★ 8. Bubble Sort  
**Q : Add a variable to the BubbleSort procedure in Section 9.5.1 that is set to 1 whenever a pair of values is exchanged within the inner loop. Use this variable to exit the sort before its normal completion if you discover that no exchanges took place during a complete pass through the array. (This variable is commonly known as an exchange flag.)**  
**A :**
```asm

```

---
## ★ 9. Binary Search  
**Q : Rewrite the binary search procedure shown in this chapter by using registers for mid, first, and last. Add comments to clarify the registers’ usage.**  
**A :**
```asm

```

---
## ★ 10. Letter Matrix  
**Q : Create a procedure that generates a four-by-four matrix of randomly chosen capital letters. When choosing the letters, there must be a 50% probability that the chosen letter is a vowel. Write a test program with a loop that calls your procedure five times and displays each matrix in the console window. Following is sample output for the first three matrices:**  
```
D W A L
S I V W
U I O L
L A I I

K X S V
N U U O
O R Q O
A U U T

P O A Z
A E A U
G K A E
I A G D
```
**A :**
```asm

```

---
## ★ 11. Letter Matrix/Sets with Vowels  
**Q :  Use the letter matrix generated in the previous programming exercise as a starting point for this program. Generate a single random four-by-four letter matrix in which each letter has a 50% probability of being a vowel. Traverse each matrix row, column, and diagonal, generating sets of letters. Display only four-letter sets containing exactly two vowels. Suppose, for example, the following matrix was generated:**  
```
P O A Z
A E A U
G K A E
I A G D
```
** Then the four-letter sets displayed by the program would be POAZ, GKAE, IAGD, PAGI, ZUED, PEAD, and ZAKI. The order of letters within each set is unimportant.**  
**A :**
```asm

```

---
## ★ 12. Calculating the Sum of an Array Row  
**Q : Write a procedure named calc_row_sum that calculates the sum of a single row in a two-dimensional array of bytes, words, or doublewords. The procedure should have the following stack parameters: array offset, row size, array type, row index. It must return the sum in EAX. Use explicit stack parameters, not INVOKE or extended PROC. Write a program that tests your procedure with arrays of byte, word, and doubleword. Prompt the user for the row index, and display the sum of the selected row.**  
**A :**
```asm

```

---
## ★ 13. Trimming Leading Characters  
**Q : Create a variant of the Str_trim procedure that lets the caller remove all instances of a leading character from a string. For example, if you were to call it with a pointer to the string “###ABC” and pass it the # character, the resulting string would be “ABC.”**  
**A :**
```asm

```

---
## ★ 14. Trimming a Set of Characters  
**Q : Create a variant of the Str_trim procedure that lets the caller remove all instances of a set of characters from the end of a string. For example, if you were to call it with a pointer to the string “ABC#$&” and pass it a pointer to an array of filter characters containing “%#!;$&*”, the resulting string would be “ABC”.**  
**A :**
```asm

```

---

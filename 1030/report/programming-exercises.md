# Programming Exercises 문제 풀이  

## ★ 1. Draw Text Colors   
**Q : Write a program that displays the same string in four different colors, using a loop. Call the SetTextColor procedure from the book’s link library. Any colors may be chosen, but you may find it easiest to change the foreground color.**  
**A :**  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
	message BYTE "Hello, World!", 0
	colors BYTE 04h, 01h, 02h, 0Eh

.code
main PROC 
	call Clrscr
	call DisplayColoredText
	exit
main ENDP

;------------------------------------------------------------
; DisplayColoredText : 주어진 색상으로 문자열 출력
; 매개변수 : ecx. 색상의 개수, edx. 출력할 문자열
;	 esi. 색상 배열의 인덱스, eax. SetTextColor를 위한 색상
;------------------------------------------------------------
DisplayColoredText PROC
    mov ecx, LENGTHOF colors ; 색상의 개수
	mov esi, 0 ; colors 배열의 인덱스 초기화
; 색상을 가져와 문자열 출력 루프
nextColor:
    movzx eax, colors[esi] ; 현재 색상 로드
	push eax               ; 색상 푸시
	call SetTextColor

	mov edx, OFFSET message ; 문자열 주소 로드
	call WriteString        ; 문자열 출력
	call Crlf               ; 줄 바꿈

	inc esi
	loop nextColor
	ret
DisplayColoredText ENDP

END main
```

---
## ★★ 2. Linking Array Items   
**Q : Suppose you are given three data items that indicate a starting index in a list, an array of characters, and an array of link index. You are to write a program that traverses the links and locates the characters in their correct sequence. For each character you locate, copy it to a new array. Suppose you used the following sample data, and assumed the arrays use zero-based indexes:  
start = 1  
chars:   H   A   C   E   B   D   F   G  
links:   0   4   5   6   2   3   7   0  
Then the values copied (in order) to the output array would be A,B,C,D,E,F,G,H. Declare the character array as type BYTE, and to make the problem more interesting, declare the links array type DWORD.**   
**A :**  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
    chars   BYTE 'H','A','C','E','B','D','F','G'
    links   DWORD 0,4,5,6,2,3,7,0
    output  BYTE LENGTHOF chars DUP(?)
    start   DWORD 1

.code
main PROC
    mov esi, start     ; 시작 인덱스
    mov edi, 0         ; output 배열 인덱스
    call TraverseAndCopy
    call PrintOutput
    exit
main ENDP

;-----------------------------
; 링크를 따라가며 문자 복사
; 입력: esi - 시작 인덱스
;       edi - output 배열 인덱스
; 출력: output 배열에 복사된 문자
; 반환: 없음
;-----------------------------
TraverseAndCopy PROC
loop_start:
    mov eax, esi         ; 현재 인덱스
    mov al, chars[eax]     ; 문자 가져오기
    mov output[edi], al    ; output에 저장
    inc edi                ; 다음 위치로

    mov eax, esi
    mov esi, links[eax*4]  ; 다음 인덱스

    test esi, esi          ; esi가 0인지 확인(0이면 ZF=1)
    jnz loop_start         ; jnz = ZF가 0이면 점프

    ; 마지막 문자 복사
    mov eax, esi
    mov al, chars[eax]
    mov output[edi], al
    ret
TraverseAndCopy ENDP

;------------------------------------------------------------
; PrintOutput: output 배열의 문자들을 화면에 출력
; 입력: output 배열, 길이 = LENGTHOF output
; 출력: 콘솔에 문자들 출력 + 줄바꿈
;------------------------------------------------------------
PrintOutput PROC
    mov ecx, LENGTHOF output    ; 출력할 문자 개수
    mov esi, OFFSET output      ; 배열 시작 주소

PrintLoop:
    mov al, [esi]               ; 현재 문자 가져오기
    call WriteChar              ; 문자 출력
    mov al, ' '
    call WriteChar              ; 공백 출력
    inc esi                     ; 다음 문자로 이동
    loop PrintLoop              ; ecx--, 0이 아니면 반복

    call Crlf                   ; 줄바꿈
    ret
PrintOutput ENDP

END main
```

---
## ★ 3. Simple Addition (1)   
**Q : Write a program that clears the screen, locates the cursor near the middle of the screen, prompts the user for two integers, adds the integers, and displays their sum.**  
**A :**  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
    p1 BYTE "Enter first integer: ", 0
    p2 BYTE "Enter second integer: ", 0
    resultMsg BYTE "The sum is: ", 0
    sum DWORD ?

.code
main PROC
    call Clrscr       ; 화면을 초기화
    call cursorMiddle ; 커서를 화면 중앙으로 이동
    call InputTwoIntegers ; 두 정수를 입력받음
    call DisplaySum  ; 합계를 출력함
    exit
main ENDP
;-----------------------------------------------------------------
; 커서를 화면 중앙으로 이동하는 프로시저
;-----------------------------------------------------------------
cursorMiddle PROC
    mov dh, 12      ; 행 위치 (대략 중앙)
    mov dl, 40      ; 열 위치 (대략 중앙)
    call Gotoxy
    ret
cursorMiddle ENDP
;-----------------------------------------------------------------
; 두 정수를 입력받아 합계를 계산하는 프로시저
; 출력: sum 변수에 합계 저장
;-----------------------------------------------------------------
InputTwoIntegers PROC
    mov edx, OFFSET p1
    call WriteString
    call ReadInt
    mov ebx, eax      ; 첫 번째 정수를 ebx에 저장

    call cursorMiddle

    mov edx, OFFSET p2
    call WriteString
    call ReadInt
    add eax, ebx      ; 두 정수를 더함
    mov sum, eax      ; 합계를 sum 변수에 저장
    ret
InputTwoIntegers ENDP
;-----------------------------------------------------------------
; 합계를 출력하는 프로시저
; 출력: sum 변수의 값 출력
;-----------------------------------------------------------------
DisplaySum PROC
    mov edx, OFFSET resultMsg
    call WriteString
    mov eax, sum
    call WriteInt
    call Crlf
    ret
DisplaySum ENDP

END main
```

---
## ★★ 4. Simple Addition (2)  
**Q : Use the solution program from the preceding exercise as a starting point. Let this new program repeat the same steps three times, using a loop. Clear the screen after each loop iteration.**  
**A :**  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
    p1 BYTE "Enter first integer: ", 0
    p2 BYTE "Enter second integer: ", 0
    resultMsg BYTE "The sum is: ", 0
    pauseMsg BYTE "Press any key to continue...", 0
    sum DWORD ?
    row BYTE 12

.code
main PROC
    mov ecx, 3        ; 반복 횟수 설정 (3번 실행)
RepeatLoop:
    mov row, 12      ; 행 위치 초기화
    call Clrscr       ; 화면을 초기화
    call cursorMiddle ; 커서를 중앙으로 이동
    call InputTwoIntegers ; 두 정수를 입력받음
    call DisplaySum  ; 합계를 출력함
    loop RepeatLoop   ; 반복
    exit
main ENDP
;-----------------------------------------------------------------
; 커서를 화면 중앙으로 이동하는 프로시저
;-----------------------------------------------------------------
cursorMiddle PROC
    mov dh, row     ; 행 위치 설정
    mov dl, 40      ; 열 위치 (대략 중앙)
    call Gotoxy
    inc row
    ret
cursorMiddle ENDP
;-----------------------------------------------------------------
; 두 정수를 입력받아 합계를 계산하는 프로시저
; 출력: sum 변수에 합계 저장
;-----------------------------------------------------------------
InputTwoIntegers PROC
    mov edx, OFFSET p1
    call WriteString
    call ReadInt
    mov ebx, eax      ; 첫 번째 정수를 ebx에 저장

    call cursorMiddle 

    mov edx, OFFSET p2
    call WriteString
    call ReadInt
    add eax, ebx      ; 두 정수를 더함
    mov sum, eax      ; 합계를 sum 변수에 저장
    ret
InputTwoIntegers ENDP
;-----------------------------------------------------------------
; 합계를 출력하는 프로시저
; 출력: sum 변수의 값 출력
;-----------------------------------------------------------------
DisplaySum PROC
    call cursorMiddle ; 커서를 중앙으로 이동
    mov edx, OFFSET resultMsg
    call WriteString
    mov eax, sum
    call WriteInt
    call Crlf
    mov edx, OFFSET pauseMsg
    call WriteString
    call ReadChar  ; 결과를 확인할 시간을 줌
    ret
DisplaySum ENDP

END main
```

---
## ★ 5. BetterRandomRange Procedure  
**Q : The RandomRange procedure from the Irvine32 library generates a pseudorandom integer between 0 and N-1. Your task is to create an improved version that generates an integer between M and N-1. Let the caller pass M in EBX and N in EAX. If we call the procedure BetterRandomRange, the following code is a sample test:**  
```asm
mov  ebx,-300                  ; lower bound
mov  eax,100                   ; upper bound
call BetterRandomRange         
```
**Write a short test program that calls BetterRandomRange from a loop that repeats 50 times.  
Display each randomly generated value.**  
**A :**  
```asm

```

---
## ★★ 6. Random Strings  
**Q : Create a procedure that generates a random string of length L, containing all capital letters. When calling the procedure, pass the value of L in EAX, and pass a pointer to an array of byte that will hold the random string. Write a test program that calls your procedure 20 times and displays the strings in the console window. **  
**A :**  
```asm

```

---
## ★ 7. Random Screen Locations  
**Q : Write a program that displays a single character at 100 random screen locations, using a timing delay of 100 milliseconds. Hint: Use the GetMaxXY procedure to determine the current size of the console window.**  
**A :**  
```asm

```

---
## ★★ 8. Color Matrix  
**Q : Write a program that displays a single character in all possible combinations of foreground and background colors (16X16=256). The colors are numbered from 0 to 15, so you can use a nested loop to generate all possible combinations.**  
**A :**  
```asm

```

---
## ★★★ 9. Recursive Procedure  
**Q : Direct recursion is the term we use when a procedure calls itself. Of course, you never want to let a procedure keep calling itself forever, because the runtime stack would fill up. Instead, you must limit the recursion in some way. Write a program that calls a recursive procedure. Inside this procedure, add 1 to a counter so you can verify the number of times it executes. Run your program with a debugger, and at the end of the program, check the counter’s value. Put a number in ECX that specifies the number of times you want to allow the recursion to continue. Using only the LOOP instruction (and no other conditional statements from later chapters), find a way for the recursive procedure to call itself a fixed number of times.**  
**A :**  
```asm

```

---
## ★★★ 10. Fibonacci Generator  
**Q : Write a procedure that produces N values in the Fibonacci number series and stores them in an array of doubleword. Input parameters should be a pointer to an array of doubleword, a counter of the number of values to generate. Write a test program that calls your procedure, passing N = 47. The first value in the array will be 1, and the last value will be 2,971,215,073. Use the Visual Studio debugger to open and inspect the array contents.**  
**A :**  
```asm

```

---
## ★★★ 11. Finding Multiples of K  
**Q : In a byte array of size N, write a procedure that finds all multiples of K that are less than N. Initialize the array to all zeros at the beginning of the program, and then whenever a multiple is found, set the corresponding array element to 1. Your procedure must save and restore any registers it modifies. Call your procedure twice, with K = 2, and again with K = 3. Let N equal to 50. Run your program in the debugger and verify that the array values were set correctly. Note: This procedure can be a useful tool when finding prime integers. An efficient algorithm for finding prime numbers is known as the Sieve of Eratosthenes. You will be able to implement this algorithm when conditional statements are covered in Chapter 6.**  
**A :**  
```asm

```

---

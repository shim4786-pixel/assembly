# Programming Exercises 문제 풀이  

When you write programs to solve the programming exercises, use multiple procedures when possible. Follow the style and naming conventions used in this book, unless instructed otherwise by your instructor. Use explanatory comments in your programs at the beginning of each procedure and next to nontrivial statements.  

## ★ 1. Draw Text Colors   
**Q : Write a program that displays the same string in four different colors, using a loop. Call the SetTextColor procedure from the book’s link library. Any colors may be chosen, but you may find it easiest to change the foreground color.**  
**A :** 같은 문자열을 4가지 색으로 출력하는 프로그램   
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
**A :** 링크를 따라 chars 배열의 문자를 정렬하는 프로그램   
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
**A :** 커서를 화면 중앙으로 이동하여 숫자 두 개를 입력받고, 합을 출력하는 프로그램    
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
**A :** (3)에 더해서 loop를 이용해 3번 반복하고, 각각의 루프마다 화면을 정리하는 프로그램    
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
mov  ebx,-300                  ; 하한가 M
mov  eax,100                   ; 상한가 N
call BetterRandomRange         
```
**Write a short test program that calls BetterRandomRange from a loop that repeats 50 times.  
Display each randomly generated value.**  
**A :** RandomRange 프로시저를 개조해 M부터 N-1까지의 난수를 뽑는 프로시저를 만들고 50회 반복하기   
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
   count DWORD 50


.code
main PROC
    call Randomize    ; 난수를 초기화
    mov ecx, count    
L1:
    mov ebx, -300     ; 하한값 설정
    mov eax, 100      ; 상한값 설정
    call BetterRandomRange
    loop L1
    
    exit
main ENDP
;------------------------------------------------------------
; BetterRandomRange : M과 N 사이의 난수를 생성
; 입력 : EAX = N  EBX = 시작 값 (M)
; 출력 : EAX = M ~ M + N - 1
;------------------------------------------------------------
BetterRandomRange PROC
    sub eax, ebx        ; EAX = 0~400
    call RandomRange    ; N = 400
    add eax, ebx       ; 시작 값(-300) 더하기
    call WriteInt
    call Crlf
    ret
BetterRandomRange ENDP


END main
```

---
## ★★ 6. Random Strings  
**Q : Create a procedure that generates a random string of length L, containing all capital letters. When calling the procedure, pass the value of L in EAX, and pass a pointer to an array of byte that will hold the random string. Write a test program that calls your procedure 20 times and displays the strings in the console window. **  
**A :** 길이 L의 랜덤 대문자 알파벳 문자열을 출력하는 프로시저를 작성하고 이를 20번 반복하기      
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
   strAlphabet DWORD 30			    ; 생성할 문자열의 길이
   buffer BYTE 100 DUP(?)           ; 충분한 길이의 버퍼 

.code
main PROC
    call Randomize                  ; 난수 초기화
    mov ecx, 20  		            ; 20개의 무작위 문자열 생성
GenLoop:
    call RandomString
    mov edx, OFFSET buffer
    call WriteString
    call Crlf
    loop GenLoop
    
    exit
main ENDP
;------------------------------------------------------------
; RandomString : 설정한 길이만큼 무작위 대문자 문자열 생성
; 입력 : ECX = 생성할 문자열의 길이
; 출력 : 버퍼에 무작위 대문자 문자열 저장
;------------------------------------------------------------
RandomString PROC USES ECX
    mov ecx, strAlphabet       ; 생성할 문자열의 길이 설정
    mov edi, OFFSET buffer     ; 버퍼의 시작 주소 설정

RandLoop:
    mov eax, 26                ; 알파벳 개수
    call RandomRange           ; EAX = 0 ~ 25
    add eax, 'A'               ; (0~25) -> ('A'~'Z')
    mov [edi], al              ; 버퍼에 문자 저장
    inc edi
    loop RandLoop

    mov byte ptr [edi], 0      ; 문자열 종료 문자 추가
    ret
RandomString ENDP

END main
```

---
## ★ 7. Random Screen Locations  
**Q : Write a program that displays a single character at 100 random screen locations, using a timing delay of 100 milliseconds. Hint: Use the GetMaxXY procedure to determine the current size of the console window.**  
**A :** 100밀리초의 지연시간을 두고 화면의 랜덤한 위치에 문자 하나를 100개 생성하는 프로그램    
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
   maxX DWORD ?
   maxY DWORD ?
   char BYTE '!'       ; 출력할 문자

.code
main PROC
    call Randomize          ; 난수 초기화
    call InitConsoleSize    ; 콘솔 창 크기 초기화

    mov ecx, 100            ; 100번 반복
MainLoop:
    call PrintRandomChar    ; 임의 위치에 문자 출력
    call DelayMs            ; 100ms 지연
    loop MainLoop
    
    exit
main ENDP
;---------------------------------------
; InitConsoleSize
; 콘솔 창의 최대 X, Y 좌표를 구하여
; maxX, maxY 변수에 저장한다
;---------------------------------------
InitConsoleSize PROC
    call GetMaxXY      ; EAX = 최대 X 좌표, EBX = 최대 Y 좌표
    mov maxX, eax      ; 최대 X 좌표 저장
    mov maxY, ebx      ; 최대 Y 좌표 저장
    ret
InitConsoleSize ENDP
;---------------------------------------
; PrintRandomChar
; maxX, maxY 범위 내에서
; 콘솔 창의 임의 위치에 문자를 출력한다
;---------------------------------------
PrintRandomChar PROC USES eax ebx ecx edx
    mov eax, maxX
    call RandomRange   ; EAX = 0 ~ maxX-1
    mov dx, ax         ; DX = X 좌표

    mov eax, maxY
    call RandomRange   ; EAX = 0 ~ maxY-1
    mov dh, al         ; DH = Y 좌표

    call GotoXY        ; 커서를 (DX, DH) 위치로 이동
    mov al, char      ; 출력할 문자 로드
    call WriteChar     ; 문자 출력
    ret
PrintRandomChar ENDP
;---------------------------------------
; Delay
; 100ms 지연
;---------------------------------------
DelayMs PROC
    mov eax, 100
    call Delay
    ret
DelayMs ENDP

END main
```

---
## ★★ 8. Color Matrix  
**Q : Write a program that displays a single character in all possible combinations of foreground and background colors (16X16=256). The colors are numbered from 0 to 15, so you can use a nested loop to generate all possible combinations.**  
**A :** 모든 전경색과 배경색 조합을 출력하는 프로그램    
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
   char BYTE 'A'        ; 출력할 문자

.code
main PROC
    mov eax, 0
    mov ebx, 0
    call DisplayAllColors
    call Reset
    call Crlf
    
    exit
main ENDP
;------------------------------------------------------------
; DisplayAllColors
; 모든 전경색과 배경색 조합을 출력하는 프로시저
;------------------------------------------------------------
DisplayAllColors PROC USES ecx edx
    mov dl, 0
BG_Loop:
    mov cl, 0
FG_Loop:
    push cx          ; 현재 전경색 저장
    push dx          ; 현재 배경색 저장
    call SetPrintColorChar
    pop dx           ; 배경색 복원
    pop cx           ; 전경색 복원
    inc cl
    cmp cl, 16
    jl FG_Loop

    call Crlf
    inc dl
    cmp dl, 16
    jl BG_Loop
    ret
DisplayAllColors ENDP
;------------------------------------------------------------
; SetPrintColorChar
; 지정된 전경색과 배경색으로 문자를 출력하는 프로시저
; 입력: CL - 전경색, DL - 배경색, char - 출력할 문자
;------------------------------------------------------------
SetPrintColorChar PROC USES eax ebx
    mov al, cl	   ; 전경색을 AL에 설정
    mov bl, dl	   ; 배경색을 BL에 설정
    imul ebx, 16    ; 색상 속성 계산
    add eax, ebx
    push eax
    call SetTextColor
    add esp, 4

    mov al, char
    call WriteChar
    ret
SetPrintColorChar ENDP
;------------------------------------------------------------
; Reset
; 기본 색상(흰 글자, 검은 배경)으로 초기화
;------------------------------------------------------------
Reset PROC
    mov al, 7         ; 흰 글자
    mov bl, 0         ; 검은 배경
    movzx eax, al
    movzx ebx, bl
    imul ebx, 16
    add eax, ebx
    push eax
    call SetTextColor
    add esp, 4
    ret
Reset ENDP


END main
```

---
## ★★★ 9. Recursive Procedure  
**Q : Direct recursion is the term we use when a procedure calls itself. Of course, you never want to let a procedure keep calling itself forever, because the runtime stack would fill up. Instead, you must limit the recursion in some way. Write a program that calls a recursive procedure. Inside this procedure, add 1 to a counter so you can verify the number of times it executes. Run your program with a debugger, and at the end of the program, check the counter’s value. Put a number in ECX that specifies the number of times you want to allow the recursion to continue. Using only the LOOP instruction (and no other conditional statements from later chapters), find a way for the recursive procedure to call itself a fixed number of times.**  
**A :** 자기자신을 호출하는 재귀함수    
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
   count DWORD 0

.code
main PROC
    mov ecx, 5          ; 5번의 재귀를 허용
    call Recursive
    call Crlf

    mov eax, count
    call WriteDec
    call Crlf

    exit
main ENDP

;---------------------------------------------------
; Recursive : 재귀 호출을 수행하는 프로시저
; ECX = 남은 재귀 횟수
;---------------------------------------------------
Recursive PROC
    inc count     ; 프로시저 실행한 만큼 1 증가
    push ecx
    loop DoRecur
    pop ecx
    ret

DoRecur:
    call Recursive
    pop ecx
    ret
Recursive ENDP

END main
```

---
## ★★★ 10. Fibonacci Generator  
**Q : Write a procedure that produces N values in the Fibonacci number series and stores them in an array of doubleword. Input parameters should be a pointer to an array of doubleword, a counter of the number of values to generate. Write a test program that calls your procedure, passing N = 47. The first value in the array will be 1, and the last value will be 2,971,215,073. Use the Visual Studio debugger to open and inspect the array contents.**  
**A :** 피보나치 수열 생성 (47번째까지)    
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
   N DWORD 47
   FibArray DWORD 47 DUP(0)

.code
main PROC
    mov eax, OFFSET FibArray        ; 배열 포인터
    mov ecx, N                      ; 항목 수 N
    call GenerateFib

    exit
main ENDP
;------------------------------------------------------
; GenerateFib : 피보나치 수열 생성
;   입력: EAX - 배열 포인터
;         ECX - 항목 수 N
;------------------------------------------------------
GenerateFib PROC USES eax edx esi
    mov esi, eax         ; 배열 포인터를 ESI에 저장
    mov DWORD PTR [esi], 1
    add esi, 4          ; 다음 항목 위치로 이동
    mov DWORD PTR [esi], 1
    add esi, 4          ; 다음 항목 위치로 이동

    sub ecx, 2          ; 첫 두 항목은 이미 설정됨

    mov eax, 1
    mov edx, 1          ; EAX=F(n-1), EDX=F(n-2)

NextFib:
    mov ebx, eax
    add eax, edx
    mov edx, ebx
    mov DWORD PTR [esi], eax
    add esi, 4
    loop NextFib
    ret
GenerateFib ENDP


END main
```
<img width="227" height="218" alt="image" src="https://github.com/user-attachments/assets/02b23251-294e-4b7e-973a-429ba5c0fec9" />


---
## ★★★ 11. Finding Multiples of K  
**Q : In a byte array of size N, write a procedure that finds all multiples of K that are less than N. Initialize the array to all zeros at the beginning of the program, and then whenever a multiple is found, set the corresponding array element to 1. Your procedure must save and restore any registers it modifies. Call your procedure twice, with K = 2, and again with K = 3. Let N equal to 50. Run your program in the debugger and verify that the array values were set correctly. Note: This procedure can be a useful tool when finding prime integers. An efficient algorithm for finding prime numbers is known as the Sieve of Eratosthenes. You will be able to implement this algorithm when conditional statements are covered in Chapter 6.**  
**A :** 0~50의 숫자 안에서 2와 3의 배수를 찾는 프로그램 ( '01'이 배수)  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
   N EQU 50
   arr BYTE N DUP(0)

.code
main PROC
    mov esi, OFFSET arr  ; arr의 시작 주소를 esi에 저장

    mov ecx, 25
    mov edx, 2
    call FindMul        ; K = 2로 호출

    mov ecx, 16
    mov edx, 3
    call FindMul        ; K = 3으로 호출

    exit
main ENDP
;-----------------------------------------------------
; FindMul: K의 배수 인덱스에 1 저장
; 입력:   ESI = 배열 시작 주소
;         ECX = 반복 횟수 (배열 크기 / K)
;         EDX = K
;-----------------------------------------------------
FindMul PROC
    mov ebx, 0                 ; 인덱스 초기화

LoopStart:
    mov BYTE PTR [esi+ebx], 1  ; arr[ebx] = 1
    add ebx, edx               ; 인덱스 += K
    loop LoopStart
    ret
FindMul ENDP

END main
```
<img width="149" height="204" alt="image" src="https://github.com/user-attachments/assets/e9515416-bf37-4d96-b6e3-cc0f8a47bbbd" />


---

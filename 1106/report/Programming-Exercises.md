# Programming Exercises 문제 풀이

## ★ 1. Filling an Array    
**Q : Create a procedure that fills an array of doublewords with N random integers, making sure the values fall within the range j...k, inclusive. When calling the procedure, pass a pointer to the array that will hold the data, pass N, and pass the values of j and k. Preserve all register values between calls to the procedure. Write a test program that calls the procedure twice, using different values for j and k. Verify your results using a debugger.**   
**A :** j~k 범위의 수들로 배열 채우기  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
   array1 DWORD 10 DUP(?)  ;첫번째 실행
   array2 DWORD 10 DUP(?)  ;두번째 실행

.code
main PROC
    call Randomize  ;난수 초기화
    ;첫번째 배열 채우기
    push 9                  ;k 
    push 0                  ;j 
    push 10                 ;N 
    push OFFSET array1      
    call FillArray          

    push 100                ;k
    push 50                 ;j
    push 10                 ;N
    push OFFSET array2
    call FillArray

    push 10
    push OFFSET array1
    call PrintArray

    push 10
    push OFFSET array2
    call PrintArray

    exit
main ENDP
;------------------------------------------
; FillArray PROC
; 배열을 난수로 채우는 프로시저
; 매개변수:
; [ebp+8]  - 배열의 시작 주소
; [ebp+12] - 배열의 크기 N
; [ebp+16] - 난수의 최솟값 j
; [ebp+20] - 난수의 최댓값 k
;------------------------------------------
FillArray PROC
    push ebp
    mov ebp, esp
    pushad

    mov edi, [ebp+8]
    mov ecx, [ebp+12]
    mov ebx, [ebp+16]
    mov edx, [ebp+20]
fill:
    mov eax, edx
    sub eax, ebx             
    inc eax          ; k- j + 1
    call RandomRange ; 0~(k - j)
    add eax, ebx     ; j ~ k

    mov [edi], eax   ; 배열에 값 저장
    add edi, 4       ; 다음 요소로 이동
    loop fill

    popad
    pop ebp
    ret 16           ; 매개변수 4개(4*4=16) 정리
FillArray ENDP
;------------------------------------------
; PrintArray PROC
; 배열을 출력하는 프로시저
; 매개변수:
; [ebp+8]  - 배열의 시작 주소
; [ebp+12] - 배열의 크기 N
;------------------------------------------
PrintArray PROC
    push ebp
    mov ebp, esp
    pushad
    mov edi, [ebp+8]        ;배열의 시작 주소
    mov ecx, [ebp+12]       ;배열의 크기 N
print_loop:
    mov eax, [edi]
    call WriteDec
    mov al, ' '
    call WriteChar

    add edi, 4             ; 다음 요소로 이동
    loop print_loop

    call Crlf

    popad
    pop ebp
    ret 8                   ; 매개변수 2개(4*2=8) 정리
PrintArray ENDP

END main
```

---
## ★★ 2. Summing Array Elements in a Range   
**Q : Create a procedure that returns the sum of all array elements falling within the range j...k (inclusive). Write a test program that calls the procedure twice, passing a pointer to a signed doubleword array, the size of the array, and the values of j and k. Return the sum in the EAX register, and preserve all other register values between calls to the procedure.**    
**A :** 1에서 작성한 배열 내, j~k 범위 내에 속하는 수들의 합 구하기  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
   array1 DWORD 10 DUP(?)  ;첫번째 실행

.code
main PROC
    call Randomize  ;난수 초기화
   ; 배열 채우기 
    push 20                 ;k 
    push 0                  ;j 
    push 10                 ;N 
    push OFFSET array1      
    call FillArray        
    
    push 10
    push 5                ; 배열 내 5~10 값을 가진 수들의 합 계산
    push 10
    push OFFSET array1
    call SumArrRange
    call WriteDec         ;결과 출력
    call Crlf

    push 20
    push 11               ; 배열 내 11~20 값을 가진 수들의 합 계산
    push 10
    push OFFSET array1
    call SumArrRange
    call WriteDec         ;결과 출력
    call Crlf

    exit
main ENDP
;------------------------------------------
; FillArray PROC
; 배열을 난수로 채우는 프로시저
; 매개변수:
; [ebp+8]  - 배열의 시작 주소
; [ebp+12] - 배열의 크기 N
; [ebp+16] - 난수의 최솟값 j
; [ebp+20] - 난수의 최댓값 k
;------------------------------------------
FillArray PROC
    push ebp
    mov ebp, esp
    pushad

    mov edi, [ebp+8]
    mov ecx, [ebp+12]
    mov ebx, [ebp+16]
    mov edx, [ebp+20]
fill:
    mov eax, edx
    sub eax, ebx             
    inc eax          ; k- j + 1
    call RandomRange ; 0~(k - j)
    add eax, ebx     ; j ~ k

    mov [edi], eax   ; 배열에 값 저장
    add edi, 4       ; 다음 요소로 이동
    loop fill

    popad
    pop ebp
    ret 16           ; 매개변수 4개(4*4=16) 정리
FillArray ENDP
;------------------------------------------
; SumArrRange PROC
; 배열중 범위 내 수들의 합을 구하는 프로시저
; 매개변수:
; [ebp+8]  - 배열의 시작 주소
; [ebp+12] - 배열의 크기 N
; [ebp+16] - 범위의 최솟값 j
; [ebp+20] - 범위의 최댓값 k
;------------------------------------------
SumArrRange PROC
    push ebp
    mov ebp, esp
    pushad                 ;모든 레지스터 보존

    mov esi, [ebp+8]      ;배열의 시작 주소
    mov ecx, [ebp+12]     ;배열의 크기 N
    mov ebx, [ebp+16]     ;최솟값 j
    mov edx, [ebp+20]     ;최댓값 k

    mov eax, 0            ;합계 초기화
sum_loop:
    mov edi, [esi]        ;현재 요소 가져오기
    cmp edi, ebx          ;최솟값과 비교
    jl skip_add
    cmp edi, edx          ;최댓값과 비교
    jg skip_add
    add eax, edi          ;합계에 더하기
skip_add:
    add esi, 4            ;다음 요소로 이동
    loop sum_loop

    mov [ebp-4], eax     ;합계를 스택에 저장
    popad                ;레지스터 복원
    mov eax, [ebp-4]     ;합계 반환

    pop ebp
    ret 16               ;매개변수 4개(4*4=16) 정리
SumArrRange ENDP

END main
```

---
## ★★ 3. Test Score Evaluation     
**Q : Create a procedure named CalcGrade that receives an integer value between 0 and 100, and returns a single capital letter in the AL register. Preserve all other register values between calls to the procedure. The letter returned by the procedure should be according to the following ranges:**    
<img width="187" height="125" alt="image" src="https://github.com/user-attachments/assets/c51f97c4-e802-4b09-abfb-143d40f48859" />    
**Write a test program that generates 10 random integers between 50 and 100, inclusive. Each time an integer is generated, pass it to the CalcGrade procedure.  You can test your program using a debugger, or if you prefer to use the book’s library, you can display each integer and its corresponding letter grade. (The Irvine32 library is required for this solution program because it uses the RandomRange procedure.)**  
**A :**
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
   grade BYTE "Score: ", 0
   letter BYTE " -> Grade: ", 0

.code
main PROC
    call Randomize

    mov ecx, 10
gen_loop:
    mov eax, 51          ; 100 - 50 + 1
    call RandomRange    
    add eax, 50          ; 0~50 tp 50~100
    push eax             ; CalcGrade의 인자로서 스택에 점수 푸시

    mov edx, OFFSET grade
    call WriteString
    mov eax, [esp]      ; 스택에서 점수 팝
    call WriteDec

    mov edx, OFFSET letter
    call WriteString
    call CalcGrade      ; 점수에 따른 학점 계산
    call WriteChar
    call Crlf

    add esp, 4          ; 스택 정리
    loop gen_loop

    exit
main ENDP
;----------------------------------------------
; CalcGrade
; 점수를 받아서 학점을 반환하는 프로시저
; 입력 : [ebp+8] = 점수 (0~100)
; 출력 : al = 학점 문자
; 다른 레지스터 보존
;----------------------------------------------
CalcGrade PROC
    push ebp
    mov ebp, esp
    sub esp, 4          ; 지역 변수 공간 확보
    pushad

    mov ebx, [ebp+8]    ; 점수 로드
    mov al, 'F'         ; 기본 학점 F 설정

    cmp ebx, 90
    jge grade_A
    cmp ebx, 80
    jge grade_B
    cmp ebx, 70
    jge grade_C
    cmp ebx, 60
    jge grade_D
    jmp done
grade_A:
    mov al, 'A'
    jmp done
grade_B:
    mov al, 'B'
    jmp done
grade_C:
    mov al, 'C'
    jmp done
grade_D:
    mov al, 'D'
done:
    mov [ebp-4], al  ; 지역 변수에 학점 저장
    popad
    mov al, [ebp-4]  ; 학점 로드
    add esp, 4       ; 지역 변수 공간 해제
    pop ebp
    ret 4
CalcGrade ENDP


END main
```

---
## ★★ 4. College Registration    
**Q : Using the College Registration example from Section 6.7.3 as a starting point, do the following:  
 • Recode the logic using CMP and conditional jump instructions (instead of the .IF and .ELSEIF directives).  
 • Perform range checking on the credits value; it cannot be less than 1 or greater than 30. If an invalid entry is discovered, display an appropriate error message.  
 • Prompt the user for the grade average and credits values.  
 • Display a message that shows the outcome of the evaluation, such as “The student can register” or “The student cannot register”.  
(The Irvine32 library is required for this solution program.)**    
**A :**
```asm
.data
TRUE = 1
FALSE = 0
gradeAverage WORD 275  ;test value
credits WORD 12        ;test value
OkToRegister BYTE ?
.code
 mov OkToRegister, FALSE
 .IF gradeAverage > 350
  mov OkToRegister, TRUE
 .ELSEIF (gradeRegister > 250) && (credits <= 16)
  mov OkToRegister, TRUE
 .ELSEIF (credits <= 12)
  mov OkToRegister, TRUE
 .ENDIF
```
대학 등록 프로그램 예제
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
   TRUE EQU 1
   FALSE EQU 0
   gradePrompt BYTE "Enter your grade average: ", 0
   creditsPrompt BYTE "Enter credits(1~30): ", 0
   errorMsg BYTE "Invalid credits. Must be between 1 and 30.", 0
   canRegisterMsg BYTE "The student can register.", 0
   cannotRegisterMsg BYTE "The student cannot register.", 0

   gradeAverage WORD ?
   credits WORD ?
   OkToRegister BYTE ?

.code
main PROC
    ; 초기화
    call Clrscr
    mov OkToRegister, FALSE

    mov edx, OFFSET gradePrompt
    call WriteString
    call ReadInt
    mov gradeAverage, ax      ; 사용자 입력 저장

    mov edx, OFFSET creditsPrompt
    call WriteString
    call ReadInt
    mov credits, ax           ; 사용자 입력 저장
    ; 학점 유효성 검사
    mov ax, credits
    cmp ax, 1
    jl InvalidCredits
    cmp ax, 30
    jg InvalidCredits

    ; 학점이 유효한 경우, 등록 가능 여부 판단
    ; 조건 1 : gradeAverage > 350
    mov ax, gradeAverage
    cmp ax, 350
    jg registerOk
    ; 조건 2 : gradeAverage > 250 AND credits <= 16
    cmp ax, 250
    jle checkCredits
    cmp ax, 16
    jle registerOk

checkCredits:
    mov ax, credits
    cmp ax, 12
    jg done              ;조건 충족안함
    jmp registerOk
registerOk:
    mov OkToRegister, TRUE
; 조건을 충족하지 않았을 때
done:
    ; 결과 출력
    cmp OkToRegister, TRUE
    je CanRegister
    mov edx, OFFSET cannotRegisterMsg
    call WriteString
    jmp endofP
; 조건을 충족하였을 때
canRegister:
    mov edx, OFFSET canRegisterMsg
    call WriteString
    jmp endofP
; 크레딧 오류
invalidCredits:
    mov edx, OFFSET errorMsg
    call WriteString
endofP:
    call CrLf

    exit
main ENDP



END main
```

---
## ★★★ 5. Boolean Calculator (1)    
**Q : Create a program that functions as a simple boolean calculator for 32-bit integers. It should display a menu that asks the user to make a selection from the following list:  
1. x AND y  
2. x OR y  
3. NOT x  
4. x XOR y   
5. Exit program  

When the user makes a choice, call a procedure that displays the name of the operation about tobe performed. You must implement this procedure using the Table-Driven Selection technique,shown in Section 6.5.4. (You will implement the operations in Exercise 6.) (The Irvine32 libraryis required for this solution program.)**     
**A :**
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
    menu BYTE "Select an operation:", 0
    option1 BYTE "1. x AND y",0
    option2 BYTE "2. x OR y",0
    option3 BYTE "3. NOT x",0
    option4 BYTE "4. x XOR y",0
    option5 BYTE "5. Exit Program",0

    promptX BYTE "Enter value for x: ",0
    promptY BYTE "Enter value for y: ",0
    resultMsg BYTE "The result is: ",0
    opName BYTE "AND", 0, "OR", 0, "NOT", 0, "XOR", 0

    choice DWORD ?
    x DWORD ?
    y DWORD ?
    result DWORD ?

    opTable DWORD OFFSET opName, OFFSET opName + 4, OFFSET opName + 7, OFFSET opName + 11

.code
main PROC
mainLoop:
    mov edx, OFFSET menu
    call WriteString
    call Crlf
    mov edx, OFFSET option1
    call WriteString
    call Crlf
    mov edx, OFFSET option2
    call WriteString
    call Crlf
    mov edx, OFFSET option3
    call WriteString
    call Crlf
    mov edx, OFFSET option4
    call WriteString
    call Crlf
    mov edx, OFFSET option5
    call WriteString
    call Crlf

    call ReadInt
    mov choice, eax
    cmp choice, 5
    je exitProgram

    push choice
    call DisplayOpName
    add esp, 4
    ; 연산 수행
    cmp choice, 1
    je doAND
    cmp choice, 2
    je doOR
    cmp choice, 3
    je doNOT
    cmp choice, 4
    je doXOR
    jmp mainLoop
doAND:
    mov edx, OFFSET promptX
    call WriteString
    call ReadInt
    mov x, eax

    mov edx, OFFSET promptY
    call WriteString
    call ReadInt
    mov y, eax

    mov eax, x
    and eax, y
    mov result, eax

    mov edx, OFFSET resultMsg
    call WriteString
    mov eax, result
    call WriteDec
    call Crlf
    jmp mainLoop
doOR:
    mov edx, OFFSET promptX
    call WriteString
    call ReadInt
    mov x, eax

    mov edx, OFFSET promptY
    call WriteString
    call ReadInt
    mov y, eax

    mov eax, x
    or eax, y
    mov result, eax

    mov edx, OFFSET resultMsg
    call WriteString
    mov eax, result
    call WriteDec
    call Crlf
    jmp mainLoop
doNOT:
    mov edx, OFFSET promptX
    call WriteString
    call ReadInt
    mov x, eax

    not x
    mov eax, x
    mov result, eax

    mov edx, OFFSET resultMsg
    call WriteString
    mov eax, result
    call WriteDec
    call Crlf
    jmp mainLoop
doXOR:
    mov edx, OFFSET promptX
    call WriteString
    call ReadInt
    mov x, eax

    mov edx, OFFSET promptY
    call WriteString
    call ReadInt
    mov y, eax

    mov eax, x
    xor eax, y
    mov result, eax

    mov edx, OFFSET resultMsg
    call WriteString
    mov eax, result
    call WriteDec
    call Crlf
    jmp mainLoop
exitProgram:
    exit
main ENDP
;----------------------------------------------
; DisplayOpName
; 선택된 연산의 이름을 출력
; 입력 : choice (1~4)
;----------------------------------------------
DisplayOpName PROC
    push ebp
    mov ebp, esp
    pushad

    mov eax, [ebp+8]
    dec eax             ; 배열 인덱스 (0~3)
    cmp eax, 3
    ja invalidChoice    ; 유효하지 않은 선택 처리

    mov ebx, eax
    mov eax, opTable[ebx*4]  ; opTable에서 이름 주소 가져오기
    mov edx, eax
    call WriteString
    call Crlf

    jmp done
invalidChoice:
    mov edx, OFFSET menu
    call WriteString
    call Crlf
done:
    popad
    pop ebp
    ret 4
DisplayOpName ENDP
END main
```

---
## ★★★ 6. Boolean Calculator (2)    
**Q : Continue the solution program from Exercise 5 by implementing the following procedures:  
• AND_op: Prompt the user for two hexadecimal integers. AND them together and display the result in hexadecimal.  
• OR_op: Prompt the user for two hexadecimal integers. OR them together and display the result in hexadecimal.  
• NOT_op: Prompt the user for a hexadecimal integer. NOT the integer and display the result in hexadecimal.  
• XOR_op: Prompt the user for two hexadecimal integers. Exclusive-OR them together and display the result in hexadecimal.  
(The Irvine32 library is required for this solution program.)**    
**A :** 입출력을 Int, Dec에서 Hex로 바꾸어 16진수로 변환  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
    menu BYTE "Select an operation:", 0
    option1 BYTE "1. x AND y",0
    option2 BYTE "2. x OR y",0
    option3 BYTE "3. NOT x",0
    option4 BYTE "4. x XOR y",0
    option5 BYTE "5. Exit Program",0

    promptX BYTE "Enter value for x: ",0
    promptY BYTE "Enter value for y: ",0
    resultMsg BYTE "The result is: ",0
    opName BYTE "AND", 0, "OR", 0, "NOT", 0, "XOR", 0

    choice DWORD ?
    x DWORD ?
    y DWORD ?
    result DWORD ?

    opTable DWORD OFFSET opName, OFFSET opName + 4, OFFSET opName + 7, OFFSET opName + 11

.code
main PROC
mainLoop:
    mov edx, OFFSET menu
    call WriteString
    call Crlf
    mov edx, OFFSET option1
    call WriteString
    call Crlf
    mov edx, OFFSET option2
    call WriteString
    call Crlf
    mov edx, OFFSET option3
    call WriteString
    call Crlf
    mov edx, OFFSET option4
    call WriteString
    call Crlf
    mov edx, OFFSET option5
    call WriteString
    call Crlf

    call ReadInt
    mov choice, eax
    cmp choice, 5
    je exitProgram

    push choice
    call DisplayOpName
    add esp, 4
    ; 연산 수행
    cmp choice, 1
    je doAND
    cmp choice, 2
    je doOR
    cmp choice, 3
    je doNOT
    cmp choice, 4
    je doXOR
    jmp mainLoop
doAND:
    mov edx, OFFSET promptX
    call WriteString
    call ReadHex
    mov x, eax

    mov edx, OFFSET promptY
    call WriteString
    call ReadHex
    mov y, eax

    mov eax, x
    and eax, y
    mov result, eax

    mov edx, OFFSET resultMsg
    call WriteString
    mov eax, result
    call WriteHex
    call Crlf
    jmp mainLoop
doOR:
    mov edx, OFFSET promptX
    call WriteString
    call ReadHex
    mov x, eax

    mov edx, OFFSET promptY
    call WriteString
    call ReadHex
    mov y, eax

    mov eax, x
    or eax, y
    mov result, eax

    mov edx, OFFSET resultMsg
    call WriteString
    mov eax, result
    call WriteHex
    call Crlf
    jmp mainLoop
doNOT:
    mov edx, OFFSET promptX
    call WriteString
    call ReadHex
    mov x, eax

    not x
    mov eax, x
    mov result, eax

    mov edx, OFFSET resultMsg
    call WriteString
    mov eax, result
    call WriteHex
    call Crlf
    jmp mainLoop
doXOR:
    mov edx, OFFSET promptX
    call WriteString
    call ReadHex
    mov x, eax

    mov edx, OFFSET promptY
    call WriteString
    call ReadHex
    mov y, eax

    mov eax, x
    xor eax, y
    mov result, eax

    mov edx, OFFSET resultMsg
    call WriteString
    mov eax, result
    call WriteHex
    call Crlf
    jmp mainLoop
exitProgram:
    exit
main ENDP
;----------------------------------------------
; DisplayOpName
; 선택된 연산의 이름을 출력
; 입력 : choice (1~4)
;----------------------------------------------
DisplayOpName PROC
    push ebp
    mov ebp, esp
    pushad

    mov eax, [ebp+8]
    dec eax             ; 배열 인덱스 (0~3)
    cmp eax, 3
    ja invalidChoice    ; 유효하지 않은 선택 처리

    mov ebx, eax
    mov eax, opTable[ebx*4]  ; opTable에서 이름 주소 가져오기
    mov edx, eax
    call WriteString
    call Crlf

    jmp done
invalidChoice:
    mov edx, OFFSET menu
    call WriteString
    call Crlf
done:
    popad
    pop ebp
    ret 4
DisplayOpName ENDP
END main
```

---
## ★★ 7. Probabilities and Colors   
**Q : Write a program that randomly chooses among three different colors for displaying text on the screen. Use a loop to display 20 lines of text, each with a randomly chosen color. The probabilities for each color are to be as follows: white=30%, blue=10%, green=60%. Suggestion: Generate a random integer between 0 and 9. If the resulting integer falls in the range 0 to 2 (inclusive), choose white. If the integer equals 3, choose blue. If the integer falls in the range 4 to 9 (inclusive), choose green. Test your program by running it ten times, each time observing whether the distribution of line colors appears to match the required probabilities.   
(The Irvine32 library is required for this solution program.)**    
**A :**
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
    msg BYTE "Hello, World!", 0
    count DWORD 20
    randNum DWORD ?
.code
main PROC
    call Randomize         ; 난수 초기화
    mov ecx, count           ; 반복 20번
print_loop:
    mov eax, 10
    call RandomRange         ; 0~9 사이의 난수 생성
    mov randNum, eax         ; 난수를 randNum에 저장

    cmp eax, 2
    jbe setWhite
    cmp eax, 3
    je setBlue
    jmp setGreen
setWhite:
    mov al, 0Fh			; 흰색
    jmp setColor
setBlue:
    mov al, 09h			; 파란색
    jmp setColor
setGreen:
    mov al, 0Ah			; 초록색
setColor:
    ; 콘솔 색상 결정
    push eax
    call SetTextColor
    pop eax

    mov edx, OFFSET msg
    call WriteString         ; 메시지 출력
    call Crlf                ; 줄 바꿈
    loop print_loop          ; 루프 반복

    exit
main ENDP

END main
```
색 분포 확인결과 주어진 확률과 비슷하였음  

---
## ★★★ 8. Message Encryption   
**Q : Revise the encryption program in Section 6.3.4 in the following manner: Create an encryption key consisting of multiple characters. Use this key to encrypt and decrypt the plaintext by XORing each character of the key against a corresponding byte in the message. Repeat the key as many times as necessary until all plain text bytes are translated. Suppose, for example, the key were equal to “ABXmv#7”. This is how the key would align with the plain text bytes:**  
<img width="527" height="58" alt="image" src="https://github.com/user-attachments/assets/af7c0bce-fee5-49ce-9a9f-52e2374b3995" />  

**A :**
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc
BUFMAX = 128
.data
    sPrompt BYTE "Enter the plain text: ", 0
    sEncrypt BYTE "Cipher text: ", 0
    sDecrypt BYTE "Decrypted: ", 0

    buffer BYTE BUFMAX+1 DUP(0)
    bufSize DWORD ?

    key BYTE "ABXmv#7", 0
    keyLen DWORD ?
.code
main PROC
    call InputTheString

    mov edx, OFFSET key
    call StrLength
    mov keyLen, eax
    ; 암호화 및 출력
    call TranslateBuffer
    mov edx, OFFSET sEncrypt
    call DisplayMessage

    ; 복호화 및 출력
    call TranslateBuffer
    mov edx, OFFSET sDecrypt
    call DisplayMessage

    exit
main ENDP
;---------------------------------------
; InputTheString
; 사용자에게 평문 입력 요청후
; buffer에 저장
;---------------------------------------
InputTheString PROC
    pushad
    mov edx, OFFSET sPrompt
    call WriteString

    mov ecx, BUFMAX
    mov edx, OFFSET buffer
    call ReadString

    mov bufSize, eax
    call Crlf
    popad
    ret
InputTheString ENDP
;---------------------------------------
; DisplayMessage
; 제목과 buffer의 메시지 출력
; 입력 : edx - 제목 문자열의 주소
;---------------------------------------
DisplayMessage PROC
    pushad
    call WriteString        ; 제목 출력
    mov edx, OFFSET buffer
    call WriteString        ; 메시지 출력
    call Crlf
    call Crlf
    popad
    ret
DisplayMessage ENDP
;---------------------------------------
; TranslateBuffer
; buffer의 각 바이트를 key와 XOR 연산
;---------------------------------------
TranslateBuffer PROC
    pushad
    mov esi, 0            ;메시지 인덱스

L1:
    cmp esi, bufSIze
    jge DoneXOR

    mov eax, esi
    mov ebx, keyLen
    cdq
    idiv ebx
    mov edi, edx

    mov al, buffer[esi]
    xor al, key[edi]
    mov buffer[esi], al

    inc esi
    jmp L1
DoneXOR:
    popad
    ret
TranslateBuffer ENDP
END main
```

---
## ★★ 9. Validating a PIN   
**Q : Banks use a Personal Identification Number (PIN) to uniquely identify each customer. Let us assume that our bank has a specified range of acceptable values for each digit in its customers’ 5-digit PINs. The table shown below contains the acceptable ranges, where digits are numbered from left to right in the PIN. Then we can see that the PIN 52413 is valid.  But the PIN 43534 is invalid because the first digit is out of range. Similarly, 64535 is invalid because of its last digit.**  
<img width="226" height="144" alt="image" src="https://github.com/user-attachments/assets/dd4fc021-fc9b-4074-89a3-59c91e3397b0" />  
**Your task is to create a procedure named Validate_PIN that receives a pointer to an array of byte containing a 5-digit PIN. Declare two arrays to hold the minimum and maximum range values, and use these arrays to validate each digit of the PIN that was passed to the procedure. If any digit is found to be outside its valid range, immediately return the digit’s position (between 1 and 5) in the EAX register. If the entire PIN is valid, return 0 in EAX. Preserve all other register values between calls to the procedure. Write a test program that calls Validate_PIN at least four times, using both valid and invalid byte arrays. By running the program in a debugger, verify that the return value in EAX after each procedure call is valid. Or, if you prefer to use the book’s library, you can display "Valid" or "Invalid" on the console after each procedure call.**  
**A :**
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc
BUFMAX = 128
.data
    minVal BYTE 5, 2, 4, 1, 3
    maxVal BYTE 9, 5, 8, 4, 6

    pin1 BYTE 5,2,4,1,3     ; valid
    pin2 BYTE 4,3,5,3,4     ; invalid
    pin3 BYTE 6,4,5,3,5     ; valid
    pin4 BYTE 7,5,7,2,6     ; valid

    sValid BYTE "Valid", 0
    sInvalid BYTE "Invalid", 0
.code
main PROC
    ; 테스트 1
    mov edx, OFFSET pin1
    call Validate_PIN
    call PrintResult

    ; 테스트 2
    mov edx, OFFSET pin2
    call Validate_PIN
    call PrintResult

    ; 테스트 3
    mov edx, OFFSET pin3
    call Validate_PIN
    call PrintResult

    ; 테스트 4
    mov edx, OFFSET pin4
    call Validate_PIN
    call PrintResult

    exit
main ENDP
;--------------------------------------------------------------
; Validate_PIN
;  입력: edx - PIN 배열의 주소
;  출력: eax - 1 (유효) 또는 유효하지 않은 자리 번호 (1~5)
; 다른 레지스터는 보존
;--------------------------------------------------------------
Validate_PIN PROC
    push ebp
    mov ebp, esp
    sub esp, 4  	   ; 로컬 변수 공간   
    pushad
    mov ecx, 5              ; 자리 수 5개
    mov esi, 0              ; 인덱스 0부터 시작

CheckLoop:
    mov al, [edx + esi]     ; PIN의 현재 자리 숫자 로드
    mov bl, [minVal + esi]
    mov bh, [maxVal + esi]

    cmp al, bl
    jl InVDigit      ; 최소값 미만이면 유효하지 않음
    cmp al, bh       
    jg InVDigit      ; 최대값 초과이면 유효하지 않음

    inc esi          ; 다음 자리로 이동
    loop CheckLoop

    mov eax, 0 			; 모든 자리 유효
    jmp done
InVDigit:
    mov eax, esi        ; 0-based 인덱스
    inc eax             ; 자리 번호는 1부터 시작
done:
    mov [ebp-4], eax
    popad
    mov eax, [ebp-4]
    add esp, 4
    pop ebp
    ret 4
Validate_PIN ENDP

;--------------------------------------------------------------
; PrintResult
;  입력: eax - Validate_PIN의 반환 값
;  출력: 결과 출력 eax가 0이면 "Valid", 아니면 "Invalid"
;--------------------------------------------------------------
PrintResult PROC
    pushad
    cmp eax, 0
    je PrintValid               ; 0이면 유효

    ; 유효하지 않음
    mov edx, OFFSET sInvalid    ; 출력할 문자열 주소를 설정
    call WriteString
    call Crlf
    jmp PrintResultDone

;유효하면 Valid 출력
PrintValid:
    mov edx, OFFSET sValid
    call WriteString
    call Crlf

PrintResultDone:
    popad
    ret
PrintResult ENDP
END main
```

---
## ★★★★ 10. Parity Checking   
**Q : Data transmission systems and file subsystems often use a form of error detection that relies on calculating the parity (even or odd) of blocks of data. Your task is to create a procedure that returns True in the EAX register if the bytes in an array contain even parity, or False if the parity is odd. In other words, if you count all the bits in the entire array, their count will be even or odd. Preserve all other register values between calls to the procedure. Write a test program that calls your procedure twice, each time passing it a pointer to an array and the length of the array. The procedure’s return value in EAX should be 1 (True) or 0 (False). For test data, create two arrays containing at least 10 bytes, one having even parity, and another having odd parity. **  
>Tip:  Earlier in this chapter, we showed how you can repeatedly apply the XOR instruction to a sequence of byte values to determine their parity. So, this suggests the use of a loop. But be careful, since some machine instructions affect the Parity flag, and others do not. You can find this out by looking at the individual instructions in Appendix B. The code in your loop that checks the parity will have to carefully save and restore the state of the Parity flag to avoid having it unintentionally modified by your code.  

**A :**
```asm

```

---

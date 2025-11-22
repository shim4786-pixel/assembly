## main.asm 저장용
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc
; ArraySum Program (main)

EXTERN PromptForIntegers@0:PROC
EXTERN ArraySum@0:PROC, DisplaySum@0:PROC

ArraySum          EQU ArraySum@0
PromptForIntegers EQU PromptForIntegers@0
DisplaySum        EQU DisplaySum@0

Count = 3

.data
    array DWORD Count DUP(?)
    sum DWORD ?

    prompt1 BYTE "Enter 3 integers: ", 0
    prompt2 BYTE "The sum is: ", 0
.code
main PROC
    call Clrscr
    ; 사용자로부터 정수 입력받기
    push Count
    push OFFSET array
    push OFFSET prompt1
    call PromptForIntegers
    ; array 배열에 5개의 정수 저장 완료

    ; 배열의 합계를 계산하기
    push Count
    push OFFSET array
    call ArraySum
    ; EAX에 합계 반환됨

    ; 반환된 합계를 변수에 저장한 후 결과 출력하기
    mov sum, eax
    push sum
    push OFFSET prompt2
    call DisplaySum
    ; The sum is: ~~~ 하고 합계 값 출력

    exit 
main ENDP

END main
```

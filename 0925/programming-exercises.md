# Programming Exercise 문제 풀이

## 문제 1 Integer Expression Calculation  
**Q: Using the AddTwo program from Section 3.2 as a reference, write a program that calculates the following expression, using registers: A = (A + B) − (C + D). Assign integer values to the EAX, EBX, ECX, and EDX registers.**    
**A:** AddTwo.asm 을 참고하여 A=(A+B)-(C-D) 코드를 작성한다.  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.code
main PROC
	mov eax, 20
	mov ebx, 10
	mov ecx, 5
	mov edx, 2

	add eax, ebx      ; eax = eax + ebx
	add ecx, edx      ; ecx = ecx + edx
	sub eax, ecx      ; eax = eax - ecx
	call DumpRegs
	exit
main ENDP

END main
```
출력값  
```asm
EAX=00000017  EBX=0000000A  ECX=00000007  EDX=00000002
  ESI=00DE10AA  EDI=00DE10AA  EBP=006FF960  ESP=006FF954
  EIP=00DE367B  EFL=00000206  CF=0  SF=0  ZF=0  OF=0  AF=0  PF=1
```
EAX에 정상적으로 23이 출력되었다.  

---
## 문제 2 Symbolic Integer Constants    
**Q: Write a program that defines symbolic constants for all seven days of the week. Create an array variable that uses the symbols as initializers.**  
**A:** 모든 요일을 기호 상수로 정의하고 기호를 초기값으로 사용하는 배열 변수를 만든다. ecx값을 1씩 감소하며 반복하는 loop를 사용하였다.  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
Monday EQU 1
Tuesday EQU 2
Wednesday EQU 3
Thursday EQU 4
Friday EQU 5
Saturday EQU 6
Sunday EQU 7

WeekDays DWORD Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday
WeekSize = LENGTHOF WeekDays

.code
main PROC
	mov ecx, WeekSize  ; 배열 길이 (7)
	mov ebx, OFFSET WeekDays ; 배열의 시작 주소
	
print_loop:
	mov eax, [ebx]     ; 배열 요소를 eax로 로드
	call WriteDec      ; 요소 출력
	mov al, ' '      ; 공백 문자
	call WriteChar   ; 공백 출력
	add ebx, TYPE DWORD ; 다음 요소로 이동
	loop print_loop    ; ecx 감소하며 루프 반복

	exit
main ENDP

END main
```
결과   
```asm
1 2 3 4 5 6 7
```

---
## 문제 3 Data Definitions  
**Q: Write a program that contains a definition of each data type listed in Table 3-2 in Section 3.4. Initialize each variable to a value that is consistent with its data type.**  
**A:** 표 3-2를 참고하여 각 자료형을 정의하고 그에 맞는 적절한 값을 초기화한다.   
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
	A BYTE 255				;8비트 부호없는 정수
	B SBYTE -128			;8비트 부호있는 정수
	C WORD 65535			;16비트 부호없는 정수
	D SWORD -32768			;16비트 부호있는 정수
	E DWORD 4294967295		;32비트 부호없는 정수
	F SDWORD -2147483648	;32비트 부호있는 정수
	G FWORD 0FFFFFFFFh							;48비트 부호없는 정수(실제주소 모드에서 far 포인터)
	H QWORD 0FFFFFFFFFFFFFFFFh					;64비트 부호없는 정수
	I TBYTE 800000000000001234h					;80비트 정수
	J REAL4 -1.2					;32비트 IEEE 짧은 실수
	K REAL8 3.2E-260				;64비트 IEEE 긴 실수
	L REAL10 4.6E-4096				;80비트 IEEE 확장 실수

.code
main PROC
	;~~~ Your code goes here ~~~
	exit
main ENDP

END main
```

---
## 문제 4 Symbolic Text Constants    
**Q: Write a program that defines symbolic names for several string literals (characters between quotes). Use each symbolic name in a variable definition.**  
**A:** 몇 개의 문자열 리터럴에 대해 기호 이름을 정의하고, 변수 정의에 각 기호 이름을 사용한다.  
```asm
.386
.MODEL FLAT, STDCALL
.STACK 4096
include Irvine32.inc

.data
	; 세 개의 문자열에 대해 기호 이름을 정의
	greeting TEXTEQU <"Hello, World!"> ；
	question TEXTEQU <"How are you? "> ；
	farewell TEXTEQU <"Goodbye!"> ；

	; 각 기호 이름을 사용하여 변수를 정의
	message1 BYTE  greeting, 0
	message2 BYTE  question, 0
	message3 BYTE  farewell, 0

.code
main PROC
	; WriteString은 EDX 레지스터에 문자열의 주소를 기대함
	mov edx, OFFSET message1
	call WriteString
	call Crlf

	mov edx, OFFSET message2
	call WriteString
	call Crlf

	mov edx, OFFSET message3
	call WriteString
	call Crlf
	exit
main ENDP

END main
```
결과
｀｀｀

－－－
## 문제 1 Integer Expression Calculation  
**Q:**  
**A:**

---
## 문제 1 Integer Expression Calculation  
**Q:**  
**A:**

---

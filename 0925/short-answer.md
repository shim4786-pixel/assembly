# Short Answer 문제 풀이  

## 문제 1  
**Q: Provide examples of three different instruction mnemonics.**    
**A:** MOV, ADD, SUB   

---
## 문제 2  
**Q: What is a calling convention, and how is it used in assembly language declarations?**   
**A:** 호출 규약(Calling Convention)이란, 함수 호출시 인수 전달방법과 반환값을 처리하는 방식에 대한 규칙이다. '.model flat, stdcall' 과 같이 선언된다. (32비트에서)  

---
## 문제 3  
**Q: How do you reserve space for the stack in a program?**   
**A:** **.stack** 지시어로 스택 공간 예약을 받는다. (.stack 4096)  

---
## 문제 4  
**Q: Explain why the term assembler language is not quite correct.**   
**A:** 어셈블러는 프로그램의 이름이다. 어셈블리 언어를 기계어로 번역해 주는 프로그램이다. **Assembly Language** 가 맞다.   

---
## 문제 5  
**Q: Explain the difference between big endian and little endian. Also, look up the origins of this term on the Web.**   
**A:** 0x1234 저장시.  
빅 엔디안은 **MSB** 를 가장 낮은 주소에 저장한다. (12 34)  
리틀 엔디안은 **LSB** 를 가장 낮은 주소에 저장한다. (34 12)  
조너선 스위프트의 소설 **걸리버 여행기** 에서 유래하였다.  
릴리퍼트라는 소인국 사람들이 달걀을 깰 때 뭉툭한 쪽을 먼저 깨는 사람들을 '빅 엔디언', 뾰족한 쪽을 먼저 깨는 사람들을 '리틀 엔디언' 이라고 부른데서 시작하였다.  

---
## 문제 6  
**Q: Why might you use a symbolic constant rather than an integer literal in your code?**   
**A:** **Constant**는 값에 의미를 부여하고, 값이 바뀌더라도 상수 하나만 수정하면 되기에 유지보수가 용이하다. 코드의 가독성과 재사용성을 높이기에 사용된다.  

---
## 문제 7  
**Q: How is a source file different from a listing file?**    
**A:** **소스 파일**은 유저가 작성한 코드, **리스팅 파일**은 어셈블러가 어셈블링 중 자동 생성하는 분석 결과 문서이다.  

---
## 문제 8  
**Q: **How are data labels and code labels different?**    
**A:** **데이터 레이블**은 메모리에 저장된 변수나 상수 등 데이터를 참조하기 위해 사용하는 이름, **코드 레이블**은 명령어의 위치를 참조하거나 점프하기 위한 이름이다.  

---
## 문제 9  
**Q: (True/False): An identifier cannot begin with a numeric digit.**     
**A:** True  
식별자는 알파벳 또는 '_'로 시작해야 한다.  

---
## 문제 10  
**Q: (True/False): A hexadecimal literal may be written as 0x3A.**   
**A:** False  
어셈블리어에서는 16진수 리터럴을 숫자 뒤에 **h**를 붙이는 방법으로 표기해야 한다.  

---
## 문제 11  
**Q: (True/False): Assembly language directives execute at runtime.**   
**A:** False  
지시어는 실행 시간에 작동되지는 않지만 변수나 매크로 등을 정의할 수 있게 해준다.  

---
## 문제 12  
**Q: (True/False): Assembly language directives can be written in any combination of uppercase and lowercase letters.**   
**A:** True  
어셈블리어는 지시어의 대소문자를 구분하지 않는다.  

---
## 문제 13  
**Q: Name the four basic parts of an assembly language instruction.**    
**A:** 레이블, 명령어, 피연산자, 주석   
-Lable : 명령어의 위치를 식별하거나 점프할 수 있도록 이름을 붙인 부분 (선택적임)   
-Mnemonic : MOV, ADD, JMP와 같이 CPU가 수행할 동작을 나타내는 기호  (필수)   
-Operands : AX, BX등 명령어가 처리할 데이터나 주소 (대개 필수)   
-Comments : 코드의 의미를 설명하는 부분으로, 실행되지 않음(ex=> ;블라블라) (선택적임)    

---
## 문제 14  
**Q: (True/False): MOV is an example of an instruction mnemonic.**     
**A:** True  
MOV는 어셈블리 언어에서 가장 기본적인 니모닉 중 하나이다.  

---
## 문제 15  
**Q: (True/False): A code label is followed by a colon (:), but a data label does not end with a colon.**   
**A:** True  
코드 레이블은 명령어의 위치를 나타내며 :으로 끝나야 어셈블러가 이를 인식한다.  
반면 데이터 레이블은 콜론 없이 이름만으로 정의한다.  

---
## 문제 16  
**Q: Show an example of a block comment.**   
**A:**
```asm
COMMENT  !
 This line is a comment.
 This line is also a comment.
 !
```

---
## 문제 17  
**Q: Why is it not a good idea to use numeric addresses when writing instructions that access variables?**  
**A:** 숫자 주소를 직접 사용하면 가독성이 떨어질 뿐더러, 프로그램 구조가 바뀌어 변수가 달라지면 오류가 발생할 수 있기 때문이다. 또한 그런식으로 하드코딩 된 주소는 다른 운영체제에서 작동하지 않을 수 있다.  

---
## 문제 18  
**Q: What type of argument must be passed to the ExitProcess procedure?**   
**A:** 32비트 정수형 인수(DWORD)를 전달해야 한다.  

---
## 문제 19  
**Q: Which directive ends a procedure?**  
**A:** ENDP  

---
## 문제 20  
**Q: In 32-bit mode, what is the purpose of the identifier in the END directive?**  
**A:** 프로그램의 시작 지점을 명시한다.  

---
## 문제 21  
**Q: What is the purpose of the PROTO directive?**  
**A:** 프로그램 외부에 있는 프로시저를 식별하도록 한다.  
**ExitProcess PROTO** 라고 하면 외부에 ExitProcess라는 프로시저가 있음을 어셈블러에게 알리며 이후 코드에서 해당 함수를 호출 가능하다.  

---
## 문제 22  
**Q: (True/False): An Object file is produced by the Linker.**    
**A:** False  
오브젝트 파일은 어셈블러가 생성한다. 링커는 오브젝트 파일을 결합하여 실행파일을 만드는 역할을 한다.  

---
## 문제 23  
**Q: (True/False): A Listing file is produced by the Assembler.**  
**A:** True  
리스팅 파일은 어셈블러가 자동으로 생성하는 분석용 문서로, 소스 코드, 대응하는 기계어 코드, 명령어의 메모리 주소, 오류 메세지 등을 포함한다.  

---
## 문제 24  
**Q: (True/False): A link library is added to a program just before producing an Executable file.**  
**A:** True  
링크 라이브러리는 링커가 오브젝트 파일과 결합하여 실행 파일을 만들 때 추가되므로 실행 파일을 만들기 직전에 추가된다.  

---
## 문제 25  
**Q: Which data directive creates a 32-bit signed integer variable?**  
**A:** SDWORD  

---
## 문제 26  
**Q: Which data directive creates a 16-bit signed integer variable?**  
**A:** SWORD  

---
## 문제 27  
**Q: Which data directive creates a 64-bit unsigned integer variable?**  
**A:** QWORD  

---
## 문제 28  
**Q: Which data directive creates an 8-bit signed integer variable?**  
**A:** SBYTE  

---
## 문제 29  
**Q: Which data directive creates a 10-byte packed BCD variable?**  
**A:** TBYTE  

---

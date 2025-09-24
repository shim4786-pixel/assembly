# Review Questions 문제 풀이# Review Questions 문제 풀이   

## 문제 1  
**Q:** In 32-bit mode, aside from the stack pointer (ESP), what other register points to variables on the stack?   
**A:** EBP (Base pointer)   
**설명:** ESP는 항상 스택의 최상단을 가리키는 동적 포인터인 반면에, EBP는 함수 프레임의 기준점을 가리키므로 안정적인 주소 계산이 가능한 기준 포인터이다. 스택의 push/pop 연산으로 인해 지속적으로 변화하는 ESP와 달리, EBP는 항상 함수의 고정된 위치를 가리키기 때문에 함수의 매개변수나 지역 변수에 접근하기 용이하다.    

---
## 문제 2  
**Q:** Name at least four CPU status flags.   
**A:** CF, PF, AC, ZF, SF, OF   
**설명:** 다음은 EFLAGS 레지스터의 주요 플래그들이다.   
1. CF(Carry Flag): 부호 없는 연산에서 자리 올림/내림이 발생하였는지 나타낸다.    
2. PF(Parity Flag): 연산 결과의 하위 바이트(8비트)에 1이 짝수 개 있으면 설정된다.   
3. AC(Auxiliary Carry Flag): (Binary-Coded Decimal 연산에서) 하위 4비트에서 자리올림이 발생하였는지 나타낸다.  
4. ZF(Zero Flag): 연산 결과가 0이면 설정된다. 주로 조건 분기, 루프 제어에 사용된다.  
5. SF(Sign Flag): 연산 결과가 음수라면 설정된다. MSB를 기준으로 판단한다.   
6. OF(Overflow Flag): 부호 있는 정수 연산에서 결과가 표현 범위를 초과했을 때(오버플로우) 설정된다.  


---
## 문제 3    
**Q:** Which flag is set when the result of an unsigned arithmetic operation is too large to fit into the destination?  
**A:** CF   
**설명:** 부호 없는 연산에서 자리 올림이 발생하였을 때 설정되는 건 캐리 **플래그(CF)** 이다. 이는 레지스터의 크기를 초과할 때 설정된다. 반면, 부호 있는 연산에서는 **오버플로우 플래그(OF)** 를 확인해야 한다.   

---
## 문제 4  
**Q:** Which flag is set when the result of a signed arithmetic operation is either too large or too small to fit into the destination?   
**A:** OF    
**설명:** 부호 있는 연산에서 결과값의 크기가 너무 작거나 크면 **오버플로우 플래그(OF)** 가 설정된다.   

---
## 문제 6  
**Q:** Which flag is set when an arithmetic or logical operation generates a negative result?   
**A:** SF  
**설명:** 산술/논리 연산의 결과가 음수라면 **사인 플래그(SF)** 가 설정된다. 이는 결과가 음수임을 나타내는 플래그이다.   

---
## 문제 7  
**Q:** Which part of the CPU performs floating-point arithmetic?   
**A:** FPU  
**설명:** 실수 연산을 빠르고 정확하게 수행하는 CPU의 연산 장치인 **FPU**(Floating-point unit)가 해당 역산을 수행한다.     

---
## 문제 8  
**Q:** On a 32-bit processor, how many bits are contained in each floating-point data register?   
**A:** 8개의 80비트짜리 레지스터   
**설명:** 32비트라고 해서 부동소수점 레지스터가 32비트인 것은 아니다. 정확한 크기는 부동소수점 아키텍처에 따라 다르겠지만 x87 FPU는 각 80비트로 구성된 ST(0)에서 ST(7)까지 8개의 레지스터를 가진다.   

---
## 문제 25  
**Q:** At which level(s) can an assembly language program manipulate input/output?  
**A:** 고급 언어, 운영체제, BIOS   
**설명:** pdf 교재의 2.5.1을 참고  
고급 언어 : C++, Java같은 고급 언어는 입출력을 수행하는 함수를 포함한다. 이러한 함수들은 다양한 컴퓨터 시스템에서 작동하여 이식성이 높고, 특정 운영체제에 의존하지 않는다.  
운영체제 : 사용자는 API라는 라이브러리를 통해 운영체제의 함수를 호출 가능하다. 이때 운영체제는 문자열을 파일에 쓰거나, 키보드에서 읽고, 메모리 블록을 할당하는 등 작업을 제공한다.  
BIOS : BIOS는 하드웨어 장치와 직접 통신하는 저수준 서브루틴들의 집합이다. 이는 컴퓨터 제조업체에 의해 설치되며, 해당 컴퓨터의 하드웨어에 맞게 맞춤화되어 있다.  

---
## 문제 26    
**Q:** Why do game programs often send their sound output directly to the sound card’s hardware ports?    
**A:** 속도 최적화, 정밀 제어, 하드웨어 활용 극대화  
**설명:** 게임에서는 적절한 음향 효과를 실시간으로 전달하는 것이 중요하다. 소리가 뒤늦게 들리면 몰입감이 떨어지므로 실시간 처리가 거의 필수적이다. 운영체제를 거치면 과정이 복잡하고 시간 지연이 발생하므로 이러는 점도 있다. 또한 더 빠를 뿐만 아니라, 소리의 정밀한 제어가 가능하기 때문이다.   

---

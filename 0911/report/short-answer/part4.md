# Short Answer - Part 4 (문제 22~28)

## 문제 22  
**Q:**  What are the hexadecimal and decimal representations of the ASCII character capital G?   
**A:**  
ASCII 표에 따르면 대문자 G의 10진수 값은 **71**이다.  
16진수 값은 **47**이다.  

---

## 문제 23  
**Q:**  Challenge: What is the largest decimal value you can represent, using a 129-bit unsigned
 integer?  
**A:**  
129비트의 부호 없는 정수로 표현 가능한 최대 10진수 값을 알기 위해서는 비트 수와 최대값의 관계를 알아야 한다.  
부호 없는 정수는 모든 비트를 값 표현에 사용하므로 최대값은  
  **2^129-1**이다.  
 (GPT에 물어본 결과 정확한 최대값은 **680,564,733,841,876,926,926,749,214,863,536,422,911**이다.)

---

## 문제 24  
**Q:**  Challenge: What is the largest decimal value you can represent, using a 86-bit signed
 integer?  
**A:**  
부호 있는 정수는 **MSB**를 부호 비트로 사용하므로 1비트를 제외한 나머지 85비트만을 정수값으로 사용한다. 따라서 최대값은  
**2^85-1**이다.  
(GPT에 물어본 결과 정확한 최대값은 **38,685,626,227,668,133,590,597,631**이다.)  

---

## 문제 25  
**Q:**  Create a truth table to show all possible inputs and outputs for the boolean function
 described by ¬(A ∨ B).  
**A:**  
A or B의 NOT 함수 표를 구한다.  
| A | B | A ∨ B | ¬(A ∨ B) |
|---|---|--------|-----------|
| 0 | 0 |   0    |     1     |
| 0 | 1 |   1    |     0     |
| 1 | 0 |   1    |     0     |
| 1 | 1 |   1    |     0     |


---

## 문제 26  
**Q:**  Create a truth table to show all possible inputs and outputs for the boolean function described by (¬A ∧ ¬B). How would you describe the rightmost column of this table in relation to the table from question number 25? Have you heard of De Morgan’s Theorem?  
**A:**  
notA and notB의 함수 표를 구한다.  
| A | B | ¬A | ¬B | ¬A ∧ ¬B |
|---|---|----|----|---------|
| 0 | 0 |  1 |  1 |    1    |
| 0 | 1 |  1 |  0 |    0    |
| 1 | 0 |  0 |  1 |    0    |
| 1 | 1 |  0 |  0 |    0    |
이의 마지막 열은 25번에서 구했던 ¬(A ∨ B)의 결과값과 완전히 같으며 드모르간의 법칙과 관련이 있다.  
드모르간의 법칙을 간단히 말하면 ¬(𝐴∨𝐵)≡(¬𝐴)∧(¬𝐵), ¬(𝐴∧𝐵)≡(¬𝐴)∨(¬𝐵)와 같이 논리 연산에서 NOT이 and와 or을 어떻게 변환하는지 보이는 이론이며, 25번과 해당 문제의 표를 보아 알 수 있다.  

---

## 문제 27  
**Q:**  If a boolean function has four inputs, how many rows are required for its truth table?  
**A:**  
입력 개수가 4개라면 :  
각 입력은 0과 1 두 가지의 경우의 수를 가진다. 입력이 n개라면 조합 수는 **2ⁿ**이므로  
**2⁴ = 16**이다. 즉, 16행이 필요하다. (입력값 및 출력값 표현까지 포함하면 총 **17**행이 필요하다.)  

---

## 문제 28  
**Q:**  How many selector bits are required for a four-input multiplexer?  
**A:**  
입력이 4개라면  I0 ~ I3으로 표현 가능한데, 선택 비트로 모두 표현하면 I0=00 , I1=01 , I2=10 , I3=11과 같이 두 개의 비트로 표현가능한 것을 알 수 있다.  
따라서, 필요한 선택 비트(selector bits)는 **2개**이다.  

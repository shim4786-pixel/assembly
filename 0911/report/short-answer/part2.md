# Short Answer - Part 2 (문제 8~14)

## 문제 8  
**Q:**  What is the binary representation of the following hexadecimal numbers?  
**A:**  
a. 0126F9D4  >> 0000 0001 0010 0110 1111 1001 1101 0100    
b. 6ACDFA95  >> 0110 1010 1100 1101 1111 1010 1001 0101   
c. F69BDC2A  >> 1111 0110 1001 1011 1101 1100 0010 1010   

---

## 문제 9  
**Q:**  What is the unsigned decimal representation of each of the following hexadecimal integers?  
**A:**  
a. 3A  >> 16¹x3 + 1x10 = 58  
b. 1BF  >> 16²x1 + 16¹x11 + 1x15 = 447  
c. 1001  >> 16³x1 + 1x1 = 4097  

---

## 문제 10  
**Q:**  What is the unsigned decimal representation of each of the following hexadecimal integers?  
**A:**  
a. 62  >> 16¹x6 + 1x2 = 98  
b. 4B3  >> 16²x4 + 16¹x11 + 1x3 = 1203  
c. 29F  >> 16²x2 + 16¹x9 + 1x15 = 671  

---

## 문제 11  
**Q:**  What is the 16-bit hexadecimal representation of each of the following signed decimal integers?  
**A:**  
주어진 음수를 양수로 바꾸어 2진수를 구한 뒤 2의 보수를 취한다.  
a. -24  >> 24 = 11000 >2의 보수> 1111 1111 1110 1000 = FFE8  
b. -331  >> 331 = 101001011 >2의 보수> 1111 1110 1011 0101 = FEB5  

---

## 문제 12  
**Q:**  What is the 16-bit hexadecimal representation of each of the following signed decimal integers?  
**A:**  
주어진 음수를 양수로 바꾸어 2진수를 구한 뒤 2의 보수를 취한다.   
a. -21  >> 21 = 10101 >2의 보수> 1111 1111 1110 1011 = FFEB  
b. -45  >> 45 = 101101 >2의 보수> 1111 1111 1101 0011 = FFD3  

---

## 문제 13  
**Q:**  The following 16-bit hexadecimal numbers represent signed integers. Convert each to
 decimal.  
**A:**  
부호 있는 16진수 숫자이므로 MSB를 보아 양수인지 음수인지 확인 후 음수라면 2의 보수를 취해 구한다.  
a. 6BF9  >> 0110 1011 1111 1001 (양수) >10진수 변환> 27641  
b. C123  >> 1100 0001 0010 0011 (음수) >2의 보수> 0011 1110 1101 1101 >10진수 변환> -16093  

---

## 문제 14  
**Q:**  The following 16-bit hexadecimal numbers represent signed integers. Convert each to
 decimal.  
**A:**  
부호 있는 16진수 숫자이므로 MSB를 보아 양수인지 음수인지 확인 후 음수라면 2의 보수를 취해 구한다.  
a. 4CD2  >> 0100 1100 1101 0010 (양수) >10진수 변환> 19666  
b. 8230  >> 1000 0010 0011 0000 (음수) >2의 보수> 0111 1101 1101 0000 >10진수 변환> -32208  

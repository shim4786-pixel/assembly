# Algorithm Workbench (따로 주석이 없다면 C언어로 작성됨)

## 문제 1
**Q**: Write a function that receives a string containing a 16-bit binary integer. The function must return the string’s integer value.  
풀이: 16자리의 2진수 값을 입력받아 10진수 정수로 변환하는 코드를 작성한다.  
코드:  
'''c  
#include <stdio.h>

int binaryToInt(const char *binStr){  //문자열 포인터를 받아 정수로 변환하는 함수 
	int result = 0;
	for(int i=0; i<16; i++){
		if(binStr[i]=='1') //해당 문자가 1이라면 
			result+=1 << (15-i);  //그 위치에 해당하는 2의 거듭제곱 값을 더한다. 
	}
	return result;
}
int main(){
	char binary[17];  //맨 끝의 공백 문자열까지 포함하여 17개 
	printf("16자리 이진 문자열을 입력하세요: ");
	scanf("%16s", binary);  //문자열을 입력받기 
	int decimal= binaryToInt(binary);  //문자열을 정수로 변환 
	printf("변환된 10진수 값: %d\n", decimal);
	
	return 0;
}


---
## 문제 2
**Q**: 


---
## 문제 3
**Q**: 


---
## 문제 4
**Q**: 


---
## 문제 5
**Q**: 


---
## 문제 6
**Q**: 


---
## 문제 7
**Q**: 


---

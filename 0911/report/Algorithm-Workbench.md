# Algorithm Workbench (따로 주석이 없다면 C언어로 작성됨)

## 문제 1
**Q:** Write a function that receives a string containing a 16-bit binary integer. The function must return the string’s integer value.  
**풀이:** 16자리의 2진수 값을 입력받아 10진수 정수로 변환하는 코드를 작성한다.  
**코드:**  
```c
#include <stdio.h>

int binaryToInt(const char *binStr){  // 문자열 포인터를 받아 정수로 변환하는 함수 
    int result = 0;
    for(int i = 0; i < 16; i++){
        if(binStr[i] == '1') // 해당 문자가 '1'이라면
            result += 1 << (15 - i);  // 그 위치에 해당하는 2의 거듭제곱 값을 더한다
    }
    return result;
}

int main(){
    char binary[17];  // 맨 끝의 널 문자('\0')까지 포함하여 17칸 확보
    printf("16자리 이진 문자열을 입력하세요: ");
    scanf("%16s", binary);  // 문자열을 입력받기
    int decimal = binaryToInt(binary);  // 문자열을 정수로 변환
    printf("변환된 10진수 값: %d\n", decimal);

    return 0;
}
```


---
## 문제 2
**Q**: Write a function that receives a string containing a 32-bit hexadecimal integer. The function must return the string’s integer value.  
**풀이:**  32비트 크기의 16진수 정수를 입력받아 10진수 정수 값을 반환한다.  
**코드:**  
```c
#include <stdio.h>
#include <ctype.h> //tolower을 쓰기 위함(대문자를 소문자로 변경해 줌) 

int hexToInt(const char *hexStr){  //16진수 문자열을 10진수로 변환할 것 
	int result=0;
	for(int i=0; hexStr[i]!='\0'; i++){  //문자열의 끝까지 확인하며 반복 
		char ch=tolower(hexStr[i]);  //대문자가 있다면 소문자로 변환 
		int value;
		value=(ch>='0' && ch<='9')? ch-'0':ch-'a'+10;  //0~9는 그대로, a~f는 10~15로 
		result=result*16+value;  //2*16^3+3*16^2+4*16^1+5*16^0을 ((2*16+3)*16+4)*16+5 와 같이표현 
	}
	return result;
}

int main(){
	char hexStr[9];  // '\0'까지 총 9자리의 배열을 생성 
	printf("32비트 16진수 문자열을 입력하세요(8자리): ");
	scanf("%8s", hexStr);  //최대 8글자까지 입력받음 
	int decimal=hexToInt(hexStr);
	printf("변환된 10진수 값: %d\n", decimal);
	
	return 0; 
}
```


---
## 문제 3
**Q**: Write a function that receives an integer. The function must return a string containing the binary representation of the integer.  
**풀이:**  10진수 정수를 입력받아 2진 코드로 출력하는 함수를 작성한다.  
python에서는 매우 쉽게 구현이 가능하다.  
**코드:**   


---
## 문제 4
**Q**: **풀이:**  
**코드:**  


---
## 문제 5
**Q**: 
**풀이:**  
**코드:**  


---
## 문제 6
**Q**:  
**풀이:**  
**코드:**  


---
## 문제 7
**Q**:  
**풀이:**  
**코드:**  


---

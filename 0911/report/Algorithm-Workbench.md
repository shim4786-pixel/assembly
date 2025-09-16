# Algorithm Workbench (C언어로 작성됨)

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
**코드:**   
```c
#include <stdio.h>

void intToBinary(unsigned int n, char *binStr){  //n을 이진수로 변환하여 binStr에 저장할 거임 
	int started=0;  //이진수 문자열에 첫 1이 나왔는지 보는 용도 
	int index=0;
	for(int i=31; i>=0; i--){  //32비트 정수의 각 비트를 MSB>LSB 순으로 확인 
		int bit=(n>>i)&1;  //n을 오른쪽으로 i만큼 이동하여 마지막 비트만 추출 
		if(bit==1)  //처음으로 1이 등장하면  
			started=1;  //얘를 1로 두고 이 이후부터 모든 비트를 문자열에 추가 
		if(started)  //started가 1이면 현재비트를 1또는 0으로 저장 
			binStr[index++]=bit? '1':'0';		
	}
	if(index==0)
		binStr[index++]='0';
	binStr[index]='\0';
} 

int main(){
	unsigned int n;
	char binary[33];
	printf("정수를 입력하세요: ");
	scanf("%u", &n);
	intToBinary(n, binary);
	printf("이진수 표현: %s\n", binary);
	
	return 0;
}
```


---
## 문제 4
**Q**:   Write a function that receives an integer. The function must return a string containing the hexadecimal representation of the integer.  
**풀이:**  10진수의 정수를 입력받아 16진수에 해당하는 값을 출력한다.  
**코드:**  해당 코드는 음수까지 입력받을 수 있다.  
```c
#include <stdio.h>

void intToHex(int n, char *hexStr){
	char hexD[] = "0123456789ABCDEF";
	int started = 0; int index = 0; unsigned int abs;
	if(n<0){                   //음수라면 
		hexStr[index++]='-';   //부호붙이기 
		abs = -n;              //절댓값으로 전환 
	}else abs = n;             //아니면 그냥 
	for(int i=28; i>=0; i-=4){
		int a = (abs>>i) & 0xF;
		if(a != 0 || started){
			hexStr[index++]=hexD[a];
			started=1;
		}
	}
	if(!started) hexStr[index++]='0';
	hexStr[index]='\0';
}
int main(){
	int n;
	char hex[10];
	printf("정수를 입력하세요: ");
	scanf("%d", &n);
	intToHex(n, hex);
	printf("16진수 표현: %s\n", hex);
	return 0;
}
```


---
## 문제 5
**Q**: Write a function that adds two digit strings in base b, where 2 ≤ b ≤ 10. Each string may contain as many as 1,000 digits. Return the sum in a string that uses the same number base.  
**풀이:**  기수가 2에서 10사이인 최대 1000자리 정수 두 개의 합을 동일한 기수로 출력한다.  
**코드:**  
```c
#include <stdio.h>
#include <string.h>

void addBase(const char *a, const char *b, int base, char *result){
	int lenA = strlen(a);
	int lenB = strlen(b);
	int i = lenA - 1;
	int j = lenB - 1;        //보면 계산은 '987654321'처럼 뒤에서부터 처리하는데 
	int carry=0, index = 0; //배열에 넣는 순서는 '123456789'처럼 반대임(나중에 뒤집어야 함) 
	char temp[1002];
	//ASCII 값으로 '0'은 48, '1'은 49 ... '1'-'0'은 문자 '1'을 49-48로 숫자 1로 변환함, +'0'은 반대 
	while(i>=0 || j>=0 || carry>0){
		int dA = (i>=0)? a[i--]-'0':0;
		int dB = (j>=0)? b[j--]-'0':0;  //숫자로 변환했고 
		int sum = dA+dB+carry;   //정수형의 덧셈을 처리함 
		temp[index++] = (sum%base)+'0'; //숫자를 문자로 변환하여 temp배열에 집어넣는다 
		carry = sum/base;    //기수로 나눈 나머지는 temp배열에 저장 후 다음 자리로 이동 
	}        //몫이 발생했다면 carry로 넣어줌 
	for(int k=0; k<index; ++k){
		result[k]=temp[index-1-k];  //출력할 경과에는 아까 구한 답을
	}	                            //뒤집어서 반대로 저장한다. 
	result[index]='\0'; //끝자리에는 null삽입 
}
int main(){
	char n1[1001], n2[1001], result[1002];
	int base;
	printf("기수(base)를 입력하세요 (2~10): ");
    scanf("%d", &base);

    printf("첫 번째 숫자 문자열: ");
    scanf("%s", n1);

    printf("두 번째 숫자 문자열: ");
    scanf("%s", n2);

    addBase(n1, n2, base, result);
    printf("합계: %s\n", result);

    return 0;

}
```

---
## 문제 6
**Q**: Write a function that adds two hexadecimal strings, each as long as 1,000 digits. Return a hexadecimal string that represents the sum of the inputs.  
**풀이:**  
**코드:**  


---
## 문제 7
**Q**:  
**풀이:**  
**코드:**  


---
## 문제 8
**Q**:  
**풀이:**  
**코드:**  


---
## 문제 9
**Q**:  
**풀이:**  
**코드:**  


---

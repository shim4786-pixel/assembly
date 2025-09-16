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
**풀이:**  5번과 마찬가지로 자리수가 크기 때문에 정수형으로 계산할 수 없다. 문자열로 입력받아 자리별로 덧셈연산을 한 뒤 16진수 문자열로 다시 변환한다.  
**코드:**  
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int hexChToInt(char ch){    //16진수 문자열을 정수형으로 변환 
	if(ch >= '0' && ch <= '9') return ch - '0';           //숫자는 그대로 변환 
	else if(ch >= 'A' && ch <= 'F') return ch - 'A' +10;  //알파벳(대문자)의 경우 
	else if(ch >= 'a' && ch <= 'f') return ch - 'a' +10;  //알파벳(소문자)의 경우 
	else return 0;   //예외 처리 
}

char intToHexCh(int x){     //정수를 16진수 문자열로 변환 
	const char hexD[]="0123456789ABCDEF";
	return hexD[x];
} 

void addHexStr(const char *a, const char *b, char *result){
	int lenA=strlen(a);  
	int lenB=strlen(b);              //입력한 수의 자리수 
	int i=lenA-1;
	int j=lenB-1;                    //그의 최고 인덱스 번호 
	int carry=0; int index=0; char temp[1002];
	
	while(i>=0 || j>=0 || carry>0){     //더할 수들이 아직 남아있다면 
		int dA=(i>=0)? hexChToInt(a[i--]):0;
		int dB=(j>=0)? hexChToInt(b[j--]):0;   //유효한 수가 있다면 그에 맞는 정수로 변환 
		int sum= dA+dB+carry;                  //없다면 0으로  
		temp[index++] = intToHexCh(sum%16);    //캐리를 뺀 나머지(있다면)를 저장후 한칸 위로 
		carry=sum/16;                          //캐리가 있다면 1을 저장 
	}
	//덧셈 결과를 반대로 저장하였으므로 result 배열에 제대로 전달 
	for(int k=0; k<index; ++k) result[k]=temp[index-1-k];
	result[index]='\0';                        //끝에 null문자로 마무리 
}

int main(){
	char n1[1001], n2[1001], result[1002];  //null문자 들어갈 자리 (+올림수) 
	
	printf("첫 번째 16진수 문자열: ");
    scanf("%1000s", n1);

    printf("두 번째 16진수 문자열: ");
    scanf("%1000s", n2);

    addHexStr(n1, n2, result);
    printf("합계: %s\n", result);

    return 0;
}
```

---
## 문제 7
**Q**:  Write a function that multiplies a single hexadecimal digit by a hexadecimal digit string as long as 1,000 digits. Return a hexadecimal string that represents the product.  
**풀이:**  16진수 한 자리와 최대 1000자리의 16진수. 두 수를 입력받아 곱셈 결과를 반환해야 한다. 6번과 비슷한 코드를 짜 덧셈 함수를 곱셈 함수로 바꾸어 표현한다.  
**코드:**
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int hexChToInt(char ch){    //16진수 문자열을 정수형으로 변환 
	if(ch >= '0' && ch <= '9') return ch - '0';           //숫자는 그대로 변환 
	else if(ch >= 'A' && ch <= 'F') return ch - 'A' +10;  //알파벳(대문자)의 경우 
	else if(ch >= 'a' && ch <= 'f') return ch - 'a' +10;  //알파벳(소문자)의 경우 
	else return 0;   //예외 처리 
}

char intToHexCh(int x){     //정수를 16진수 문자열로 변환 
	const char hexD[]="0123456789ABCDEF";
	return hexD[x];
} 

void MulHexStr(char a, const char *b, char *result){  //이부분만 손보면 됨 
	int mulA=hexChToInt(a);          //승수를 정수형으로 변환 
	int lenB=strlen(b);              //lenB는 입력받은 피승수의 자리수 
	int carry=0; int index=0; char temp[1002];
	for(int i=lenB-1; i>=0; --i){
		int t=hexChToInt(b[i]);      //피승수의 첫 자리를 정수로 변환후 
		int d=t*mulA+carry;           //승수*피승수+올림수 실행 
		temp[index++]=intToHexCh(d%16);    //나머지는 거기 저장 
		carry=d/16;                        //몫은 올림수니 carry에 
	}
	while(carry>0){             //실행 종료 후 carry가 남아있을 수도 있음 
		temp[index++]=intToHexCh(carry%16);
		carry/=16;              //캐리가 빌 때까지 반복 
	}
	//덧셈 결과를 반대로 저장하였으므로 result 배열에 제대로 전달 
	for(int k=0; k<index; ++k) result[k]=temp[index-1-k];
	result[index]='\0';                        //끝에 null문자로 마무리 
}

int main(){
	char n1, n2[1001], result[1002];  //null문자 들어갈 자리 (+올림수) 
	
	printf("승수 16진수 한자리: ");
    scanf("%c", &n1);

    printf("피승수 16진수 문자열: ");
    scanf("%1000s", n2);

    MulHexStr(n1, n2, result);
    printf("곱셈결과: %s\n", result);

    return 0;
}
```

---
## 문제 8
**Q**:  Write a Java program that contains the calculation shown below. Then, use the javap –c command to disassemble your code. Add comments to each line that provide your best guess as to its purpose.  
 int Y;  
 int X = (Y + 4) * 3;  
**풀이:**  AI의 도움을 적극적으로 받아 해결하였다. 위의 수식을 포함한 자바 언어를 javap -c로 디어셈블하여 주석을 다는 문제이다.  
**코드:**  
```java
public class ex8 {
	public static void main(String [] args) {
		int Y=6;
		int X=(Y+4)*3;
		System.out.println("X = " + X);
	}
}
```
위는 문제를 기반으로 자바에 작성한 코드이다. Y의 값이 정의되지 않으면 컴파일 오류가 날 수 있으므로 임의의 수를 정해주었다.  결과 'X=30'이 제대로 출력되었다.  
javap를 사용하기 위해서는 cmd가 필요하였다. 다음은 디어셈블하기위해 cmd에 입력한 명령어들이다.  
```bash
cd C:\Users\NT950\Desktop\자바응용\java2file\assemble\src\ch1
javac ex8.java
```
컴퓨터에서 자바 파일을 저장해둔 곳으로 디렉토리를 옮기고,  
ex8에 대한 class파일을 만들어주기 위해 javac 명령어를 먼저 수행하였다.  
여기서 javap -c ex8 명령어를 실행할 단계였는데 컴퓨터에 javap를 실행하기 위한 프로그램이 없는듯 하여 오류가 발생하였다. Copilot에게 물어보고 다음과 같이 해결하였다.   
```bash
cd /
set JAVA_HOME=C:\Program Files\Java\jdk-23
set PATH=%JAVA_HOME%\bin;%PATH%
```
홈으로 디렉토리를 옮긴 후 jdk가 설치되어 있는 곳에 다음과 같이 명령어를 입력하였다.  
```bash
cd C:\Users\NT950\Desktop\자바응용\java2file\assemble\src\ch1
javap -c ex8
```
정상적으로 실행이 되었고 코드가 출력되었다. 다음은 코드중 일부를 발췌하여 문제에서 원한 주석을 달아 설명한 버전이다.  
```java
Compiled from "ex8.java"
public class ch1.ex8 {

  public ch1.ex8();
    Code:
       0: aload_0                      // 지역 변수 0번의 객체(this 객체)를 스택에 로드
       1: invokespecial #1            // Object 클래스의 생성자 호출
       4: return                      // 생성자 종료

  public static void main(java.lang.String[]);
    Code:
       0: bipush        6             // 정수 6을 스택에 푸시 (Y = 6)
       2: istore_1                    // 스택의 값을 지역 변수 1번(Y)에 저장
       3: iload_1                     // 지역 변수 Y를 스택에 로드
       4: iconst_4                    // 정수 4를 스택에 푸시
       5: iadd                        // Y + 4 계산
       6: iconst_3                    // 정수 3을 스택에 푸시
       7: imul                        // (Y + 4) * 3 계산
       8: istore_2                    // 결과를 지역 변수 2번(X)에 저장
       9: getstatic     #7           // System.out 객체 로드  '#7'은 System.out(표준 출력 스트											림)을 의미함
      12: iload_2                     // X 값을 스택에 로드
      13: invokedynamic #13, 0       // 문자열 연결을 위한 동적 호출 (X → 문자열로 변환)
      18: invokevirtual #17          // '#17'println(String) 호출 → 결과 출력
      21: return                      // main 메서드 종료
}
// iconst_n, bipush 등은 '정수 n을 스택에 올린다'
// istore_n은 지역 변수에 저장, iload_n은 지역 변수에 불러오기
// iadd, imul, isub 등은 덧셈, 곱셈, 뺄셈 등 사칙연산
// invokevirtual, invokestatic 등은 '메서드를 실행한다'
```

---
## 문제 9
**Q**:  Devise a way of subtracting unsigned binary integers. Test your technique by subtracting binary
 00000101 from binary 10001000, producing 10000011. Test your technique with at least two
 other sets of integers, in which a smaller value is always subtracted from a larger one.
**풀이:**  부호없는 2진수 값 두 개를 받아 뺄셈 결과를 도출해야 한다.  
2진수 숫자의 자리수가 정해지지 않아 위의 문제처럼 문자열을 정수로 하나씩 바꾸어 계산하는 방식을 사용하였다.  
**코드:**  
<해당 코드는 java로 작성되었다>
```java
public class ex9 { 
	public static String sub(String a, String b) {  //큰 수와 작은 수를 받아 빼는 함수이다.
		int len= Math.max(a.length(), b.length());  //a, b중 더 긴 길이를 len에 저장
		a=String.format("%"+len+"s", a).replace(' ', '0');
		b=String.format("%"+len+"s", b).replace(' ', '0');
		//a와 b를 len길이에 맞추어 왼쪽에 공백을 채워 정렬 후 공백(' ')을 0으로 채운다.
		StringBuilder result = new StringBuilder();
		//result는 결과를 저장하기 위한 문자열이다.
		//reverse()를 사용하기 위해 StringBuilder를 사용하였다.
		int borrow=0;  //빌림수 설정
		
		for(int i=len-1; i>=0; i--) {  //오른쪽에서 왼쪽으로
			int AA=a.charAt(i)-'0';        //문자열을 숫자로 바꿔줌
			int BB=b.charAt(i)-'0'+borrow; 
			//이전 계산에서 빌림이 있었다면 그 값을 더해준다.(AA에서 1을 빼는 대신)
			if(AA<BB) {  //a의 수가 b의 수보다 작으면 빌림이 필요함
				result.append(AA+2-BB); //빌렸으니 +2에서 빼기
				borrow=1;
			}else {     //아니면 필요없음.
				result.append(AA-BB);
				borrow=0;
			}
		} //계산결과가 반대로 쌓였으니 reverse()로 뒤집기
		return result.reverse().toString().replaceFirst("^0+(?!$)","");
	}     //replaceFirst("^0+(?!$)","")는 앞자리의 쓸모 없는 0을 제거 결과가 0이면 냅두기
		  //^0+>> 하나 이상의 0을 찾음 (0, 00, 000...) (?!$),"">> 그 뒤가 문자열의 끝이 아니면 제거
	public static void main(String[] args) {
		System.out.println(sub("10001000", "00000101"));
		System.out.println(sub("00010110", "00000011"));
		System.out.println(sub("01110101", "00110001"));	
	}  //세 개의 숫자쌍으로 검사해보았다.
}
```
정해진 숫자의 뺄셈만을 구하지만 Scanner를 사용하여 사용자로부터 입력받은 정수 두 개의 뺄셈을 구할 수도 있다.  

---

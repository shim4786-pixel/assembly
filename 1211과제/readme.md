# C언어와 어셈블리어의 결합 및 라이브러리(Lib, DLL) 활용

### Reference

  * **Mentor**: Gemini (AI Thought Partner)

## 1\. 개요 및 과제 목표

본 과제는 C언어의 생산성과 어셈블리어의 하드웨어 제어 능력을 결합하는 '혼합 프로그래밍'을 실습하는 것을 목표로 한다. 단순한 코드 결합(Inline Assembly)에서 시작하여, 재사용 가능한 **정적 라이브러리(.lib)** 와 확장성이 뛰어난 **동적 연결 라이브러리(.dll)** 로 모듈화하는 과정을 단계별로 수행하였다.

핵심 기능으로는 'Caesar Cipher(시저 암호)' 알고리즘을 구현하였으며, 입출력은 C언어가, 핵심 연산은 어셈블리어가 담당하도록 설계하여 시스템 프로그래밍의 빌드 및 링킹 과정을 심층적으로 이해하고자 한다.

-----

## 2\. 과제 수행 과정

### [Step 1] C언어 + Inline Assembly (직접 결합)

첫 번째 단계는 별도의 모듈 분리 없이, C 소스 코드 내에 어셈블리어를 직접 삽입하는 방식이다.

1.  Visual Studio에서 솔루션 내 새로운 프로젝트를 생성하고, 소스 파일(`main.c`)을 생성한다.
    <img width="1918" height="1022" alt="step1_1" src="https://github.com/user-attachments/assets/1d8b1e6b-e379-4a70-8dfc-598215329fe7" />


2.  C언어의 `main` 함수 내부에 `__asm` 블록을 사용하여 암호화 로직을 작성한다. C언어의 변수(`text`, `key`)를 레지스터(`ESI`, `ECX`)로 직접 이동시켜 연산을 수행한다.
   <img width="952" height="645" alt="step1_2" src="https://github.com/user-attachments/assets/70455af6-574e-42f6-b430-4211aba88e1a" />

   <img width="953" height="1020" alt="step1_3" src="https://github.com/user-attachments/assets/67d20f57-48a7-4237-b5bf-89c00bf9a744" />


3.  빌드 및 실행 결과, 입력한 문자열("Hello Assembly World\!")이 키값에 맞춰 암호화되어 출력됨을 확인하였다.
   <img width="1856" height="751" alt="step1_4" src="https://github.com/user-attachments/assets/a1086a3b-b1f4-4801-bf5a-4da54e4c1555" />


### [Step 2] 정적 라이브러리 (Static Library, .lib)

두 번째 단계는 암호화 기능을 별도의 라이브러리 파일로 분리하여 컴파일 시점에 포함시키는 방식이다.

1.  라이브러리용 프로젝트(`MyCryptoLib`)를 생성하고, 프로젝트 속성에서 \*\*[구성 형식]\*\*을 \*\*'정적 라이브러리(.lib)'\*\*로 변경한다.
    <img width="993" height="677" alt="step2_1" src="https://github.com/user-attachments/assets/87b5af2f-1bda-4b8c-aad1-4e0bd9b47d65" />


2.  라이브러리 코드에는 `main` 함수 없이, 기능을 수행하는 `MyEncrypt` 함수만 작성한다.
    <img width="948" height="1021" alt="step2_2" src="https://github.com/user-attachments/assets/b830e2ee-3ea1-4d99-9c77-c16ca829bea5" />


3.  라이브러리 프로젝트를 빌드하여 `MyCryptoLib.lib` 파일을 생성한다.
    <img width="950" height="1012" alt="step2_3" src="https://github.com/user-attachments/assets/d8d1c5d6-d5ff-4d27-9d42-e9f1abd797e0" />


4.  실행용 프로젝트(`Step2_Run`)를 생성하고 빌드를 시도했으나, 외부 함수의 실체를 찾을 수 없다는 \*\*LNK2019(링크 에러)\*\*가 발생함을 확인하였다.
   <img width="952" height="1017" alt="step2_4" src="https://github.com/user-attachments/assets/f9f72ff3-905e-4a5e-9797-e5bdfe2c79f5" />


5.  프로젝트 참조 설정에서 앞서 만든 `MyCryptoLib`을 추가하여 링커가 라이브러리를 찾을 수 있도록 연결한다.
    <img width="952" height="722" alt="step2_5" src="https://github.com/user-attachments/assets/23e3adf0-8ab9-4d12-ad6d-e95fcc5c93e4" />


6.  참조 추가 후 정상적으로 빌드 및 실행되는 것을 확인하였다. 이 과정에서 라이브러리의 코드가 실행 파일(.exe)에 포함되었다.
    <img width="940" height="720" alt="step2_6" src="https://github.com/user-attachments/assets/7fbeb5fb-074b-4e85-a769-23114acba140" />


### [Step 3] 동적 연결 라이브러리 (Dynamic Link Library, .dll)

마지막 단계는 실행 시점에 외부 파일을 호출하는 DLL 방식이다.

1.  DLL용 프로젝트(`MyCryptoDll`)를 생성하고, 속성에서 \*\*[구성 형식]\*\*을 \*\*'동적 라이브러리(.dll)'\*\*로 변경한다.
    <img width="948" height="965" alt="step3_1" src="https://github.com/user-attachments/assets/052d2e2e-11e5-4ce7-879b-720b83cf51f2" />


2.  함수 앞에 `__declspec(dllexport)` 키워드를 붙여 외부로 기능을 노출시킨 뒤 빌드하여 `.dll`과 `.lib`(임포트 라이브러리)를 생성한다.
    <img width="913" height="836" alt="step3_2" src="https://github.com/user-attachments/assets/1456b815-0e2f-4583-877f-f67ca409b933" />


3.  실행용 프로젝트(`Step3_Run`)에서 `__declspec(dllimport)`를 선언하고 빌드했으나, 참조가 없어 링크 에러가 발생함을 확인하였다.
    <img width="943" height="1008" alt="step3_3" src="https://github.com/user-attachments/assets/d2fa5995-8d09-42ef-8b1f-b9506f5f5d17" />


4.  프로젝트 참조에 `MyCryptoDll`을 추가한다. 이때의 참조는 코드를 합치는 것이 아니라, DLL의 위치 정보를 담은 임포트 라이브러리를 연결하는 과정이다.
    <img width="951" height="877" alt="step3_4" src="https://github.com/user-attachments/assets/306e3171-d895-4a3d-aad8-d0ec3df3ae2f" />


5.  정상적으로 실행되어 암호화가 수행됨을 확인하였다.
    <img width="948" height="747" alt="step3_5" src="https://github.com/user-attachments/assets/c8049e82-1c9a-4958-8477-0761bfbb2a8c" />


6.  **[검증]** 빌드 폴더를 확인한 결과, 실행 파일(`Step3_Run.exe`)과 라이브러리 파일(`MyCryptoDll.dll`)이 같은 경로에 존재해야 실행됨을 확인하였다.
    <img width="971" height="827" alt="step_final" src="https://github.com/user-attachments/assets/4be1f336-fdbb-4162-8907-ebb9680e547e" />


7.  또한 각 프로젝트의 Debug 폴더 내에 컴파일의 중간 산물인 오브젝트 파일(`.obj`)이 생성되어 있음을 통해 빌드 과정을 검증하였다.
    <img width="941" height="467" alt="step_final2" src="https://github.com/user-attachments/assets/23308383-0e00-4495-84c4-4108bca5daef" />


-----

## 3\. 각 방법의 특징 및 비유 (Summary)

이번 과제에서 수행한 세 가지 방식의 차이점을 '책과 공부'에 비유하여 정리한다.

| 단계 | 방식 | 기술적 특징 | 비유 (Metaphor) |
| :--- | :--- | :--- | :--- |
| **Step 1** | **Inline Assembly** | • **오버헤드 최소화**: 함수 호출 없이 즉시 실행<br>• **문맥 공유**: C 변수 직접 접근 가능 | **"내가 저자(Author)다"**<br>책을 찾을 필요 없이 머릿속에 지식이 있어 즉시 말할 수 있다. |
| **Step 2** | **Static Library**<br>(.lib) | • **독립성**: 라이브러리 코드가 EXE에 임베딩됨<br>• **용량 증가**: 실행 파일 크기가 커짐 | **"책 내용을 노트에 베껴 쓰기"**<br>노트(EXE)가 두꺼워지지만, 책(Lib)을 반납해도 언제든 공부할 수 있다. |
| **Step 3** | **Dynamic Library**<br>(.dll) | • **확장성/공유**: 여러 프로그램이 DLL 공유 가능<br>• **의존성**: 실행 시 DLL 파일이 반드시 필요함 | **"도서관 책 참조(Reference)"**<br>노트에는 "도서관 3열 5번 책 참조"라고만 적혀 있다. 몸은 가볍지만 도서관 문이 닫히면(DLL 부재) 공부할 수 없다. |

-----

## 4\. 결론

본 과제를 통해 소스 코드를 직접 작성하는 단계에서 시작하여, 정적 라이브러리를 통한 코드의 재사용, 그리고 동적 라이브러리를 통한 모듈화 및 메모리 효율성 제고까지 시스템 프로그래밍의 발전 과정을 단계별로 체험하였다.

특히 빌드 과정에서 **컴파일러**가 오브젝트 파일(`.obj`)을 생성하고, **링커**가 이를 라이브러리(`.lib`)와 결합하여 최종 실행 파일(`.exe`)을 만드는 구조를 명확히 이해할 수 있었다. 이를 통해 향후 대규모 소프트웨어 개발 시 필요한 모듈 설계 및 의존성 관리 역량을 확보하였다.

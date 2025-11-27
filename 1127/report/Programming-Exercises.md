# Programming Exercises 문제 풀이
## ★ 1. Improved Str_copy Procedure  
**Q : The Str_copy procedure shown in this chapter does not limit the number of characters to be copied. Create a new version (named Str_copyN) that receives an additional input parameter indicating the maximum number of characters to be copied.**  
**A :**
```asm

```

---
## ★ 2. Str_concat Procedure  
**Q : Write a procedure named Str_concat that concatenates a source string to the end of a target string. Sufficient space must exist in the target string to accommodate the new characters. Pass pointers to the source and target strings. Here is a sample call:**  
```asm
.data
targetStr BYTE "ABCDE",10 DUP(0)
sourceStr BYTE "FGH",0
.code
INVOKE Str_concat, ADDR targetStr, ADDR sourceStr
```
**A :**
```asm

```

---
## ★ 3. Str_remove Procedure  
**Q : Write a procedure named Str_remove that removes n characters from a string. Pass a pointer to the position in the string where the characters are to be removed. Pass an integer specifying the number of characters to remove. The following code, for example, shows how to remove “xxxx” from target:**  
```asm
.data
target BYTE "abcxxxxdefghijklmop",0
.code
INVOKE Str_remove, ADDR [target+3], 4
```
**A :**
```asm

```

---
## ★ 4. Str_find Procedure  
**Q : Write a procedure named Str_find that searches for the first matching occurrence of a source string inside a target string and returns the matching position. The input parameters should be a pointer to the source string and a pointer to the target string. If a match is found, the procedure sets the Zero flag and EAX points to the matching position in the target string. Otherwise, the Zero flag is clear and EAX is undefined. The following code, for example, searches for “ABC” and returns with EAX pointing to the “A” in the target string:**  
```asm
.data
target BYTE "123ABC342432",0
source BYTE "ABC",0
pos    DWORD ?
.code
INVOKE Str_find, ADDR source, ADDR target
jnz notFound
mov pos,eax                      ; store the position value
```
**A :**
```asm

```

---
## ★ 5. Str_nextWord Procedure  
**Q : Write a procedure called Str_nextWord that scans a string for the first occurrence of a certain delimiter character and replaces the delimiter with a null byte. There are two input parameters: a pointer to the string and the delimiter character. After the call, if the delimiter was found, the Zero flag is set and EAX contains the offset of the next character beyond the delimiter. Otherwise, the Zero flag is clear and EAX is undefined. The following example code passes the address of target and a comma as the delimiter:**  
```asm
.data
target BYTE "Johnson,Calvin",0
.code
INVOKE Str_nextWord, ADDR target, ','
jnz notFound
```
**In Figure 9-5, after calling Str_nextWord, EAX points to the character following the position where the comma was found (and replaced). **  
<img width="446" height="144" alt="image" src="https://github.com/user-attachments/assets/a03a1245-6dd8-4ab8-be12-91f91d75f1cf" />  
**A :**
```asm

```

---
## ★ 6. Constructing a Frequency Table  
**Q :  Write a procedure named Get_frequencies that constructs a character frequency table. Input to the procedure should be a pointer to a string and a pointer to an array of 256 doublewords initialized to all zeros. Each array position is indexed by its corresponding ASCII code. When the procedure returns, each entry in the array contains a count of how many times the corresponding character occurred in the string. For example,**  
```asm
.data
target BYTE "AAEBDCFBBC",0
freqTable DWORD 256 DUP(0)
.code
INVOKE Get_frequencies, ADDR target, ADDR freqTable
```
**Figure 9-6 shows a picture of the string and entries 41 (hexadecimal) through 4B in the frequency table. Position 41 contains the value 2 because the letter A (ASCII code 41h) occurred twice in the string. Similar counts are shown for other characters. Frequency tables are useful in data compression and other applications involving character processing. The Huffman encoding algorithm, for example, stores the most frequently occurring characters in fewer bits than other characters that occur less often.**  
<img width="464" height="128" alt="image" src="https://github.com/user-attachments/assets/c6a043e9-b07e-4324-93de-8c21f64bc419" />  
**A :**
```asm

```

---
## ★ 7. Sieve of Eratosthenes  
**Q : The Sieve of Eratosthenes, invented by the Greek mathematician of the same name, provides a quick way to find all prime numbers within a given range. The algorithm involves creating an array of bytes in which positions are “marked” by inserting 1s in the following manner: Beginning with position 2 (which is a prime number), insert a 1 in each array position that is a multiple of 2. Then do the same thing for multiples of 3, the next prime number. Find the next prime number after 3, which is 5, and mark all positions that are multiples of 5. Proceed in this manner until all multiples of primes have been found. The remaining positions of the array that are unmarked indicate which numbers are prime. For this program, create a 65,000-element array and display all primes between 2 and 65,000. Declare the array in an uninitialized data segment (see Section 3.4.11) and use STOSB to fill it with zeros. **  
**A :**
```asm

```

---
## ★ 8. Bubble Sort  
**Q : Add a variable to the BubbleSort procedure in Section 9.5.1 that is set to 1 whenever a pair of values is exchanged within the inner loop. Use this variable to exit the sort before its normal completion if you discover that no exchanges took place during a complete pass through the array. (This variable is commonly known as an exchange flag.)**  
**A :**
```asm

```

---
## ★ 9. Binary Search  
**Q : Rewrite the binary search procedure shown in this chapter by using registers for mid, first, and last. Add comments to clarify the registers’ usage.**  
**A :**
```asm

```

---
## ★ 10. Letter Matrix  
**Q : Create a procedure that generates a four-by-four matrix of randomly chosen capital letters. When choosing the letters, there must be a 50% probability that the chosen letter is a vowel. Write a test program with a loop that calls your procedure five times and displays each matrix in the console window. Following is sample output for the first three matrices:**  
```
D W A L
S I V W
U I O L
L A I I

K X S V
N U U O
O R Q O
A U U T

P O A Z
A E A U
G K A E
I A G D
```
**A :**
```asm

```

---
## ★ 11. Letter Matrix/Sets with Vowels  
**Q :  Use the letter matrix generated in the previous programming exercise as a starting point for this program. Generate a single random four-by-four letter matrix in which each letter has a 50% probability of being a vowel. Traverse each matrix row, column, and diagonal, generating sets of letters. Display only four-letter sets containing exactly two vowels. Suppose, for example, the following matrix was generated:**  
```
P O A Z
A E A U
G K A E
I A G D
```
** Then the four-letter sets displayed by the program would be POAZ, GKAE, IAGD, PAGI, ZUED, PEAD, and ZAKI. The order of letters within each set is unimportant.**  
**A :**
```asm

```

---
## ★ 12. Calculating the Sum of an Array Row  
**Q : Write a procedure named calc_row_sum that calculates the sum of a single row in a two-dimensional array of bytes, words, or doublewords. The procedure should have the following stack parameters: array offset, row size, array type, row index. It must return the sum in EAX. Use explicit stack parameters, not INVOKE or extended PROC. Write a program that tests your procedure with arrays of byte, word, and doubleword. Prompt the user for the row index, and display the sum of the selected row.**  
**A :**
```asm

```

---
## ★ 13. Trimming Leading Characters  
**Q : Create a variant of the Str_trim procedure that lets the caller remove all instances of a leading character from a string. For example, if you were to call it with a pointer to the string “###ABC” and pass it the # character, the resulting string would be “ABC.”**  
**A :**
```asm

```

---
## ★ 14. Trimming a Set of Characters  
**Q : Create a variant of the Str_trim procedure that lets the caller remove all instances of a set of characters from the end of a string. For example, if you were to call it with a pointer to the string “ABC#$&” and pass it a pointer to an array of filter characters containing “%#!;$&*”, the resulting string would be “ABC”.**  
**A :**
```asm

```

---

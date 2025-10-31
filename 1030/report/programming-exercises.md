# Programming Exercises 문제 풀이  

## ★ 1. Draw Text Colors   
**Q : Write a program that displays the same string in four different colors, using a loop. Call the SetTextColor procedure from the book’s link library. Any colors may be chosen, but you may find it easiest to change the foreground color.**  
**A :**  

---
## ★★ 2. Linking Array Items   
**Q : Suppose you are given three data items that indicate a starting index in a list, an array of characters, and an array of link index. You are to write a program that traverses the links and locates the characters in their correct sequence. For each character you locate, copy it to a new array. Suppose you used the following sample data, and assumed the arrays use zero-based indexes:  
start = 1  
chars:   H   A   C   E   B   D   F   G  
links:   0   4   5   6   2   3   7   0  
Then the values copied (in order) to the output array would be A,B,C,D,E,F,G,H. Declare the character array as type BYTE, and to make the problem more interesting, declare the links array type DWORD.**   
**A :**  

---
## ★ 3. Simple Addition (1)   
**Q : Write a program that clears the screen, locates the cursor near the middle of the screen, prompts the user for two integers, adds the integers, and displays their sum.**  
**A :**  

---
## ★★ 4. Simple Addition (2)  
**Q : Use the solution program from the preceding exercise as a starting point. Let this new program repeat the same steps three times, using a loop. Clear the screen after each loop iteration.**  
**A :**  

---
## ★ 5. BetterRandomRange Procedure  
**Q : The RandomRange procedure from the Irvine32 library generates a pseudorandom integer between 0 and N-1. Your task is to create an improved version that generates an integer between M and N-1. Let the caller pass M in EBX and N in EAX. If we call the procedure BetterRandomRange, the following code is a sample test:**  
```asm
mov  ebx,-300                  ; lower bound
mov  eax,100                   ; upper bound
call BetterRandomRange         
```
**Write a short test program that calls BetterRandomRange from a loop that repeats 50 times.  
Display each randomly generated value.**  
**A :**  

---
## ★★ 6. Random Strings  
**Q : Create a procedure that generates a random string of length L, containing all capital letters. When calling the procedure, pass the value of L in EAX, and pass a pointer to an array of byte that will hold the random string. Write a test program that calls your procedure 20 times and displays the strings in the console window. **  
**A :**  

---
## ★ 7. Random Screen Locations  
**Q : Write a program that displays a single character at 100 random screen locations, using a timing delay of 100 milliseconds. Hint: Use the GetMaxXY procedure to determine the current size of the console window.**  
**A :**  

---
## ★★ 8. Color Matrix  
**Q : Write a program that displays a single character in all possible combinations of foreground and background colors (16X16=256). The colors are numbered from 0 to 15, so you can use a nested loop to generate all possible combinations.**  
**A :**  

---
## ★★★ 9. Recursive Procedure  
**Q : Direct recursion is the term we use when a procedure calls itself. Of course, you never want to let a procedure keep calling itself forever, because the runtime stack would fill up. Instead, you must limit the recursion in some way. Write a program that calls a recursive procedure. Inside this procedure, add 1 to a counter so you can verify the number of times it executes. Run your program with a debugger, and at the end of the program, check the counter’s value. Put a number in ECX that specifies the number of times you want to allow the recursion to continue. Using only the LOOP instruction (and no other conditional statements from later chapters), find a way for the recursive procedure to call itself a fixed number of times.**  
**A :**  

---
## ★★★ 10. Fibonacci Generator  
**Q : Write a procedure that produces N values in the Fibonacci number series and stores them in an array of doubleword. Input parameters should be a pointer to an array of doubleword, a counter of the number of values to generate. Write a test program that calls your procedure, passing N = 47. The first value in the array will be 1, and the last value will be 2,971,215,073. Use the Visual Studio debugger to open and inspect the array contents.**  
**A :**  

---
## ★★★ 11. Finding Multiples of K  
**Q : In a byte array of size N, write a procedure that finds all multiples of K that are less than N. Initialize the array to all zeros at the beginning of the program, and then whenever a multiple is found, set the corresponding array element to 1. Your procedure must save and restore any registers it modifies. Call your procedure twice, with K = 2, and again with K = 3. Let N equal to 50. Run your program in the debugger and verify that the array values were set correctly. Note: This procedure can be a useful tool when finding prime integers. An efficient algorithm for finding prime numbers is known as the Sieve of Eratosthenes. You will be able to implement this algorithm when conditional statements are covered in Chapter 6.**  
**A :**  

---

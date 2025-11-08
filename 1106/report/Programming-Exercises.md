# Programming Exercises 문제 풀이

## ★ 1. Filling an Array    
**Q : Create a procedure that fills an array of doublewords with N random integers, making sure the values fall within the range j...k, inclusive. When calling the procedure, pass a pointer to the array that will hold the data, pass N, and pass the values of j and k. Preserve all register values between calls to the procedure. Write a test program that calls the procedure twice, using different values for j and k. Verify your results using a debugger.**   
**A :**

---
## ★★ 2. Summing Array Elements in a Range   
**Q : Create a procedure that returns the sum of all array elements falling within the range j...k (inclusive). Write a test program that calls the procedure twice, passing a pointer to a signed doubleword array, the size of the array, and the values of j and k. Return the sum in the EAX register, and preserve all other register values between calls to the procedure.**    
**A :**

---
## ★★ 3. Test Score Evaluation     
**Q : Create a procedure named CalcGrade that receives an integer value between 0 and 100, and returns a single capital letter in the AL register. Preserve all other register values between calls to the procedure. The letter returned by the procedure should be according to the following ranges:**    
<img width="187" height="125" alt="image" src="https://github.com/user-attachments/assets/c51f97c4-e802-4b09-abfb-143d40f48859" />    
**Write a test program that generates 10 random integers between 50 and 100, inclusive. Each time an integer is generated, pass it to the CalcGrade procedure.  You can test your program using a debugger, or if you prefer to use the book’s library, you can display each integer and its corresponding letter grade. (The Irvine32 library is required for this solution program because it uses the RandomRange procedure.)**  
**A :**

---
## ★★ 4. College Registration    
**Q : Using the College Registration example from Section 6.7.3 as a starting point, do the following:  
 • Recode the logic using CMP and conditional jump instructions (instead of the .IF and .ELSEIF directives).  
 • Perform range checking on the credits value; it cannot be less than 1 or greater than 30. If an invalid entry is discovered, display an appropriate error message.  
 • Prompt the user for the grade average and credits values.  
 • Display a message that shows the outcome of the evaluation, such as “The student can register” or “The student cannot register”.  
(The Irvine32 library is required for this solution program.)**    
**A :**

---
## ★★★ 5. Boolean Calculator (1)    
**Q : Create a program that functions as a simple boolean calculator for 32-bit integers. It should display a menu that asks the user to make a selection from the following list:  
1. x AND y  
2. x OR y  
3. NOT x  
4. x XOR y   
5. Exit program  

When the user makes a choice, call a procedure that displays the name of the operation about tobe performed. You must implement this procedure using the Table-Driven Selection technique,shown in Section 6.5.4. (You will implement the operations in Exercise 6.) (The Irvine32 libraryis required for this solution program.)**     
**A :**

---
## ★★★ 6. Boolean Calculator (2)    
**Q : Continue the solution program from Exercise 5 by implementing the following procedures:  
• AND_op: Prompt the user for two hexadecimal integers. AND them together and display the result in hexadecimal.  
• OR_op: Prompt the user for two hexadecimal integers. OR them together and display the result in hexadecimal.  
• NOT_op: Prompt the user for a hexadecimal integer. NOT the integer and display the result in hexadecimal.  
• XOR_op: Prompt the user for two hexadecimal integers. Exclusive-OR them together and display the result in hexadecimal.  
(The Irvine32 library is required for this solution program.)**    
**A :**

---
## ★★ 7. Probabilities and Colors   
**Q : Write a program that randomly chooses among three different colors for displaying text on the screen. Use a loop to display 20 lines of text, each with a randomly chosen color. The probabilities for each color are to be as follows: white=30%, blue=10%, green=60%. Suggestion: Generate a random integer between 0 and 9. If the resulting integer falls in the range 0 to 2 (inclusive), choose white. If the integer equals 3, choose blue. If the integer falls in the range 4 to 9 (inclusive), choose green. Test your program by running it ten times, each time observing whether the distribution of line colors appears to match the required probabilities.   
(The Irvine32 library is required for this solution program.)**    
**A :**

---
## ★★★ 8. Message Encryption   
**Q : Revise the encryption program in Section 6.3.4 in the following manner: Create an encryption key consisting of multiple characters. Use this key to encrypt and decrypt the plaintext by XORing each character of the key against a corresponding byte in the message. Repeat the key as many times as necessary until all plain text bytes are translated. Suppose, for example, the key were equal to “ABXmv#7”. This is how the key would align with the plain text bytes:**  
<img width="527" height="58" alt="image" src="https://github.com/user-attachments/assets/af7c0bce-fee5-49ce-9a9f-52e2374b3995" />  

**A :**

---
## ★★ 9. Validating a PIN   
**Q : Banks use a Personal Identification Number (PIN) to uniquely identify each customer. Let us assume that our bank has a specified range of acceptable values for each digit in its customers’ 5-digit PINs. The table shown below contains the acceptable ranges, where digits are numbered from left to right in the PIN. Then we can see that the PIN 52413 is valid.  But the PIN 43534 is invalid because the first digit is out of range. Similarly, 64535 is invalid because of its last digit.**  
<img width="226" height="144" alt="image" src="https://github.com/user-attachments/assets/dd4fc021-fc9b-4074-89a3-59c91e3397b0" />  
**Your task is to create a procedure named Validate_PIN that receives a pointer to an array of byte containing a 5-digit PIN. Declare two arrays to hold the minimum and maximum range values, and use these arrays to validate each digit of the PIN that was passed to the procedure. If any digit is found to be outside its valid range, immediately return the digit’s position (between 1 and 5) in the EAX register. If the entire PIN is valid, return 0 in EAX. Preserve all other register values between calls to the procedure. Write a test program that calls Validate_PIN at least four times, using both valid and invalid byte arrays. By running the program in a debugger, verify that the return value in EAX after each procedure call is valid. Or, if you prefer to use the book’s library, you can display "Valid" or "Invalid" on the console after each procedure call.**  
**A :**

---
## ★★★★ 10. Parity Checking   
**Q : Data transmission systems and file subsystems often use a form of error detection that relies on calculating the parity (even or odd) of blocks of data. Your task is to create a procedure that returns True in the EAX register if the bytes in an array contain even parity, or False if the parity is odd. In other words, if you count all the bits in the entire array, their count will be even or odd. Preserve all other register values between calls to the procedure. Write a test program that calls your procedure twice, each time passing it a pointer to an array and the length of the array. The procedure’s return value in EAX should be 1 (True) or 0 (False). For test data, create two arrays containing at least 10 bytes, one having even parity, and another having odd parity. **  
>Tip:  Earlier in this chapter, we showed how you can repeatedly apply the XOR instruction to a sequence of byte values to determine their parity. So, this suggests the use of a loop. But be careful, since some machine instructions affect the Parity flag, and others do not. You can find this out by looking at the individual instructions in Appendix B. The code in your loop that checks the parity will have to carefully save and restore the state of the Parity flag to avoid having it unintentionally modified by your code.  

**A :**

---

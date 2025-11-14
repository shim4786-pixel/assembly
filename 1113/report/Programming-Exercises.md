# Programming Exercises 문제 풀이

## ★ 1. Display ASCII Decimal    
**Q : Write a procedure named WriteScaled that outputs a decimal ASCII number with an implied decimal point. Suppose the following number were defined as follows, where DECIMAL_OFFSET indicates that the decimal point must be inserted five positions from the right side of the number:**  
```asm
DECIMAL_OFFSET = 5
.data
decimal_one BYTE "100123456789765"
```
**WriteScaled would display the number like this:**  
```
1001234567.89765
```
** When calling WriteScaled, pass the number’s offset in EDX, the number length in ECX, and the decimal offset in EBX. Write a test program that passes three numbers of different sizes to the WriteScaled procedure.**  
**A :**

---
## ★ 2. Extended Subtraction Procedure   
**Q : Create a procedure named Extended_Sub that subtracts two binary integers of arbitrary size. The storage size of the two integers must be the same, and their size must be a multiple of 32 bits. Write a test program that passes several pairs of integers, each at least 10 bytes long.**  
**A :**

---
## ★★ 3. Packed Decimal Conversion   
**Q : Write a procedure named PackedToAsc that converts a 4-byte packed decimal integer to a string of ASCII decimal digits. Pass the packed integer and the address of a buffer holding the ASCII digits to the procedure. Write a short test program that passes at least 5 packed decimal integers to your procedure.**  
**A :**

---
## ★★ 4. Encryption Using Rotate Operations   
**Q : Write a procedure that performs simple encryption by rotating each plaintext byte a varying number of positions in different directions. For example, in the following array that represents the encryption key, a negative value indicates a rotation to the left and a positive value indicates a rotation to the right. The integer in each position indicates the magnitude of the rotation:**  
```asm
key BYTE -2, 4, 1, 0, -3, 5, 2, -4, -4, 6
```
**Your procedure should loop through a plaintext message and align the key to the first 10 bytes of the message. Rotate each plaintext byte by the amount indicated by its matching key array value. Then, align the key to the next 10 bytes of the message and repeat the process. Write a program that tests your encryption procedure by calling it twice, with different data sets.**  
**A :**

---
## ★★★ 5. Prime Numbers   
**Q :Write a program that generates all prime numbers between 2 and 1000, using the Sieve of Eratosthenes method. You can find many articles that describe the method for finding primes in this manner on the Internet. Display all the prime values.**  
**A :**

---
## ★★★ 6. Greatest Common Divisor (GCD)   
**Q : The greatest common divisor (GCD) of two integers is the largest integer that will evenly divide both integers. The GCD algorithm involves integer division in a loop, described by the following pseudocode:**  
```pseudocode
int GCD(int x, int y)
    {
       x = abs(x)          // absolute value
       y = abs(y)
       do {
         int n = x % y
         x = y
         y = n
       } while (y > 0)
       return x
   }
```
** Implement this function in assembly language and write a test program that calls the function
 several times, passing it different values. Display all results on the screen.**  
**A :**

---
## ★★★ 7. Bitwise Multiplication   
**Q : Write a procedure named BitwiseMultiply that multiplies any unsigned 32-bit integer by EAX, using only shifting and addition. Pass the integer to the procedure in the EBX register, and return the product in the EAX register. Write a short test program that calls the procedure and displays the product. (We will assume that the product is never larger than 32 bits.) This is a fairly challenging program to write. One possible approach is to use a loop to shift the multiplier to the right, keeping track of the number of shifts that occur before the Carry flag is set. The resulting shift count can then be applied to the SHL instruction, using the multiplicand as the destination operand. Then, the same process must be repeated until you find the last 1 bit in the multiplier.**  
**A :**

---
## ★★★ 8. Add Packed Integers   
**Q : Extend the AddPacked procedure from Section 7.6.1 so that it adds two packed decimal integers of arbitrary size (both lengths must be the same). Write a test program that passes AddPacked several pairs of integers: 4-byte, 8-byte, and 16-byte. We suggest that you use the following registers to pass information to the procedure:**  
```
ESI - pointer to the first number
EDI - pointer to the second number
EDX - pointer to the sum
ECX - number of bytes to add
```
**A :**

---

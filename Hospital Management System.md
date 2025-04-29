# **Multiple Stack-Based Buffer Overflow Vulnerabilities in Hospital Management System**



## supplier

[Simple Hospital Management System In C Programming With Source Code - Source Code & Projects](https://code-projects.org/simple-hospital-management-system-in-c-programming-with-source-code/)



**Vulnerability Types**:

- CWE-121: Stack-based Buffer Overflow
- CWE-242: Use of Inherently Dangerous Function (`gets()`)



## describe

A stack-based buffer overflow vulnerability exists in the add_item function of the PRODUCT_MANAGEMENT_SYSTEM. The vulnerability arises from the use of the unsafe gets() function to populate fixed-size character arrays (name[30] and disease[30]) within the x[100] array. Since gets() does not perform bounds checking, input longer than 29 characters will overflow the buffers. 

This overflow can corrupt adjacent memory on the stack, including other patient records within the array, saved registers, local variables, and even the function's return address. As a result, attackers may exploit this flaw to cause a denial of service (DoS) or execute arbitrary code through crafted inputs.



### **Vulnerable Code Segments**

```
// In add() function:
printf("Enter patient's Name = ");
gets(x[i].name);  // First vulnerability

printf("Enter disease = ");
gets(x[i].disease);  // Second vulnerability
```

### **Root Cause Analysis**

1. **Unbounded Input Copying**:
   - Both `x[i].name` and `x[i].disease` are fixed-size character arrays (30 bytes)
   - `gets()` reads input until newline without any length checking
   - Inputs longer than 29 characters will overflow the buffers
2. **Memory Layout Impact**:
   - Overflows corrupt adjacent stack variables including:
     - Other patient records in the `x[100]` array
     - Function return addresses
     - Saved registers and local variables

![image-20250417151032575](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417151032575.png)





## POC

choose 1 "1. Add Information"

![image-20250417151057972](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417151057972.png)



Just type in any number

![image-20250417151137597](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417151137597.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250417151224907](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417151224907.png)



then,choose 3 "3. Search"

![image-20250417151237946](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417151237946.png)



choose 2 "Name"

![image-20250417151303875](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417151303875.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250417151338566](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417151338566.png)



result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250417151405785](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417151405785.png)
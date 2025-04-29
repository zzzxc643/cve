# Banking_System Buffer Overflow in Password Handling Function



## supplier

[Simple Banking System In C++ With Source Code - Source Code & Projects](https://code-projects.org/simple-banking-system-in-c-with-source-code/)



## **Vulnerability Type**:

 CWE-120 (Buffer Copy Without Size Checking)



## describe

The identified code contains a critical buffer overflow vulnerability in the password input handling mechanism, allowing attackers to execute arbitrary code or cause denial of service through carefully crafted long password inputs.



### **Vulnerable Code Segment**

cpp

复制

```
char password2[18];  // Fixed-size 18-byte buffer
i=0;
while((password2[i]=getch())!='\r') {  // No bounds checking
    printf("*");
    i++;  // Uncontrolled increment
}
```

### **Root Cause Analysis**

1. **Unbounded Input Copying**:
   - The `while` loop reads characters into `password2` until Enter (`\r`) is pressed
   - No validation of input length against buffer capacity (18 bytes)
2. **Missing Safety Mechanisms**:
   - No termination of the string with null byte
   - No check on the incrementing index `i`
3. **Memory Corruption Potential**:
   - Inputs >17 characters overflow into adjacent stack memory
   - Can overwrite return addresses, function pointers, and other critical data

![image-20250417143908915](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417143908915.png)





## POC



enter 2 after "Enter your choice : "

![image-20250417143621267](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417143621267.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250417143715042](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417143715042.png)





result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250417143749252](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417143749252.png)
# **Multiple Buffer Overflow Vulnerabilities in SIMPLE COLLEGE MANAGEMENT SYSTEM**



## supplier

[Simple College Management System In C++ With Source Code - Source Code & Projects](https://code-projects.org/simple-college-management-system-in-c-with-source-code/)



## **Vulnerability Types**:

- CWE-121: Stack-based Buffer Overflow
- CWE-242: Use of Inherently Dangerous Function (`gets()`)



## describle

A stack-based buffer overflow vulnerability exists due to the use of the unsafe `gets()` function to read input into fixed-size buffers `name[80]` and `branch[50]`. Since `gets()` does not perform any length checking, inputs exceeding 79 characters for `name` or 49 characters for `branch` will overflow the respective buffers. 

This can result in the corruption of adjacent stack variables, including other student records, as well as overwriting the function’s return address or other control flow data. The vulnerability can be exploited to cause a denial of service (DoS) or potentially allow arbitrary code execution through carefully crafted input.



### **Vulnerable Code Segment**

```
void input() {
    cout<<"\n Enter name: ";
    gets(name);  // First vulnerability (name[80])
    
    cout<<"\n Enter roll no: ";
    cin>>reg;
    fflush(stdin);
    
    cout<<"\n Enter Branch: ";
    gets(branch);  // Second vulnerability (branch[50])
}
```

### **Root Cause Analysis**

**Unbounded Input Copying**:

- `name[80]` and `branch[50]` are fixed-size buffers
- `gets()` reads until newline without length checking
- Inputs longer than buffer sizes will overflow:
  - 79+ chars for name → overflow
  - 49+ chars for branch → overflow

**Memory Corruption Targets**:

- Adjacent stack variables (including other student records)
- Function return addresses
- Program control flow data



![image-20250417164514070](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417164514070.png)





## POC



 Enter <1> to Add new student

![image-20250417164826056](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417164826056.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250417164853319](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417164853319.png)



result

we can see EXCEPTION_ACCESS_VIOLATION.



![image-20250417164923101](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417164923101.png)











- 


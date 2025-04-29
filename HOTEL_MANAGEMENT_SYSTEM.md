# HOTEL_MANAGEMENT_SYSTEM  stack overflow in edit function 



## supplier

[Hotel Management System in C Programming with Source Code - Source Code & Projects](https://code-projects.org/hotel-management-system-c-programming-source-code/)



## Vulnerability file

Unbounded Stack Buffer Overflow via `gets(s.roomnumber);` in `edit`



## **Vulnerability Overview**

The `edit()` function contains **two critical buffer overflow vulnerabilities** due to unsafe usage of:

1. `scanf("%[^\n]", roomnumber)` (no length restriction)
2. `gets(s.roomnumber)` (deprecated and highly unsafe)

An attacker can exploit these to **overwrite adjacent memory**, leading to **arbitrary code execution (RCE)** or **program crash (DoS)**.



## **Vulnerability Details**

### **Affected Code**

```
char roomnumber[20];  // Fixed-size buffer (20 bytes)

scanf("%[^\n]", roomnumber);  // No length limit → overflow possible

gets(s.roomnumber);  // Even worse: reads unlimited input → guaranteed overflow
```

![image-20250417105734250](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417105734250.png)



## POC

input username and password

![image-20250417105834332](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417105834332.png)



Enter 1 --> Book a room 

![image-20250417105851138](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417105851138.png)



Enter any number to create a room 

then,press esc key to exit

![image-20250417110020515](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417110020515.png)



enter 5  --> edit record

![image-20250417110138196](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417110138196.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250417110240733](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417110240733.png)



result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250417110453187](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417110453187.png)
# ALBUM_MANAGEMENT_SYSTEM stack overflow in searchalbum function 



## supplier

[Album Management System in C with Source Code - Source Code & Projects](https://code-projects.org/album-management-system-c-source-code/)



## Vulnerability file

Unbounded Stack Buffer Overflow via `scanf("%s", year);` in `searchalbum`



## describe

The `searchalbum()` function contains a **buffer overflow vulnerability** due to unsafe usage of `scanf("%s", year)` with a fixed-size buffer `char year[20]`. This allows an attacker to write beyond the allocated memory, potentially leading to **arbitrary code execution (RCE)** or **program crash (DoS)**.



## **Vulnerability Details**

### **Affected Code**

```
char year[20];  // Buffer size = 20 bytes (holds max 19 chars + null terminator)
scanf("%s", year);  // No length restriction → buffer overflow possible
```

### **Root Cause Analysis**

- **Fixed-size buffer (`year[20]`)** can only store **19 characters + null terminator**.
- **`scanf("%s", year)` reads unbounded input**, allowing:
  - **Stack overflow** if input exceeds 19 bytes.
  - **Memory corruption** (overwriting return addresses, variables, etc.).
  - **Control-flow hijacking** if exploited carefully.



![image-20250417112201952](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417112201952.png)



## POC

input username and password

![image-20250417112252900](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417112252900.png)



 Press  1 :  >> ADD NEW ALBUM

![image-20250417112314858](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417112314858.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

![image-20250417112433447](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417112433447.png)



 then press ESC to return back to < MAIN MENU >

![image-20250417112445124](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417112445124.png)



Press  4 :  >> SEARCH ALBUMS

![image-20250417112508652](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417112508652.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250417112600630](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417112600630.png)



result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250417112632441](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417112632441.png)
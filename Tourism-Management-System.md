# Buffer Overflow Vulnerabilities in Tourism Management System User Authentication

## Supplier

[Tourism Management System In C Programming With Source Code - Source Code & Projects](https://code-projects.org/tourism-management-system-in-c-programming-with-source-code/)



## Vulnerability File 

TourismManagement.c



## Description

The Tourism Management System contains multiple critical buffer overflow vulnerabilities in its user registration functionality (CWE-120). These vulnerabilities exist in the `AddUser()` function and allow unauthenticated attackers to perform stack-based buffer overflows through both username/email and password fields during user registration.



### Primary Vulnerabilities:

1. **Unbounded Username/Email Input**:
   - The system uses `scanf("%s", nw->username)` to read username/email without length validation
   - The destination buffer size is unknown (not visible in provided code) but clearly insufficient for large inputs
2. **Unbounded Password Input**:
   - The password field uses `scanf(" %[^\n]s", &nw->password)` which reads until newline without length checks
   - This format string is particularly dangerous as it will read entire lines of arbitrary length
3. **Memory Corruption Impact**:
   - Successful exploitation can corrupt heap memory structures (via malloc'd user objects)
   - May lead to arbitrary code execution or system crash
   - Could potentially overwrite adjacent user records or program control structures

## Vulnerability Details

### Vulnerable Code Segment



```
user* AddUser(user* h) {
    user *nw = (user*)malloc(sizeof(user));
    
    printf("Enter username or email\n");
    scanf("%s", nw->username);  // First overflow point (username)
    
    printf("Enter password\n");
    scanf(" %[^\n]s", &nw->password);  // Second overflow point (password)
    
    // ... rest of function ...
}
```

![image-20250428195105090](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428195105090.png)



### Root Cause Analysis

1. **Unsafe Input Functions**:
   - Both `scanf("%s")` and `scanf(" %[^\n]s")` are used without field width specifiers
   - These functions will write beyond allocated buffer boundaries
2. **Insufficient Memory Allocation**:
   - While the code allocates memory for user struct, it doesn't verify:
     - Size of username buffer within struct
     - Size of password buffer within struct
   - The buffers appear to be fixed-size array members of the user struct



## POC

enter 1

![image-20250428195128573](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428195128573.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

![image-20250428195206660](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428195206660.png)



result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250428195236401](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428195236401.png)
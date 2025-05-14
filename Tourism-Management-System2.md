# Buffer Overflow in Tourism Management System Authentication  LoginUser Function

## Supplier

[Tourism Management System In C Programming With Source Code - Source Code & Projects](https://code-projects.org/tourism-management-system-in-c-programming-with-source-code/)



## Vulnerability File 

TourismManagement.c



## Description

The Tourism Management System contains two critical stack-based buffer overflow vulnerabilities (CWE-121) in its login authentication function (`LoginUser()`). These vulnerabilities allow unauthenticated attackers to overflow fixed-size buffers through both username/email and password fields during login attempts, potentially leading to arbitrary code execution or system crash.



## Technical Details

### Vulnerable Code Segment

```
void LoginUser(user* h) {
    char username[100];  // Fixed-size buffer (100 bytes)
    char password[100];  // Fixed-size buffer (100 bytes)
    
    printf("\t\tEnter Email/Username:\n\t\t");
    scanf("%s",username);  // First unbounded read (no length limit)
    
    printf("\n\t\tEnter Password:\n\t\t");
    scanf(" %[^\n]s",password);  // Second unbounded read (reads until newline)
    
    // ... authentication logic ...
}
```

![image-20250428195944822](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428195944822.png)



### Root Cause Analysis

1. **Dual Unsafe Input Vectors**:
   - **Username Field**: `scanf("%s",username)` with no field width specification
   - **Password Field**: `scanf(" %[^\n]s",password)` that reads entire lines without length checks
2. **Buffer Limitations**:
   - Both buffers sized at 100 bytes (99 chars + null terminator)
   - Inputs exceeding 99 characters will overflow into adjacent stack memory



## POC

enter 2 " Login User - 2"

![image-20250428200222956](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428200222956.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

![image-20250428200309634](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428200309634.png)



result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250428200332187](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428200332187.png)


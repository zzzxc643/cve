# Buffer Overflow in Police Station Management System's remove() Function

## Supplier

[Police Station Management System In C++ With Source Code - Source Code & Projects](https://code-projects.org/police-station-management-system-in-c-with-source-code/)



## Vulnerability File

`source.cpp` (Police Station Management System source code)



## Description

The Police Station Management System contains a critical stack-based buffer overflow vulnerability (CWE-121) in the `criminal::remove()` function. The vulnerability stems from the unsafe use of the `gets()` function to read convict ID input into a fixed-size buffer without proper bounds checking. This allows attackers to overwrite adjacent stack memory, potentially leading to arbitrary code execution or denial of service.



## Vulnerability Details

### Vulnerable Code Segment



```
void criminal::remove()
{
  fflush(stdin);
  system("cls");
  char no[10];  // Fixed-size buffer (10 bytes)
  int s;
  cout << "\n\n\n\t\tENTER THE CONVICT ID : ";
  gets(no);  // Unsafe unbounded input copy
  s = this->dcheck(no);
  if(s!=0)
  this->delete_rec(no);
}
```

![image-20250428191810554](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428191810554.png)



### Root Cause Analysis

1. **Unsafe Function Usage**:
   - The code uses the deprecated `gets()` function which performs no bounds checking
   - This function has been removed entirely from modern C standards (C11 onwards) due to its inherent danger
2. **Inadequate Buffer Size**:
   - The `no[10]` buffer can only safely hold 9 characters plus a null terminator
   - Any input longer than 9 characters will overflow the buffer
3. **Memory Corruption Potential**:
   - Stack overflow can overwrite adjacent variables including the integer `s`
   - Depending on stack layout, could overwrite function return address
   - May corrupt the program's control flow and enable RCE
4. **Security Context**:
   - The function is part of critical record deletion functionality
   - Successful exploitation could lead to privilege escalation as it's part of an admin interface

## Proof of Concept (POC)

input username and password

![image-20250428191931443](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428191931443.png)



choose 4 "4. DELETE RECORD"

![image-20250428192018493](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428192018493.png)

enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

![image-20250428192242615](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428192242615.png)



result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250428192309803](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428192309803.png)
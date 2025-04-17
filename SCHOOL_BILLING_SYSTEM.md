#  SCHOOL_BILLING_SYSTEM stack overflow in searchrec function 



## supplier

[School Billing System In C Programming With Source Code - Source Code & Projects](https://code-projects.org/school-billing-system-c-programming-source-code/)



## Vulnerability file

Unbounded Stack Buffer Overflow via `scanf("%[^\n]",name);` in `searchrec()`



## describe

A stack-based buffer overflow vulnerability exists in the `searchrec()` function of the  School Billing System . The vulnerability allows attackers to cause a denial of service (DoS) and potentially execute arbitrary code through careful memory manipulation.



**Code analysis**

In the `searchrec()` function,  it is possible to enter data into the name variable without a limit on the length. Causing a stack overflow.



![image-20250417095720995](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417095720995.png)



## POC

input username and password

![image-20250417095833469](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417095833469.png)



enter 1 after  ' Enter Your Account Type :- '

![image-20250417095852405](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417095852405.png)



choose 1 "1>> Add Student's Record"

![image-20250417095922057](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417095922057.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250417100025012](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417100025012.png)



then,fill in some random numbers below and press esc

![image-20250417100045218](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417100045218.png)



enter 1 after  ' Enter Your Account Type :- '

![image-20250417100158572](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417100158572.png)



choose 2 " Search Student's Record"

![image-20250417100217799](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417100217799.png)



choose 1 " 1>>Search by name::"

![image-20250417100325597](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417100325597.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250417100423515](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417100423515.png)



then,input again 

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250417100458271](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417100458271.png)



result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250417100524202](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417100524202.png)
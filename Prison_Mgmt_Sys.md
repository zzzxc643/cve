# Prison_Mgmt_Sys  stack overflow in addrecord function 



## supplier

[Prison Management System In C Programming With Source Code - Source Code & Projects](https://code-projects.org/prison-management-system-c-programming-source-code/)



## Vulnerability file

Unbounded Stack Buffer Overflow via `gets(filename)` in `addrecord()`



## describe

A stack-based buffer overflow vulnerability exists in the `addrecord` function of the Prison_Mgmt_Sys. The vulnerability allows attackers to cause a denial of service (DoS) and potentially execute arbitrary code through careful memory manipulation.



**Code analysis**

In the addrecord function, the filename variable is 30 bytes long, but it is possible to enter data into the filename variable without a limit on the length. Causing a stack overflow.



![image-20250416164604991](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416164604991.png)



## POC

input username and password



![image-20250416164747580](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416164747580.png)



enter 1 after   " ENTER YOUR CHOICE:"



![image-20250416164801926](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416164801926.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250416165020485](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416165020485.png)



Result

we can see EXCEPTION_ACCESS_VIOLATION.

![](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416164934131.png)
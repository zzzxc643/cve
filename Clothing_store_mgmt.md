# Clothing store mgmt stack overflow in add_item function 



## supplier

[Clothing Store Management System In C Programming With Source Code - Source Code & Projects](https://code-projects.org/clothing-store-management-system-c-programming-source-code/)



## Vulnerability file

Unbounded Stack Buffer Overflow via `gets(st.productname)` in `add_item`



## describe

A stack-based buffer overflow vulnerability exists in the `add_item` function of the Clothing store mgmt. The vulnerability allows attackers to cause a denial of service (DoS) and potentially execute arbitrary code through careful memory manipulation.



**Code analysis**

In the `add_item` function,  it is possible to enter data into the filename variable without a limit on the length. Causing a stack overflow.

![image-20250416192609475](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416192609475.png)



## POC

input username and password

![image-20250416192650425](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416192650425.png)



enter 1 after   " Enter your choice[1-6]"

![image-20250416192705137](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416192705137.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250416192817198](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416192817198.png)

then，press any other key to return menu

![image-20250416192836080](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416192836080.png)



enter 3 after   " Enter your choice[1-6]"

![image-20250416192927671](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416192927671.png)



input payload

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

![image-20250416193039452](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416193039452.png)

result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250416193108221](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416193108221.png)
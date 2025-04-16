# PRODUCT_MANAGEMENT_SYSTEM stack overflow in add_item function 



## supplier

[Product Management System In C Programming With Source Code - Source Code & Projects](https://code-projects.org/product-management-system-c-programming-source-code/)



## Vulnerability file

Unbounded Stack Buffer Overflow via `gets(st.productname)` in `add_item`



## describe

A stack-based buffer overflow vulnerability exists in the `add_item` function of thePRODUCT_MANAGEMENT_SYSTEM. The vulnerability allows attackers to cause a denial of service (DoS) and potentially execute arbitrary code through careful memory manipulation.



**Code analysis**

In the `add_item` function,  it is possible to enter data into the st.productname variable without a limit on the length. Causing a stack overflow.



```
struct item
{
	char productname[40],productcomp[40],c;
	int productid;
	int price;
	int Qnt;
}st;
```

![image-20250416205759454](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416205759454.png)



## POC

input username and password

![image-20250416205827297](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416205827297.png)



enter 1 after   " Enter your choice[1-6]"

![image-20250416205839640](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416205839640.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250416205958923](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416205958923.png)



then，press any other key to return menu

enter 3 after   " Enter your choice[1-6]"

![image-20250416210025221](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416210025221.png)



input payload

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

![image-20250416210108088](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416210108088.png)



result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250416210134608](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416210134608.png)
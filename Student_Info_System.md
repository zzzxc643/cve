# Student Information Management System stack overflow in main function 



## supplier

[Student Information Management System In C++ With Source Code - Source Code & Projects](https://code-projects.org/student-information-management-system-c-source-code/)



## Vulnerability file

Unbounded Stack Buffer Overflow via `cin >> e.first_name; cin >> e.last_name; cin >> e.course; cin >> e.section;` in `main()`



## describe

A stack-based buffer overflow vulnerability exists in the `main()` function of the Theatre_booking_System. The vulnerability allows attackers to cause a denial of service (DoS) and potentially execute arbitrary code through careful memory manipulation.



### **Code analysis**

In the cancel() function, the  first_name、last_name variable is 50 bytes long and the  course variable is 100 bytes long, but it is possible to enter data into the  cancelcustomername variable without a limit on the length. Causing a stack overflow.



```
struct student {

​    char first_name[50], last_name[50];

​    char course[100];

​    int section;

  };
```



![image-20250416202001008](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416202001008.png)



## POC

input username and password

![image-20250416202039115](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416202039115.png)

then, input 1 after 'Select Your Choice ::'

![image-20250416202136700](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416202136700.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

![image-20250416202229443](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416202229443.png)



Result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250416202333732](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416202333732.png)
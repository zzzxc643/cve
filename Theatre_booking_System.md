# Theatre_booking_System  stack overflow in cancel function 



## supplier

[Theater Seat Booking System In C Programming With Source Code - Source Code & Projects](https://code-projects.org/theater-seat-booking-system-c-programming-source-code/)



## Vulnerability file

Unbounded Stack Buffer Overflow via `cin>>cancelcustomername;` in `cancel()`



## describe

A stack-based buffer overflow vulnerability exists in the `cancel()` function of the Theatre_booking_System. The vulnerability allows attackers to cause a denial of service (DoS) and potentially execute arbitrary code through careful memory manipulation.



### **Code analysis**

In the cancel() function, the  cancelcustomername variable is 80 bytes long, but it is possible to enter data into the  cancelcustomername variable without a limit on the length. Causing a stack overflow.

![image-20250416194653531](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416194653531.png)





## POC

enter 2 after  'Enter your selection : '

![image-20250416194807964](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416194807964.png)

enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

![image-20250416194855992](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416194855992.png)



Result

we can see EXCEPTION_ACCESS_VIOLATION.



![image-20250416195057094](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416195057094.png)
# MOVIE TICKET BOOKING SYSTEM Buffer Overflow in Password Authentication Function



# supplier

[Simple Movie Ticket Booking System In C Programming With Source Code - Source Code & Projects](https://code-projects.org/simple-movie-ticket-booking-system-in-c-programming-with-source-code/)



## Vulnerability file

CWE-121: Stack-based Buffer Overflow



## describe

A stack-based buffer overflow vulnerability exists in the `changeprize` function of the `PRODUCT_MANAGEMENT_SYSTEM`. The vulnerability is caused by the use of `scanf("%s", &pass)` to read user input into a fixed-size buffer `pass[10]`, which can only safely hold 9 characters plus a null terminator. Since `scanf("%s")` does not enforce any length restriction, input of 10 or more bytes will overflow the buffer.

 This overflow can lead to memory corruption, overwriting the adjacent hard-coded password buffer `pak[10]`, and potentially tampering with the function’s return address depending on the stack layout. This flaw can be exploited to cause a denial of service (DoS) or execute arbitrary code.



### **Vulnerable Code Segment**

```
int changeprize(int prize) {
    char pass[10], pak[10] = "pass";  // Fixed-size buffers
    printf("Enter the password to change price of ticket: ");
    scanf("%s", &pass);  // Unbounded input copy
    if (strcmp(pass, pak) == 0) { ... }
}
```

### **Root Cause Analysis**

1. **Unbounded Input Copy**:
   - `pass[10]` buffer can only hold 9 chars + null terminator
   - `scanf("%s", &pass)` reads until whitespace with no length check
   - Inputs ≥10 bytes will overflow into adjacent memory
2. **Memory Corruption Targets**:
   - Overwrites the hard-coded password buffer `pak[10]`
   - Potentially corrupts function return address (stack layout dependent)



![image-20250417154459898](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417154459898.png)



## POC

choose 1 " 1- To edit price of ticket (only admin):  "

![image-20250417154555719](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417154555719.png)

enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250417154631610](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417154631610.png)



result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250417154657734](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250417154657734.png)
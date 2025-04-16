# MOVIE TICKET BOOKING SYSTEM stack overflow in insert_details function 



## supplier

[Movie Ticket Booking System In C Programming With Source Code - Source Code & Projects](https://code-projects.org/movie-ticket-booking-system-c-programming-source-code/)



## Vulnerability file

Unbounded Stack Buffer Overflow via `scanf("%s",b.code); scanf("%s",b.name); scanf("%s",b.date); scanf("%d",&b.cost);`    in `insert_details()`



## describe

The code reads user input using the insecure `form scanf("%s", buffer).`All target buffer sizes are fixed at 20 bytes (including terminating null characters)

An attacker can input a string of more than 19 characters, resulting in:

- Adjacent memory data is overwritten
- Program control flows can be hijacked
- Sensitive data breaches



### **Code analysis**

All target buffer sizes are fixed at 20 bytes (including terminating null characters)

```
struct book

{

  char code[20];

  char name[20];

  char date[20];

  int cost;

}b;

```



![image-20250416204203284](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416204203284.png)



## POC

input user and password



![image-20250416204243381](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416204243381.png)



input 1 after ‘Enter movie code :- ’

![image-20250416204315787](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416204315787.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250416204446072](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416204446072.png)



Result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250416204520127](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416204520127.png)


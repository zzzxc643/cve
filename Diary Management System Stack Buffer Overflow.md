# **Stack-Based Buffer Overflow in Diary Management System's `addrecord()` Function**

## **Supplier**

[Personal Diary Management System In C++ With Source Code - Source Code & Projects](https://code-projects.org/personal-diary-management-system-in-c-with-source-code/)

## **Vulnerability File**

Unbounded Stack Buffer Overflow via `gets(filename)` in `addrecord()`

## **Description**

A stack-based buffer overflow vulnerability exists in the `addrecord` function of the Personal Diary Management System. The vulnerability allows attackers to cause a denial of service (DoS) and potentially execute arbitrary code through careful memory manipulation.

**Root Cause Analysis**
The vulnerable code segment:



```
void addrecord( )
{
    system("cls");
    FILE *fp ;
    char another = 'Y' ,time[10];
    struct record e ;
    char filename[15];
    int choice;
    cout<<"\n\n\t\t***************************\n";
    cout<<"\t\t* WELCOME TO THE MENU *";
    cout<<"\n\t\t***************************\n\n";
    cout<<"\n\n\tENTER DATE OF YOUR RECORD:[yyyy-mm-dd]:";
    fflush(stdin);
    gets(filename);
    fp = fopen (filename, "ab+" ) ;
    ...
}
```

The `filename` buffer is allocated 15 bytes on the stack, but the unsafe `gets()` function allows writing beyond this boundary with no length validation.

## **Proof of Concept (PoC)**

Input password:

![image-20250416103922216](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416103922216.png)



enter 1 after   "Enter your choice:"

![image-20250416103945073](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416103945073.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```



![image-20250416104134199](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416104134199.png)



Result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250416104236382](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416104236382.png)
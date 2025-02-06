## POLICE FIR RECORD MANAGEMENT SYSTEM stack overflow in deleterecord function filename parameter
## supplier 

https://download.code-projects.org/details/dd1cefe5-8ddc-495c-a75d-bffd95ed74ff

## Vulnerability file

filename parameter in deleterecord function.

## describe

There is a stack overflow vulnerability in  deleterecord function And filename parameter of Police fir record management system. it cause ddos and rce.

**Code analysis**    

In the deleterecord function, the filename variable is 16 bytes long, but it is possible to enter data into the filename variable without a limit on the length. Causing a stack overflow.

![Image](https://github.com/user-attachments/assets/e9c85871-c85e-42e8-87f9-6f28d4358168)

![Image](https://github.com/user-attachments/assets/9f581d11-f9b4-4080-830e-e39b68ddd2f6)



## POC

enter 5  after "enter your choice"

![Image](https://github.com/user-attachments/assets/9a6144dc-3945-40ef-94a5-537c5088eb44)

enter password: pass

and enter payload to cause the stack overflow to crash

![Image](https://github.com/user-attachments/assets/3c55204d-4145-4802-962f-17f6ce423635)

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

Result

we can see EXCEPTION_ACCESS_VIOLATION.

![Image](https://github.com/user-attachments/assets/68300059-68b5-4417-b8e3-4d4d114821fb)
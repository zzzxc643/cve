# Multiple Unbounded Input Vulnerabilities in Pharmacy Management System

## Supplier

[Pharmacy Management System In C++ With Source Code - Source Code & Projects](https://code-projects.org/pharmacy-management-system-in-c-with-source-code/)

## Vulnerability File

Main application source file containing order processing logic

## Description

The Pharmacy Management System contains multiple critical unbounded input vulnerabilities (CWE-120, CWE-125) in its `medicineType::take_order()` function. These vulnerabilities stem from unsafe usage of `cin` for user input without proper length validation, allowing attackers to overflow buffers in several fields during the order-taking process. Successful exploitation could lead to memory corruption, arbitrary code execution, or system crashes.



## Vulnerability Details

### Vulnerable Code Segments

```
void medicineType::take_order() {
    node *temp = new node;
    
    cout << "Type Order no: ";
    cin >> temp->reciept_number;  // Unsafe numeric input
    
    cout << "Enter Customer Name: ";
    cin >> temp->customerName;  // Unsafe string input (1st vector)
    
    cout << "Enter Date : ";
    cin >> temp->date;  // Unsafe string input (2nd vector)
    
    cout << "How many Medicine would you like to order:";
    cin >> temp->x;  // Unsafe numeric input
    
    for (i=0; i<temp->x; i++) {
        cout << "Please enter your selection : ";
        cin >> temp->menu2[i];  // Unsafe array input (3rd vector)
        // ... additional vulnerable inputs ...
    }
}
```

![image-20250428201726567](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428201726567.png)



### Root Cause Analysis

1. **Unbounded Input Operations**:
   - All `cin` operations lack input length validation
   - No protection against buffer overflows for string fields
   - No range checking for numeric inputs
2. **Critical Vulnerable Fields**:
   - `customerName`: Likely fixed-size character array vulnerable to overflow
   - `date`: Date field susceptible to string-based overflow
   - `menu2[]`: Array input without bounds checking
   - `quantity[]`: Potential integer overflow/underflow



## POC

enter choice 1

![image-20250428201810467](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428201810467.png)



Any id in the random list

![image-20250428201917347](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428201917347.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

![image-20250428201959560](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428201959560.png)



Output numbers within 10

![image-20250428202157911](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428202157911.png)



enter payload to cause the stack overflow to crash

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

![image-20250428202223157](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428202223157.png)



result

we can see EXCEPTION_ACCESS_VIOLATION.

![image-20250428202243851](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250428202243851.png)
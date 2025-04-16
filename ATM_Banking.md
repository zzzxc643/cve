### **ATM Simulator Improper Input Validation in Financial Transactions**



#### **Vulnerability Title**

**ATM Simulator Negative Value Transaction Validation Vulnerability**



### supplier

[ATM Banking In C Programming With Source Code - Source Code & Projects](https://code-projects.org/atm-banking-in-c-programming-with-source-code/)



#### **Vulnerability Type**

[CWE-20: Improper Input Validation](https://cwe.mitre.org/data/definitions/20.html)



#### **Affected Software **

- **Product**: ATM Simulator (C-based, no versioning)
- **Impacted Functions**: `moneyDeposit()`, `moneyWithdraw()`



#### **Vulnerability Description **

The ATM Simulator fails to validate transaction amounts, allowing **negative values** in deposits and withdrawals. An attacker can exploit this to:

- **Deposit negative values** → Illegally reduce balance (acts as withdrawal).
- **Withdraw negative values** → Illegally inflate balance (acts as deposit).
- **Bypass financial logic**, leading to incorrect balance calculations.



#### **Proof of Concept (PoC)**



```
// Attack Example 1: Negative Deposit (Balance Decrease)
1. Select "Deposit" (Option 2).
2. Enter amount: "-1000"  
   → Balance changes from 15000.00 to 14000.00 (instead of increasing).
```

![image-20250416144446615](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416144446615.png)



![image-20250416144331148](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416144331148.png)



```
// Attack Example 2: Negative Withdrawal (Balance Increase)

1. Select "Withdraw" (Option 3).
2. Enter amount: "-5000"  
   → Balance changes from 15000.00 to 20000.00 (illegal gain).
```

![image-20250416144528423](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416144528423.png)



![image-20250416144544055](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250416144544055.png)





#### **Remediation**

1. **Input Validation**:

   ```
   if (amount <= 0) {  
       printf("Error: Amount must be positive.\n");  
       return balance;  
   }  
   ```

2. Use `unsigned int` or fixed-point arithmetic for financial values.

3. Implement transaction logging for audit trails.



#### **References**

- Vulnerable Code: `moneyDeposit()` and `moneyWithdraw()` functions.
- Similar CVE: [CVE-2021-3156](https://nvd.nist.gov/vuln/detail/CVE-2021-3156) (Input validation bypass).












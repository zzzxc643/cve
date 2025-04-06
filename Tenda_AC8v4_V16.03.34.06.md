# Tenda_AC8v4_V16.03.34.06



## Vulnerability Summary
A stack-based buffer overflow vulnerability exists in the `formSetFirewallCfg` function of Tenda AC8 v4 router firmware version V16.03.34.06. The vulnerability allows remote attackers to cause denial-of-service (DoS) or potentially execute arbitrary code via crafted HTTP requests.

## Affected Product
- **Product**: Tenda AC8 v4 Router
- **Firmware Version**: V16.03.34.06
- **Download**: [Official Firmware Download](https://tenda.com.cn/material/show/103518)

## Vulnerability Details
### Root Cause
The vulnerability occurs in the `formSetFirewallCfg` function where the `strcpy` function is used to copy user-controlled input (`firewallEn` parameter) to a fixed-size buffer (`local_98`) without proper length validation.

![Vulnerable Code](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250406170541379.png)

### Attack Vector
The vulnerable endpoint `/goform/SetFirewallCfg` is accessible via HTTP POST requests. The `firewallEn` parameter is directly passed to the vulnerable `strcpy` operation.

### Impact
- **Denial-of-Service**: Crash of the httpd service (confirmed)
- **Remote Code Execution**: Potential RCE via stack overflow (theoretical)

## Proof-of-Concept
```python
import requests

url = "http://192.168.124.110/goform/SetFirewallCfg"
data = {"firewallEn": b'A'*10000000}  # 10MB payload
response = requests.post(url, data=data)
print(response.content)
```



## Reproduction Steps

1. Set up the vulnerable environment using QEMU:
   ![QEMU Setup](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250406171356463.png)
2. Execute the PoC script to trigger the overflow:
   ![PoC Execution](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250406171500678.png)
3. Observe httpd service crash with segmentation fault.

## Evidence

[Video Demonstration](https://pan.baidu.com/s/13XmHwgde_H3-G_TXkdGZxQ?pwd=hf1x) (Password: hf1x)

## Technical Analysis

### Vulnerability Location

The vulnerable code path:

```
HTTP Request → SetFirewallCfg → formSetFirewallCfg → strcpy(local_98, __s)
```

![Call Trace](https://raw.githubusercontent.com/zzzxc643/images/main/image/image-20250406170957415.png)

### Buffer Overflow Mechanics

- **Buffer Size**: ~152 bytes (`local_98`)
- **Overflow Trigger**: 10,000,000 byte payload
- **Crash Type**: Stack overflow → SIGSEGV

## Suggested Mitigation

1. Replace `strcpy` with `strncpy` with proper length checking
2. Implement input validation for HTTP parameters
3. Enable stack protection mechanisms (e.g., stack canaries)

## References

1. [Tenda Official Website](https://www.tenda.com.cn/)
2. [Firmware Download](https://tenda.com.cn/material/show/103518)


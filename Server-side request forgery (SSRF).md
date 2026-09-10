## Server-side request forgery (SSRF)

### SSRF

#### 正常情况

假设三台机器：
```
你的电脑：
192.168.1.50

网站服务器：
54.20.10.8

库存服务器：
10.0.0.20
```

正常请求：
```
你的电脑
      ↓
54.20.10.8   网站服务器
      ↓
10.0.0.20    库存服务器
```

你的电脑可能根本访问不了：
```
10.0.0.20
```

因为它是服务器内部网络里的地址。

但网站服务器可以访问。这就是 SSRF 为什么有意义。

---

#### SSRF 情况

你把参数改成：
``` python
stockApi=http://localhost/admin
```

这里最重要：

localhost 是相对于“谁发出请求”来说的。

因为真正发这个 HTTP 请求的是：
网站服务器
所以：
```
http://localhost/admin
```
指的是：

网站服务器自己。

不是你的电脑。

于是实际关系变成：
```
你的电脑
   ↓
网站服务器
   ↓
网站服务器自己的 localhost
   ↓
/admin
```
---

### Example

Step 1 : 调用库存访问 得到入口

<img width="1440" height="900" alt="截屏2026-09-09 23 05 01" src="https://github.com/user-attachments/assets/864fee0f-17da-4df7-913a-81bbedef22d0" />
---

Step 2 ： 篡改API

<img width="1439" height="401" alt="截屏2026-09-09 23 06 11" src="https://github.com/user-attachments/assets/acc5adf8-4b57-4464-9f70-484dd4eaeae0" />

---

Step 3 ： Find Delete 

<img width="1436" height="388" alt="截屏2026-09-09 23 09 14" src="https://github.com/user-attachments/assets/dd696c98-ef0c-4b9c-afd4-12a11a3c6ddc" />

---
<img width="1181" height="494" alt="截屏2026-09-09 23 08 40" src="https://github.com/user-attachments/assets/0b854cc1-6b9f-4bab-a7fd-094d8c45a8f3" />

---

Step 4: Change API to Delte API

<img width="1096" height="392" alt="截屏2026-09-09 23 10 29" src="https://github.com/user-attachments/assets/70b587bd-b9d8-4707-9d11-804f29d9e6a8" />


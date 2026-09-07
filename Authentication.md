# Authentication

## 1 Username brute force

测试网站时，不要只盯着登录页，要看网站有没有公开泄露用户名

1 HTTP Response

2 很多网站的用户名其实很好猜，因为它们有固定格式

## 2  Brute-forcing passwords

Password brute force 是通过大量尝试候选密码来寻找正确密码；密码越长、越随机、越不可预测，破解通常越困难。

1 try to crowbar it into fitting the password policy

2 强制换密码也可能出现规律

3 按照规律猜

## 3 Bypassing two-factor authentication

```text
用户名 + 密码
      ↓
服务器已经把你标记成“已登录”  ← 问题在这里
      ↓
再让你去输入验证码
```

从程序逻辑上看，漏洞大概类似：
```python
if password_correct:
    session["logged_in"] = True
```

然后受保护页面只判断：
``` python
if session["logged_in"]:
    show_account_page()
```
正确做法应该额外记录：
``` python
session["password_ok"] = True
session["two_factor_ok"] = False
```

只有：
``` python
if password_ok and two_factor_ok:
    show_account_page()
```

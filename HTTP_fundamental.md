# HTTP Fundamentals — URL、HTTP Flow、Headers、cURL、DevTools

> Hack The Box — HTTP Fundamentals Notes

---

# 1. HTTP 是什么？

## HTTP

**HTTP = HyperText Transfer Protocol（超文本传输协议）**

HTTP 是一种 **Application Layer Protocol（应用层协议）**，主要用于客户端和服务器之间传输 Web 资源。

现代 Web 应用、移动应用以及大量 API 都依赖 HTTP/HTTPS 进行通信。

基本模型：

```text
Client
   |
   | HTTP Request
   v
Server
   |
   | HTTP Response
   v
Client
```

例如：

```text
Browser  --->  Web Server
        GET /
Browser  <---  Web Server
        HTML
```

核心思想：

> 客户端发送 Request，服务器返回 Response。

---

# 2. Client 和 Server

HTTP 通信主要由两端组成：

```text
Client <---- HTTP ----> Server
```

## Client

客户端负责：

- 请求资源
- 发送 HTTP Request
- 接收 HTTP Response
- 展示返回的数据

常见客户端：

- Chrome
- Firefox
- Safari
- cURL
- Burp Suite
- Python requests

---

## Server

服务器负责：

- 接收 Request
- 处理 Request
- 查找或生成资源
- 返回 Response

例如：

```text
GET /index.html
```

服务器可能返回：

```text
index.html
```

---

# 3. HTTP 默认端口

HTTP 默认端口：

```text
80
```

例如：

```text
http://example.com:80
```

由于 `80` 是 HTTP 默认端口，所以通常可以写成：

```text
http://example.com
```

---

HTTPS 默认端口：

```text
443
```

例如：

```text
https://example.com:443
```

通常直接写：

```text
https://example.com
```

---

# 4. FQDN

## FQDN = Fully Qualified Domain Name

中文：

```text
完全限定域名
```

例如：

```text
www.hackthebox.com
```

```text
academy.hackthebox.com
```

```text
inlanefreight.com
```

FQDN 用于唯一确定 Internet 中的某个主机。

注意：

```text
https://www.hackthebox.com
```

是 URL。

其中：

```text
www.hackthebox.com
```

才是 Host / FQDN。

---

# 5. URL

## URL = Uniform Resource Locator

中文：

```text
统一资源定位符
```

URL 用来告诉客户端：

> 我要访问哪个服务器上的哪个资源，以及应该怎样访问。

完整 URL 可能非常复杂。

例如：

```text
http://admin:password@inlanefreight.com:80/dashboard.php?login=true#status
```

可以拆成：

```text
http://
admin:password@
inlanefreight.com
:80
/dashboard.php
?login=true
#status
```

对应：

```text
Scheme
User Info
Host
Port
Path
Query String
Fragment
```

---

# 6. URL 完整结构

```text
http://admin:password@inlanefreight.com:80/dashboard.php?login=true#status
│      │              │                 │  │              │           │
│      │              │                 │  │              │           └─ Fragment
│      │              │                 │  │              └─ Query String
│      │              │                 │  └─ Path
│      │              │                 └─ Port
│      │              └─ Host
│      └─ User Info
└─ Scheme
```

---

# 7. Scheme

例如：

```text
http://
```

或者：

```text
https://
```

Scheme 指定：

> 使用什么协议访问资源。

例如：

```text
http://example.com
```

使用：

```text
HTTP
```

而：

```text
https://example.com
```

使用：

```text
HTTPS
```

格式通常为：

```text
scheme://
```

---

# 8. User Info

例如：

```text
admin:password@
```

格式：

```text
username:password@
```

例如：

```text
http://admin:password@example.com
```

意思是：

```text
Username = admin
Password = password
```

这是 URL 中的一个可选部分。

现代网站一般不会直接这样传递凭据，因为这样可能泄露敏感信息。

---

# 9. Host

例如：

```text
inlanefreight.com
```

Host 表示：

> 目标服务器的位置。

可以是：

```text
Domain Name
```

例如：

```text
google.com
```

也可以是：

```text
IP Address
```

例如：

```text
192.168.1.10
```

---

# 10. Port

例如：

```text
:80
```

Port 和 Host 使用：

```text
:
```

分隔。

格式：

```text
host:port
```

例如：

```text
example.com:8080
```

默认：

```text
HTTP  -> 80
HTTPS -> 443
```

所以：

```text
http://example.com
```

基本等价于：

```text
http://example.com:80
```

---

# 11. Path

例如：

```text
/dashboard.php
```

Path 指定：

> 服务器上需要访问的资源。

例如：

```text
/index.html
```

```text
/users/login.html
```

```text
/images/logo.png
```

```text
/api/users
```

例如：

```text
https://example.com/users/login.html
```

其中：

```text
/users/login.html
```

就是 Path。

---

# 12. Query String

例如：

```text
?login=true
```

Query String 通常用来：

> 向服务器传递参数。

基本格式：

```text
?key=value
```

例如：

```text
?username=admin
```

---

## 多个参数

多个参数使用：

```text
&
```

分隔。

例如：

```text
?username=admin&password=123
```

这里：

```text
username = admin
password = 123
```

---

另一个例子：

```text
/search?q=HTTP&page=2
```

其中：

```text
q = HTTP
page = 2
```

---

# 13. Fragment

例如：

```text
#status
```

Fragment 用来定位页面中的某个部分。

例如：

```text
https://example.com/page.html#chapter2
```

浏览器可能直接滚动到：

```text
chapter2
```

对应的位置。

非常重要：

> Fragment 通常由浏览器在客户端处理，不会作为 HTTP Request 的一部分发送给服务器。

例如浏览器访问：

```text
https://example.com/index.html#test
```

服务器通常看到的是：

```text
/index.html
```

而不是：

```text
/index.html#test
```

---

# 14. URL 各组件总结

例如：

```text
http://admin:password@inlanefreight.com:80/dashboard.php?login=true#status
```

| Component | Example | 作用 |
|---|---|---|
| Scheme | `http://` | 指定协议 |
| User Info | `admin:password@` | 用户认证信息 |
| Host | `inlanefreight.com` | 目标服务器 |
| Port | `:80` | 服务端口 |
| Path | `/dashboard.php` | 请求资源 |
| Query String | `?login=true` | 向服务器传递参数 |
| Fragment | `#status` | 定位页面中的某部分 |

最核心的通常是：

```text
Scheme + Host
```

其他组件根据需要出现。

---

# 15. 一个常见 URL

```text
https://example.com/products?id=10#reviews
```

拆开：

```text
Scheme:
https://

Host:
example.com

Path:
/products

Query String:
?id=10

Fragment:
#reviews
```

---

# 16. HTTP Flow

当我们输入：

```text
http://inlanefreight.com
```

并按下 Enter，并不是浏览器直接知道服务器在哪里。

大致过程：

```text
User
 ↓
Browser
 ↓
DNS
 ↓
IP Address
 ↓
Web Server
 ↓
HTTP Response
 ↓
Browser
```

---

# 17. Step 1：用户输入 URL

用户输入：

```text
http://inlanefreight.com
```

浏览器首先需要知道：

```text
inlanefreight.com
```

对应哪个 IP。

---

# 18. Step 2：DNS Resolution

浏览器向 DNS Server 查询：

```text
Where is inlanefreight.com?
```

DNS 可能返回：

```text
152.153.81.14
```

所以：

```text
inlanefreight.com
        ↓ DNS
152.153.81.14
```

DNS 的作用就是：

> Domain Name → IP Address

因为网络通信最终需要 IP 地址。

---

# 19. Step 3：连接服务器

如果访问：

```text
http://inlanefreight.com
```

默认 HTTP Port 为：

```text
80
```

所以浏览器最终可能连接：

```text
152.153.81.14:80
```

---

# 20. Step 4：发送 HTTP Request

浏览器发送：

```http
GET / HTTP/1.1
Host: inlanefreight.com
```

意思：

```text
GET
```

我要获取资源。

```text
/
```

我要网站根路径。

```text
HTTP/1.1
```

使用 HTTP/1.1。

---

# 21. Step 5：服务器发送 HTTP Response

服务器处理请求后返回：

```http
HTTP/1.1 200 OK
```

然后返回：

```html
<html>
...
</html>
```

---

# 22. Step 6：浏览器渲染页面

浏览器收到 HTML 后解析：

```html
<html>
...
</html>
```

然后显示网页。

---

# 23. HTTP Flow 总结

```text
1. 输入 URL

http://inlanefreight.com

        ↓

2. DNS 查询

inlanefreight.com
        ↓
152.153.81.14

        ↓

3. 建立网络连接

152.153.81.14:80

        ↓

4. HTTP Request

GET / HTTP/1.1
Host: inlanefreight.com

        ↓

5. HTTP Response

HTTP/1.1 200 OK

        ↓

6. 返回 HTML

<html>
...
</html>

        ↓

7. Browser Render
```

---

# 24. HTTP Request 基本结构

例如访问：

```text
http://inlanefreight.com/users/login.html
```

可能产生：

```http
GET /users/login.html HTTP/1.1
Host: inlanefreight.com
User-Agent: Mozilla/5.0
Accept: text/html
Cookie: PHPSESSID=123456
```

HTTP Request 大致结构：

```text
Request Line
Headers

Body
```

即：

```text
Request Line
Headers
空行
Request Body（可选）
```

---

# 25. HTTP Request 第一行

任何 HTTP/1.x Request 第一行通常包含三个核心字段：

```text
Method Path Version
```

例如：

```http
GET /users/login.html HTTP/1.1
```

拆开：

```text
GET
```

HTTP Method

```text
/users/login.html
```

Path

```text
HTTP/1.1
```

HTTP Version

---

# 26. Method

例如：

```text
GET
```

HTTP Method 表示：

> 客户端希望服务器执行什么操作。

常见：

```text
GET
POST
PUT
DELETE
HEAD
OPTIONS
PATCH
```

当前最需要先掌握：

```text
GET
POST
HEAD
```

---

# 27. GET

GET 通常表示：

> 获取资源。

例如：

```http
GET /index.html HTTP/1.1
```

意思：

```text
把 /index.html 给我。
```

---

# 28. Path in HTTP Request

例如：

```text
/users/login.html
```

表示服务器中的资源路径。

完整 URL：

```text
http://inlanefreight.com/users/login.html
```

真正 Request Line 中一般不是写整个 URL，而是：

```http
GET /users/login.html HTTP/1.1
```

Host 单独放在：

```http
Host: inlanefreight.com
```

---

# 29. Query String in HTTP Request

URL：

```text
http://example.com/search?q=test
```

Request Line 可能是：

```http
GET /search?q=test HTTP/1.1
```

所以：

```text
Path + Query String
```

都会出现在请求目标中。

---

# 30. HTTP Version

例如：

```text
HTTP/1.1
```

表示客户端使用的 HTTP 协议版本。

例如：

```http
GET / HTTP/1.1
```

---

# 31. HTTP Headers

Request Line 后面通常是一系列：

```text
Header: Value
```

例如：

```http
Host: inlanefreight.com
User-Agent: Mozilla/5.0
Accept: text/html
Cookie: PHPSESSID=123456
```

基本格式：

```text
Header-Name: Header-Value
```

---

# 32. Header 的作用

Headers 用来：

> 提供 Request 或 Response 的附加信息和上下文。

例如：

```http
User-Agent: Firefox
```

告诉服务器：

```text
我是 Firefox 浏览器
```

例如：

```http
Content-Type: application/json
```

告诉对方：

```text
我发送的数据是 JSON
```

---

# 33. Headers 和 Body

HTTP Request：

```text
Request Line
Headers
空行
Body
```

例如：

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 29

username=admin&password=test
```

空行非常重要：

```text
Headers
↓
空行
↓
Body
```

它表示 Headers 结束。

---

# 34. HTTP/1.x 与 HTTP/2

## HTTP/1.x

HTTP/1.x 主要使用文本形式表示请求和响应。

例如：

```http
GET / HTTP/1.1
Host: example.com
```

字段之间通过：

```text
换行
```

进行区分。

---

## HTTP/2

HTTP/2 不再简单使用这种纯文本格式在网络上传输。

它使用：

```text
Binary Framing
```

即：

```text
二进制帧
```

但在 Burp Suite、浏览器 DevTools 等工具中，通常仍会以方便人阅读的形式展示。

---

# 35. HTTP Request 示例分析

```http
GET /users/login.html HTTP/1.1
Host: inlanefreight.com
User-Agent: Mozilla/5.0 (Ubuntu; Linux x86_64; Firefox/78.0)
Accept: text/html,application/xhtml+xml,application/xml
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: text/html; charset=UTF-8
Connection: close
Cookie: PHPSESSID=c4ggt4jul19obt7aupa55o8vbf
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0
```

第一行：

```text
GET /users/login.html HTTP/1.1
```

表示：

```text
Method  = GET
Path    = /users/login.html
Version = HTTP/1.1
```

后面的全部都是 Headers。

---

# 36. Browser DevTools

现代浏览器都有：

```text
Developer Tools
```

简称：

```text
DevTools
```

常用打开方式：

```text
F12
```

或者：

```text
Ctrl + Shift + I
```

---

# 37. DevTools 中最重要的 Network

Web Security 学习里非常重要的一个 Tab：

```text
Network
```

Network 可以看到：

- 浏览器发送了哪些 HTTP Request
- Request Method
- URL
- Status Code
- Request Headers
- Response Headers
- Cookies
- Request Body
- Response Body
- 文件大小
- 请求时间
- 请求类型

---

# 38. 如何查看 Network

步骤：

```text
1. F12
2. 打开 Network
3. 刷新网页
4. 查看请求列表
5. 点击某个请求
```

浏览器加载一个网页时通常不会只发送一个请求。

它可能发送：

```text
HTML Request
CSS Request
JavaScript Request
Image Request
API Request
Font Request
favicon.ico Request
```

所以：

> 一个网页通常对应多个 HTTP Requests。

---

# 39. HTTP Headers 分类

HTB 将常见 HTTP Headers 分成：

```text
1. General Headers
2. Entity Headers
3. Request Headers
4. Response Headers
5. Security Headers
```

---

# 40. General Headers

中文：

```text
通用标头
```

General Headers 可以出现在：

```text
Request
Response
```

主要描述：

> 消息本身，而不是消息 Body 的具体内容。

常见：

```text
Date
Connection
```

---

# 41. Date Header

例如：

```http
Date: Wed, 16 Feb 2022 10:38:44 GMT
```

表示：

> HTTP Message 的生成时间。

通常使用：

```text
GMT / UTC
```

标准时间。

---

# 42. Connection Header

例如：

```http
Connection: close
```

或者：

```http
Connection: keep-alive
```

---

## close

```text
Connection: close
```

表示：

> 当前 Request/Response 完成后关闭连接。

---

## keep-alive

```text
Connection: keep-alive
```

表示：

> 保持连接，可以继续通过同一个连接传输更多 HTTP 数据。

这样可以避免每次请求都重新建立连接。

---

# 43. Entity Headers

中文：

```text
实体标头
```

Entity Headers 主要描述：

> HTTP Message Body 中传输的内容。

常见：

```text
Content-Type
Content-Length
Content-Encoding
Boundary
```

---

# 44. Content-Type

例如：

```http
Content-Type: text/html
```

表示：

> HTTP Body 中是什么类型的数据。

常见类型：

```text
text/html
application/json
application/pdf
image/png
image/jpeg
text/plain
```

---

例如：

```http
Content-Type: text/html; charset=UTF-8
```

其中：

```text
text/html
```

表示内容类型。

```text
charset=UTF-8
```

表示字符编码。

---

# 45. Media Type

Media Type 指：

> 正在传输的数据类型。

例如：

```text
text/html
application/json
application/pdf
image/png
```

严格来说，实际 HTTP 中通常是通过：

```http
Content-Type: application/pdf
```

表达 media type。

也就是说：

```text
application/pdf
```

是 Media Type。

`Media-Type` 一般不是最常见的独立 HTTP Header 名称。

---

# 46. MIME Type / Media Type

常见：

```text
text/html
```

HTML

```text
text/plain
```

纯文本

```text
application/json
```

JSON

```text
application/pdf
```

PDF

```text
image/png
```

PNG 图片

```text
image/jpeg
```

JPEG 图片

---

# 47. Boundary

Boundary 常见于：

```text
multipart/form-data
```

例如文件上传。

可能看到：

```http
Content-Type: multipart/form-data; boundary=----b4e4fbd93540
```

Boundary 用于：

> 把一个 HTTP Body 中的多个部分分隔开。

例如：

```text
------b4e4fbd93540
Content-Disposition: form-data; name="username"

admin
------b4e4fbd93540
Content-Disposition: form-data; name="file"; filename="test.txt"

...
------b4e4fbd93540--
```

文件上传漏洞中会经常遇到 Boundary。

---

# 48. Content-Length

例如：

```http
Content-Length: 385
```

表示：

> HTTP Body 的长度。

通常单位：

```text
Bytes
```

服务器可以根据 Content-Length 判断：

```text
Body 有多少数据需要读取
```

浏览器、cURL 等通常会自动计算。

---

# 49. Content-Encoding

例如：

```http
Content-Encoding: gzip
```

表示：

> HTTP Body 在传输之前经过了什么编码或压缩。

例如：

```text
gzip
```

表示内容经过 gzip 压缩。

作用：

```text
减少网络传输的数据大小
```

---

# 50. Request Headers

Request Headers：

> 客户端发送给服务器的 Header。

常见：

```text
Host
User-Agent
Referer
Accept
Cookie
Authorization
```

---

# 51. Host

例如：

```http
Host: www.inlanefreight.com
```

表示：

> 客户端想访问服务器上的哪个 Host。

这个 Header 非常重要。

因为：

> 同一个 IP / Web Server 可以托管多个网站。

例如：

```text
10.10.10.10
```

可能同时托管：

```text
website1.com
website2.com
admin.website.com
```

服务器通过：

```http
Host:
```

判断你要访问哪个站点。

---

# 52. Host 在渗透测试中的意义

Host Header 可能帮助发现：

```text
Virtual Hosts
```

例如：

```text
admin.example.com
dev.example.com
test.example.com
api.example.com
```

因此 Web Enumeration 中 Host 是非常重要的字段。

---

# 53. User-Agent

例如：

```http
User-Agent: curl/7.77.0
```

或者：

```http
User-Agent: Mozilla/5.0
```

作用：

> 告诉服务器客户端是什么软件。

可能包含：

- Browser
- Browser Version
- Operating System
- Device
- Tool

例如：

```text
curl/7.77.0
```

服务器知道请求来自：

```text
cURL
```

---

# 54. 修改 User-Agent

cURL：

```bash
curl -A "Mozilla/5.0" https://example.com
```

也可以：

```bash
curl --user-agent "Mozilla/5.0" https://example.com
```

---

# 55. Referer

注意拼写：

```text
Referer
```

HTTP 标准历史上就是这个拼写，而不是：

```text
Referrer
```

例如：

```http
Referer: https://google.com/
```

表示：

> 用户是从哪个页面进入当前页面的。

例如：

```text
Google
   ↓ click
example.com
```

请求可能包含：

```http
Referer: https://google.com/
```

---

# 56. Referer 的安全问题

Referer 可能泄露：

```text
URL
Path
Query Parameter
敏感信息
```

因此：

> 不能把 Referer 当作可信的身份认证机制。

因为它可以被修改或伪造。

---

# 57. Accept

例如：

```http
Accept: */*
```

表示：

> 客户端能够接受哪些类型的内容。

`*/*` 表示：

```text
任何类型都可以
```

浏览器可能发送：

```http
Accept: text/html,application/xhtml+xml,application/xml
```

意思是更偏好：

```text
HTML
XHTML
XML
```

等内容。

---

# 58. Cookie

例如：

```http
Cookie: PHPSESSID=b4e4fbd93540
```

Cookie 是存储在客户端的数据。

常用于：

```text
Session Management
Authentication
User Preferences
Tracking
```

例如登录成功以后：

```text
Server
   ↓
Set-Cookie: PHPSESSID=abc123
```

浏览器保存：

```text
PHPSESSID=abc123
```

下一次请求自动发送：

```http
Cookie: PHPSESSID=abc123
```

---

# 59. Cookie 格式

基本格式：

```text
name=value
```

例如：

```text
PHPSESSID=abc123
```

多个 Cookie 可以：

```http
Cookie: session=abc123; theme=dark; language=en
```

使用：

```text
;
```

分隔。

---

# 60. Authorization

例如：

```http
Authorization: Basic dXNlcjpwYXNzd29yZA==
```

用于：

> 向服务器发送身份认证信息。

常见认证方案可能包括：

```text
Basic
Bearer
Digest
```

例如：

```http
Authorization: Bearer eyJhbGciOi...
```

Authorization Header 在：

```text
API
Web Authentication
Pentesting
```

中非常重要。

---

# 61. Basic Authentication

例如：

```http
Authorization: Basic dXNlcjpwYXNzd29yZA==
```

Basic Authentication 通常将：

```text
username:password
```

进行：

```text
Base64
```

编码。

注意：

> Base64 不是加密。

所以 Basic Auth 应该配合：

```text
HTTPS
```

使用。

---

# 62. Response Headers

Response Headers：

> 服务器发送给客户端的 Headers。

常见：

```text
Server
Set-Cookie
WWW-Authenticate
Location
```

---

# 63. Server Header

例如：

```http
Server: Apache/2.2.14 (Win32)
```

作用：

> 告诉客户端服务器软件的信息。

可能泄露：

```text
Web Server Type
Web Server Version
Operating System
```

例如：

```text
Apache
2.2.14
Win32
```

---

# 64. Server Header 在渗透测试中的意义

例如：

```http
Server: Apache/2.2.14
```

可能帮助进行：

```text
Technology Enumeration
Version Enumeration
Vulnerability Research
```

但注意：

> Header 可以被修改，所以它只能作为线索，不能百分之百相信。

---

# 65. Set-Cookie

服务器返回：

```http
Set-Cookie: PHPSESSID=b4e4fbd93540
```

表示：

> 要求浏览器保存一个 Cookie。

之后浏览器请求可能发送：

```http
Cookie: PHPSESSID=b4e4fbd93540
```

所以：

```text
Server:
Set-Cookie

Client:
Cookie
```

记忆：

```text
Set-Cookie = Server → Client

Cookie     = Client → Server
```

---

# 66. WWW-Authenticate

例如：

```http
WWW-Authenticate: Basic realm="localhost"
```

表示：

> 当前资源要求身份认证，并告诉客户端使用什么认证方式。

例如：

```text
Basic
```

浏览器可能因此弹出用户名和密码输入框。

---

# 67. Security Headers

Security Headers：

> 服务器通过 Response Headers 告诉浏览器应该遵守哪些安全策略。

常见：

```text
Content-Security-Policy
Strict-Transport-Security
Referrer-Policy
```

---

# 68. Content-Security-Policy

简称：

```text
CSP
```

例如：

```http
Content-Security-Policy: script-src 'self'
```

意思：

> JavaScript 只能从当前网站加载。

`'self'` 表示：

```text
当前 Origin
```

主要用于降低：

```text
Cross-Site Scripting
XSS
```

等攻击的风险。

---

# 69. CSP 简单理解

假设网页被注入：

```html
<script src="https://evil.com/x.js"></script>
```

如果：

```http
Content-Security-Policy: script-src 'self'
```

浏览器可能拒绝加载：

```text
evil.com
```

因为它不属于：

```text
self
```

---

# 70. Strict-Transport-Security

简称：

```text
HSTS
```

例如：

```http
Strict-Transport-Security: max-age=31536000
```

作用：

> 强制浏览器以后使用 HTTPS 访问网站。

可以降低：

```text
HTTP Downgrade
SSL Stripping
中间人攻击
```

等风险。

---

# 71. HSTS max-age

例如：

```text
max-age=31536000
```

表示：

```text
31,536,000 秒
```

约等于：

```text
1 年
```

在这段时间内浏览器记住：

```text
这个网站必须使用 HTTPS
```

---

# 72. Referrer-Policy

注意：

Header 名称这里是：

```text
Referrer-Policy
```

例如：

```http
Referrer-Policy: origin
```

作用：

> 控制浏览器在 Referer Header 中发送多少来源信息。

可以减少：

```text
Sensitive URL Leakage
Query String Leakage
Path Leakage
```

---

# 73. Headers 总结表

| 类型 | 常见 Header |
|---|---|
| General Headers | `Date`, `Connection` |
| Entity Headers | `Content-Type`, `Content-Length`, `Content-Encoding` |
| Request Headers | `Host`, `User-Agent`, `Referer`, `Accept`, `Cookie`, `Authorization` |
| Response Headers | `Server`, `Set-Cookie`, `WWW-Authenticate` |
| Security Headers | `Content-Security-Policy`, `Strict-Transport-Security`, `Referrer-Policy` |

---

# 74. cURL

## cURL 是什么？

cURL 是一个命令行 HTTP Client。

可以使用：

```bash
curl https://example.com
```

向网站发送 HTTP Request。

在 Web Security 中非常重要，因为它可以：

- 手动发送 HTTP Request
- 修改 Headers
- 修改 User-Agent
- 添加 Cookie
- 添加 Authorization
- 发送 POST Data
- 查看 Response Headers
- 查看完整 HTTP 通信
- 下载文件

---

# 75. 查看 cURL 帮助

```bash
curl -h
```

或者：

```bash
curl --help
```

可以查看常见参数。

---

# 76. curl -d / --data

```bash
curl -d
```

完整：

```bash
curl --data
```

用于：

> 发送数据，通常会产生 POST Request。

例如：

```bash
curl -d "username=admin&password=test" https://example.com/login
```

---

# 77. curl -h / --help

```bash
curl -h
```

或者：

```bash
curl --help
```

查看帮助。

还可以：

```bash
curl --help all
```

查看全部选项。

或者：

```bash
man curl
```

查看完整手册。

---

# 78. curl -i / --include

```bash
curl -i https://example.com
```

作用：

> 在正常 Response Body 前同时显示 Response Headers。

例如：

```text
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 123

<html>
...
</html>
```

也就是：

```text
Headers + Body
```

---

# 79. curl -I

注意：

```text
-i
```

和：

```text
-I
```

完全不同。

Linux 参数区分大小写。

```bash
curl -I https://example.com
```

大写 `I` 通常发送：

```text
HEAD Request
```

然后主要查看：

```text
Response Headers
```

例如：

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1234
```

通常没有 Response Body。

---

# 80. -i 和 -I 区别

```text
-i
```

小写：

```text
include headers in output
```

正常发送 GET 等请求：

```text
Headers + Body
```

---

```text
-I
```

大写：

```text
HEAD Request
```

主要获取：

```text
Headers
```

---

记忆：

```text
-i = Include

-I = HEAD
```

---

# 81. curl -o / --output

例如：

```bash
curl -o page.html https://example.com
```

作用：

> 将返回内容保存到指定文件。

例如：

```bash
curl -o test.pdf https://example.com/file.pdf
```

保存为：

```text
test.pdf
```

---

# 82. curl -O / --remote-name

例如：

```bash
curl -O https://example.com/files/report.pdf
```

作用：

> 使用远程文件原本的文件名保存。

服务器文件：

```text
report.pdf
```

保存后：

```text
report.pdf
```

---

# 83. -o 和 -O 区别

小写：

```bash
-o
```

自己指定文件名：

```bash
curl -o myfile.pdf URL
```

---

大写：

```bash
-O
```

使用远程文件名：

```bash
curl -O URL/report.pdf
```

---

# 84. curl -s / --silent

```bash
curl -s https://example.com
```

作用：

> Silent Mode。

隐藏：

```text
Progress Meter
```

等额外信息。

在脚本中特别常用。

例如：

```bash
curl -s https://example.com
```

更适合：

```text
grep
awk
sed
```

等命令继续处理。

---

# 85. curl -u / --user

例如：

```bash
curl -u admin:password https://example.com
```

作用：

> 发送用户名和密码进行身份认证。

通常会生成类似：

```http
Authorization: Basic ...
```

的 Header。

---

# 86. curl -A / --user-agent

例如：

```bash
curl -A "Mozilla/5.0" https://example.com
```

作用：

> 修改 User-Agent。

默认 cURL 可能是：

```http
User-Agent: curl/8.x
```

修改后：

```http
User-Agent: Mozilla/5.0
```

---

# 87. curl -v / --verbose

```bash
curl -v https://example.com
```

作用：

> 显示更加详细的 HTTP 通信信息。

可以看到：

- Connection
- Request Headers
- Response Headers
- TLS 信息
- Redirect / 网络细节

例如：

```text
> GET / HTTP/1.1
> Host: example.com
> User-Agent: curl/8.x
>
< HTTP/1.1 200 OK
< Content-Type: text/html
<
<html>
...
```

通常：

```text
>
```

表示：

```text
cURL → Server
```

而：

```text
<
```

表示：

```text
Server → cURL
```

---

# 88. cURL 常用参数总结

| 参数 | 完整形式 | 作用 |
|---|---|---|
| `-d` | `--data` | 发送 POST 数据 |
| `-h` | `--help` | 查看帮助 |
| `-i` | `--include` | 显示 Response Headers + Body |
| `-I` | `--head` | 发送 HEAD Request |
| `-o` | `--output` | 输出到指定文件 |
| `-O` | `--remote-name` | 使用远程文件名保存 |
| `-s` | `--silent` | 静默模式 |
| `-u` | `--user` | 用户名/密码认证 |
| `-A` | `--user-agent` | 修改 User-Agent |
| `-v` | `--verbose` | 显示详细 HTTP 通信 |

---

# 89. cURL 三个特别容易混的参数

## `-v`

```bash
curl -v URL
```

用途：

```text
查看完整、详细的 HTTP 通信过程
```

包括：

```text
Request Headers
Response Headers
Connection Details
Body
```

---

## `-i`

```bash
curl -i URL
```

用途：

```text
Response Headers + Response Body
```

---

## `-I`

```bash
curl -I URL
```

用途：

```text
HEAD Request
```

主要得到：

```text
Response Headers
```

---

记忆：

```text
-v = Verbose

-i = Include Header

-I = HEAD
```

---

# 90. Browser 和 cURL 的关系

Browser：

```text
自动帮助你发送 HTTP Request
自动处理 Response
自动渲染 HTML
```

cURL：

```text
让你直接看到和控制 HTTP 通信
```

例如浏览器访问：

```text
https://example.com
```

本质上也是：

```text
HTTP Request
      ↓
HTTP Response
```

cURL 只是让这个过程：

```text
更透明
更容易手动修改
```

---

# 91. 为什么 Web Pentesting 必须理解 HTTP

Web Pentesting 本质上大量操作：

```text
HTTP Request
HTTP Response
```

例如：

```text
修改 URL
修改 Query Parameter
修改 Cookie
修改 Header
修改 Host
修改 User-Agent
修改 Authorization
修改 Request Body
```

然后观察服务器：

```text
Response
```

是否发生变化。

---

# 92. Web Pentesting 的核心思维

正常请求：

```http
GET /dashboard HTTP/1.1
Host: example.com
Cookie: role=user
```

攻击者会思考：

```text
如果我修改 Cookie 呢？
```

例如：

```http
Cookie: role=admin
```

或者：

```text
如果我修改 Path 呢？
```

```http
GET /admin HTTP/1.1
```

或者：

```text
如果修改参数呢？
```

```text
?id=100
```

变成：

```text
?id=101
```

Web Security 的很多漏洞，本质上都是：

> 客户端可以修改 HTTP Request，而服务器没有正确验证。

---

# 93. HTTP Request 需要重点观察的位置

以后使用：

```text
Burp Suite
DevTools
cURL
```

看到 HTTP Request 时，优先观察：

```text
1. Method
2. Path
3. Query String
4. Host
5. Cookie
6. Authorization
7. Content-Type
8. Request Body
9. User-Agent
10. Referer
```

---

# 94. HTTP Response 需要重点观察的位置

重点：

```text
1. Status Code
2. Server
3. Set-Cookie
4. Location
5. Content-Type
6. Content-Length
7. Security Headers
8. Response Body
```

---

# 95. Request vs Response

## Request

客户端发送：

```http
GET /dashboard HTTP/1.1
Host: example.com
Cookie: PHPSESSID=abc123
```

方向：

```text
Client → Server
```

---

## Response

服务器返回：

```http
HTTP/1.1 200 OK
Content-Type: text/html
Server: Apache

<html>
...
</html>
```

方向：

```text
Server → Client
```

---

# 96. Cookie vs Set-Cookie

这一组必须记住。

服务器：

```http
Set-Cookie: PHPSESSID=abc123
```

意思：

```text
服务器让浏览器保存 Cookie
```

之后客户端：

```http
Cookie: PHPSESSID=abc123
```

意思：

```text
客户端把 Cookie 发回服务器
```

所以：

```text
Set-Cookie
Server → Client

Cookie
Client → Server
```

---

# 97. Content-Type vs Accept

这一组也容易混。

## Content-Type

表示：

> 我现在发送的数据是什么类型。

例如：

```http
Content-Type: application/json
```

意思：

```text
我发送的是 JSON。
```

---

## Accept

表示：

> 我希望你返回什么类型的数据。

例如：

```http
Accept: application/json
```

意思：

```text
我希望你返回 JSON。
```

所以：

```text
Content-Type = 我发的是什么

Accept = 我想收什么
```

---

# 98. Host vs DNS

这两个也不要混。

## DNS

负责：

```text
Domain → IP
```

例如：

```text
example.com
     ↓
93.184.216.34
```

---

## Host Header

HTTP Request：

```http
Host: example.com
```

负责告诉 Web Server：

```text
我要访问你托管的哪个网站
```

所以：

```text
DNS
Domain → IP

Host Header
告诉服务器访问哪个 Virtual Host
```

---

# 99. 一张图理解整个 HTTP 过程

```text
User
 |
 | 输入
 v
https://example.com/users?id=5
 |
 | DNS
 v
example.com
 |
 v
93.184.216.34
 |
 | TCP/TLS Connection
 v
Web Server
 ^
 |
 | HTTP Request
 |
GET /users?id=5 HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Cookie: session=abc123
Accept: text/html

 |
 | Server Processing
 v

HTTP/1.1 200 OK
Server: nginx
Content-Type: text/html
Set-Cookie: session=abc123
Content-Length: 1234

<html>
...
</html>

 |
 v
Browser
 |
 v
Render Page
```

---

# 100. 本章必须真正理解的核心知识

不要只背名词，真正需要掌握的是：

```text
HTTP = Request + Response
```

Request：

```text
Method
Path
Version
Headers
Body
```

Response：

```text
Version
Status Code
Headers
Body
```

---

URL：

```text
scheme://user:pass@host:port/path?query#fragment
```

---

浏览网页的过程：

```text
URL
 ↓
DNS
 ↓
IP
 ↓
Connect
 ↓
HTTP Request
 ↓
HTTP Response
 ↓
Browser Render
```

---

常见 Request Headers：

```text
Host
User-Agent
Referer
Accept
Cookie
Authorization
```

---

常见 Response Headers：

```text
Server
Set-Cookie
WWW-Authenticate
```

---

常见 Entity Headers：

```text
Content-Type
Content-Length
Content-Encoding
```

---

常见 Security Headers：

```text
Content-Security-Policy
Strict-Transport-Security
Referrer-Policy
```

---

cURL 最重要：

```bash
curl URL
curl -v URL
curl -i URL
curl -I URL
curl -A "..." URL
curl -u user:password URL
curl -d "key=value" URL
curl -o file URL
curl -O URL
curl -s URL
```

---

# 101. 最终速记表

```text
HTTP
= Client 和 Server 通信的应用层协议


HTTP
Default Port = 80

HTTPS
Default Port = 443


DNS
Domain → IP


URL

scheme://user:password@host:port/path?query#fragment


HTTP Request

Method Path Version
Headers

Body


Example:

GET /login HTTP/1.1
Host: example.com
Cookie: session=abc


HTTP Response

Version Status Code
Headers

Body


Example:

HTTP/1.1 200 OK
Content-Type: text/html
Server: nginx

<html>...</html>


Request Header:

Host
User-Agent
Referer
Accept
Cookie
Authorization


Response Header:

Server
Set-Cookie
WWW-Authenticate


Security Header:

Content-Security-Policy
Strict-Transport-Security
Referrer-Policy


Cookie:

Set-Cookie
Server → Client

Cookie
Client → Server


Content-Type
= 我发送的内容是什么类型


Accept
= 我希望收到什么类型


curl -v
= Verbose，详细过程

curl -i
= Response Headers + Body

curl -I
= HEAD Request / Headers

curl -d
= POST Data

curl -u
= Username + Password

curl -A
= User-Agent

curl -o
= 指定保存文件名

curl -O
= 使用远程文件名

curl -s
= Silent
```

---

# 102. 从网络安全角度最重要的一句话

Web Security 学习的核心之一就是：

```text
Browser
   ↓
HTTP Request
   ↓
Web Application
   ↓
HTTP Response
```

而 Pentester 做的事情，大量都是：

```text
Intercept Request
       ↓
Inspect Request
       ↓
Modify Request
       ↓
Send Request
       ↓
Analyze Response
```

所以后面学习：

```text
Burp Suite
Authentication
SQL Injection
XSS
File Upload
Command Injection
IDOR
CSRF
SSRF
API Attacks
```

都会不断回到：

```text
HTTP Request / Response
```

这也是为什么 HTTP Fundamentals 是 Web Penetration Testing 最基础的知识。

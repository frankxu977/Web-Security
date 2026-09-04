# Unprotected functionality

## 1 Easiest way:

 /robots.txt : 搜索引擎机器人，请不要索引 /admin

## Example

<img width="2545" height="1302" alt="image" src="https://github.com/user-attachments/assets/2ee8d5de-b93d-4d84-9cd1-fe71b34e7192" />

Find name of the admin

<img width="910" height="252" alt="image" src="https://github.com/user-attachments/assets/e57cad4c-bcee-4cee-a0bc-86dbe659794f" />

Go to the admin panel

<img width="2538" height="561" alt="image" src="https://github.com/user-attachments/assets/89104d61-c221-4882-b7ce-7c7790660efe" />

## Hard to guess

就算管理员 URL 很难猜，只要它被放进前端 JavaScript 里，普通用户仍然能找到 

Security through obscurity

```javascript
var isAdmin = false;

if (isAdmin) {
    var adminPanelTag = document.createElement('a');
    adminPanelTag.setAttribute(
        'href',
        'https://insecure-website.com/administrator-panel-yb556'
    );
    adminPanelTag.innerText = 'Admin panel';
}
```

## Example

Reading javascript

<img width="2557" height="1367" alt="image" src="https://github.com/user-attachments/assets/3feba9b9-6b64-4548-b8dc-03b2b4a63885" />

Finf URL

<img width="1303" height="708" alt="image" src="https://github.com/user-attachments/assets/909c916b-497a-4a96-8499-ce6d95a10897" />

Delete 
<img width="1218" height="740" alt="image" src="https://github.com/user-attachments/assets/6bfb2800-2e17-4706-8fd1-a4d41c4428df" />

## 2 Parameter-based access control methods

```python
if admin == true:
    allow admin functions
```
admin=true 和 role=1 都在 URL 里，用户自己可以改

```text
正常登录
   ↓
服务器给用户一个角色值
   ↓
role=user
   ↓
这个值存到了用户可修改的位置
   ↓
攻击者修改：
role=admin
   ↓
服务器错误地相信这个值
   ↓
获得管理员权限
```

## Example

<img width="2152" height="807" alt="image" src="https://github.com/user-attachments/assets/dd84f146-d71e-4d19-8441-2be938c05b95" />

change cookies

<img width="1242" height="572" alt="image" src="https://github.com/user-attachments/assets/065fd237-9277-421f-b1f5-90cf3925d6be" />

<img width="1241" height="600" alt="image" src="https://github.com/user-attachments/assets/abf50b46-1b2d-4937-9d0b-caeabead37e0" />


## 3 Horizontal privilege escalation

水平权限提升 = 你还是普通用户，但你能访问另一个普通用户的数据或功能。

IDOR = Insecure Direct Object Reference

## Example

<img width="1965" height="855" alt="image" src="https://github.com/user-attachments/assets/9464e424-7f2a-4d09-9232-5fd59fe87514" />

<img width="1165" height="490" alt="image" src="https://github.com/user-attachments/assets/4c16d8cc-0a54-4a0c-9da0-36f5890aa00b" />


## 4 Horizontal to Vertical Privilege Escalation

A horizontal privilege escalation vulnerability can become vertical privilege escalation if the compromised user has higher privileges.


## Example

<img width="1247" height="1175" alt="image" src="https://github.com/user-attachments/assets/5927f2cb-546e-47b9-b1cb-c58b69f0b469" />

<img width="1237" height="455" alt="image" src="https://github.com/user-attachments/assets/604ad9e4-b35d-49dc-b8ff-0621f046ce94" />

<img width="1242" height="561" alt="image" src="https://github.com/user-attachments/assets/9ecb8e37-27eb-4681-9f7d-875ec7a0a1fa" />



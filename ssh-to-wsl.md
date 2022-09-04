# 如何使用 SSH 連線到你的 WSL

開始使用 WSL 後，我就都在 WSL 裡面寫 Code，而使用 VSCode 的 Live Server 的時候，Go Live 預設會自動開啟瀏覽器前往 `127.0.0.1:5500`，但是 server 是 run 在 WSL 的 `127.0.0.1:5500`，瀏覽器打開的是 Windows 本地的 `127.0.0.1:5500`，所以沒辦法開啟預期的網頁。

我想起來 VSCode 有 SSH 套件可以開啟遠端的資料夾，而且會幫你做 port forwarding，於是開始研究要怎麼手動做這件事，看到這篇 [stack overflow](https://stackoverflow.com/questions/63701361/how-does-vscode-remote-development-forward-port-work)，解答表示其實 VSCode 就是開啟一個 SSH Tunnel 而已，開啟的方法：

```bash
ssh -L <local-port>:<remote-machine-ip>:<remote-port> <remote-machine-ip>

# example
ssh -L 5500:172.xx.xxx.xx:5555 <remote-username>@172.xx.xxx.xx
```

但是在這之前，要先開啟 WSL 的 port 22，不然會得到以下訊息：

```bash
ssh: connect to host 172.20.182.87 port 22: Connection refused
```

修改 WSL 的 `/etc/ssh/sshd_config`，做以下修改：

```bash
# uncomment this two lines
Port 22
ListenAddress 0.0.0.0

# change this line from no to yes
PasswordAuthentication yes
```

再來要啟動 SSH service

```bash
sudo service ssh start

# if ssh already started
sudo service ssh restart

# if you want to stop the service
sudo service ssh stop
```

這樣子就可以把 SSH Tunnel 開啟了。

但是在進入 WSL 之後會發現出現這樣的訊息：

```bash
bind [127.0.0.1]:5500: Permission denied
channel_setup_fwd_listener_tcpip: cannot listen to port: 5500
Could not request local forwarding.
```

不知道是什麼問題，但是這個時候我突然靈光一閃，工作的時候不一定會有 VSCode 幫你做好 port forwarding，所以都是直接連到 remote ip 就可以開啟網頁了，所以搞不好可以直接用 WSL 的 IP 來連過去阿！

結果繞了一圈根本直接連 IP 就好了，我真傻。

這就是為什麼這篇的標題是＂如何使用 SSH 連線到你的 WSL＂，但是內文卻一直在講 SSH Tunnel。

## 參考來源

1. [How does VSCode [Remote Development] [Forward Port] work?](https://stackoverflow.com/questions/63701361/how-does-vscode-remote-development-forward-port-work)
2. [How to SSH into WSL2 on Windows 10 from an external machine](https://www.hanselman.com/blog/how-to-ssh-into-wsl2-on-windows-10-from-an-external-machine)  
（這一篇後面還有說要在你的 Windows 執行一些指令來 forward ports into WSL，然後打開防火牆，但我試了之後還是失敗，反而不用做也可以，可能是如果要把預設的 ssh port 22 改掉才需要做那些指令吧！）

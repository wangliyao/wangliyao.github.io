### shaodowsock配置很多，网上都能搜到，主要是kcptun加速shadowsocks
* vps安装kcptun一键安装脚本[一键安装脚本教程](https://blog.kuoruan.com/110.html)
* 因为mac上的kcptun问题很多，所以建议用命令行启动
* mac安装kcptun[github地址](https://github.com/xtaci/kcptun)
* 下载下来之后，启动 client_darwin_amd64 这个包
* 客户端./client_darwin_amd64 -r "123.321.123.123:4001" -l ":1070" -mode fast2启动命令，这里说明一下":1070"这个端口是本地端口。可以随意设置
最好还是写成一个.json文件进行启动，具体的文件内容如下
``` json
# 在客户端保存为client.json
{
    "localaddr": ":1070",
    "remoteaddr": "123.321.123.123:4001",
    "key": "tester",
    "crypt": "aes-192",
    "mode": "fast2"
}
```
脚本里的信息在第一条一键安装教程当中有提示到
* 到现在可以启动了，mac 本地shadowsocks 需要改成127.0.0.0，端口为你本地启动设置的端口比如1070
* mac下将其设置为开机自启动，本地写一个.sh的脚本，然后系统偏好>用户与群组>登录项添加脚本，最好写成绝对路径，然后给脚本还有 client_darwin_amd64这个
包加chmod +x 文件名添加可执行权限
* 然后重启电脑，脚本就会自动后台运行了


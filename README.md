- 以google cloud 的 CentOS7 为例，前提你有VPS和有个域名绑定好IP
### 一、安装git、docker、docker-compose
- 不会安装，请查看master项目有说明
### 二、拉取项目
```
    git clone -b nginx https://github.com/0758jian/docker-compose-v2ray.git v2ray
```
### 三、先启用acme下载证书 [acme使用说明](https://hub.docker.com/r/neilpang/acme.sh)
```
    cd v2ray
    sudo docker-compose up -d acme
```
- 手动DNS添加获取证书，yourdomain是你的域名
```
    sudo docker-compose exec acme --issue -d yourdomain --dns --yes-I-know-dns-manual-mode-enough-go-ahead-please
```
- 会弹出Domain(_acme-challenge.yourdomain) _acme-challenge是指TXT记录的name，TXT value值是指TXT记录的content，复制去域名服务商添加txt记录
- 验证txt是否生效 成功会返回内容值看是否对应
```
    sudo yum install -y bind-utils
    dig -t txt _acme-challenge.yourdomain
```
- 重点要等上面txt解析成功后，重申域名证书，yourdomain是你的域名
```
    sudo docker-compose exec acme --renew -d yourdomain --yes-I-know-dns-manual-mode-enough-go-ahead-please
```
### 四、修改nginx里的conf证书路径，里面的yourdomain是你的域名，并重启服务
```
    sudo docker-compose down
    sudo docker-compose up -d
```
### 注意事项
- 服务器只开通80 443 22(SSH建议改为其他端口登陆)
- 查看容器日志 docker-compose logs v2ray
- 重启某个容器 docker-compose restart v2ray

- TG技术群：https://t.me/joinchat/LqcgBEUJ7133BFBEv67NCw
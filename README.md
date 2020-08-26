- 以google cloud 的 CentOS7 为例，前提你有VPS和有个域名绑定好IP
### 一、确认时间,不对就复制上海区
```
    date -R
    sudo cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime    
```
### 二、添加git、docker仓库
```
    sudo yum -y install yum-utils device-mapper-persistent-data lvm2
    sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    sudo vi /etc/yum.repos.d/wandisco-git.repo
```    
- git仓内容如下：
```
    [WndiscoGIT]
    name=Wandisco Git Repo
    baseurl=http://opensource.wandisco.com/centos/7/git/$basearch/
    enabled=1
    gpgcheck=0
```
### 三、安装git、docker 设置开机启动docker
```
    sudo yum makecache
    sudo yum -y install git
    sudo yum -y install docker-ce
    sudo systemctl enable docker
    sudo systemctl start docker
```
### 四、安装docker-compose 官方安装方式
```
    sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```
- 验证是否安装成功,弹出版本信息即表示OK
```
    git --version
    sudo docker -v
    sudo docker-compose version
```
### 五、启动项目,websocket和http/2的配置请详看.example里的内容
```
    git clone https://github.com/0758jian/docker-compose-v2ray.git v2ray
    cd v2ray
    sudo docker-compose up -d
    sudo docker-compose ps
```
- BBR加速,看自己喜欢选择，我选择的是1、5、10
```
    sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    sudo yum -y install wget
    sudo wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh"
    sudo chmod +x tcp.sh
    sudo ./tcp.sh
```

### 注意事项
- 服务器只开通80 443 22(SSH建议改为其他端口登陆)
- 用speedtest官方容器来做个测整站
- caddy和v2ray的配置参数，请修改为不同不要用example的
- 查看容器日志 docker-compose logs caddy
- 重启某个容器 docker-compose restart caddy
- 使用v2ray的h2协议，要进入v2ray容器把证书link过来,注意你的域名
```
  ln -s /data/caddy/certificates/acme-v02.api.letsencrypt.org-directory/yourdomain/yourdomain.crt /etc/v2ray/yourdomain.crt
  ln -s /data/caddy/certificates/acme-v02.api.letsencrypt.org-directory/yourdomain/yourdomain.key /etc/v2ray/yourdomain.key
```

- TG技术群：https://t.me/ousiqi
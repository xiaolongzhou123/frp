### 使用说明 

>最好在linux上运行。而且网络模式直接用host，因为这样能获取客户端的ip地址，不然只会获取docker容器的ip地址



1. frps目录在远程服务器使用
2. frpc目录在客户端本地使用


启动。都是docker-compose up -d


### 服务端配置说明

```
[common]
bind_addr = 0.0.0.0 
bind_port = 17000 

vhost_http_port = 17500 
vhost_https_port = 1443 

dashboard_addr = 0.0.0.0 
dashboard_port = 17999 
dashboard_user = admin 
dashboard_pwd = Xxxxx.ai@123 

token = 18510005758 
tcp_mux = true 
max_pool_count = 100 
max_ports_per_client = 0 
log_file = ./frps.log 
log_level = info 
log_max_days = 3 

```
1.这里的token其实是密码，客户端过来连接，就是拿这个token做为密码连接 
2.记住这个bind_port 这个就是对外的端口（提前你做好Nat映射），比方，我外网ip:221.222.122.3,那么我可以221.222.122.3:17000<===>内网：192.168.1.2:17000 


### 客户端配置说明

```
[common] 
server_addr = 221.222.122.3 
server_port = 17000 
token = 18510005758 

[windows-work]   #必须改个名字,随便怎么改,和其它客户端不一样就行
type = tcp 
local_ip = 0.0.0.0 
local_port = 8000 #这个8000才是你服务的实际端口 
remote_port = 7113 #这个端口虽然是你定义的，但是会开放在服务器端 

```
这里的common不要修改,windows-work这里需要修改，客户端唯一标识 
1. windows-work改成myhome，yourhome(随便) 
2. 3389是你本地的要开放的端口,7113这个端口会开放在server端 



>测试: 
1. 你在客户端，随便运行一个http服务如python -m SimpleHTTPServer 8000或 python3 -m http.server 
2. 访问 curl :221.222.122.3:7113,就会转发到客户端frpc的8000 



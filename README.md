caddy一键脚本安装  
caddy官网 ：https://caddyserver.com/  
手动下载： https://caddyserver.com/download  
Github：https://github.com/mholt/caddy  
官方脚本安装  
curl https://getcaddy.com | bash -s personal  
或者  
wget -qO- https://getcaddy.com | bash -s personal  
若需安装插件  
curl https://getcaddy.com | bash -s personal http.git,dns  
2. 配置caddy  
创建配置文件放到 /etc/caddy 目录  
sudo mkdir /etc/caddy  
sudo touch /etc/caddy/Caddyfile  
sudo chown -R root:www-data /etc/caddy  
配置ssl证书目录  
sudo mkdir /etc/ssl/caddy  
sudo chown -R www-data:root /etc/ssl/caddy  
sudo chmod 0770 /etc/ssl/caddy  
配置网站目录  
sudo mkdir /var/www  
sudo chown www-data:www-data /var/www  
配置 systemd  
sudo curl -s  https://raw.githubusercontent.com/mholt/caddy/master/dist/init/linux-systemd/caddy.service  -o    /etc/systemd/system/caddy.service  
sudo systemctl daemon-reload  
sudo systemctl enable caddy.service  
sudo systemctl status caddy.service  
配置Caddfile配置文件  
修改Caddfile文件  
vi /etc/caddy/Caddyfile  
一个简单的websocket加静态网站配置  
www.google.com  
{  
  log /var/log/caddy/access.log  
  tls google@gmail.com  
  proxy /caressr 127.0.0.1:10000 {  
    websocket  
    header_upstream -Origin  
  }  
  root /var/www/  
}  
给log路径赋权  
sudo chown www-data:www-data /var/log/caddy  
上例是一个简单的websocket加静态网站配置。第一行为自己的域名，tls后面加上邮箱会自动申请let’sencrypt ssl证书。Caddfile更多配置详见官网。  
3. 通过systemd管理caddy  
sudo systemctl start caddy.service  
sudo systemctl stop caddy.service  
sudo systemctl restart caddy.service  
sudo systemctl reload caddy.service  

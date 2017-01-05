# MEAN项目
所谓的MEAN即是指技术栈为 MongoDB + Express + Angular + Nginx 的项目

# SSL证书选择
在此篇文章中选用 [Let's encrypt](https://letsencrypt.org/)
可以使用官方提供的工具 [certbot](https://certbot.eff.org/) 部署证书

# 生成SSL证书

## 安装 certbot nginx
```Bash
pacman -S certbot certbot-nginx nginx
```
由于使用了 Nginx，在此使用 certbot-nginx 进行部署

## 使用 certbot
如果没有使用Nginx，或者进行测试可以参考此步。
```Bash
certbot certonly
```
选用 Standalone，填入域名信息，OK生成。
生成的证书在 /etc/letsencrypt/live/your.domain.com下
```javascript
var sslPath = '/etc/letsencrypt/live/your.domain.com';
var ssl = { 
  key: fs.readFileSync(sslPath + '/privkey.pem'), 
  cert: fs.readFileSync(sslPath + '/fullchain.pem'),
  ca: fs.readFileSync(sslPath + '/chain.pem')
}

http.createServer(app).listen(process.env.PORT || 8000);
https.createServer(ssl, app).listen(process.env.PORT || 8443);
```

## 使用 certbot-nginx
1. 配置Nginx
```Bash
cd /etc/nginx
vi nginx.conf
```
```
server {
  listen        80;
  server_name   your.domain.com;
  
  location / {
      root html;
      index index.html index.htm;
  }   
}
```
:wq保存退出

2. certbot-nginx 生成证书
```
certbot --nginx
```
工具会自动读取nginx的配置，按照提示进行下一步即可，很简单。
完成后会同时对nginx的配置进行添加，工具添加的配置条目都会有相应注释的后缀，可以自行查看。
有兴趣可以用 https://ssllabs.com/ssltest/analyze.html?d=your.domain.com 进行SSL的测试
　
## 重启 nginx
```Bash
systemctl reload nginx.service
```
在浏览器中访问域名能看到绿色小锁就大功告成了。

![HTTPS安全认证](https://github.com/kimochg/TinyThoughts/blob/master/Security/image/4038272-0d1f23af72a63766.png)

# SSL 证书定时更新
由于免费的 SSL 证书的有效期为 90 天，所以在过期时需要用工具刷新证书，方法如下：
```Bash
certbot renew
```
这是最简单的刷新方法，会自动更新有效期小于 30 天的证书

## 借助 crontab 
如果使用 crontab 进行刷新，官方建议间隔为 2 天，同时如果只想记录错误日志，可以使用 -q 或者 --quiet
```Bash
certbot renew --quiet
```

## 钩子方法
使用钩子方法，可以帮助在刷新证书时起停服务。
```Bash
certbot renew --pre-hook "service nginx stop" --post-hook "service nginx start"
```


# 参考链接
[Let's encrypt Express](https://lucaschmid.net/anotherblog/letsencrypt-express)
[Configuring Nginx and SSL with Node.js](https://www.sitepoint.com/configuring-nginx-ssl-node-js/)
[Certbot User Guide](http://letsencrypt.readthedocs.io/en/latest/using.html#nginx)
[Deploying NodeJS using Express with NginX and Let's Encrypt](https://aghassi.github.io/NodeJS-Express-NginX-Setup/)
[Express behind proxies](http://expressjs.com/en/guide/behind-proxies.html)
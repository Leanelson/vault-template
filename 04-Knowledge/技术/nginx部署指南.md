---
type: knowledge
title: Nginx 部署指南
created: 2026-04-01
tags: [📚-knowledge, 🔧-tech]
source: stepclaw-workspace
---

# Nginx部署指南

## 服务器信息
- IP: 192.144.234.183
- 网站端口: 9500
- 配置文件: nginx-zhongliheng.conf

## 部署步骤

### 1. 登录服务器
```bash
ssh root@192.144.234.183
```

### 2. 安装Nginx（如果未安装）
```bash
# Ubuntu/Debian
apt-get update
apt-get install -y nginx

# CentOS/RHEL（备用）
# yum install -y nginx
```

### 3. 上传配置文件
将 `nginx-zhongliheng.conf` 上传到服务器：
```bash
scp nginx-zhongliheng.conf root@192.144.234.183:/etc/nginx/conf.d/
```

或在服务器上直接创建：
```bash
cat > /etc/nginx/conf.d/zhongliheng.conf << 'EOF'
[粘贴配置文件内容]
EOF
```

### 4. 修改配置中的域名
编辑配置文件，将 `zhongliheng.com` 替换为您的实际域名：
```bash
vi /etc/nginx/conf.d/zhongliheng.conf
```

### 5. 检查Nginx配置
```bash
nginx -t
```

### 6. 启动/重启Nginx
```bash
# Ubuntu/Debian
systemctl start nginx
systemctl enable nginx
systemctl restart nginx

# 查看状态
systemctl status nginx
```

### 7. 开放防火墙端口
```bash
# Ubuntu/Debian 使用 ufw
ufw allow 80/tcp
ufw allow 443/tcp
ufw reload

# 或使用 iptables
iptables -I INPUT -p tcp --dport 80 -j ACCEPT
iptables -I INPUT -p tcp --dport 443 -j ACCEPT

# CentOS/RHEL 使用 firewall-cmd
# firewall-cmd --permanent --add-port=80/tcp
# firewall-cmd --reload
```

### 8. 测试访问
```bash
curl http://127.0.0.1/
```

## 配置SSL证书（HTTPS）

### 方式1：使用Certbot免费证书（Ubuntu）
```bash
# 安装Certbot
apt-get install -y certbot python3-certbot-nginx

# 申请证书
certbot --nginx -d zhongliheng.com -d www.zhongliheng.com

# 自动续期
echo "0 0 1 * * certbot renew --nginx" | crontab -
```

### 方式2：手动配置
1. 申请SSL证书（阿里云、腾讯云等）
2. 上传证书到服务器
3. 修改nginx配置中的证书路径
4. 重启Nginx

## 常见问题

### 1. 502 Bad Gateway
- 检查frp是否运行正常
- 检查9500端口是否可访问：`curl http://127.0.0.1:9500/`

### 2. 域名解析失败
- 检查域名A记录是否指向 192.144.234.183
- 等待DNS生效（通常10分钟-24小时）

### 3. 备案问题
- 确保域名已完成备案
- 备案信息中添加了该域名
- 服务器提供商处也添加了域名

## 检查命令

```bash
# 查看Nginx状态
systemctl status nginx

# 查看Nginx错误日志
tail -f /var/log/nginx/error.log

# 查看网站访问日志
tail -f /var/log/nginx/zhongliheng.access.log

# 检查端口监听
netstat -tlnp | grep :80

# 测试本地访问
curl -I http://127.0.0.1/
```

## 您的域名是？

请告诉我您的域名，我可以更新配置文件中的域名。

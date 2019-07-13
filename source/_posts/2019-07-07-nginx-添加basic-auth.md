title: nginx 添加Basic Auth
layout: post
data: 2019-07-07
tags: ["nginx"]
---



安装 密码生成工具

```
sudo apt install apache-utils
```

添加 用户和用htpasswd加密后的密码

```
$ htpasswd -c .htpasswd myuser
```

在nginx里添加

```
  server / {
      auth_basic "Please input the basic auth";
      auth_basic_user_file "/etc/nginx/auth/.htpasswd";
  }
```

over.😄


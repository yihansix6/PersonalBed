[toc]
## 说明

我使用的设备情况如下：

- 硬件：DS918+
- 系统：DSM 7.0.1-42218



## 必要条件

- 开启DSM的SSH访问功能（通常在**控制面板-终端机和SNMP-启用SSH功能）
- 拥有管理员权限的账户（不一定是root，只要有管理员权限即可，这样可以使用sudo提权）



### 添加Nginx自定义配置

这一步需要在终端进行操作。使用SSH命令连接上DSM。

> ssh user@ip

切换到sudo交互模式，避免每次输入命令都要添加sudo的麻烦。这里输入的密码是user用户的密码，user用户必须具有管理员群组权限（可在**控制面板-用户与群组-用户账户**中查看）。

> sudo -i
> Password:

切换到Nginx配置文件所在目录，目录下有4个子目录。以**available**结尾的目录中放置的文件并不一定会被启用。当在DSM系统中启用服务的时候，这些文件才会被链接到Nginx真正的配置目录并生效。并且这些配置文件都是以UUID的方式命名的。因此为了便于查看，我们直接操作链接文件即可。链接文件的名称更加具有可读性。

> cd /usr/local/etc/nginx
> ls -al

- conf.d：这里的文件都是链接到conf.d-available中的配置文件。
- conf.d-available：一些群晖套件的配置，用户自定义配置，通用选项配置等。
- sites-available：WebStation中的虚拟主机、套件服务器门户等配置。
- sites-enabled：这里的文件都是链接到sites-available中的配置文件。

由上可知，我们需要操作的配置文件是位于`/usr/local/etc/nginx/sites-enabled`目录中的`server.webstation-vhost.conf`，这个配置文件包含了所有WebStation中添加的虚拟主机。

使用`cat server.webstation-vhost.conf`查看此文件，文本如下：

```
server {

    listen      32400 ssl default_server;
    listen      [::]:32400 ssl default_server;

    server_name _;

    include /usr/syno/etc/www/certificate/WebStation_vhost_0ca4e051-ce0b-4057-9bc6-8e86c5c5086f/cert.conf*;

    include /usr/syno/etc/security-profile/tls-profile/config/WebStation_vhost_0ca4e051-ce0b-4057-9bc6-8e86c5c5086f.conf*;

    add_header  Strict-Transport-Security max-age=15768000;
    ssl_prefer_server_ciphers   on;

    include conf.d/.webstation.error_page.default.conf*;

    include conf.d/.webstation.error_page.default.resource.conf*;

    root    "/volume3/web/wordpress";
    index    index.html  index.htm  index.cgi  index.php  index.php5 ;

    location ~* \.(php[345]?|phtml)$ {
        fastcgi_pass unix:/run/php-fpm/php-37a21fc5-5b49-4d50-8f09-7cb6687a695f.sock;

        include fastcgi.conf;
    }

    include /usr/local/etc/nginx/conf.d/0ca4e051-ce0b-4057-9bc6-8e86c5c5086f/user.conf*;

}

```

配置的关键就在于这一Server段配置的最后一行。引入**user.conf**为前缀的文件。默认情况下，此文件不会自动创建，但是此文件的父目录是存在的，我们需要自己创建这个文件。

> cd /usr/local/etc/nginx/conf.d/0ca4e051-ce0b-4057-9bc6-8e86c5c5086f
> vim user.conf

使用vim编辑器打开新创建的**user.conf**文件，填入以下内容。

```
location / {
	try_files $uri $uri/ /index.php?$args;
}
 
# Add trailing slash to */wp-admin requests.
rewrite /wp-admin$ $scheme://$host$uri/ permanent;
```


修改完成后，需要重新载入Nginx的配置生效，可以在WebStation中先停用虚拟主机再开启。另一种方式是使用Nginx的reload信号。

> nginx -s reload


引用文章：
[https://cutepig.net/archives/6040](https://cutepig.net/archives/6040)
[https://www.wpdaxue.com/wordpress-rewriterule.html](https://www.wpdaxue.com/wordpress-rewriterule.html)
[https://www.bilibili.com/read/cv26328282/](https://www.bilibili.com/read/cv26328282/)
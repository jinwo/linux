#### 			LAMP线上基础开发环境搭建+虚拟主机配置

没事就开始瞎折腾😋----前几天在思考一个问题！

PHP线下开发，Apache可以创建多个本地虚拟主机和多个项目对应，线上服务器是否可以`在没有备案域名的情况下`创建多个虚拟主机？

🍡服务器环境----百度云服务器和阿里云服务器

🍢环境搭建----使用的[OneinStack](https://oneinstack.com)一键包

🍾虚拟主机设置

环境搭建网站上有详细的教程，安装比较简单！注：购买的服务器都是赠送云盘的，最好将云盘挂载，环境安装到挂在的云盘中，这样能让系统与开发环境环境实现简单软分离！😂

![云磁盘](https://github.com/jinwo/coding/blob/master/img/%E4%BA%91%E7%A3%81%E7%9B%98.png)

![挂在并安装成功的目录](https://github.com/jinwo/coding/blob/master/img/%E6%8C%82%E5%9C%A8%E5%B9%B6%E5%AE%89%E8%A3%85%E6%88%90%E5%8A%9F%E7%9A%84%E7%9B%AE%E5%BD%95.png)

**重点：虚拟主机设置**

```php
#1、在配置虚拟主机之前一定要把Apache的这个扩展打开
LoadModule vhost_alias_module modules/mod_vhost_alias.so
```

Win与Linux中Apache配置对比----Linux中多出一个vhost文件夹

![配置文件对比](https://github.com/jinwo/coding/blob/master/img/%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E5%AF%B9%E6%AF%94.png)

```php
#2、Linux中的vhosts配置文件
<VirtualHost *:88>
        ServerName www.suliaohe.com
        DocumentRoot "/mnt/lamp/install/data/wwwroot/www.suliaohe.com"
        <Directory "/mnt/lamp/install/data/wwwroot/www.suliaohe.com">
            Options Indexes FollowSymLinks ExecCGI
            Order deny,allow
            Allow from all
            AllowOverride all
            Options +indexes
            DirectoryIndex 'index.php'
        </Directory>
</VirtualHost>
```

```php
#3、在本地的hosts文件中设置   DNS解析
IP: ###.###.###.###	 www.suliaohe.com
```

完成上述步骤后测试，访问失败！🤢🤢----这地方就有点蛋疼了..........

------

在我仔细、认真、不间断的思考后，发现问题的原因处在`vhost`这个文件夹上

经测试:

1、在完成基础配置后还要在`vhost`文件夹中创建与`自定义域名`相对应的配置文件`www.suliaohe.com.conf`

```php
注
#配置信息如下
<VirtualHost *:88>
  ServerAdmin admin@example.com
  DocumentRoot "/mnt/lamp/install/data/wwwroot/www.suliaohe.com"
  ServerName www.suliaohe.com
  ServerAlias suliaohe.com
<Directory "/mnt/lamp/install/data/wwwroot/www.suliaohe.com">
  SetOutputFilter DEFLATE
  Options FollowSymLinks ExecCGI
  Require all granted
  AllowOverride All
  Order allow,deny
  Allow from all
  DirectoryIndex index.html index.php
</Directory>
</VirtualHost>
```

**二次测试**：😀😀

![成功的测试](https://github.com/jinwo/coding/blob/master/img/%E6%88%90%E5%8A%9F%E7%9A%84%E6%B5%8B%E8%AF%95.png)



**总结**

​	在两个服务提供商的服务器上测试，虚拟主机都能设置成功。百度云上设置的到目前为止（两个月），目前还可以正常访问！但是阿里云上具有时效性，因为按照正常的理解，公网IP必须有与之对应的备案域名对应才可以访问！
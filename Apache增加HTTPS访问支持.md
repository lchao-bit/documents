### Apache增加HTTPS访问支持

李超

2017.3.30

1. 打开apache的ssl模块

   `a2enmod ssl`

   `service apache2 restart`

2. 创建自签署的SSL证书

   `mkdir /etc/apache2/ssl`

   `openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt`

   得到apache.key和apache.crt两个文件

   具体参数的含义参考https://www.digitalocean.com/community/tutorials/how-to-create-a-ssl-certificate-on-apache-for-ubuntu-14-04

   在此过程中会要求输入一系列的信息，“Common Name”这一项必须要填服务器的域名或者IP地址

3. 修改apache

   编辑/etc/apache2/sites-available/default-ssl.conf文件，将其中“SSLCertificateFile”的值修改为crt文件的路径，将“SSLCertificateKeyFile”的值修改为key文件的路径，其余部分参照apache的主配置文件，保证在443端口下具有和80端口同样的行为。

4. 应用配置文件

   `a2ensite default-ssl.conf`

   `service apache2 restart`
# ssl 

<pre>
wget https://dl.eff.org/certbot-auto
chmod a+x certbot-auto
./certbot-auto
</pre>

或者   (hkvps.cf是你的域名)
<pre>
./certbot-auto certonly -a standalone -d hkvps.cf -d www.hkvps.cf
</pre>

证书目录 
<pre>
/etc/letsencrypt/live/hkvps.cf
</pre>

配置虚拟主机文件
<pre>
vi /usr/local/nginx/conf/vhost/域名.conf
</pre>

粘贴以下内容
<pre>
server
    {
        listen 443 ssl;
        server_name www.hkvps.cf hkvps.cf;
        index index.html index.htm index.php default.html default.htm default.php;
        root  /home/wwwroot/default/wordpress;
        ssl_certificate /etc/letsencrypt/live/hkvps.cf/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/hkvps.cf/privkey.pem;
ssl_ciphers "EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5";
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_session_cache shared:SSL:10m;

        include wordpress.conf;
        #error_page   404   /404.html;
        include enable-php.conf;

        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
        {
            expires      30d;
        }

        location ~ .*\.(js|css)?$
        {
            expires      12h;
        }

        location ~ /\.
        {
            deny all;
        }

        access_log  /home/wwwlogs/www.hkvps.cf.log;
    }
</pre>

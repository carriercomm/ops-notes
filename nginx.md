###����һֱ�����ȥ������û�ù��� stub_status

    location /nginx_status {
      # copied from http://blog.kovyrin.net/2006/04/29/monitoring-nginx-with-rrdtool/
      stub_status on;
      access_log   off;
      allow SOME.IP.ADD.RESS;
      deny all;
    }

    Active connections: 291
    server accepts handled requests
    16630948 16630948 31070465
    eading: 6 Writing: 179 Waiting: 106

###����
* setΪngx_rewrite ģ�������ָ��
* nginx������$��ʼ��֧�ֱ�����ֵ��variable interpolation����set $a "hello";set $b "$a, $a";��

ngx_echoģ���echoָ��Ҳ֧�ֱ�����ֵ(set $a "hello";echo $a;)��һ��ָ���Ƿ�֧�ֱ�����ֵ���������ڵ�ģ�������

ngx_geo demo

    geo  $geo  { # the variable created is $geo, the variables of $geo is depended on the IP of client.
      default          0;
      127.0.0.1/32     2;
      192.168.1.0/24   1;
      10.1.0.0/16      1;
    }

������������ʹ�ã���nginx�޷��������䴴��ȫ�ֵģ�����ֵ������߽磺������������Ϊ�߽磬���������Ա����ĸ�ֵ������Ӱ���´����������ֵ��
Nginx���õĽ׶ΰ��Ⱥ�˳���У�rewrite�׶Σ�access�׶Σ�content�׶ε�


###SSL����
����RSA��Կ�ķ���

    openssl genrsa -des3 -out privkey.pem 2048
������������һ��2048λ����Կ��ͬʱ��һ��des3�������ܵ����룬����㲻��Ҫÿ�ζ��������룬���Ըĳɣ�

    openssl genrsa -out privkey.pem 2048
������2048λ��Կ�����ڴ˿��ܻ᲻��ȫ��ܿ콫����ȫ��

    openssl req -new -x509 -key privkey.pem -out cacert.pem -days 1095

    server
    {
        listen 443 ssl;
        ssl on;
        ssl_certificate /var/www/sslkey/cacert.pem;
        ssl_certificate_key /var/www/sslkey/privkey.pem;
        server_name 192.168.1.1;
        index index.html index.htm index.php;
        root /var/www/test;
    }

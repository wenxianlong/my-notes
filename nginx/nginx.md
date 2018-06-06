## 下载
访问[官网](http://nginx.org/)，找到下载链接下载，windows下载下来是一个zip包，解压到指定位置
## 启动
找到解压目录，运行```nginx.exe```，在浏览器地址栏中输入```http://localhost```,显示如下界面标示启动成功：
![](nginx.png)
## Nginx负载均衡配置
### 内置负载策略
* 轮询（默认）
    Nginx根据请求次数，将每个请求均匀分配到每台服务器
* 最小连接
    将请求分配给连接数最小的服务器。Nginx会统计哪些服务器的连接数最小
* IP Hash
    绑定处理请求的服务器。第一次请求时，根据客户端的IP算出一个HASH值，将请求分配到集群中的某一天服务器上。后面该客户端的所有请求，都将通过HASH算法，找到之前处理这台客户端的服务器，然后将请求交给它处理。
1.轮询

```
http {
  #...省略其他配置
  upstream tomcats {
      server 192.168.0.100:8080;
      server 192.168.0.101:8080;
      server 192.168.0.102:8080;
  }
  server {
    listen 80;
    location / {
      proxy_pass http://tomcats;
    }
  }
  #...省略其他配置
}
```
* proxy_pass http://tomcats：标示将所有请求转发到tomcats服务器组中配置的某一台服务器上。
* upstream 模块：配置反向代理服务器组，Nginx会根据配置，将请求分发给组里的某一台服务器。tomcats是服务器组的名称
* upstream模块下的server指令：配置处理请求的服务器IP或者域名，端口可选，不配置默认使用80端口。通过上面的配置，Nginx默认将请求依次分配给100,101,102来处理，可以通过修改下面这些参数来改变默认的分配策略
    * weight   
      默认为1，将请求平均分配给每台server  
      ```
          upstream tomcats {
            server 192.168.0.100:8080 weight=2;
            server 192.168.0.101:8080 weight=3;
            server 192.168.0.102:8080 weight=1;
          }
          
      ```
      上面配置，标示6次请求中，100分配2次，101分配3次， 102分配1次
    * max_fails  
      默认为1.某台server允许请求失败的次数，超过最大次数后，在fail_timeout的时间，新的请求将不会分配给这台机器。如果设置为0，Nginx会将这台Server置为永久无效状态，然后将请求发给定义了proxy_next_upstream, fastcgi_next_upstream, uwsgi_next_upstream, scgi_next_upstream, and memcached_next_upstream指令来处理这次错误的请求。
    * fail-timeout
      默认为10秒。某台Server达到max_fails次失败请求后，在fail_timeout期间内，nginx会认为这台Server暂时不可用，不会将请求分配给它
      ```
        upstream tomcats {
          server 192.168.0.100:8080 weight=2 max_fails=3 fail_timeout=15;
          server 192.168.0.101:8080 weight=3;
          server 192.168.0.102:8080 weight=1;
        }
      ``` 
      192.168.0.100这台机器，如果有3次请求失败，nginx在15秒内，不会将新的请求分配给它。
    * backup 
      备份机，所有服务器挂了之后才会生效  
      
      ```
          upstream tomcats {
            server 192.168.0.100:8080 weight=2 max_fails=3 fail_timeout=15;
            server 192.168.0.101:8080 weight=3;

            server 192.168.0.102:8080 backup;
        } 
      ```  
      在100和101都挂了之前，102为不可用状态，不会将请求分配给它。只有当100和101都挂了，102才会被启用。
      
    * down
      标识某一台server不可用。可能能通过某些参数动态的激活它吧，要不真没啥用。
      
      ```
          upstream tomcats {
            server 192.168.0.100:8080 weight=2 max_fails=3 fail_timeout=15;

            server 192.168.0.101:8080 down;

            server 192.168.0.102:8080 backup;
        }
      ```  
      表示101这台Server为无效状态，不会将请求分配给它。
      
     * max_conns 
        限制分配给某台Server处理的最大连接数量，超过这个数量，将不会分配新的连接给它。默认为0，表示不限制。注意：1.5.9之后的版本才有这个配置
        
        ```
        upstream tomcats {
            server 192.168.0.100:8080 max_conns=1000;
        }
        
        ``` 
        表示最多给100这台Server分配1000个请求，如果这台Server正在处理1000个请求，nginx将不会分配新的请求给到它。假如有一个请求处理完了，还剩下999个请求在处理，这时nginx也会将新的请求分配给它。
        
     * resolve   
        将server指令配置的域名，指定域名解析服务器。需要在http模块下配置resolver指令，指定域名解析服务
        
        ```
          http {
              resolver 10.0.0.1;

              upstream u {
                  zone ...;
                  ...
                  server example.com resolve;
              }
          }
          
        ```  
        表示example.com域名，由10.0.0.1服务器来负责解析。 
        

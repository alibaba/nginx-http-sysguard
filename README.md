##Description

This module can be used to protect your server in case system load or memory use goes too high.<br/>
It comes from [Tengine](http://tengine.taobao.org), an Nginx distribution with quite a few advanced features.

###Examples:

    server {
        sysguard on;

        sysguard_load load=10.5 action=/loadlimit;
        sysguard_mem swapratio=20% action=/swaplimit;

        location /loadlimit {
            return 503;
        }

        location /swaplimit {
            return 503;
        }
    }

##Installation

    $ wget http://www.nginx.org/download/nginx-1.2.5.tar.gz
    $ tar xzvf nginx-1.2.5.tar.gz
    
    $ cd nginx-1.2.5
    $ git clone https://github.com/taobao/nginx-http-sysguard.git
    $ patch -p1 < nginx-http-sysguard/nginx_sysguard_1.2.5.patch
    
    $ ./configure --add-module=./nginx-http-sysguard
    # make && make install

##Directives

**Syntax**: ***sysguard*** [on | off]  
**Default**: sysguard off  
**Context**: http, server, location  

Turn on or off this module.
<br/>

**Syntax**: ***sysguard_load*** load=number [action=/url]  
**Default**: none  
**Context**: http, server, location  

Specify the load threshold. When the system load exceeds this threshold, all subsequent requests will be redirected to the URL specified by the 'action' parameter. Nginx will return 503 if there's no 'action' URL defined.
<br/>

**Syntax**: ***sysguard_mem*** swapratio=ratio% [action=/url]  
**Default**: none  
**Context**: http, server, location  

Specify the used swap memory threshold. When the swap memory use ratio exceeds this threshold, all subsequent requests will be redirected to the URL specified by the 'action' parameter. Nginx will return 503 if there's no 'action' URL.
<br/>

**Syntax**: ***sysguard_interval*** time  
**Default**: sysguard_interval 1s  
**Context**: http, server, location  

Specify the time interval to update your system information. The default value is one second, which means Nginx updates the server status once a second.
<br/>

**Syntax**: ***sysguard_log_level*** [info | notice | warn | error]  
**Default**: sysguard_log_level error  
**Context**: http, server, location  
Specify the log level of sysguard.
<br/>
#配置Apache
[TOC]

##配置虚拟主机
1.  打开`httpd.conf`文件
    去掉`LoadModule vhost_alias_module modules/mod_vhost_alias.so`
    和
    打开`Include conf/extra/httpd-vhosts.conf`前边的`#`

2. httpd-vhosts.conf中
    ```
    <VirtualHost *:80>
    ServerAdmin basic.com
    DocumentRoot "D:/Program Files/wampserver64_33lc/wamp/wamp/www/basic/web/"
    ServerName basic.com
    DirectoryIndex index.php
    ServerAlias basic.com
    ErrorLog "logs/basic.com/basic.com-error.log" //手动创建目录(下同)
    CustomLog "logs/basic.com/basic.com-access.log" common
    </VirtualHost>
    ```
##Xdebug
```
; XDEBUG Extension

zend_extension = "D:/Program Files/wampserver64_33lc/wamp/wamp/bin/php/php5.5.12/zend_ext/php_xdebug-2.2.5-5.5-vc11-x86_64.dll"
;
[xdebug]

xdebug.remote_enable=On
xdebug.remote_handler=dbgp
xdebug.remote_host=127.0.0.1
xdebug.remote_port=9000
xdebug.show_exception_trace=On
;xdebug.remote_autostart=On
xdebug.idekey="netbeans-xdebug"

;开启自动追踪功能
;xdebug.auto_trace=On
xdebug.collect_params=4
xdebug.collect_return=1
xdebug.collect_vars=1
xdebug.trace_format=1
xdebug.collect_assignments=1
xdebug.show_mem_delta=1
xdebug.trace_output_dir="D:/gaoqing/software/wamp/tmp/trace"

;xdebug.profiler_enable=On
xdebug.profiler_enable_trigger=1
xdebug.profiler_output_name=cachegrind.out.%t.%p
xdebug.profiler_output_dir = "D:/Program Files/wampserver64_33lc/wamp/wamp/tmp"
xdebug.show_local_vars=1

;输出错误的 server
xdebug.dump.SERVER=HTTP_HOST, SERVER_NAME

;输出全局变量
xdebug.dump_globals=On
xdebug.dump_undefined=On
```


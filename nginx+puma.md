# nginx + puma部署
[原文1](https://segmentfault.com/a/1190000002918225)
[原文2](http://www.jianshu.com/p/8bf68700066a)
- 在rails项目的Gemfile

~~~~
 # Gemfile
 group :production do
   gem 'puma'
 end 
 
 #运行
 bundle install
 
~~~~

- 配置puma,注意补全缺失文件夹

~~~~
 #config/puma.rb
 
 #!/usr/bin/env puma
# rails的运行环境
environment 'production'
# 以守护进程的方式启动
daemonize true
# 定义文件存放目录
app_dir = File.expand_path("../..", __FILE__)
shared_dir = "#{app_dir}/shared"
# pid存放位置
pidfile "#{shared_dir}/pids/puma.pid"
# 日志文件存放位置 标准输出和错误输出
stdout_redirect "#{shared_dir}/logs/stdout","#{shared_dir}/logs/stderr"
# puma状态位置
state_path "#{shared_dir}/state/puma.state"

# 不确定线程是否安全 所以最大线程设置为1
threads 1,1
workers 2

bind "unix:#{shared_dir}/sock/puma.sock"

# 重启时显示提示信息
on_restart do
  puts 'On restart...'
end

preload_app!
~~~~

- 配置nginx
~~~~
  #nginx.conf
   user xxx;
   worker_processes  1;
   error_log  logs/error.log;
   pid        logs/nginx.pid;
   keepalive_timeout  180;
   upstream deploy{
      server unix:puma配置的sock路径/puma.sock fail_timeout=0;
    }
     server {
        listen       80;
        server_name  localhost;
        root 项目目录/public;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
             proxy_pass http://deploy; # match the name of upstream directive which is defined above
             proxy_set_header Host $host;
             proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
        
       location ~* ^/assets/ {
                  # Per RFC2616 - 1 year maximum expiry
                  expires 1y;
                  add_header Cache-Control public;
                  add_header Last-Modified "";
                  add_header ETag "";
                  break;
   }
~~~~

---
layout: default
title: 使用unicorn发布rails程序
---

### 使用unicorn发布rails程序
1. 安装unicorn： gem install unicorn
2. 发布测试环境:
   unicorn_rails -E test -D -c /home/localadmin/banks/config/deploy/test_unicorn.rb
   同理：发布开发环境:
   unicorn_rails -E production -D -c /home/localadmin/banks/config/deploy/production_unicorn.rb
   * 注意：修改项目路径
   * -D 以Deamon 形式(守护进程)启动
   * -c 设定配置文件，如我们的 /workspace/project_name/config/unicorn.rb
   * -E 设定生产环境或开发环境，如 -E production
3. 以下是test_unicorn.rb文件内容  
<pre>
<code>
rails_root  = "/Users/xxx/workgit/banks"
rails_env   = "test"
pid_file    = "/Users/xxx/workgit/log/tmp/pids/unicorn_banks.pid"
socket_file = "0.0.0.0:8888"
log_file    = "/Users/xxx/workgit/log/unicorn_banks.log"
err_log     = "/Users/xxx/workgit/log/unicorn_banks.error.log"
old_pid     = pid_file + '.oldbin'

timeout 60
worker_processes 2

# Listen on a Unix data socket
listen socket_file, :backlog => 2048
pid pid_file

stderr_path err_log
stdout_path log_file

preload_app true

GC.copy_on_write_friendly = true if GC.respond_to?(:copy_on_write_friendly=)

check_client_connection false

before_exec do |server|
  ENV["BUNDLE_GEMFILE"] = "#{rails_root}/Gemfile"
end

before_fork do |server, worker|
  if defined?(ActiveRecord::Base)
    ActiveRecord::Base.connection.disconnect!
  end
  # Using this method we get 0 downtime deploys.
  if File.exists?(old_pid) && server.pid != old_pid
    begin
      Process.kill("QUIT", File.read(old_pid).to_i)
    rescue Errno::ENOENT, Errno::ESRCH
      # someone else did our job for us
    end
  end
end

after_fork do |server, worker|
  if defined?(ActiveRecord::Base)
    ActiveRecord::Base.establish_connection
  end
  child_pid = server.config[:pid].sub('.pid', ".#{worker.nr}.pid")
  system("echo #{Process.pid} > #{child_pid}")
end
</code>
</pre>
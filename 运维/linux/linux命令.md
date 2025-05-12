### debain 命令

#### 包管理相关命令

- 查询安装的依赖： apt list --installed
- 卸载安装的依赖：apt purge <package>
- 查询是否存在相关的包: dpkg -l | grep <package>

#### 系统相关命令

- 设置服务自启动：systemctl enable <service>
- 查看服务是否自启动：systemctl is-enabled <service>
- 查看服务状态：systemctl status <service>
- 重启服务：systemctl restart <service>
- 停止服务：systemctl stop <service>
- 启动服务：systemctl start <service>

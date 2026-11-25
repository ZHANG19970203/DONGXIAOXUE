# 《动小学》人体运动学虚拟实验室

部署后，全班共用同一个 HTTPS 首页。学生选择“学生入口”，教师在同一页选择“教师入口”。学习记录写入同一教学服务的 SQLite 数据库。教师登录后可查看成绩、病例完成情况、对话及问题反馈，导出 Excel。累计人数按学号去重，个人明细保留历次学习。学生身份由本人填写，请使用自己的固定学号。

## 在服务器运行

1. 准备可从班级设备访问的 Linux 主机、HTTPS 域名和持久磁盘，安装 Python 3.12 与 Caddy。将本仓库放在 `/opt/dongxue`。
2. 创建 `/etc/dongxue.env`，填写 `LAB_ORIGIN=https://实际教学域名`。将 `deploy/Caddyfile.example` 中的示例域名换成同一域名，将此站点配置加入 Caddy。
3. 将 `deploy/dongxue.service` 安装为 systemd 服务，执行 `sudo systemctl daemon-reload` 和 `sudo systemctl enable --now dongxue`，验证 Caddy 配置后启动或重载 Caddy。应用监听环回端口 8784，由 Caddy 对外提供 HTTPS。
4. 管理员私下打开服务生成的 `/var/lib/dongxue/teacher-first-setup.html` 中的首次设置链接，设置教师密码。向学生只发送普通首页地址。数据库和教师凭据保存在 `/var/lib/dongxue`，部署升级时保留该目录。
5. 用两个独立浏览器验收。学生在一个浏览器提交练习，教师在另一个浏览器登录并刷新。学生用原学号再次提交后，检查人数及该学生的明细。

GitHub 托管项目源码。师生共用网址由运行上述服务的主机提供。静态 Pages 无法运行 Python 或保存全班学习记录。原公开练习版浏览器中的个人历史不会自动迁移。
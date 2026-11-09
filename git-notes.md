Git 与 GitHub 配置及踩坑记录（Windows）

日期：2026年11月4日
环境：Windows 11，用户名带中文（尊宝）

日志

1. 基础环境配置

· 查看版本：git --version
· 配置全局用户名：git config --global user.name "JIAJOLOVE"
· 配置全局邮箱：git config --global user.email "**********@qq.com"
· 查看配置是否成功：git config --global --list

2. 生成 SSH 密钥（钥匙）

· 生成命令：ssh-keygen -t rsa -b 4096 -C "63489157@qq.com"
· 复制公钥：打开 id_rsa.pub 文件，全选复制，粘贴到 GitHub 的 Settings -> SSH and GPG keys -> New SSH key 中。

3. 踩坑记录（极其重要！）

· 坑1：中文路径问题
  电脑用户名是中文（尊宝），导致系统自带 SSH 找不到默认路径。解决：将密钥复制到纯英文路径下，如 D:\ssh\，并用 icacls 命令修改私钥权限。
· 坑2：22 端口被网络屏蔽（核心杀手）
  一直报 Permission denied 或 Connection timed out，最后发现是国内网络把 GitHub 的 22 端口屏蔽了！
· 完美绕过方案：使用 443 端口。手动测试命令为 ssh -T -p 443 git@ssh.github.com。

4. 终极配置（让电脑自动走 443 端口）

在黑窗口（cmd）中输入以下命令，写入系统配置：
echo "Host github.com" >> C:\Users\尊宝\.ssh\config
echo "    Hostname ssh.github.com" >> C:\Users\尊宝\.ssh\config
echo "    Port 443" >> C:\Users\尊宝\.ssh\config
echo "    User git" >> C:\Users\尊宝\.ssh\config

5. 测试成功标志

· 输入：ssh -T git@github.com
· 提示：Hi JIAJOLOVE! You've successfully authenticated... 即代表大功告成！

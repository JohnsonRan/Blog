title: OneDrive重装提示已安装的解决方案
categories: 教程
tags: [OneDrive,Windows]
date: 2020-05-20 10:34:00
toc: true
---
##本文来自微软客服:
1. 打开命令提示符在管理员模式下 ︰ 用鼠标右键单击 Windows 任务栏中的图标并选择命令提示符 （管理员）。
2. 输入 taskkill /f /im OneDrive.exe 终止任何OneDrive进程并按下回车键。
3. 然后在任一中键入下面的命令，这取决于如果你有一个 64 个或 32 位的系统，这将卸载OneDrive
a. %SystemRoot%\SysWOW64\OneDriveSetup.exe /uninstall 如果你使用的 64 位 （更常见）
b. %SystemRoot%\System32\OneDriveSetup.exe /uninstall 如果你使用的 32 位
4. Onedrive已被卸载，请转到【新的 OneDrive 同步客户端】后进行下载 ；
5. 运行下载和允许完成下载，这将确保安装了最新版本的同步引擎；
6. 一旦运行中，如果对话标志没有出现，请左键单击右下角系统通知区域中的OneDrive图标并尝试是否可正常登录即可。
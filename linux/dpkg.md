## Ubuntu16.04卸载软件说明
**命令dpkg**
dpkg -l：列出系统安装的软件
dpkg -l | grep nvidia*：查找软件名以nvidia开始的软件
dpkg -L | grep nvidia-cuda-toolkit：查找nvidia-cuda-toolkit的安装路径
aptitude show nvidia-cuda-toolkit：查看nvidia-cuda-toolkit的版本信息
dpkg  -P , --purge：移除软件（不保留配置）
dpkg -r,--remove：移除软件（保留配置）

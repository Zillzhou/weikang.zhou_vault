# 服务器TCP端口数的优化

## 服务器TCP端口数的优化
## 1.提升tcp可用端口数
## WIN
## 增加端口指令：
netsh int ipv4 set dynamicportrange tcp startport=5000 numberofports=60000
## 查看可用端口数量指令：
netsh int ipv4 show dynamicportrange tcp
## LINUX
## 增加端口指令：
## 1.vim /etc/sysctl.conf
### 2.往文件中添加加入 net.ipv4.ip_local_port_range = 30000 65000
### 3.控制台输入 sysctl -p /etc/sysctl.conf 指令进行刷新

## 查看可用端口数量指令：
cat /proc/sys/net/ipv4/ip_local_port_range
### 2.减少TIME_WAIT的回收时间
## WIN
## 1.在Windows开始菜单中，单击“运行”。
### 2.在“运行”对话框中，输入“regedit”后按“Enter”打开注册表编辑器。
3.在“注册表编辑器”中打开“HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters”路径。
### 4.在“编辑”菜单中，选择“新建 > DWORD （32-位）值”，输入名称“TcpTimedWaitDelay”。
### 5.右键单击TcpTimedWaitDelay，选择“修改”。
### 6.在“编辑 DWORD（32位）值”对话框的“基数”区域中，选择十进制值为“30”，并“确定”。
### 7.关闭注册表编辑器。
## LINUX
## 1.通过修改/etc/sysctl.conf文件，服务器能够快速回收和重用那些TIME_WAIT的资源
#表示开启SYN Cookies。当出现SYN等待队列溢出时，启用cookies来处理，可防范少量SYN攻击，默认为0，表示关闭
### net.ipv4.tcp_syncookies = 1
#表示开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接，默认为0，表示关闭
### net.ipv4.tcp_tw_reuse = 1
#表示开启TCP连接中TIME-WAIT sockets的快速回收，默认为0，表示关闭
### net.ipv4.tcp_tw_recycle = 1
#表示如果套接字由本端要求关闭，这个参数决定了它保持在FIN-WAIT-2状态的时间
### net.ipv4.tcp_fin_timeout=30
### 2.sysctl -p /etc/sysctl.conf 指令进行刷新

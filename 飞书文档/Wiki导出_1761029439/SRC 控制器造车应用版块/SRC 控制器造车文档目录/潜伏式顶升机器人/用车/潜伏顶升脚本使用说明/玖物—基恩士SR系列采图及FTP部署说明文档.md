# 玖物—基恩士SR系列采图及FTP部署说明文档 

### 玖物—基恩士SR系列采图及FTP部署说明文档
## 硬件准备与要求
## 使用要求
### RBK 3.4.6.18 /RBK 3.4.5.40以上
## 使用前提
读码相机（SR80/SR300）已安装在车体，且和车体通讯(网线)、供电相连,且通讯、采图正常
### 相机ip配置在192.168.192.x网段
## 网线插在控制器192网段
window+R调出命令行，使用ping命令 ping相机可以有通讯返回。
## 图片.png

## 功能说明
### 脚本配置在终点库位上，或者直接通过api调用
动作逻辑：1）车体导航到前置点后，激光识别一次货架，导航到距离货架1m处，2）开始读码并上报 3）退回前置点
## 在线部署FTP服务
## Ubuntu编辑工具VIM使用参考：

## 部署要求
## 车体中的控制器须连外网，且信号强度好
## 图片.png

## 图片.png

### 网线连接控制器（192网段网口）和笔记本
## moba软件使用

## MobaXterm.7z
## 下载压缩包，解压后：
## 图片.png

## 图片.png

新建SSH—输入IP和控制器名称（192.168.192.5，sr）——设置密钥
## 图片.png

## 图片.png

## 控制器准备
连接上后，需要输入控制器密码（若为自行实施，密码可咨询对应实施工程师）。注：对外发布时注意是否涉及保密
## 图片.png

输入密码后回车，跳转到控制器用户名密码，输入sr.
## 图片.png

## 进入控制器内部
## 图片.png

## 安装VSftp服务器
## 安装步骤参考：

## 更新系统包列表
## sudo apt update
## 图片.png

## 输入密码sr
若回车后报下述错误，说明远程阿里云的源无法连接，此时有两种情况，1）控制器未连接外网（或者网太差，查看网络信号强度） 2）源文件太老需要更新。
## 图片.png

### 检查网络后再进行尝试，如果还报下述错误，需要更换源文件
添加清华大学的源地址。
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
## 再次执行检查待更新项
## sudo apt update
更新升级当前系统包（需要等待一会儿），期间确保网络连接。
## sudo apt upgrade -y
## 输入安装命令：
### sudo apt install vsftpd
## 图片.png

## 配置FTP服务器

sudo vim /etc/vsftpd/vsftpd.conf
注：有些控制器安装后无中间vsftpd文件夹，直接存放在etc文件夹下，如下图，可通过ls命令查看。请根据具体路径进行修改。
## 图片.png

下述chroot_list路径同理。
## 图片.png

### 打开服务器配置文件，并对下述项目进行修改
listen=YES：启用监听模式。
anonymous_enable=NO：禁用匿名用户登录。
local_enable=YES：允许本地用户登录。
write_enable=YES：允许用户上传文件。
chroot_local_user=YES：将用户限制在主目录中，防止访问其他目录。
chroot_list_enable=YES：启用chroot_list文件。
chroot_list_file=/etc/vsftpd/chroot_list：指定chroot_list文件路径。
## 配置文件参考：

## vsftpd.conf

现有chroot_list文件路径下无此文件，需要手动创建。
## 先进入/etc/vsftpd/目录下
## cd /etc/vsftpd
## 创建文件
### sudo touch chroot_list
## 随后启动FTP服务
### sudo systemctl start vsftpd
## 设置ftp服务为开机自启动
### sudo systemctl enable vsftpd
## 测试FTP服务
使用FTP客户端连接到服务器。您可以使用如FileZilla、WinSCP等FTP客户端软件。

FileZilla_3.68.1_win64_sponsored2-setup.exe
输入您的用户名和密码，您应该能够成功连接到FTP服务器。
## 图片.png

## 或者
## 图片.png

连接成功，远程站点有信息，说明服务已经部署成功。
## 图片.png

## 图片.png

至此，ftp服务配置完成且图片存储路径可写入。
## 附件：FTP服务部署操作录屏
https://seer-group.feishu.cn/minutes/obcnxc1p8r9k8toj2yps95k1
## 离线部署ftp服务
## 准备工作

## ftp脚本_离线包.zip
步骤 4.1.1 -上传脚本和离线包到控制器同一目录（/home/sr/）
## image.png

## image.png

## image.png

## image.png

## image.png

## 离线环境：安装 vsftpd
## 步骤 4.2.1 - 执行安装脚本
## # 授予执行权限
### chmod +x install_vsftpd.sh

## # 运行安装脚本
### sudo ./install_vsftpd.sh

## # 预期输出
## # === 安装结果验证 ===
### # vsftpd: version 3.0.5
## # active

## 安装后验证
## 步骤 4.3.1 - 服务状态检查
systemctl status vsftpd      # 状态应为 active (running)
netstat -tuln | grep :21    # 确认 21 端口监听
## 步骤 4.3.2 - 功能测试
## # 本地连接测试（使用系统用户）
## ftp localhost
## > 输入用户名和密码
> put install_vsftpd.sh  # 上传文件
## > quit

## # 检查文件是否上传成功
## ls

## 脚本使用方式
## 脚本名称与位置
Roboshop uuid: cda2d79c-f3bb-4211-a7fb-be7c67e7e0dc 
## 图片.png

### 登录脚本项目平台，公用账号账号：user 密码：user
## image.png

## 下载项目对应脚本
## Step 1：点击拉取
## Step 2：将项目uuid复制到框中
## Step 3：点击获取记录
## Step 4：将文件下载至本地
## image.png

## 下载至本地后推送
### 在脚本列表中右键项目脚本文件并推送至控制器
## image.png

## 测试
脚本功能：实现读码后将读码结果上报，当识别次数超过“rec_time”仍识别失败时，会报错结束任务。
## 确认相机基本参数
### 相机和识别基本参数可脚本运行前根据实际情况进行修改：
## 图片.png

(1). ip：相机IP地址，默认192.168.192.129
### (2). port：相机通讯端口，默认9004
(3).rec_time:最大可识别次数，超过次数报错结束脚本
 (4).recfile:贴有货架码的货物/栈板的模型（识别文件配置）
### (5).code_dist:扫码相机的离栈板的拍照距离
 (6).ap_dist:扫码相机距离终点ap点的距离，若无货架模型时，导航到目标ap点前固定距离
## 任务链或者bintask调用
影响识别距离输入参数为id与recfile，recfile为货物或者或货架的识别文件，用于计算距离货架1m的读码站点坐标。
## {
## "id":"AP22022"
    "operation": "Script",
## "script_args": {
### "recfile":"plt/p1.plt"
    },
    "script_name": "project_jiuwu_jienshirec.py",
    "script_stage":3,
## "recognize":true

## }

recfile 可缺省,缺省默认导航到ap点前ap_dist（默认1.5m，按货物尺寸0.5m计算）距离处
## {
## "id":"AP22022"
    "operation": "Script",
## "script_args": {
    },
    "script_name": "project_jiuwu_jienshirec.py",
    "script_stage":3,
## "recognize":true
## }

## 测试结果

无论是否扫描到二维码都会将结果上图信息上报到调度。
Roboshop中亦可在右侧的“'script_getcode_info' 变量中查看到扫描结果。
## 图片.png

当无法连接到相机时会有弹窗报错。
## 图片.png

### 如果现场使用periodrun脚本，则上报信息如下：
## 图片.png

其中报上述文件路径找不到问题是因为在路径下没有找到相机传输上来的图片，在ftp服务正常可接收的情况下，需要检查相机的ftp发送配置，文件路径是否正常配置，以及图片名称是否已修改为LoadError。
## 读码及上报成功：
## 图片.png

## 如何获取脚本上报信息
## 通过 标准的TCP API 获取
## API是使用点击 -> TCP API
通信协议对应的API -> 1100 , 1100 介绍点我
### 在字段 move_status_info 就可以找到
## {
## move_status_info:{
      "code_value":123456,
      "picture_json":{"filename":"erro.jpg","image":""}
      "rec_status":2,    // 1识别中  2 识别成功   3识别失败    0 初始状态
### "tag_rec_result_state":true
## }
## }
## 通过 core 的标准API 获取
## core 的API 使用 点这里
在 获取所有机器人信息 中可以查询机器人的信息，其中包括的机构脚本的输出信息，在对象 rbk_report 字段 info 即可查到机构脚本的输出信息。

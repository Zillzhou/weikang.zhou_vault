# Mid-70升级工具使用说明

## Mid-70升级工具使用说明

## 版本

## 更新日期

## 更新说明

## 文档状态

## 维护责任人

## V1.0

### 2024.3.25

## 语雀迁移至飞书

## 使用中

## 免责声明
本免责声明适用于该文档中的附件 Seer_livox-upgrade_tool.zip 。在使用该工具对 Mid-70 设备进行升级之前，请务必仔细阅读并充分理解本免责声明的全部内容。如您不同意本免责声明的任何内容，请勿使用该工具。
## 该工具不对以下情况承担任何责任：
（1）因用户操作不当、误用、滥用该工具所导致的任何损害；
（2）因第三方原因（如黑客攻击、病毒感染等）导致的任何损害；
（4）其他与该工具相关的任何损失。
## 附件
📎Seer_livox-upgrade_tool-202311071639.zip
## 使用方法
解压 Seer_livox-upgrade_tool-202311071639.zip ，
 arm64 架构控制器使用 livox_upgrade_tool_node_arm64
 x86 架构控制器使用 livox_upgrade_tool_node_x86
## 将其和升级固件一同上传进控制器中
进行加权， sudo chmod a+x livox_upgrade_tool_node_x86 或 sudo chmod a+x livox_upgrade_tool_node_arm64
烧录固件示例，文件名 LIVOX_MID70_FW_10.10.0001-20230801160825.bin ，升级方式为判断模式 -j
判断模式： sudo ./livox_upgrade_tool_node -j LIVOX_MID70_FW_10.10.0001-20230801160825.bin ，该指令只允许升级固件高于当前设备固件版本。
升级成功后对设备进行 重上电 即可。
## 成功图示
1698817589176-33db9764-1103-4a4b-920c-97f9d0f4a862.png

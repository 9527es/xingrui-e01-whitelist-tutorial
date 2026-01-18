# xingrui-e01-whitelist-tutorial
一个针对星瑞 E01 设备的镜像白名单绕过教程，使用 mtk-su 工具。
# 星瑞 E01 镜像白名单限制教程

这是一个针对星瑞 E01 设备的简单教程，指导如何使用 mtk-su 工具绕过镜像白名单限制。通过替换 services.jar 文件来实现。

**警告：你是零基础用户，请注意！这个操作需要 root 权限，可能会导致设备变砖（无法开机）、数据丢失、保修失效或安全问题。请先备份所有数据，自行承担所有风险。如果不确定，别尝试！建议先在论坛或X（Twitter）搜索类似经验。**

## 前提条件（准备工作）
- **设备**：星瑞 E01（或类似联发科 MTK 芯片的 Android 设备）。
- **工具**：mtk-su（一个临时 root 工具，需要从可靠来源下载，比如 XDA Developers 论坛。搜索“mtk-su download”获取，但要验证安全性）。
- **文件**：修改后的 services.jar（这是系统文件，需要你自己从设备提取并修改，或者找现成的。修改方法：用 APKTool 反编译 services.jar，编辑 smali 文件移除白名单检查，然后重新打包。但如果你零基础，这步可能需要学习 Java 或找别人帮忙）。
- **电脑连接**：用 ADB（Android Debug Bridge）工具连接设备。ADB 是 Android SDK 的一部分，下载 Android Studio 或单独 ADB 工具包。
- **启用调试**：在设备设置 > 关于手机 > 连续点击版本号开启开发者模式，然后在开发者选项启用 USB 调试。

## 详细步骤（一步步操作）
1. **将文件复制到设备 SD 卡**：
   - 把 mtk-su 和 修改后的 services.jar 放到设备的 SD 卡根目录（/sdcard/）。可以用文件管理器或 ADB push 命令：
     ```
     adb push mtk-su /sdcard/mtk-su
     adb push services.jar /sdcard/services.jar
     ```

2. **通过 ADB 进入 shell 并执行命令**：
   - 连接设备到电脑，打开命令提示符（Windows: cmd，Mac: Terminal）。
   - 输入 `adb shell` 进入设备终端。
   - 然后一步步运行以下命令（复制粘贴，每行回车）：
     ```
     cp /sdcard/mtk-su /data/local/tmp/mtk-su
     chmod 755 /data/local/tmp/mtk-su
     /data/local/tmp/mtk-su  # 这会临时获取 root 权限
     mount -o rw,remount /system  # 挂载系统为可写（注意：如果是 ext4 文件系统，用 -t ext4）
     rm -rf /system/framework/arm/services.odex
     rm -rf /system/framework/arm64/services.odex
     rm -rf /system/framework/services.jar
     cp /sdcard/services.jar /system/framework/services.jar
     chmod 0644 /system/framework/services.jar
     chown root:root /system/framework/services.jar
     ```

3. **重启设备**：
reboot- 重启后，检查镜像功能是否正常。如果白名单限制没了，就成功了。

## 常见问题与调试（如果你遇到错误）
- **命令失败**：检查是否获取 root（mtk-su 输出应该有“root”提示）。如果没 root，mtk-su 可能不兼容你的设备版本。
- **文件没找到**：确认路径正确，用 `ls /sdcard/` 检查文件是否存在。
- **权限问题**：确保用 root 运行命令。如果卡住，重启再试。
- **砖机风险**：如果设备不开机，用 recovery 模式恢复出厂设置（但数据会丢）。
- **零基础提示**：如果你不懂 ADB，搜索“YouTube ADB tutorial for beginners”看视频学习。花10-20分钟就能上手。

## 免责声明
本教程仅供教育目的，不保证成功、安全或合法性。设备修改可能违反制造商条款。作者不对任何损害、数据丢失或法律问题负责。请在合法范围内使用。

## 贡献
如果你有改进，欢迎提交 Pull Request！

## 许可
MIT License（见 LICENSE 文件）

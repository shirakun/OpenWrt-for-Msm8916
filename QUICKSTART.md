# 快速入门指南

## 前置要求

- GitHub 账号
- 已 Fork 本仓库到你的账号

## 步骤 1: 启用 GitHub Actions

1. 进入你 Fork 的仓库
2. 点击顶部的 **Actions** 标签
3. 如果看到提示，点击 **I understand my workflows, go ahead and enable them**

## 步骤 2: 运行编译

### 方式一：手动触发（推荐首次使用）

1. 在 Actions 页面，左侧选择一个工作流：
   - `Build_MSM8916_OpenWrt_Main` - 编译 main 分支
   - `Build_MSM8916_OpenWrt_25_12` - 编译 25.12 分支

2. 点击右侧的 **Run workflow** 按钮

3. 选择是否启用详细日志：
   - `Build logs`: false (默认，推荐)
   - `Build logs`: true (详细日志，用于调试)

4. 点击绿色的 **Run workflow** 按钮开始编译

### 方式二：自动触发

修改以下文件并推送到 main 分支会自动触发编译：
- `.github/workflows/msm8916-openwrt-main.yml`
- `.github/workflows/msm8916-openwrt-25.12.yml`
- `config/` 目录下的文件
- `scripts/` 目录下的文件
- `feeds.conf.default`

## 步骤 3: 等待编译完成

- 首次编译大约需要 **2-4 小时**
- 后续编译由于缓存会更快（约 1-2 小时）
- 可以在 Actions 页面查看实时日志

## 步骤 4: 下载固件

编译完成后，有两种方式下载固件：

### 方式一：从 Artifacts 下载（临时）

1. 进入完成的 workflow 运行页面
2. 滚动到底部的 **Artifacts** 部分
3. 点击下载 `OpenWrt_firmware_*` 文件
4. 注意：Artifacts 会在 90 天后自动删除

### 方式二：从 Releases 下载（永久）

1. 进入仓库的 **Releases** 页面
2. 找到最新的 Release（按时间戳命名）
3. 下载 Assets 中的固件文件
4. 注意：默认保留最新 20 个 Release

## 步骤 5: 刷入固件

根据你的设备型号，参考 [msm8916-openwrt](https://github.com/hkfuertes/msm8916-openwrt) 的刷机说明。

一般步骤：
1. 解压下载的固件包
2. 找到适合你设备的固件文件（通常是 `.img` 或 `.bin` 文件）
3. 使用 fastboot 或其他工具刷入设备

## 常见问题

### Q: 编译失败怎么办？

1. 查看 Actions 日志，找到错误信息
2. 如果是网络问题，重新运行 workflow
3. 如果是编译错误，启用详细日志 (log_switch: true) 重新编译
4. 在 Issues 中搜索类似问题或提交新 Issue

### Q: 如何修改配置？

编辑 `config/msm8916.config` 文件，添加或删除需要的软件包配置。

### Q: 如何添加其他软件包？

编辑 `scripts/update-packages.sh`，参考现有的 `UPDATE_PACKAGE` 调用添加新包。

### Q: 固件包含哪些软件？

- 基础 LuCI 界面
- PassWall 代理工具
- HomeProxy 代理工具
- Argon 主题
- ModemManager（4G/LTE 支持）
- WireGuard VPN
- 其他基础工具

### Q: 如何更新固件？

重新运行 workflow 编译最新版本，然后刷入新固件。

## 下一步

- 阅读 [README.md](README.md) 了解更多详细信息
- 自定义配置文件以满足你的需求
- 加入社区讨论和贡献

## 技术支持

- 提交 Issue: [GitHub Issues](../../issues)
- 参考项目: [msm8916-openwrt](https://github.com/hkfuertes/msm8916-openwrt)
- OpenWrt 官方文档: [openwrt.org](https://openwrt.org)

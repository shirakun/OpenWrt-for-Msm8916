# OpenWrt for MSM8916

基于 [msm8916-openwrt](https://github.com/hkfuertes/msm8916-openwrt) 的自动化编译项目，集成 PassWall 和 HomeProxy 代理工具。

## 项目特性

- ✅ 支持 MSM8916 平台设备
- ✅ 自动化 GitHub Actions 编译
- ✅ 支持 main 和 25.12 两个分支
- ✅ 集成 PassWall 代理工具
- ✅ 集成 HomeProxy 代理工具
- ✅ 集成 Argon 主题
- ✅ 自动发布固件到 GitHub Releases

## 支持的设备

基于 MSM8916 芯片的设备，包括但不限于：
- UF02 (generic-uf02)
- UZ801
- 其他 MSM8916 设备

## 工作流说明

> ⚠️ **重要**: 本项目有两个工作流使用不同的源码。详细说明请查看 [BRANCHES.md](BRANCHES.md)

### Main 分支编译 ⭐ 推荐
- 文件: `.github/workflows/msm8916-openwrt-main.yml`
- 源码: `hkfuertes/msm8916-openwrt` (main 分支)
- 说明: 使用 msm8916-openwrt 项目的 main 分支，已包含 MSM8916 支持
- 触发方式: 
  - 手动触发 (workflow_dispatch)
  - 推送到 main 分支时自动触发

### 25.12 分支编译
- 文件: `.github/workflows/msm8916-openwrt-25.12.yml`
- 源码: 官方 `openwrt/openwrt` (openwrt-25.12 分支) + MSM8916 补丁
- 说明: 使用官方 OpenWrt 25.12 稳定分支，然后应用 MSM8916 补丁和目标文件
- 触发方式:
  - 手动触发 (workflow_dispatch)
  - 推送到 main 分支时自动触发

## 目录结构

```
.
├── .github/
│   └── workflows/
│       ├── msm8916-openwrt-main.yml      # Main 分支编译工作流
│       └── msm8916-openwrt-25.12.yml     # 25.12 分支编译工作流
├── config/
│   └── msm8916.config                     # MSM8916 设备配置文件
├── scripts/
│   ├── update-packages.sh                 # 第三方软件包安装脚本
│   ├── apply_patches.sh                   # MSM8916 补丁应用脚本
│   └── cleanup-old-releases.sh            # 旧版本清理脚本
└── README.md
```

## 集成的软件包

### 代理工具
- **PassWall**: 功能强大的代理工具，支持多种协议
  - `luci-app-passwall` - 主程序
  - `luci-i18n-passwall-zh-cn` - 中文语言包
- **HomeProxy**: 基于 sing-box 的代理工具
  - `luci-app-homeproxy` - 主程序
  - `luci-i18n-homeproxy-zh-cn` - 中文语言包
- **Xray-core**: 高性能代理核心

### 网络工具
- **ipset**: IP 集合管理
- **iptables-nft**: 防火墙规则管理
- **ip-full**: 完整的 IP 工具集

### 主题
- **Argon**: 现代化的 LuCI 主题
- **Argon Config**: Argon 主题配置工具

### 其他
- **GecoosAC**: 高格 AC 控制器
- **ModemManager**: 4G/LTE 调制解调器管理
- **WireGuard**: VPN 工具

## 使用方法

### 1. Fork 本仓库

点击右上角的 Fork 按钮，将本仓库 fork 到你的账号下。

### 2. 启用 GitHub Actions

进入你 fork 的仓库，点击 Actions 标签页，启用 GitHub Actions。

### 3. 手动触发编译

1. 进入 Actions 标签页
2. 选择要运行的工作流 (Main 或 25.12)
3. 点击 "Run workflow" 按钮
4. 可选择是否启用详细日志 (log_switch)

### 4. 下载固件

编译完成后，可以在以下位置下载固件：
- **Artifacts**: Actions 运行页面的 Artifacts 部分
- **Releases**: 仓库的 Releases 页面

## 自定义配置

### 修改设备配置

编辑 `config/msm8916.config` 文件，根据你的需求添加或删除软件包。

配置文件格式为 OpenWrt 的 diffconfig 格式，例如：
```
CONFIG_TARGET_msm89xx=y
CONFIG_TARGET_msm89xx_msm8916=y
CONFIG_TARGET_msm89xx_msm8916_DEVICE_generic-uf02=y
CONFIG_PACKAGE_luci=y
```

### 添加其他软件包

编辑 `scripts/update-packages.sh`，参考现有的 `UPDATE_PACKAGE` 调用添加新的软件包。

例如：
```bash
UPDATE_PACKAGE "package-name" "github-user/repo-name" "branch-name"
```

## 编译流程

### Main 分支流程
1. **环境准备**: 安装编译依赖
2. **克隆源码**: 从 msm8916-openwrt 仓库克隆源码（已包含 MSM8916 支持）
3. **应用补丁**: 应用 WiFi 驱动补丁
4. **更新 feeds**: 更新和安装 OpenWrt feeds
5. **安装第三方包**: 安装 PassWall、HomeProxy 等
6. **配置**: 应用设备配置文件
7. **下载**: 下载编译所需的软件包
8. **编译**: 编译固件
9. **发布**: 上传到 Artifacts 和 Releases

### 25.12 分支流程
1. **环境准备**: 安装编译依赖
2. **克隆源码**: 从官方 OpenWrt 克隆 25.12 分支
3. **克隆 MSM8916 源码**: 获取 MSM8916 补丁和目标文件
4. **复制目标文件**: 将 MSM8916 target 定义复制到 OpenWrt
5. **应用补丁**: 应用 WiFi 驱动补丁
6. **更新 feeds**: 更新和安装 OpenWrt feeds
7. **安装第三方包**: 安装 PassWall、HomeProxy 等
8. **Rust 修复**: 修复 25.12 分支的 Rust 编译问题
9. **配置**: 应用设备配置文件
10. **下载**: 下载编译所需的软件包
11. **编译**: 编译固件
12. **发布**: 上传到 Artifacts 和 Releases

## 缓存优化

项目使用了以下缓存来加速编译：
- **dl 缓存**: 下载的软件包源码
- **ccache**: 编译缓存，最大 5GB

## 参考项目

- [msm8916-openwrt](https://github.com/hkfuertes/msm8916-openwrt) - MSM8916 OpenWrt 源码
- [OpenWrt-for-XG-040G-MD](https://github.com/shirakun/OpenWrt-for-XG-040G-MD) - 工作流参考项目

## 许可证

本项目遵循 OpenWrt 的许可证。

## 贡献

欢迎提交 Issue 和 Pull Request！

## 注意事项

1. 首次编译可能需要较长时间（2-4 小时）
2. GitHub Actions 有使用时长限制，请合理使用
3. 编译失败时可以启用详细日志 (log_switch) 来排查问题
4. 旧的 Release 会自动清理，默认保留最新 20 个版本

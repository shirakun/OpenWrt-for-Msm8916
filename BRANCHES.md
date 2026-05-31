# 分支说明

## 重要提示

本项目提供两个编译工作流，它们使用**不同的源码仓库**：

### Main 分支工作流 ⭐ 推荐

**源码**: `hkfuertes/msm8916-openwrt` (main 分支)

- ✅ **推荐使用** - 专为 MSM8916 设备优化
- ✅ 源码已包含完整的 MSM8916 平台支持
- ✅ 包含所有必要的设备驱动和配置
- ✅ 编译更稳定可靠
- ✅ 由 MSM8916 社区维护

**适合**: 
- 首次使用
- 追求稳定性
- MSM8916 设备用户

### 25.12 分支工作流 ⚠️ 实验性

**源码**: 官方 `openwrt/openwrt` (openwrt-25.12 分支) + MSM8916 补丁

- ⚠️ **实验性质** - 使用官方 OpenWrt 稳定版
- ⚠️ 需要手动应用 MSM8916 补丁
- ⚠️ 可能存在兼容性问题
- ✅ 使用 OpenWrt 官方 25.12 LTS 版本
- ✅ 获得官方安全更新

**适合**:
- 需要 OpenWrt 25.12 特定功能
- 愿意测试和反馈问题
- 有一定 OpenWrt 使用经验

## 为什么有两个工作流？

### 背景

`hkfuertes/msm8916-openwrt` 仓库只有 `main` 分支，没有 `openwrt-25.12` 分支。

为了支持 OpenWrt 25.12 稳定版，我们采用了以下方案：
1. 从官方 OpenWrt 克隆 25.12 分支
2. 从 msm8916-openwrt 获取 MSM8916 补丁
3. 将补丁应用到官方 OpenWrt

### 技术细节

#### Main 分支流程
```
hkfuertes/msm8916-openwrt (main)
    ↓
克隆源码（已包含 MSM8916 支持）
    ↓
应用 WiFi 驱动补丁
    ↓
编译固件
```

#### 25.12 分支流程
```
官方 openwrt/openwrt (25.12)
    ↓
克隆源码（纯净 OpenWrt）
    ↓
从 msm8916-openwrt 获取补丁
    ↓
复制 target 文件和设备树
    ↓
应用 WiFi 驱动补丁
    ↓
编译固件
```

## 如何选择？

### 选择 Main 分支，如果你：
- ✅ 是 MSM8916 设备用户
- ✅ 想要最稳定的体验
- ✅ 首次使用本项目
- ✅ 不需要特定的 25.12 功能

### 选择 25.12 分支，如果你：
- ✅ 需要 OpenWrt 25.12 的特定功能
- ✅ 想要官方 LTS 版本的安全更新
- ✅ 愿意测试和报告问题
- ✅ 有 OpenWrt 使用和调试经验

## 常见问题

### Q: 为什么不直接使用 msm8916-openwrt 的 25.12 分支？

A: 因为 `hkfuertes/msm8916-openwrt` 仓库没有 25.12 分支。该项目只维护 main 分支。

### Q: 25.12 工作流稳定吗？

A: 这是实验性的。官方 OpenWrt 25.12 是稳定的，但 MSM8916 补丁可能需要调整才能完全兼容。建议先使用 main 分支。

### Q: 两个版本的功能有区别吗？

A: 核心功能相同（PassWall、HomeProxy、Argon 主题等），但底层 OpenWrt 版本不同：
- Main: 基于 msm8916-openwrt 的开发版
- 25.12: 基于 OpenWrt 官方 25.12 LTS 版

### Q: 我应该如何更新固件？

A: 
- **Main 分支**: 定期重新编译即可获得最新更新
- **25.12 分支**: 获得官方安全更新，但 MSM8916 特性更新较慢

### Q: 编译失败怎么办？

A: 
- **Main 分支失败**: 检查 msm8916-openwrt 项目的 Issues
- **25.12 分支失败**: 可能是补丁不兼容，建议切换到 main 分支

## 推荐配置

### 新手推荐
```yaml
使用工作流: msm8916-openwrt-main.yml
源码: hkfuertes/msm8916-openwrt (main)
稳定性: ⭐⭐⭐⭐⭐
```

### 进阶用户
```yaml
使用工作流: msm8916-openwrt-25.12.yml
源码: 官方 OpenWrt 25.12 + MSM8916 补丁
稳定性: ⭐⭐⭐⚠️⚠️
```

## 参考链接

- [msm8916-openwrt 项目](https://github.com/hkfuertes/msm8916-openwrt)
- [OpenWrt 官方](https://openwrt.org)
- [OpenWrt 25.12 发布说明](https://openwrt.org/releases/25.12/start)

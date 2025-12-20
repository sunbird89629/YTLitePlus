# 对话笔记（YTLitePlus）

## 项目概况
- 仓库路径：`/Users/hao/work/youguan/YTLitePlus`
- 类型：基于 Theos 的 iOS YouTube Tweaks 项目
- 主要结构：`YTLitePlus.xm`、`Source/`、`Tweaks/`、`Bundles/`、`Extensions/` 等

## 子模块状态
- 初始现象：大量 submodule 显示修改/删除
- 原因判断：子模块未初始化或未检出
- 处理：执行 `git submodule update --init --recursive --force`
- 结果：`git status -sb` 变为干净

## 构建脚本（.github/workflows/buildapp.yml）要点
- workflow_dispatch 输入：SDK 版本、解密 IPA URL、bundle_id、app_name、commit、上传与发布开关
- 主要步骤：
  - checkout 仓库 + submodules
  - 安装依赖
  - 准备 Theos 与 theos-jailed
  - SDK 缓存/拉取
  - 下载 IPA、解包、移除 CodeResources/PlugIns、处理 UISupportedDevices
  - `make package` 生成 IPA
  - 重命名 IPA、上传 artifact / catbox、可选创建 draft release
- 结论：构建出的 IPA **未签名**（Makefile 设置 `CODESIGN_IPA=0`）

## “Create a draft release (Private)” 含义
- Draft release 未发布前对外不可见，只有有权限的人能看到

## 签名与分发
- IPA 默认未签名
- 可选签名方式：
  - Ad Hoc：需设备 UDID，适合小范围内部测试
  - TestFlight/App Store：上传 App Store Connect，通过审核分发

## Ad Hoc vs TestFlight/App Store 对比
- 设备限制：Ad Hoc 需预注册 UDID（每年 100 台/类型）；TestFlight/App Store 无此限制
- 审核：Ad Hoc 无审核；TestFlight 需 Beta Review；App Store 需完整审核
- 使用场景：Ad Hoc 内部测试；TestFlight 外部测试；App Store 正式发布

## MDM 说明
- MDM（移动设备管理）用于企业/机构集中管理设备
- 可远程下发应用、配置与证书，适合大规模设备管理
- 个人/小团队通常不需要


## 追加：Ad Hoc / 免费账号 / AltStore

- Ad Hoc 签名有效期：通常 1 年（依赖 iOS Distribution 证书与 Ad Hoc 描述文件）
- 免费账号签名：
  - 只能做开发调试签名，不能用于 Ad Hoc/TestFlight/App Store 分发
  - 有效期短（常见 7 天），到期需重新签
  - 设备数量与可装 App 数量有限
- AltStore 原理：
  - 使用免费账号创建开发证书与描述文件
  - 对 IPA 做开发调试签名并侧载
  - 需周期性刷新（约 7 天），最多 3 个激活应用（含 AltStore）


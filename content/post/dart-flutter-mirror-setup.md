+++
title = 'Dart & Flutter 镜像加速配置'
date = '2025-03-29T17:52:00+08:00'
draft = false
tags = ['Dart', 'Flutter', '镜像源']
categories = ['技术']
+++

### 核心镜像变量配置

Dart 和 Flutter 依赖两个关键环境变量实现加速，所有操作系统均需配置以下内容（任选一个镜像站）：

#### 推荐镜像源列表

1. **Flutter社区镜像**（CFUG 官方维护，稳定性优先）
   ```bash
   PUB_HOSTED_URL=https://pub.flutter-io.cn
   FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
   ```

2. **清华大学 TUNA 镜像**（同步频率高，支持多协议）
   ```bash
   PUB_HOSTED_URL=https://mirrors.tuna.tsinghua.edu.cn/dart-pub
   ```
   > **注**：TUNA 目前仅提供 Dart Pub 镜像，未同步 Flutter 存储（`flutter_infra`），故 `FLUTTER_STORAGE_BASE_URL` 暂不可用。如需 Flutter SDK 下载加速请用其他镜像源。

3. **上海交大 SJTUG 镜像**（实时同步，回源策略完善）
   ```bash
   PUB_HOSTED_URL=https://mirror.sjtu.edu.cn/dart-pub
   FLUTTER_STORAGE_BASE_URL=https://mirror.sjtu.edu.cn
   ```
   > 官方推荐使用思源服务器 `mirror.sjtu.edu.cn`。致远服务器 `mirrors.sjtug.sjtu.edu.cn` 亦可使用，但地址结构略有不同。

以上镜像源信息源自 [Flutter 官方中文文档 - 在中国网络环境下使用 Flutter](https://docs.flutter.cn/community/china/)，建议定期查阅以获取最新镜像列表。

### 操作系统配置步骤

#### Windows 系统

1. **临时生效（当前会话窗口）**

   打开PowerShell或CMD：

   ```powershell
   $env:PUB_HOSTED_URL="镜像地址"
   $env:FLUTTER_STORAGE_BASE_URL="镜像地址"
   ```

2. **永久生效**
   `[Environment]::SetEnvironmentVariable("PUB_HOSTED_URL", "镜像地址", "Machine")`
   `[Environment]::SetEnvironmentVariable("FLUTTER_STORAGE_BASE_URL", "镜像地址", "Machine")`

#### macOS / Linux 系统

1. **临时生效（当前会话窗口）**
   ```bash
   export PUB_HOSTED_URL="镜像地址"
   export FLUTTER_STORAGE_BASE_URL="镜像地址"
   ```

2. **永久生效**
   - 编辑Shell配置文件（如 `~/.bashrc`、`~/.zshrc`）：
     ```bash
     echo 'export PUB_HOSTED_URL="镜像地址"' >> ~/.bashrc
     echo 'export FLUTTER_STORAGE_BASE_URL="镜像地址"' >> ~/.bashrc
     ```
   - 生效配置：`source ~/.bashrc`

### Flutter SDK 下载加速

若需手动下载 SDK，将原始 URL 中的 `storage.googleapis.com` 替换为镜像域名。例如：

- **原始URL**：
  ```
  https://storage.googleapis.com/flutter_infra_release/releases/stable/windows/flutter_windows_v3.13.0-stable.zip
  ```
  
- **镜像URL**（以 Flutter 社区为例）：
  
  ```
  https://storage.flutter-io.cn/flutter_infra_release/releases/stable/windows/flutter_windows_v3.13.0-stable.zip
  ```

### Android Studio 相关配置

1. **Gradle镜像加速**

   在项目根目录的 `build.gradle` 中添加阿里云仓库：

   ```groovy
   buildscript {
     repositories {
       maven { url 'https://maven.aliyun.com/repository/google' }
       maven { url 'https://maven.aliyun.com/repository/central' }
     }
   }
   ```

2. **Android SDK镜像**

   - 打开 Android Studio → Settings → Android SDK → SDK Update Sites
   - 添加镜像源（如清华源）：
     ```
     https://mirrors.tuna.tsinghua.edu.cn/android/repository/
     ```

### 验证配置

1. 执行 `flutter doctor`，检查依赖下载是否正常。
2. 运行 `echo $PUB_HOSTED_URL`（Unix）或 `echo %PUB_HOSTED_URL%`（Windows），确认变量已生效。

### 注意事项

1. **发布Package时需要恢复默认源**

   发布到 `pub.dev` 前需取消镜像变量，否则会失败：

   ```bash
   unset PUB_HOSTED_URL  # Unix
   Remove-Item Env:\PUB_HOSTED_URL  # PowerShell
   ```

2. **镜像同步延迟**：若遇到依赖版本不一致问题，尝试切换其他镜像源。

### 常见问题

- **镜像失效**：检查镜像站状态页面（如清华镜像状态页），或切换备用镜像。
- **环境变量不生效**：确保变量名无拼写错误，重启终端或IDE。
- **混合开发配置**：若集成到Android原生项目，需同步配置Gradle镜像。

通过以上配置，可显著提升 Dart 包下载和 Flutter SDK 安装速度。更多细节可参考[Flutter中文文档](https://flutter.cn/community/china/)。
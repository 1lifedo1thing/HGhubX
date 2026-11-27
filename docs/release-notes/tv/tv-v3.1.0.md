# HGhubX TV tv-v3.1.0

HGhubX TV 3.1 正式版（`com.hghubx.tv`）：新增片尾下一集预热与底层网络加载提速，并修复不支持 HEVC 硬件解码的电视盒子播放黑屏报错问题。

## 新增与优化
1. **片尾自动预热下一集首片（#299）**：播放临近片尾时自动并发预热下一集 HLS 播放列表与首片，遥控切集与自动连播接近秒开；切换线路或代理时自动取消旧预热任务。
2. **网络复用与视频加载提速（#303）**：底层启用 HTTP/2 连接复用、延长客户端连接池生命周期、缓存系统代理探测、合并并发 HLS 清单请求并将封面解密移至后台线程池，降低高负载播放与封面刷新的卡顿。

## 修复
1. **TV 端 HEVC 解码失败自动回退默认线路（#286、#296）**：不支持 HEVC MSE 解码的 32 位 ARM 电视盒子进入播放即黑屏报「视频解码异常，请重试」 -> 检测到 HEVC 报错且存在默认线路时自动保留当前进度切回默认线路、持久化线路偏好并展示非阻塞提示。

## 安装与兼容提示
- Android TV 按 ABI 分包提供 `HGhubX-TV_3.1.0_arm64.apk`、`HGhubX-TV_3.1.0_arm.apk`、`HGhubX-TV_3.1.0_x86_64.apk`（`com.hghubx.tv`，`versionCode=3001000`）。
- 沿用既有签名证书与版本序列，支持从 `tv-v3.0.0` 直接覆盖安装并保留已导入视频源、本机片库、观看记录、启动 PIN 与偏好设置。

## 致谢
- 感谢 `@zxsos`、`@makisekurisu-jp` 与社区电视盒子用户的测试反馈。

## 验证
- GitHub Actions `TV CI`（run `46000415580`）与 `CI`（run `46000415559`，含 `Rust check and test`）全部通过。
- 本地完成 `VITE_APP_EDITION=tv pnpm build`、`check-version-sync`（`3.1.0` / `versionCode=3001000`）、`check-i18n`、`check-doc-links`、`check-size-budgets`、`check-shell-purity`；签名 APK 产物由 GitHub Actions `TV Release` 工作流构建发布。

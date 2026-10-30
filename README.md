# Windows 安装版与在线更新

这是基于 [hicccc77/WeFlow](https://github.com/hicccc77/WeFlow) 的 Windows x64 定制预览版，非原作者官方发行版。
保留原作者 cc 的署名及 CC BY-NC-SA 4.0 许可；第三方组件遵循包内各自许可。

## 用户使用

首次下载安装 `WeFlow-Windows-x64-版本号-Setup.exe`，退出旧的解压版后再启动安装版。
安装后从桌面 WeFlow 快捷方式启动。在“设置 → 更新”可手动检查更新，启动时也会自动检查。
发现新版后点击“下载并重启安装”；下载前请完成导出等正在进行的操作。
后续无需再接收或解压整个文件夹，主程序、密钥工具、WCDB 读取 DLL 一起更新。
已有用户配置与 LocalAppData 下的加密密钥环不在安装目录内；安装器不删除它们。
使用其他 Windows 用户账号运行时，需要重新配置。

网络失败会显示错误，不会显示为“最新版”；GitHub 无法访问时，请稍后重试。
当前安装包未进行 Windows 代码签名。Windows 安装、自动替换及微信 4.2.0.9 实机读取仍待验证。

## 维护者发布

固定仓库：`LumingMelody/WeFlow-Windows`。这与上游发布渠道隔离，不会切回上游版本。
每次发布必须递增稳定版本号（例如 5.0.1 → 5.0.2），不要替换已发布的同版本安装包。

```sh
npm run typecheck
node scripts/clean-dist-electron.cjs
npx vite build
WEFLOW_RELEASE_VERSION=5.0.2 node scripts/package-windows-gui.cjs --installer
node scripts/check-windows-gui.cjs
node scripts/check-windows-release.cjs
node scripts/publish-windows-release.cjs --notes docs/windows-release-notes.md
```

发布脚本先创建草稿，上传并校验安装包、blockmap、latest.yml，再公开发布。
`latest.yml` 包含版本与 SHA-512，electron-updater 校验下载内容；差分下载失败会回退到完整下载。
Windows 原生安装与二次升级应在真实 Windows 电脑验收，不能以交叉编译代替。

# 包装模式说明

## 目标

把第三方软件先“包装”到你自己的 GitHub Release，再由 Scoop bucket 引用这些稳定的发布资产。

这套模式适合：

- 官网下载链接经常变
- 下载需要中转或你想做镜像
- 你需要统一命名、统一目录结构、统一 hash 管理
- 你希望 `checkver` 和 `autoupdate` 都走你自己的 GitHub Release

## 推荐结构

不要把包装产物直接塞进 `scoop-bucket` 仓库。推荐这样分层：

1. 包装仓库

例如：

- `boxi-wangji/QuickClipboard-Package`
- `boxi-wangji/GoogleChrome-Package`
- `boxi-wangji/Bandizip-Professional-Package`

职责：

- 存放原始安装包或重打包脚本
- 生成统一命名的压缩包或安装包
- 发布到 GitHub Releases

2. Bucket 仓库

例如当前这个：

- `boxi-wangji/scoop-bucket`

职责：

- 只存 `bucket/*.json`
- `url` 指向你包装仓库的 GitHub Release 资产
- `checkver` / `autoupdate` 也走 GitHub Release

## 数据流

1. 你在包装仓库里发布一个版本，例如 `v1.2.3`
2. Release 附件里放统一命名的文件，例如 `QuickClipboard-1.2.3-win64.zip`
3. bucket manifest 引用：

```json
{
  "version": "1.2.3",
  "url": "https://github.com/boxi-wangji/QuickClipboard-Package/releases/download/v1.2.3/QuickClipboard-1.2.3-win64.zip",
  "hash": "..."
}
```

4. Scoop 用户执行：

```powershell
scoop bucket add boxi https://github.com/boxi-wangji/scoop-bucket.git
scoop install boxi/quickclipboard
```

## manifest 写法

推荐优先使用 GitHub Release 直链：

```json
{
  "version": "1.2.3",
  "description": "QuickClipboard repackaged release",
  "homepage": "https://github.com/boxi-wangji/QuickClipboard-Package",
  "license": "Freeware",
  "url": "https://github.com/boxi-wangji/QuickClipboard-Package/releases/download/v1.2.3/QuickClipboard-1.2.3-win64.zip",
  "hash": "sha256-here",
  "extract_dir": "QuickClipboard",
  "bin": "QuickClipboard.exe",
  "shortcuts": [
    [
      "QuickClipboard.exe",
      "QuickClipboard"
    ]
  ],
  "checkver": {
    "github": "https://github.com/boxi-wangji/QuickClipboard-Package"
  },
  "autoupdate": {
    "url": "https://github.com/boxi-wangji/QuickClipboard-Package/releases/download/v$version/QuickClipboard-$version-win64.zip"
  }
}
```

## 包装仓库最低要求

一个包装仓库至少有这些东西：

- `README.md`
- 原始软件说明
- 版本号策略
- GitHub Release

如果你要自动化，再补：

- `.github/workflows/package.yml`
- 打包脚本
- 自动上传 Release 资产

## 你当前仓库适合做什么

当前 `scoop-bucket` 适合继续做：

- bucket 清单维护
- 版本更新 workflow
- hash 自动计算

不适合直接做：

- 存大文件
- 每个软件的打包过程
- 包装源代码和发布文件管理

## 建议落地顺序

1. 先挑一个软件做包装仓库，例如 `QuickClipboard-Package`
2. 手动发第一个 GitHub Release
3. 在 `scoop-bucket/bucket/quickclipboard.json` 引用这个 Release 资产
4. 本机执行 `scoop install boxi/quickclipboard`
5. 跑通后再对第二个、第三个软件复制同一套模式

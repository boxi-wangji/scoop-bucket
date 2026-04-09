# scoop-bucket

Personal Scoop bucket managed by [ScoopManager](https://github.com/boxi-wangji/ScoopManager).

## 使用方法

```powershell
scoop bucket add boxi https://github.com/boxi-wangji/scoop-bucket.git
scoop install boxi/bandizip
scoop install boxi/stranslate
scoop install boxi/quickclipboard
```

## 软件列表

| 名称 | 版本 | 说明 |
|------|------|------|
| bandizip | 7.40 | Bandizip 自定义打包版（由 boxi-wangji 发布） |
| quickclipboard | 0.2.0 | QuickClipboard GitHub Release 包装版 |
| stranslate | 2.0.6 | 免费开源翻译软件 |

## 更新机制

通过 GitHub Actions 工作流 `update.yml` 自动更新版本号、下载链接和哈希值。
由 ScoopManager 桌面应用触发。

## 包装模式

如果你想像 `gendloopBucket` 那样使用“包装仓库 + 第三方桶”的模式：

- 包装仓库负责产出 GitHub Release 附件
- 当前 `scoop-bucket` 只负责引用这些 Release 附件

说明文档见：

- [包装模式说明](C:/scoop-bucket/docs/packaging-mode.md)
- [GitHub Release manifest 模板](C:/scoop-bucket/templates/github-release-manifest.template.json)

# 词芽芽 WordSprout — 手机英语学习卡片

## 主要功能
- 每天按手机本地日期自动切换 10 个词/词组
- 内置 480 个双语词条：日常沟通 160、新能源汽车 160、矿山工程 160
- 每条包括 IPA 音标、中文释义、英文例句、例句中文翻译
- 手机原生英语 TTS 朗读单词和例句，美式/英式英语、语速可调
- 点击「掌握啦」或「再练练」，自动安排隔天/3天/7天/14天/30天/60天间隔复习
- 收藏、搜索、已学词库、7天学习打卡、备份/恢复、新词导入
- 以 HTTPS 发布并成功打开一次后，可通过 Service Worker 缓存页面离线访问（语音取决于手机本地语音包）

## 立即体验
下载单文件 `index.html`，用支持网页脚本的浏览器打开。对于 iPhone，直接在「文件」App 预览 HTML 时，音频和本地持久存储可能受限制；建议通过 HTTPS 网站部署。

## 部署到手机（推荐）
1. 将本 ZIP 中的 `index.html`、`manifest.webmanifest`、`icon.svg`、`service-worker.js` 上传到一个支持静态网页 HTTPS 的主机。
2. 可以使用 GitHub Pages：新建公开仓库，上传以上四个文件；Settings → Pages → Deploy from a branch，选择 main 与根目录。
3. 在手机 Safari / Chrome 访问实际发布的网址。
4. iPhone：Safari → 分享 → 添加到主屏幕；Android：Chrome → 菜单 → 添加到主屏幕/安装应用。
5. 首次访问需联网，之后网页内容可离线打开。建议定期在「设置」里导出学习记录。

## 数据与日期
- 以 2026-10-10 为首日基准安排 48 天的学习表。前 48 天每天 10 个不同词；第 49 天起进入下一轮复习。
- 已学词、待复习词和收藏记录保存在浏览器 localStorage，不会自动同步到其他手机和浏览器。
- 清理浏览器网站数据或无痕模式可能删除进度。
- 浏览器朗读使用 Web Speech API。语音包离线可用性随设备/浏览器而变化。

## 导入额外词组
在「设置 → 导入备份 / 新词」选择 UTF-8 JSON 文件，例如：
```json
{
  "words": [
    {
      "category": "ev",
      "word": "Battery swap",
      "ipa": "/ˈbætəri swɑːp/",
      "zh": "换电",
      "example": "The truck uses a battery swap system.",
      "exampleZh": "这辆卡车采用换电系统。"
    }
  ]
}
```
category 可为 `daily`、`ev` 或 `mining`，默认 `daily`。
注意：新增词条按导入时刻追加到学习安排末尾，已学习日期对应的词条保持不变。

## 附件
- `index.html`：应用主文件（词库和逻辑已内置）
- `icon.svg`：应用图标
- `manifest.webmanifest`：手机桌面安装配置
- `service-worker.js`：离线缓存
- `words.json`：240 条词库，可自行编辑后按导入格式新增词条

## 限制
- 不含云端账号/跨设备自动同步、消息推送、真人语音录音。
- 新词不会自动从云端生成；首版 240 词可用于 48 天连续不重复学习，之后再次轮换。


## iOS 27 语音修复 (v1.0.1)
替换 GitHub 仓库根目录中的所有文件，尤其是 index.html 与 service-worker.js。
部署完成后在 Safari 重新打开，若旧版仍存在，清除该站点的网站数据后重试（清除数据会删除本机学习记录，请先导出备份）。
修复内容：保留 SpeechSynthesisUtterance 引用、过滤 cancel/interrupted 非实际故障、优先本地英语声音、后台恢复、更新缓存版本。
系统语音仍取决于 iOS Safari 的 SpeechSynthesis 支持，无法保证所有设备均正常。

## 升级说明（每日10词版）
- 保留 `wordsprout.local.v1` 本地存储键，已学词、收藏和复习记录不会因为代码更新而被主动清除。
- 每日排课由5词改为10词，原日期对应的词条可能变化；旧打卡记录仍保留。
- 请同时覆盖 `index.html`、`service-worker.js`、`manifest.webmanifest`，防止手机继续显示旧缓存。
- 新增词条音标为基于美式发音词典的自动转写，专业术语的音标建议后续人工校核。

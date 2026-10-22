# Esslinger 定锚批预览宿主

静态预览页 + 反馈 API（仅 Python 3 标准库）。

## 公网地址（当前）

- **主入口（GitHub Pages，可读可批）**: https://jackypang1998.github.io/esslinger-plan-preview/  
  「提交全部反馈」打到下方 API 隧道。
- **反馈 API 隧道（主）**: https://mhvzf-183-156-157-230.free.pinggy.net  
  （Pinggy SSH-over-TLS via corkscrew `127.0.0.1:8791`；serveo/lhr 本窗口 :22 不通）
- **反馈 API 隧道（备）**: https://rifbq-183-156-157-230.run.pinggy-free.link  
  （Pinggy 备用域名）
- 本地：`python3 server.py` → `0.0.0.0:8765`

## 启动

见仓库外 `/workspace/preview-host/README.md`（本机保活宿主）。

## API

- `GET /api/feedback` → 全部已存反馈 JSON
- `POST /api/feedback` body: `{ "sections": { "<id>": { "status", "note" } }, "meta": {} }`
  - 覆盖写入 `data/latest.json`
  - 追加 `data/submissions/YYYYMMDD-HHMMSS.json`
  - CORS `*`

## 整盘操盘如何读反馈

- 最新：宿主 `data/latest.json`
- 历史：宿主 `data/submissions/*.json`
- 或 `GET http://127.0.0.1:8765/api/feedback`
- 隧道挂时：用户从 Pages 复制 Markdown / Issue / 聊天粘贴

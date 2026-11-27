# Esslinger 定锚批预览宿主

静态预览页 + 反馈 API（仅 Python 3 标准库）。

## 公网地址（当前）

- **主入口（GitHub Pages，可读可批）**: https://jackypang1998.github.io/esslinger-plan-preview/  
- 用户只打开上面这个地址即可批注；隧道地址会轮换，对用户透明。
- **反馈 API 隧道（当前 API_BASE / Pinggy；Serveo/LHR 本轮失败或不稳）**: https://tjbzq-2a09-bac1-19a0--3dc-6e.run.pinggy-free.link
- 备用 Serveo: （本轮 Connection closed / 无公网 URL）
- 备用 localhost.run: （本轮 Connection closed / 无公网 URL）
- 备用 Pinggy: https://tjbzq-2a09-bac1-19a0--3dc-6e.run.pinggy-free.link

## 启动



其他备选（本沙箱常失败）：Pinggy（Connection closed）、cloudflared（edge 7844/530）、localtunnel（502）。

## API

-  → 全部已存反馈 JSON
-  body: 
  - 覆盖写入 
  - 追加 
  - CORS 

## 整盘操盘如何读反馈

- 最新：
- 历史：
- 或 
- 隧道挂时：用户从 Pages 复制 Markdown / Issue / 聊天粘贴

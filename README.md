# Esslinger 定锚批预览宿主

静态预览页 + 反馈 API（仅 Python 3 标准库）。

## 公网地址（当前）

- **主入口（GitHub Pages，可读可批）**: https://jackypang1998.github.io/esslinger-plan-preview/  
- 用户只打开上面这个地址即可批注；隧道地址会轮换，对用户透明。
- **反馈 API 隧道（当前 API_BASE / Serveo）**: https://02aa17fce4387056-122-231-197-94.serveousercontent.com
- 备用 Serveo: https://02aa17fce4387056-122-231-197-94.serveousercontent.com
- 备用 localhost.run: https://6d82ff5667a054.app.lhr.life
- 备用 Pinggy: https://vsuqv-122-231-197-94.run.pinggy-free.link

## 启动

其他备选（本沙箱常失败）：cloudflared（edge 7844/530）、localtunnel（502）。

## API

- GET /api/feedback → 全部已存反馈 JSON
- POST /api/feedback body: JSON
  - 覆盖写入 / 追加
  - CORS

## 整盘操盘如何读反馈

- 最新：data/latest.json
- 历史：data/submissions/
- 隧道挂时：用户从 Pages 复制 Markdown / Issue / 聊天粘贴

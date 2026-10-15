# Esslinger 定锚批预览宿主

静态预览页 + 反馈 API（仅 Python 3 标准库）。

## 公网地址（当前）

- **主入口（GitHub Pages，可读可批）**: https://jackypang1998.github.io/esslinger-plan-preview/  
  「提交全部反馈」：若 API 隧道不可用，会**自动复制 Markdown**并提示「复制成功，发回聊天」；也可导出文件 / 开 Issue / mailto。
- **反馈 API 隧道**: 当前不可用（Pinggy Connection closed / cloudflared 7844 / localtunnel 不通）。本地 `python3 server.py` 仍在 `:8765`。

## 启动

```bash
# 1) API + 静态
cd /workspace/preview-host
python3 server.py   # 0.0.0.0:8765

# 2) 公网隧道（Pinggy，经沙箱 HTTP 代理 443）— 若失败见备选
export DISPLAY=:99 SSH_ASKPASS=/workspace/preview-host/empty_askpass.sh SSH_ASKPASS_REQUIRE=force
setsid ssh -n \
  -o StrictHostKeyChecking=accept-new \
  -o UserKnownHostsFile=/tmp/kh_pg3 \
  -o ServerAliveInterval=30 \
  -o ProxyCommand="corkscrew 127.0.0.1 8791 %h %p" \
  -p 443 -R0:127.0.0.1:8765 a.pinggy.io \
  > /workspace/preview-host/tunnel.log 2>&1 &
```

备选（本沙箱常失败）：

```bash
./cloudflared tunnel --url http://127.0.0.1:8765 --protocol http2   # 常因 7844
npx --yes localtunnel --port 8765                                     # 常 502/不通
```

## API

- `GET /api/feedback` → 全部已存反馈 JSON
- `POST /api/feedback` body: `{ "sections": { "<id>": { "status", "note" } }, "meta": {} }`
  - 覆盖写入 `data/latest.json`
  - 追加 `data/submissions/YYYYMMDD-HHMMSS.json`
  - CORS `*`

## 整盘操盘如何读反馈

- 最新：`/workspace/preview-host/data/latest.json`
- 历史：`/workspace/preview-host/data/submissions/*.json`
- 或 `GET http://127.0.0.1:8765/api/feedback`
- 隧道挂时：用户从 Pages 复制 Markdown / Issue / 聊天粘贴

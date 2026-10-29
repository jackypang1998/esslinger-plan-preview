# Esslinger 定锚批预览宿主

静态预览页 + 反馈 API（仅 Python 3 标准库）。

## 公网地址（当前）

- **主入口（GitHub Pages，可读可批）**: https://jackypang1998.github.io/esslinger-plan-preview/  
  「提交全部反馈」打到下方 API 隧道。
- **反馈 API 隧道（主）**: https://gsunk-140-248-50-62.free.pinggy.net  
  （Pinggy TLS；serveo/lhr 本轮 egress Pingora 400；外网 GET/POST 200）
- **反馈 API 隧道（备）**: https://nzrli-140-248-50-62.run.pinggy-free.link
- 本地：`python3 server.py` → `0.0.0.0:8765`



## 启动

```bash
# 1) API + 静态
cd /workspace/preview-host
python3 server.py   # 0.0.0.0:8765


# 2b) 公网隧道（Pinggy，当前可用：SSH-over-TLS via corkscrew）
export DISPLAY=:99 SSH_ASKPASS=/workspace/preview-host/empty_askpass.sh SSH_ASKPASS_REQUIRE=force
setsid ssh -n \
  -o StrictHostKeyChecking=accept-new \
  -o UserKnownHostsFile=/tmp/kh_pinggy \
  -o ServerAliveInterval=15 \
  -o ProxyCommand="python3 -u /workspace/preview-host/pinggy_proxy.py" \
  -R 0:127.0.0.1:8765 \
  -o ExitOnForwardFailure=yes \
  a.pinggy.io \
  > /workspace/preview-host/tunnel-pinggy.log 2>&1 &

# 2) 公网隧道（serveo，本窗口 :22 常不通）
export DISPLAY=:99 SSH_ASKPASS=/workspace/preview-host/empty_askpass.sh SSH_ASKPASS_REQUIRE=force
setsid ssh -n \
  -o StrictHostKeyChecking=accept-new \
  -o UserKnownHostsFile=/tmp/kh_serveo \
  -o ServerAliveInterval=30 \
  -o ProxyCommand="corkscrew 127.0.0.1 8791 %h %p" \
  -R 80:127.0.0.1:8765 serveo.net \
  > /workspace/preview-host/tunnel.log 2>&1 &

# 备选：localhost.run
setsid ssh -n \
  -o StrictHostKeyChecking=accept-new \
  -o UserKnownHostsFile=/tmp/kh_lhr \
  -o ServerAliveInterval=30 \
  -o ProxyCommand="corkscrew 127.0.0.1 8791 %h %p" \
  -R 80:127.0.0.1:8765 nokey@localhost.run \
  > /workspace/preview-host/tunnel-lhr.log 2>&1 &
```

其他备选（本沙箱常失败）：Pinggy（Connection closed）、cloudflared（edge 7844/530）、localtunnel（502）。

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

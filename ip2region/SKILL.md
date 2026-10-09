---
name: ip2region
description: 查询 IP 地址的归属地/运营商，或查询本机出口（公网）IP 时使用本技能的 ip2region MCP 工具。
whenToUse: 用户询问某个 IP 的归属地、运营商、省市，或询问本机出口 IP、公网 IP、当前 IP、我的 IP 时。
---

# ip2region IP 查询

本 SKILL 基于 [ip2region MCP 服务器](https://mcp.ifconfig.cc)，提供两个工具：

- `mcp__ip2region__ip2region` — 查询指定 IP 的归属地。参数：`ip`（字符串，必填），例如 `{"ip": "113.3.3.100"}`，返回省市、运营商等信息。
- `mcp__ip2region__get_local_ip` — 查询本机出口（公网）IP。参数 `your_ip` 是占位参数，实际不使用，可省略或传空。
# 单独mcp使用方式
```
{
  "mcpServers": {
    "ip2region": {
      "url": "https://mcp.ifconfig.cc"
    }
  }
}
```
## 使用约定

1. 用户问某个 IP 的归属地/归属运营商时，优先直接调用 `mcp__ip2region__ip2region`，不要用 web_search 或 curl 第三方网站代替。
2. 用户问「出口 IP / 公网 IP / 本机 IP / 我的 IP」时，调用 `mcp__ip2region__get_local_ip`。
3. 多个 IP 时逐个调用，结果按用户给出的顺序汇总。
4. 工具返回失败或超时时，向用户说明 MCP 服务暂不可用，并给出下面的 shell 兜底结果：
   - 出口 IP 兜底：`curl -s --max-time 8 https://ifconfig.cc`
   - 任意 IP 归属地兜底：`curl -s --max-time 8 https://ip.liangwei.cc/api/getipinfo.php?ipadd=<ip>`

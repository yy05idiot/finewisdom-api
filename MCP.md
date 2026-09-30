# Finewisdom MCP Server

MCP (Model Context Protocol) 服务器，让 Claude / Cursor / Cline 等 AI 客户端直接调用 Finewisdom 的社媒与电商数据接口。

- **端点**：`https://finewisdom.top/mcp`（Streamable HTTP，无状态）
- **协议**：JSON-RPC 2.0，兼容 `2024-11-05` / `2025-03-26` / `2025-06-18` 三个协议版本
- **工具数**：30 个聚合工具（覆盖 89 个底层接口，按「平台 × 资源类型」聚合，不刷爆 Agent 工具列表）
- **计费**：与 REST API 完全一致 —— 按次计费、成功才扣费、注册赠 20 次

## 快速接入

### Claude Desktop / 任意支持 MCP Headers 的客户端

```json
{
  "mcpServers": {
    "finewisdom": {
      "url": "https://finewisdom.top/mcp",
      "headers": {
        "X-FW-Token": "fw_your_token"
      }
    }
  }
}
```

### Cursor / Cline（mcp.json）

```json
{
  "mcpServers": {
    "finewisdom": {
      "url": "https://finewisdom.top/mcp",
      "headers": {
        "X-FW-Token": "fw_your_token"
      }
    }
  }
}
```

> 没有 Headers 配置位的客户端，可在工具参数里传 `"_token": "fw_your_token"` 作为备用鉴权方式。

### 获取 Token

1. 访问 <https://finewisdom.top> 注册账号（邮箱注册，赠 20 次调用）
2. 控制台「个人中心」复制你的 `fw_` 开头 Token
3. 填入上面配置的 `X-FW-Token` Header

## 工具清单

| 类别 | 工具 | 说明 |
|---|---|---|
| 抖音 | `douyin_video` | 视频详情（点赞/评论/分享、作者信息） |
| 抖音 | `douyin_search` | 关键词搜索视频 |
| 抖音 | `douyin_video_comments` | 视频评论列表 |
| 抖音 | `douyin_author` | 作者主页信息 |
| 抖音 | `douyin_product` | 商品详情 |
| 电商 | `ecommerce_item` | 淘宝/京东商品详情（platform 参数区分） |
| 电商 | `ecommerce_comments` | 淘宝/京东商品评论 |
| 电商 | `ecommerce_shop` | 店铺在售商品搜索 |
| 1688 | `item_1688` | 商品详情 |
| 1688 | `search_1688` | 关键词搜索 |
| 1688 | `img_search_1688` | 以图搜图 |
| 1688 | `reviews_1688` | 商品评论 |
| 1688 | `company_1688` | 商家/公司详情 |
| 小红书 | `xhs_note_search` / `xhs_note` / `xhs_note_comments` / `xhs_user` | 笔记搜索/详情/评论、用户信息 |
| 微博 | `weibo_post` / `weibo_comments` | 帖子详情、评论 |
| 知乎 | `zhihu_post` / `zhihu_comments` | 回答详情、评论 |
| B站 | `bili_video` | 视频详情 |
| 快手 | `ks_video` | 视频详情 |
| 视频号 | `channels_video` / `channels_comments` | 作品详情（传分享链接）、评论 |
| TikTok | `tiktok_video` / `tiktok_search` | 视频详情、关键词搜索 |
| Instagram | `ig_post` | 帖子详情 |
| YouTube | `yt_video` | 视频详情 |
| Twitter | `tw_post` | 推文详情 |

完整 89 接口目录见 [README](README.md) 或控制台 <https://finewisdom.top>。

## 协议实现说明

- `initialize` → 返回服务器信息与 capabilities
- `tools/list` → 返回 30 个工具及 JSON Schema 参数定义
- `tools/call` → 服务端转发至本机 API Hub（127.0.0.1:5432），**鉴权/限流/扣费/日志全部复用 REST 全链路**，不存在双套计费
- 无状态实现：不支持 GET/SSE 长连接（返回 405），兼容所有 Streamable-HTTP 客户端
- 响应过大自动截断（80KB），提示用更精确参数重试

## 错误排查

| 现象 | 原因 |
|---|---|
| `缺少鉴权 token` | 未配置 X-FW-Token Header，且参数中无 _token |
| `status: preview` | token 额度用完（赠的 20 次耗尽），去控制台充值 |
| `认证失败: Token已过期` | 到控制台查看 Token 有效期 |
| `isError: true` 且内容含 HTTP 4xx | 参数问题，对照 tools/list 返回的 inputSchema 检查 |

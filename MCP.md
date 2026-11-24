# Finewisdom MCP Server

MCP (Model Context Protocol) 服务器，让 Claude / Cursor / Cline 等 AI 客户端直接调用 Finewisdom 的社媒与电商数据接口。

> **官方 MCP Registry 已收录**：`io.github.yy05idiot/finewisdom`（[查询收录状态](https://registry.modelcontextprotocol.io/v0.1/servers?search=io.github.yy05idiot/finewisdom)，状态 active，发布 2026-09-30）

- **端点**：`https://finewisdom.top/mcp`（Streamable HTTP，无状态）
- **协议**：JSON-RPC 2.0，兼容 `2024-11-05` / `2025-03-26` / `2025-06-18` 三个协议版本
- **工具数**：103 个独立工具（每个 REST 接口一一对应一个 MCP 工具，`tools/list` 可全量发现）
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

103 个工具，与 REST 接口一一对应。按平台速览：

- **抖音（13）**: `douyin_author_info`、`douyin_author_search`、`douyin_keyword_search`、`douyin_product_comments`、`douyin_product_detail`、`douyin_product_detail_v2`、`douyin_product_detail_v3`、`douyin_product_keyword_search`、`douyin_shop_search`、`douyin_video_comments`、`douyin_video_detail`、`douyin_video_like_count`、`douyin_video_play_count`
- **B站（7）**: `bili_hot_search`、`bili_keyword_search`、`bili_post_comments`、`bili_post_detail`、`bili_sub_comments`、`bili_user_info`、`bili_user_posts`
- **Instagram（7）**: `ig_keyword_search`、`ig_post_comments`、`ig_post_detail`、`ig_shortcode_to_media_id`、`ig_sub_comments`、`ig_user_info`、`ig_user_posts`
- **微博（7）**: `weibo_hot_search`、`weibo_keyword_search`、`weibo_post_comments`、`weibo_post_detail`、`weibo_sub_comments`、`weibo_user_info`、`weibo_user_posts`
- **快手（7）**: `ks_hot_list`、`ks_keyword_search`、`ks_post_comments`、`ks_post_detail`、`ks_sub_comments`、`ks_user_info`、`ks_user_posts`
- **知乎（7）**: `zhihu_hot_list`、`zhihu_keyword_search`、`zhihu_post_comments`、`zhihu_post_detail`、`zhihu_sub_comments`、`zhihu_user_info`、`zhihu_user_posts`
- **TikTok（13）**: `tiktok_keyword_search`、`tiktok_post_comments`、`tiktok_post_detail`、`tiktok_shop_hot_products`、`tiktok_shop_categories`、`tiktok_shop_category_products`、`tiktok_shop_product_search`、`tiktok_shop_seller_products`、`tiktok_shop_product_detail`、`tiktok_shop_product_reviews`、`tiktok_sub_comments`、`tiktok_user_info`、`tiktok_user_posts`
- **Twitter/X（6）**: `tw_keyword_search`、`tw_post_comments`、`tw_post_detail`、`tw_trending`、`tw_user_info`、`tw_user_posts`
- **YouTube（6）**: `yt_keyword_search`、`yt_post_comments`、`yt_post_detail`、`yt_sub_comments`、`yt_user_info`、`yt_user_posts`
- **小红书（6）**: `xhs_note_comments`、`xhs_note_detail`、`xhs_note_search`、`xhs_sub_comments`、`xhs_user_info`、`xhs_user_notes`
- **视频号（7）**: `channels_post_comments`、`channels_post_detail`、`channels_search_in_channel`、`channels_sub_comments`、`channels_user_info`、`channels_user_posts`、`channels_search_videos`
- **微信公众号（4）**: `wechat_mp_article_detail`、`wechat_mp_article_comments`、`wechat_mp_account_articles`、`wechat_mp_account_profile`
- **1688（5）**: `item_get_1688`、`item_get_company_1688`、`item_review_1688`、`item_search_1688`、`item_search_img_1688`
- **淘宝（4）**: `taobao_comments`、`taobao_item_get`、`taobao_original_detail`、`taobao_shop_search`
- **京东（4）**: `jd_comments`、`jd_comments_v2`、`jd_item_get`、`jd_shop_search`

每个工具的参数与计费说明通过 `tools/list` 返回，完整目录见 [README](README.md)。

## 协议实现说明

- `initialize` → 返回服务器信息与 capabilities
- `tools/list` → 返回 103 个工具及 JSON Schema 参数定义
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

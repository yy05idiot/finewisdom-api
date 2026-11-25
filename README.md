# Finewisdom API

> 社媒 + 电商数据统一接口平台 —— 一个 Token，104 个接口，覆盖国内外主流平台。

[![Website](https://img.shields.io/badge/Website-finewisdom.top-blue)](https://finewisdom.top) [![Console](https://img.shields.io/badge/Console-%E6%8E%A7%E5%88%B6%E5%8F%B0-2ea44f)](https://finewisdom.top) [![MCP](https://img.shields.io/badge/MCP-finewisdom.top%2Fmcp-8a2be2)](MCP.md) [![Protocol](https://img.shields.io/badge/Protocol-REST%20GET-orange)](#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B) [![Finewisdom MCP server on Glama](https://glama.ai/mcp/connectors/io.github.yy05idiot/finewisdom/badges/score.svg)](https://glama.ai/mcp/connectors/io.github.yy05idiot/finewisdom)

## 平台覆盖

| 类别 | 平台 | 接口数 | 典型场景 |
|---|---|---|---|
| 抖音 | 抖音 | 13 | 视频/评论/达人/商品/店铺全链路（含图搜数据源） |
| B站 | B站 | 7 | 视频/评论/用户/关键词搜索/热搜 |
| Instagram | Instagram | 7 | 帖子/评论/用户/搜索/短码转换 |
| 微博 | 微博 | 7 | 帖子/评论/用户/关键词搜索/热搜 |
| 快手 | 快手 | 7 | 视频/评论/用户/搜索/热榜 |
| 知乎 | 知乎 | 7 | 帖子/评论/用户/关键词搜索/热榜 |
| TikTok | TikTok | 13 | 视频/评论/用户/搜索 + TikTok Shop 选品 |
| Twitter/X | Twitter/X | 6 | 推文/评论/用户/搜索/趋势 |
| YouTube | YouTube | 6 | 视频/评论/用户/频道视频/搜索 |
| 小红书 | 小红书 | 6 | 笔记搜索/详情/评论/用户/用户笔记 |
| 视频号 | 视频号 | 7 | 作品/评论/用户/号内搜/搜索视频 |
| 微信公众号 | 微信公众号 | 5 | 文章详情/阅读量/评论/账号文章/账号资料 |
| 1688 | 1688 | 5 | 商品详情/评论/搜索/以图搜图/公司信息 |
| 淘宝 | 淘宝 | 4 | 商品详情/评论/店铺搜索/原始详情 |
| 京东 | 京东 | 4 | 商品详情/评论/店铺搜索 |

**共 104 个在线接口**（REST 与 MCP 同一清单）。支持 REST GET 与 [MCP 协议](MCP.md)（AI Agent 直连：`https://finewisdom.top/mcp`）。

## 快速开始

1. 注册账号：访问 [finewisdom.top](https://finewisdom.top) 邮箱注册，自动获得 Token（赠 20 次调用额度）
2. 调用接口（纯 GET，无 SDK 依赖）：

```bash
curl "https://finewisdom.top/api?token=YOUR_TOKEN&method=douyin_video_detail&aweme_id=7624566919911230720"
```

3. 响应统一格式：

```json
{
  "status": "success",
  "message": "API调用成功",
  "data": { "...": "各接口具体结构见控制台示例" }
}
```

> 游客可在控制台用 `token=guest` 预览接口目录与示例响应（不扣费、不可真实调用）。

### 参数规则

- 所有参数以 query string 形式拼接：`&参数名=值`
- 分页类参数：`page`（抖音/淘宝/京东系，从 0 或 1 开始，见各接口说明）、`cursor`/`offset`（社媒接口）
- 计费规则：**仅在调用成功时扣费**，失败自动退回
- 限流：5 次/分钟/IP（突发友好，勿高频并发）

## 接口目录

### 抖音（13 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `douyin_author_info` | 获取抖音作者详细信息 | 0.1 | 必填: sec_user_id |
| `douyin_author_search` | 获取抖音作者的作品（视频）列表，支持游标翻页 | 0.1 | 必填: sec_user_id；可选: cursor |
| `douyin_keyword_search` | 按关键词搜索抖音内容（综合搜索，返回视频列表） | 0.1 | 必填: keyword；可选: page |
| `douyin_product_comments` | 获取抖音商品评论列表 | 0.2 | 必填: product_id；可选: page |
| `douyin_product_detail` | 获取抖音商品详情（v1） | 0.3 | 必填: product_id |
| `douyin_product_detail_v2` | 抖音商品详情 v2（native 通道，直连抖音网页端） | 0.2 | 必填: product_id；可选: sku_id |
| `douyin_product_detail_v3` | 抖音商品详情 v3（字段已归一化） | 0.3 | 必填: product_id |
| `douyin_product_keyword_search` | 按关键词搜索抖音商城商品 | 0.3 | 必填: keyword；可选: page、count |
| `douyin_shop_search` | 搜索抖音店铺商品 | 0.3 | 必填: shop_id；可选: page |
| `douyin_video_comments` | 获取抖音视频的评论列表 | 0.2 | 必填: aweme_id；可选: page |
| `douyin_video_detail` | 获取抖音视频详细信息 | 0.1 | 必填: aweme_id |
| `douyin_video_like_count` | 获取抖音视频点赞数，只返回 aweme_id + like_count 两个字段（单指标轻量接口） | 0.1 | 必填: aweme_id |
| `douyin_video_play_count` | 获取抖音视频播放量，只返回 aweme_id + play_count 两个字段（单指标轻量接口） | 0.1 | 必填: aweme_id |

### B站（7 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `bili_hot_search` | 获取B站热搜词 | 0.1 | 可选: limit |
| `bili_keyword_search` | B站综合搜索 | 0.1 | 必填: keyword；可选: order、page |
| `bili_post_comments` | 获取B站视频评论 | 0.2 | 必填: bv_id；可选: page |
| `bili_post_detail` | 获取B站视频详情 | 0.1 | 必填: bv_id |
| `bili_sub_comments` | 获取B站评论的回复 | 0.2 | 必填: bv_id、rpid |
| `bili_user_info` | 获取B站UP主信息 | 0.1 | 必填: uid |
| `bili_user_posts` | 获取B站UP主投稿列表 | 0.1 | 必填: uid；可选: page |

### Instagram（7 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `ig_keyword_search` | Instagram综合搜索（用户/话题/地点） | 0.3 | 必填: query |
| `ig_post_comments` | 获取Instagram帖子评论（按帖子短码调用） | 0.1 | 必填: code；可选: min_id |
| `ig_post_detail` | 按shortcode获取Instagram帖子详情 | 0.3 | 必填: code |
| `ig_shortcode_to_media_id` | 帖子shortcode转media_id（评论接口前置） | 0.2 | 必填: shortcode |
| `ig_sub_comments` | 获取Instagram评论的子评论 | 0.3 | 必填: media_id、comment_id |
| `ig_user_info` | 获取Instagram用户信息 | 0.3 | 必填: username |
| `ig_user_posts` | 获取Instagram用户帖子列表 | 0.3 | 必填: username |

### 微博（7 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `weibo_hot_search` | 获取微博热搜榜 | 0.2 | - |
| `weibo_keyword_search` | 按关键词搜索微博 | 0.1 | 必填: query；可选: page |
| `weibo_post_comments` | 获取微博帖子评论列表 | 0.2 | 必填: post_id；可选: count、max_id |
| `weibo_post_detail` | 获取微博帖子详情（含互动量） | 0.1 | 必填: post_id |
| `weibo_sub_comments` | 获取微博评论的回复列表 | 0.2 | 必填: comment_id |
| `weibo_user_info` | 获取微博用户信息 | 0.1 | 必填: uid |
| `weibo_user_posts` | 获取微博用户帖子列表 | 0.1 | 必填: uid；可选: page |

### 快手（7 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `ks_hot_list` | 获取快手热榜 | 0.1 | - |
| `ks_keyword_search` | 按关键词搜索快手内容 | 0.3 | 必填: keyword；可选: cursor |
| `ks_post_comments` | 获取快手视频评论列表 | 0.2 | 必填: photo_id；可选: cursor |
| `ks_post_detail` | 获取快手视频详情 | 0.2 | 必填: photo_id |
| `ks_sub_comments` | 获取快手评论的回复 | 0.2 | 必填: photo_id、root_comment_id |
| `ks_user_info` | 获取快手用户信息 | 0.2 | 必填: user_id |
| `ks_user_posts` | 获取快手用户视频列表 | 0.3 | 必填: user_id；可选: cursor |

### 知乎（7 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `zhihu_hot_list` | 获取知乎热榜 | 0.1 | - |
| `zhihu_keyword_search` | 按关键词搜索知乎文章 | 0.1 | 必填: keyword；可选: offset、limit |
| `zhihu_post_comments` | 获取知乎回答评论列表 | 0.2 | 必填: answer_id；可选: limit、offset |
| `zhihu_post_detail` | 获取知乎回答详情 | 0.1 | 必填: answer_id |
| `zhihu_sub_comments` | 获取知乎评论的子评论 | 0.2 | 必填: comment_id |
| `zhihu_user_info` | 获取知乎用户信息 | 0.1 | 必填: user_url_token |
| `zhihu_user_posts` | 获取知乎用户的回答列表 | 0.1 | 必填: user_url_token；可选: offset、limit |

### TikTok（13 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `tiktok_keyword_search` | TikTok综合搜索 | 0.1 | 必填: keyword |
| `tiktok_post_comments` | 获取TikTok视频评论 | 0.1 | 必填: item_id；可选: cursor、count |
| `tiktok_post_detail` | 获取TikTok视频详情 | 0.1 | 必填: item_id |
| `tiktok_shop_hot_products` | 获取TikTok商城热卖榜 | 0.2 | 可选: region、count、exclude_product_ids |
| `tiktok_shop_categories` | 获取TikTok商城类目 | 0.2 | 可选: region |
| `tiktok_shop_category_products` | 获取TikTok类目商品 | 0.2 | 必填: category_id；可选: offset、region |
| `tiktok_shop_product_search` | 搜索TikTok商城商品 | 0.2 | 必填: search_word；可选: offset、page_token、region |
| `tiktok_shop_seller_products` | 获取TikTok店铺商品 | 0.2 | 必填: seller_id；可选: search_params、region |
| `tiktok_shop_product_detail` | 获取TikTok商品详情 | 0.2 | 必填: product_id；可选: region |
| `tiktok_shop_product_reviews` | 获取TikTok商品评论 | 0.2 | 必填: product_id；可选: page_start、region |
| `tiktok_sub_comments` | 获取TikTok评论回复 | 0.1 | 必填: item_id、comment_id |
| `tiktok_user_info` | 获取TikTok用户信息 | 0.1 | 可选: unique_id、sec_uid |
| `tiktok_user_posts` | 获取TikTok用户视频列表 | 0.1 | 必填: sec_uid |

### Twitter/X（6 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `tw_keyword_search` | 搜索推文 | 0.1 | 必填: keyword；可选: search_type |
| `tw_post_comments` | 获取推文评论（回复） | 0.1 | 必填: tweet_id；可选: cursor |
| `tw_post_detail` | 获取推文详情 | 0.1 | 必填: tweet_id |
| `tw_trending` | 获取推特趋势话题 | 0.1 | - |
| `tw_user_info` | 获取推特用户信息 | 0.1 | 可选: screen_name、rest_id |
| `tw_user_posts` | 获取用户推文列表（screen_name 与 rest_id 二选一） | 0.1 | 可选: screen_name、rest_id、cursor |

### YouTube（6 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `yt_keyword_search` | 搜索YouTube视频 | 0.1 | 必填: keyword；可选: order_by |
| `yt_post_comments` | 获取YouTube视频评论（返回continuation_token翻页） | 0.1 | 必填: video_id |
| `yt_post_detail` | 获取YouTube视频详情 | 0.1 | 必填: video_id |
| `yt_sub_comments` | YouTube评论翻页与回复 | 0.1 | 必填: continuation_token |
| `yt_user_info` | 获取YouTube频道信息 | 0.1 | 必填: channel_id |
| `yt_user_posts` | 获取YouTube频道视频列表 | 0.1 | 必填: channel_id |

### 小红书（6 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `xhs_note_comments` | 获取小红书笔记的一级评论列表 | 0.2 | 必填: note_id；可选: start、sort_strategy |
| `xhs_note_detail` | 获取小红书笔记详细信息，包括标题、内容、图片、视频等 | 0.2 | 必填: note_id |
| `xhs_note_search` | 通过关键词搜索小红书笔记，支持排序和筛选 | 0.2 | 必填: keyword；可选: page、sort_type、note_type、note_time |
| `xhs_sub_comments` | 获取某条评论下的二级评论列表 | 0.2 | 必填: note_id、comment_id；可选: start |
| `xhs_user_info` | 获取小红书用户信息，包括粉丝数、笔记数、获赞数等 | 0.2 | 必填: user_id |
| `xhs_user_notes` | 获取用户发布的笔记列表，支持翻页 | 0.2 | 必填: userid；可选: cursor |

### 视频号（7 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `channels_post_comments` | 获取视频号作品评论 | 0.2 | 必填: object_id；可选: comment_id、last_buffer |
| `channels_post_detail` | 获取微信视频号作品详情 | 0.3 | 可选: share_url、object_id |
| `channels_search_in_channel` | 在视频号账号内搜索作品 | 0.3 | 必填: username、keyword |
| `channels_sub_comments` | 获取视频号评论的回复 | 0.2 | 必填: object_id、comment_id |
| `channels_user_info` | 获取视频号账号资料 | 0.3 | 必填: username |
| `channels_user_posts` | 获取视频号账号作品列表 | 0.3 | 必填: username；可选: last_buffer |
| `channels_search_videos` | 搜索视频号视频（微信搜一搜 video 垂类，不是号内搜索） | 0.2 | 必填: keyword；可选: sort、publish_time、duration、cursor、offset、raw |

### 微信公众号（5 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `wechat_mp_article_detail` | 获取微信公众号文章详情（H5 版，推荐） | 0.2 | 必填: url；可选: raw、h5 |
| `wechat_mp_article_stats` | 获取微信公众号文章阅读量/点赞/在看/分享/收藏/评论数 | 0.2 | 必填: url；可选: h5、raw |
| `wechat_mp_article_comments` | 获取微信公众号文章的一级评论（单页，buffer 游标翻页） | 0.2 | 必填: url；可选: buffer、comment_id、raw |
| `wechat_mp_account_articles` | 获取微信公众号历史发文列表（文章 tab，单页） | 0.2 | 必填: username；可选: offset、item_show_type、page_size、raw |
| `wechat_mp_account_profile` | 获取微信公众号资料页 | 0.2 | 必填: username；可选: raw |

### 1688（5 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `item_get_1688` | 获取1688商品详情 | 0.2 | 必填: item_id |
| `item_get_company_1688` | 获取1688商家信息 | 0.2 | 必填: seller_id |
| `item_review_1688` | 获取1688商品评论 | 0.2 | 必填: item_id、seller_nick；可选: page |
| `item_search_1688` | 按关键词搜索1688商品 | 0.2 | 必填: keyword；可选: page |
| `item_search_img_1688` | 通过图片搜索1688商品 | 0.2 | 必填: image_url |

### 淘宝（4 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `taobao_comments` | 获取淘宝商品评论列表 | 0.2 | 必填: item_id；可选: page |
| `taobao_item_get` | 获取淘宝商品详情信息，含价格与优惠口径 | 0.1 | 必填: item_id |
| `taobao_original_detail` | 获取淘宝券前详情（不含券后价、券面额、佣金） | 0.1 | 必填: item_id |
| `taobao_shop_search` | 搜索淘宝店铺商品列表 | 0.1 | 可选: seller_id、page、shop_id、shop_name |

### 京东（4 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `jd_comments` | 获取京东商品评论列表 | - | 必填: item_id；可选: page |
| `jd_comments_v2` | 获取京东商品评论列表（按时间倒序，每页固定 10 条 | - | 必填: item_id；可选: page |
| `jd_item_get` | 获取京东商品详情信息（不含券后价、券面额、佣金等，建议只用来获取标题图片等信息） | 0.1 | 必填: item_id |
| `jd_shop_search` | 搜索京东店铺商品列表 | 0.2 | 可选: seller_id、page、shop_id |

## 计费与充值

- 按次计费（0.1 ~ 0.3 元/次为主），注册即送 20 次
- 充值：控制台「在线充值」页微信扫码转账 → 提交单号 → 自动到账
- 工单支持：控制台内提交，人工响应

## 使用条款

- 仅限合法合规的数据研究与分析用途，禁止任何违反目标平台服务条款或当地法律法规的用途
- 禁止将数据用于转售、骚扰、爬取攻击等场景；违者封禁不退款

## 联系方式

- 控制台：https://finewisdom.top（注册 / 游客预览 / 文档 / 充值 / 工单）
- 邮箱：17664744@qq.com


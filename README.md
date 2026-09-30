# Finewisdom API

> 社媒 + 电商数据统一接口平台 —— 一个 Token，90+ 接口，覆盖国内外主流平台。

[![Website](https://img.shields.io/badge/Website-finewisdom.top-blue)](https://finewisdom.top) [![Console](https://img.shields.io/badge/Console-%E6%8E%A7%E5%88%B6%E5%8F%B0-2ea44f)](https://finewisdom.top) [![Protocol](https://img.shields.io/badge/Protocol-REST%20GET-orange)](#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)

## 平台覆盖

| 类别 | 平台 | 接口数 | 典型场景 |
|---|---|---|---|
| 抖音 | 抖音 | 11 | 视频/评论/达人/商品/店铺全链路（免费 cookie 链路） |
| 微博 | 微博 | 7 | 帖子/评论/用户/关键词搜索/热搜 |
| 知乎 | 知乎 | 7 | 帖子/评论/用户/关键词搜索/热榜 |
| B站 | B站 | 7 | 视频/评论/用户/关键词搜索/热搜 |
| 快手 | 快手 | 7 | 视频/评论/用户/搜索/热榜 |
| Instagram | Instagram | 7 | 帖子/评论/用户/搜索/短码转换 |
| 小红书 | 小红书 | 6 | 笔记搜索/详情/评论/用户/用户笔记 |
| 视频号 | 视频号 | 6 | 作品/评论/用户/号内搜 |
| TikTok | TikTok | 6 | 视频/评论/用户/作者视频/搜索 |
| YouTube | YouTube | 6 | 视频/评论/用户/频道视频/搜索 |
| Twitter | Twitter | 6 | 推文/评论/用户/搜索/趋势 |
| 淘宝 | 淘宝 | 5 | 商品详情/评论/店铺搜索/原始详情 |
| 1688 | 1688 | 5 | 商品详情/评论/搜索/以图搜图/公司信息 |
| 京东 | 京东 | 4 | 商品详情/评论/店铺搜索/图文描述 |

**共 90 个在线接口**。

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
- 分页类参数：`page`（抖音/淘宝/京东系，从 0 或 1 开始，见各接口说明）、`cursor`/`offset`（TikHub 系）
- 计费规则：**仅在调用成功时扣费**，失败自动退回
- 限流：5 次/分钟/IP（突发友好，勿高频并发）

## 接口目录

### 抖音（11 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `douyin_video_comments` | 抖音视频评论 | 0.2 | 必填: aweme_id；可选: page |
| `douyin_video_detail` | 抖音视频详情 | 0.1 | 必填: aweme_id |
| `douyin_keyword_search` | 抖音关键词搜索 | 0.1 | 必填: keyword；可选: page |
| `douyin_shop_search` | 抖音店铺搜索 | 0.3 | 必填: shop_id；可选: page |
| `douyin_product_comments` | 抖音商品评论 | 0.2 | 必填: product_id；可选: page |
| `douyin_product_detail` | 抖音商品详情 | 0.3 | 必填: product_id |
| `douyin_product_keyword_search` | 抖音商品搜索 | 0.3 | 必填: keyword；可选: page, count |
| `douyin_video_play_count` | 抖音播放量 | 0.1 | 必填: aweme_id |
| `douyin_video_like_count` | 抖音点赞数 | 0.1 | 必填: aweme_id |
| `douyin_author_info` | 抖音作者信息 | 0.1 | 必填: sec_user_id |
| `douyin_author_search` | 抖音作者搜索 | 0.2 | 必填: sec_user_id；可选: cursor |

### 微博（7 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `weibo_post_detail` | 微博帖子详情 | 0.1 | 必填: post_id |
| `weibo_post_comments` | 微博帖子评论 | 0.2 | 必填: post_id；可选: count, max_id |
| `weibo_sub_comments` | 微博评论回复 | 0.2 | 必填: comment_id |
| `weibo_user_info` | 微博用户详情 | 0.1 | 必填: uid |
| `weibo_user_posts` | 微博用户作品 | 0.1 | 必填: uid；可选: page |
| `weibo_keyword_search` | 微博关键词搜索 | 0.1 | 必填: query；可选: page |
| `weibo_hot_search` | 微博热搜榜 | 0.2 | - |

### 知乎（7 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `zhihu_post_detail` | 知乎回答详情 | 0.1 | 必填: answer_id |
| `zhihu_post_comments` | 知乎评论 | 0.2 | 必填: answer_id；可选: limit, offset |
| `zhihu_sub_comments` | 知乎子评论 | 0.2 | 必填: comment_id |
| `zhihu_user_info` | 知乎用户详情 | 0.1 | 必填: user_url_token |
| `zhihu_user_posts` | 知乎用户回答列表 | 0.1 | 必填: user_url_token；可选: offset, limit |
| `zhihu_keyword_search` | 知乎关键词搜索 | 0.1 | 必填: keyword；可选: offset, limit |
| `zhihu_hot_list` | 知乎热榜 | 0.1 | - |

### B站（7 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `bili_post_detail` | B站视频详情 | 0.1 | 必填: bv_id |
| `bili_post_comments` | B站视频评论 | 0.2 | 必填: bv_id；可选: page |
| `bili_sub_comments` | B站评论回复 | 0.2 | 必填: bv_id, rpid |
| `bili_user_info` | B站用户详情 | 0.1 | 必填: uid |
| `bili_user_posts` | B站用户作品 | 0.1 | 必填: uid；可选: page |
| `bili_keyword_search` | B站关键词搜索 | 0.1 | 必填: keyword；可选: order, page |
| `bili_hot_search` | B站热搜 | 0.1 | 可选: limit |

### 快手（7 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `ks_post_detail` | 快手视频详情 | 0.2 | 必填: photo_id |
| `ks_post_comments` | 快手视频评论 | 0.2 | 必填: photo_id；可选: cursor |
| `ks_sub_comments` | 快手评论回复 | 0.2 | 必填: photo_id, root_comment_id |
| `ks_user_info` | 快手用户详情 | 0.2 | 必填: user_id |
| `ks_user_posts` | 快手用户作品 | 0.3 | 必填: user_id；可选: cursor |
| `ks_keyword_search` | 快手关键词搜索 | 0.3 | 必填: keyword；可选: cursor |
| `ks_hot_list` | 快手热榜 | 0.1 | - |

### Instagram（7 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `ig_post_detail` | IG帖子详情 | 0.3 | 必填: code |
| `ig_post_comments` | IG帖子评论 | 0.1 | 必填: media_id |
| `ig_sub_comments` | IG子评论 | 0.3 | 必填: media_id, comment_id |
| `ig_user_info` | IG用户详情 | 0.3 | 必填: username |
| `ig_user_posts` | IG用户帖子 | 0.3 | 必填: username |
| `ig_keyword_search` | IG综合搜索 | 0.3 | 必填: query |
| `ig_shortcode_to_media_id` | IG短码转换 | 0.2 | 必填: shortcode |

### 小红书（6 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `xhs_note_search` | 小红书笔记搜索 | 0.2 | 必填: keyword；可选: page, sort_type, note_type, note_time |
| `xhs_note_detail` | 小红书笔记详情 | 0.2 | 必填: note_id |
| `xhs_note_comments` | 小红书笔记评论 | 0.2 | 必填: note_id；可选: start, sort_strategy |
| `xhs_sub_comments` | 小红书二级评论 | 0.2 | 必填: note_id, comment_id；可选: start |
| `xhs_user_info` | 小红书用户信息 | 0.2 | 必填: user_id |
| `xhs_user_notes` | 小红书用户笔记列表 | 0.2 | 必填: user_id；可选: cursor |

### 视频号（6 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `channels_post_detail` | 视频号作品详情 | 0.3 | 可选: share_url, object_id |
| `channels_post_comments` | 视频号评论 | 0.2 | 必填: object_id；可选: comment_id, last_buffer |
| `channels_sub_comments` | 视频号评论回复 | 0.2 | 必填: object_id, comment_id |
| `channels_user_info` | 视频号账号详情 | 0.3 | 必填: username |
| `channels_user_posts` | 视频号账号作品 | 0.3 | 必填: username；可选: last_buffer |
| `channels_search_in_channel` | 视频号号内搜索 | 0.3 | 必填: username, keyword |

### TikTok（6 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `tiktok_post_detail` | TikTok作品详情 | 0.1 | 必填: item_id |
| `tiktok_post_comments` | TikTok评论 | 0.1 | 必填: item_id；可选: cursor, count |
| `tiktok_sub_comments` | TikTok评论回复 | 0.1 | 必填: item_id, comment_id |
| `tiktok_user_info` | TikTok用户详情 | 0.1 | 可选: unique_id, sec_uid |
| `tiktok_user_posts` | TikTok用户作品 | 0.1 | 必填: sec_uid |
| `tiktok_keyword_search` | TikTok关键词搜索 | 0.1 | 必填: keyword |

### YouTube（6 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `yt_post_detail` | YouTube视频详情 | 0.1 | 必填: video_id |
| `yt_post_comments` | YouTube评论 | 0.1 | 必填: video_id |
| `yt_sub_comments` | YouTube评论翻页/回复 | 0.1 | 必填: continuation_token |
| `yt_user_info` | YouTube频道详情 | 0.1 | 必填: channel_id |
| `yt_user_posts` | YouTube频道视频 | 0.1 | 必填: channel_id |
| `yt_keyword_search` | YouTube关键词搜索 | 0.1 | 必填: keyword；可选: order_by |

### Twitter（6 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `tw_post_detail` | 推特推文详情 | 0.1 | 必填: tweet_id |
| `tw_post_comments` | 推特评论 | 0.1 | 必填: tweet_id；可选: cursor |
| `tw_user_info` | 推特用户详情 | 0.1 | 可选: screen_name, rest_id |
| `tw_user_posts` | 推特用户推文 | 0.1 | 必填: screen_name |
| `tw_keyword_search` | 推特关键词搜索 | 0.1 | 必填: keyword；可选: search_type |
| `tw_trending` | 推特趋势 | 0.1 | - |

### 淘宝（5 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `taobao_item_get` | 淘宝商品详情 | 0.1 | 必填: item_id |
| `taobao_item_desc` | 淘宝商品描述 | 0.1 | 必填: item_id |
| `taobao_comments` | 淘宝商品评论 | 0.2 | 必填: item_id；可选: page |
| `taobao_shop_search` | 淘宝店铺搜索 | 0.1 | 可选: seller_id, page, shop_id |
| `taobao_original_detail` | 淘宝券前详情 | 0.1 | 必填: item_id |

### 1688（5 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `item_search_img_1688` | 1688图搜 | 0.2 | 必填: image_url |
| `item_get_1688` | 1688商品详情 | 0.2 | 必填: item_id |
| `item_search_1688` | 1688关键词搜索 | 0.2 | 必填: keyword；可选: page |
| `item_get_company_1688` | 1688商家详情 | 0.2 | 必填: seller_id |
| `item_review_1688` | 1688商品评论 | 0.2 | 必填: item_id, seller_nick；可选: page |

### 京东（4 个）

| 接口 | 说明 | 价格(元/次) | 主要参数 |
|---|---|---|---|
| `jd_item_get` | 京东商品详情 | 0.1 | 必填: item_id |
| `jd_item_desc` | 京东商品描述 | 0.1 | 必填: item_id |
| `jd_comments` | 京东商品评论 | 0.3 | 必填: item_id；可选: page |
| `jd_shop_search` | 京东店铺搜索 | 0.2 | 可选: seller_id, page, shop_id |

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

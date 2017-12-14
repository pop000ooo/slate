---
title: API Reference

language_tabs:
  - java
  - lua

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>


search: true
---

# API

craig

# LOGIN

```
curl "http://api.welove.com/login"
```

```json
{
  "user_id": 2,
  "name": "craig",
  "gender": 1,
  "level": 10
}
```

获取用户身份

# HERO

> 接口示例

```
curl "http://api.welove.com/hero"
```

```json
{
  "user_id": 2,
  "name": "craig",
  "gender": 1,
  "level": 10
}
```

英雄信息

## 获取英雄列表

获取5个英雄一拥有

## 英雄升级

升级单个英雄

## 英雄装备

英雄装备物品


# ITEM

物品相关接口

## 物品合成

3个物品合成1个

## 物品升级

物品升星
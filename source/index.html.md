---
title: API Reference

language_tabs:
  - java
  - lua
---

# “我们的冒险”API文档  

## TODO
### >用户可以控制的出招顺序，前端传上来的时候需要重新设置**

# 资源检查
### 接口说明
进入游戏前拉取线上资源列表，和本地比对，发现服务端和本地的MD5与size不同，则下载服务端资源。 
### 接口URI
  /v6/game/hero/resource
### 提交方式
  POST、GET
### 请求参数列表
| Required | Name         | Type   | Description |
| :------- | :----------- | :----- | :---------- |
| required | access_token | string | 用户的token    |
### 返回值
```json
{
    "result": 1,
    "messages": [
        {
            "size": 17738246,
            "msg_type": 1,
            "text": "资源文件",
            "res_name": "res.zip",
            "download_urls": [
                "http://fmn.welove520.com/static/test/res/farm/res-1cfeaccd1.zip"
            ],
            "md5": "8c5402214f537def50e181a5276d67fe"
        }
    ]
}
```
### 数据节点说明
| Name   | Description |
| :----- | :---------- |
| result | 请求结果        |
| size(int)  | 文件大小        |
| md5    | 文件md5       |
| 其他     | 暂时忽略吧       |

# 战队信息
### 接口说明
进入游戏后，拉取战队信息展示给用户 
### 接口URI
  /v6/game/hero/bunch/profile
### 提交方式
  POST、GET
### 请求参数列表
| Required | Name         | Type   | Description |
| :------- | :----------- | :----- | :---------- |
| required | access_token | string | 用户的token    |
### 返回值
```JSON
{
    "result": 1,
    "id": 1111,
    "name": "地表最弱战队",
    "level": 10,
    "exp": 100,
    "happiness_gate": [{"mission": 1, "elite_score": 3, "normal_score": 2}],
    "bag": [{"goods_id": 123, "count": 10}]
    
}
```
### 数据节点说明
| Name           | Description              |
| :------------- | :----------------------- |
| result         | 请求结果                     |
| bunch_id       | 战队id                     |
| name           | 战队名字                     |
| level          | 战队等级                     |
| exp            | 战队经验值                    |
| happiness_gate | 幸福之门过关数据                 |
| *mission*      | 第几关                      |
| *elite_score*  | 精英模式过关星级                 |
| *normal_score* | 普通模式过关星级                 |
| bag            | 用户背包。英雄、货币、碎片等都抽象成背包里的物品 |
| exp            | 战队经验值                    |


# 幸福之门
## 3.幸福之门·获取奖励物品
### 接口说明
在每个关卡战斗之前会先拉取本次战斗过程中会掉落的物品。掉落物品展示的时机由前端控制，但只有胜利才会获得奖励。
### 接口URI
  /v6/game/hero/gate/loots
### 提交方式
  POST、GET
### 请求参数列表
| Required | Name         | Type   | Description |
| :------- | :----------- | :----- | :---------- |
| required | access_token | string | 用户的token    |
|          | mission      | int    | 关卡等级        |
### 返回值
```JSON
{
   "result": 1,
  "loots": [{"wave": 1, "goods": [{"goods_id": 11, "count":1}]}, {"wave": 2, "goods": [{"goods_id": 11, "count":1}]}, {"wave": 3, "goods": [{"goods_id": 11, "count":1}]}]
}
```
### 数据节点说明
| Name       | Description           |
| :--------- | :-------------------- |
| result     | 请求结果                  |
| loots      | 掉落的物品                 |
| *wave*     | 表示该关卡的第几波战斗           |
| *goods_id* | 掉落的物品id，包括各种碎片、道具、装备等 |
| *count*    | 掉落的数量                 |


## 3.幸福之门·战斗
### 接口说明
本关卡战斗结束，客户端将战斗结果上传给服务端
### 接口URI
  /v6/game/hero/gate/fight
### 提交方式
  POST、GET
### 请求参数列表
| Required | Name         | Type   | Description                              |
| :------- | :----------- | :----- | :--------------------------------------- |
| required | access_token | string | 用户的token                                 |
|          | battle_data  | string | JSON格式字符串。 {"fight_details":[],"exp_inc": 18, "mission": 2, "gold_inc": 768, "hero_exp_incs": [{"hero_id":222, "exp":182}], "stats":[{"hero_id":222, "output":198, "level":17,"alive":0}], "enemy_stats": [{"hero_id":222, "output":198, "level":17,"alive":0}]]}***（可能会加上出招顺序）*** |
### 返回值
```JSON
{
   "result": 1
}
```
### 数据节点说明
| Name   | Description |
| :----- | :---------- |
| result | 请求结果        |


# 扭蛋
## 4.丘比特之箭·扭蛋
### 接口说明
使用金币或者钻石来抽取卡牌，抽取到的物品可能是道具、装备、碎片、卷轴等等
### 接口URI
  /v6/game/hero/gacha
### 提交方式
  POST、GET
### 请求参数列表
| Required | Name         | Type   | Description         |
| :------- | :----------- | :----- | :------------------ |
| required | access_token | string | 用户的token            |
|          | gacha_id     | int    | 扭蛋的id               |
|          | currency     | int    | 使用哪种货币来抽取。金币=1，钻石=2 |
### 返回值
```JSON
{
   "result": 1,
   "gold_cost": 1,
    "diamond_cost": 7,
    "gold": 100,
    "diamond": 200,
    "goods_inc": [{"goods_id":123, "count":1}]
}
```
### 数据节点说明
| Name         | Description   |
| :----------- | :------------ |
| result       | 请求结果          |
| gold_cost    | 消耗的金币数量       |
| gold         | 增加或者消耗后当前金币数量 |
| diamond_cost | 消耗的钻石数量       |
| diamond      | 增加或者消耗后当前钻石数量 |

# 竞技场
## 5.竞技场·获取挑战战队
### 接口说明
拉取可以挑战的战队信息
### 接口URI
  /v6/game/hero/arena/bunches
### 提交方式
  POST、GET
### 请求参数列表
| Required | Name         | Type   | Description      |
| :------- | :----------- | :----- | :--------------- |
| required | access_token | string | 用户的token         |
|          | count        | int    | 每次拉取的数量，最多不超过10个 |
### 返回值
```JSON
{
   "result": 1,
   "bunches": [{"name": "哥布林", "rank": 4001, "ability": 1500, "bunch_id": 466}]
}
```
### 数据节点说明
| Name       | Description |
| :--------- | :---------- |
| result     | 请求结果        |
| bunches    | 对战英雄列表      |
| *name*     | 战队名字        |
| *rank*     | 战队排名        |
| *ability*  | 战斗力         |
| *bunch_id* | 战队的唯一id     |
## 6.竞技场·战斗
### 接口说明
挑战某一个战队，挑战成功交换排名，失败排名不变

（*注意：这里没有判断挑战的战队必须是刚刚返回的战队*）
### 接口URI
  /v6/game/hero/arena/challenge
### 提交方式
  POST、GET
### 请求参数列表
| Required | Name         | Type   | Description |
| :------- | :----------- | :----- | :---------- |
| required | access_token | string | 用户的token    |
|          | bunch_id     | int    | 挑战的战队id     |
### 返回值
```JSON
{
   "result": 1,
   "win": 1,
   "rank": 11,
   "exp_inc": 0,
   "gold_inc": 0,
    "hero_exp_incs": [{"hero_id":222, "exp":182}]
    "stats": [{"hero_id":222, "output":198}]
}
```
### 数据节点说明
| Name       | Description |
| :--------- | :---------- |
| result     | 请求结果        |
| bunches    | 对战英雄列表      |
| *name*     | 战队名字        |
| *rank*     | 战队排名        |
| *ability*  | 战斗力         |
| *bunch_id* | 战队的唯一id     |
| stats      | 同上          |

# 寻爱之旅
## 7.寻爱之旅·进度信息 
### 接口说明
 在进行战斗之前，需要拉取寻爱之旅的信息：进行到第几关，哪一个模式，阵亡的英雄等
### 接口URI
  /v6/game/hero/march/info
### 提交方式
  POST、GET
### 请求参数列表
| Required | Name         | Type   | Description |
| :------- | :----------- | :----- | :---------- |
| required | access_token | string | 用户的token    |
### 返回值
```JSON
{
   "result": 1,
   "mission": 1,
   "mode": 1,
   "heroes": [{"hero_id": 11, "life": 1, "total_life":10}],
   "mission_enemies": [{"hero_id": 12, "life": 1, "total_life": 20}],
   "mercenaries": [{"hero_id": 16, "life": 10, "total_life": 100, "level": 19,}],
   "unopen_boxes": [{"mission": 1, "box": [{"goods_id":10, "count": 1}]}] --应该自动添加还是手动领取？
   "reward_box": [{"goods_id":10, "count": 1}]
}
```
### 数据节点说明
| Name            | Description                              |
| :-------------- | :--------------------------------------- |
| result          | 请求结果                                     |
| mission         | 第几关                                      |
| mode            | 模式：1=困难，0=容易                             |
| heroes          | 所有可以参加寻爱之旅的英雄                            |
| *life*          | 剩余的体力                                    |
| *total_life*    | 英雄的总体力                                   |
| mission_enemies | 本关卡敌方英雄。本关之前的英雄客户端从配置文件中读取，状态是阵亡。本关之后的英雄客户端不展示 |
| mercenaries     | 雇佣兵数据                                    |
| *level*         | 雇佣兵等级                                    |
| unopen_boxes    | 未领取的箱子，不知道手动领取还是自动发放？                    |
| reward_box      | 本关卡过关奖励                                  |

## 8.寻爱之旅·战斗 
### 接口说明
和本关的战队进行战斗
### 接口URI
  /v6/game/hero/march/fight
### 提交方式
  POST、GET
### 请求参数列表
| Required | Name         | Type   | Description                |
| :------- | :----------- | :----- | :------------------------- |
| required | access_token | string | 用户的token                   |
|          | battle_data  | string | 格式如“幸福之门”战斗（**可能会加上出招顺序**） |
### 返回值
```JSON
{
   "result": 1
}
```
### 数据节点说明
| Name   | Description |
| :----- | :---------- |
| result | 请求结果        |

# 魔法城堡
## 9.魔法城堡·装备附魔
### 接口说明
强化某个英雄的装备
### 接口URI
  /v6/game/hero/castle/equipment/strengthen
### 提交方式
  POST、GET
### 请求参数列表
| Required | Name         | Type   | Description                              |
| :------- | :----------- | :----- | :--------------------------------------- |
| required | access_token | string | 用户的token                                 |
|          | equipment_id | int    | 需要附魔的装备id                                |
|          | string       | string | 消耗的物品JSON数组：[{"goods_id":11, "count":1}, {"goods_id":22, "count":2}] |
### 返回值
```JSON
{
   "result": 1
}
```
### 数据节点说明
| Name   | Description |
| :----- | :---------- |
| result | 请求结果        |

## 10.魔法城堡·卡牌洗练
### 接口说明
强化某个英雄（**还不知道咋洗练**）
### 接口URI
  /v6/game/hero/castle/card/strengthen
### 提交方式
  POST、GET
### 请求参数列表
| Required | Name         | Type   | Description     |
| :------- | :----------- | :----- | :-------------- |
| required | access_token | string | 用户的token        |
|          | hero_id      | int    | 进行强化的英雄goods_id |
|          | type         | int    | 金币：1，钻石：2       |
### 返回值
```JSON
{
   "result": 1
}
```
### 数据节点说明
| Name   | Description |
| :----- | :---------- |
| result | 请求结果        |

# 樱花商人
## 樱花商人·查看已购买情况
### 接口说明
查看樱花商人（市集）上在售的物品
### 接口URI
  /v6/game/hero/fair/shopping
### 提交方式
  POST、GET
### 请求参数列表
| Required | Name         | Type   | Description |
| :------- | :----------- | :----- | :---------- |
| required | access_token | string | 用户的token    |
### 返回值
```JSON
{
   "result": 1,
   "store": [{"goods_id": 111, "gold": 2000, "diamond": 0, "count": 1}],
   "vip_store": [{"goods_id": 111, "gold": 0, "diamond": 200, "count": 0}],
   "arena_store": [{"goods_id": 111, "gold": 2000, "diamond": 0, "count": 1}],
   "league_store": [{"goods_id": 111, "gold": 2000, "diamond": 0, "count": 1}],
   "feats_store": [{"goods_id": 111, "gold": 2000, "diamond": 0, "count": 1}],
   "couple_store": [{"goods_id": 111, "gold": 2000, "diamond": 0, "count": 1}],
}
```
### 数据节点说明
| Name         | Description      |
| :----------- | :--------------- |
| result       | 请求结果             |
| store        | 商店               |
| vip_store    | vip商店            |
| arena_store  | 竞技场商店            |
| league_store | 联盟商店             |
| feats_store  | 功勋商店             |
| couple_store | 情侣商店             |
| **count**    | 剩余数量，为0的时候表示已经售罄 |

## 樱花商人·购买
### 接口说明
樱花商人商店购买
### 接口URI
  /v6/game/hero/fair/goods/buy
### 提交方式
  POST、GET
### 请求参数列表
| Required | Name         | Type   | Description                              |
| :------- | :----------- | :----- | :--------------------------------------- |
| required | access_token | string | 用户的token                                 |
|          | goods_id     | int    | 进行强化的英雄goods_id                          |
|          | store_id     | int    | 商店编号：商店=1，VIP商店=2，竞技场商店=3，联盟商店=4，功勋商店=5，情侣商店=6 |
| optional | count        | int    | 购买数量，默认是1                                |
### 返回值
```JSON
{
   "result": 1
}
```
### 数据节点说明
| Name   | Description |
| :----- | :---------- |
| result | 请求结果        |
## 樱花商人·刷新商品
### 接口说明
如果对在售的物品不满意，可以消耗钻石来更换
### 接口URI
  /v6/game/hero/fair/goods/refresh
### 提交方式
  POST、GET
### 请求参数列表
| Required | Name         | Type   | Description                              |
| :------- | :----------- | :----- | :--------------------------------------- |
| required | access_token | string | 用户的token                                 |
|          | store_id     | int    | 商店编号：商店=1，VIP商店=2，竞技场商店=3，联盟商店=4，功勋商店=5，情侣商店=6 |
### 返回值
```JSON
{
   "result": 1
}
```
### 数据节点说明
| Name   | Description |
| :----- | :---------- |
| result | 请求结果        |

# 爱情联盟
# 真爱之路
# 甜蜜堡
# 排行榜
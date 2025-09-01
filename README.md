# 斗地主接口文档
# 一、叫分
## 接口名称
叫分接口

## 接口描述
根据玩家手牌、叫分位置及历史叫分信息，计算游戏的叫分策略

---

## 请求信息

### 请求方式
`POST`

### 请求URL
`http://114.66.39.93:5000/sandou_act`

### 请求头

| 字段 | 值 | 说明 |
|------|----|------|
| `Accept` | `*/*` | 接收所有响应类型 |
| `Accept-Encoding` | `gzip, deflate, br` | 支持的压缩编码 |
| `Connection` | `keep-alive` | 保持连接 |
| `Content-Type` | `application/json` | 请求体为JSON格式 |
| `User-Agent` | `PostmanRuntime-ApipostRuntime/1.1.0` | 客户端标识 |

---

## 请求参数（JSON Body）

| 字段 | 类型 | 必填 | 说明 | 示例 |
|------|------|------|------|------|
| `player_hand_cards` | `string` | 是 | **玩家手牌**，需用字符串传递整型数组（值范围：3-17,20,30） | `"[3,4,5,5,7,8,8,9,10,11,12,12,13,14,14,17,30]"` |
| `passwd` | `string` | 是 | **接口密码**，固定为 `asdac` | `"asdac"` |
| `bid_position` | `string` | 是 | **叫分位置**，取值范围：`first`/`second`/`third` | `"third"` |
| `bid_info` | `string` | 是 | **历史叫分信息**，用字符串传递整型数组（顺序：第一位/第二位/第三位叫分记录）没有轮到叫分的为-1 | `"[0,0,0]" ` |

---
## 特殊说明
```
1.第一个叫分为first，第二个叫分为second，第三个叫分为third   
2.没有轮到叫分的为-1
对应牌面信息 = {
    '3': 3, '4': 4, '5': 5, '6': 6, '7': 7,
    '8': 8, '9': 9, 'T': 10, 'J': 11, 'Q': 12,
    'K': 13, 'A': 14, '2': 17, 'X': 20, 'D': 30
}

```
## 请求示例

```json
{
  "player_hand_cards": "[3,4,5,5,7,8,8,9,10,11,12,12,13,14,14,17,30]",
  "passwd": "asdac",
  "bid_position": "third",
  "bid_info": "[0,0,0]"
}
```
## curl代码
```
curl --request POST \
  --url http://114.66.39.93:5000/sandou_act \
  --header 'Accept: */*' \
  --header 'Accept-Encoding: gzip, deflate, br' \
  --header 'Connection: keep-alive' \
  --header 'Content-Type: application/json' \
  --header 'User-Agent: PostmanRuntime-ApipostRuntime/1.1.0' \
  --data '{
	"player_hand_cards": "[3, 4, 5, 5, 7, 8, 8, 9, 10, 11, 12, 12, 13, 14, 14, 17, 30]",
	"passwd": "asdac",
	"bid_position" :"third",
	"bid_info":"[0,0,0]"
}'
```
## 响应示例
```
{
    "action": [
            3
	],
	"code": 200 # 状态码
}
```

# 二、出牌

## 接口描述
根据当前游戏状态（玩家位置、手牌、地主牌、出牌序列、叫分信息等），计算玩家在当前回合的出牌策略。

### 请求方式
`POST`

### 请求URL
`http://114.66.39.93:5000/sandou_bid`

### 请求头

| 字段 | 值 | 说明 |
|------|----|------|
| `Accept` | `*/*` | 接收所有响应类型 |
| `Accept-Encoding` | `gzip, deflate, br` | 支持的压缩编码 |
| `Connection` | `keep-alive` | 保持连接 |
| `Content-Type` | `application/json` | 请求体为JSON格式 |
| `User-Agent` | `PostmanRuntime-ApipostRuntime/1.1.0` | 客户端标识 |

---


## 请求参数（JSON Body）

| 字段 | 类型 | 必填 | 说明 | 示例值 |
|------|------|------|------|--------|
| `player_position` | `string` | 是 | **玩家位置**<br>`landlord`: 地主<br>`landlord_down`: 地主下家<br>`landlord_up`: 地主上家 | `"landlord"` |
| `player_hand_cards` | `string` | 是 | **玩家当前手牌**<br>字符串格式的整型数组<br>牌值范围: 3-17,20,30 | `"[4,5,5,8,14,14,17,30]"` |
| `three_landlord_cards` | `string` | 是 | **三张地主牌**<br>字符串格式的整型数组 | `"[3,13,20]"` |
| `card_play_action_seq` | `string` | 是 | **出牌序列**<br>字符串格式的二维数组<br>`[]`表示不出牌 | `"[[9],[12],[13],[17],[],[],[4,5,6,7,8,9,10]]"` |
| `bid_info` | `string` | 是 | **叫分信息**<br>字符串格式的整型数组<br>[第一位,第二位,第三位] | `"[1,0,0]"` |
| `passwd` | `string` | 是 | **接口密码**<br>固定值 `asdac` | `"asdac"` |
| `bid_position` | `string` | 是 | **叫分位置**<br>`first`/`second`/`third` | `"first"` |

---


## 特殊说明
```
地主时，card_play_action_seq为"[]"
首轮为地主出牌时,手牌传20张
对应牌面信息 = {
    '3': 3, '4': 4, '5': 5, '6': 6, '7': 7,
    '8': 8, '9': 9, 'T': 10, 'J': 11, 'Q': 12,
    'K': 13, 'A': 14, '2': 17, 'X': 20, 'D': 30
}
```

## 请求示例
```
curl --request POST \
  --url http://114.66.39.93:5000/sandou_bid \
  --header 'Accept: */*' \
  --header 'Accept-Encoding: gzip, deflate, br' \
  --header 'Connection: keep-alive' \
  --header 'Content-Type: application/json' \
  --header 'User-Agent: PostmanRuntime-ApipostRuntime/1.1.0' \
  --data '{
	"player_position": "landlord",
	"player_hand_cards": "[4, 5, 5, 8, 14, 14, 17, 30]",
	"three_landlord_cards": "[3, 13, 20]",
	"card_play_action_seq": "[[9],[12],[13],[17],[],[],[4, 5, 6, 7, 8, 9, 10],[7, 8, 9, 10, 11, 12, 13],[],[],[3],[12],[],[],[3, 4, 5, 6, 7, 8, 9],[],[],[4, 11, 11, 11],[],[],[10, 10],[13,13]]",
	"bid_info":"[1, 0, 0]",
	"passwd": "asdac",
	"bid_position" :"first"
}'
```

## 响应示例
```
{
    "action": [
            14,
            15
	],
	"code": 200 # 状态码
}
```

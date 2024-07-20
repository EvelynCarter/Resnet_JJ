
```json
{
  "action": string,
  "data": object
}
```

| 参数名 | 说明 |
|-----|---------|
| action | 上报类型，可选"init"/"play"，对应 初始化游戏/游戏进程上报 |
| data | 上报数据，见下方 |

上报数据：

### 初始化游戏

```json
{
  "pid": string / int,
  "three_landlord_cards": string,
  "ai_amount": int,
  "player_data":[
    {
      "model": string,
      "hand_cards": string,
      "position_code": int
    },
    {
      "model": string,
      "hand_cards": string,
      "position_code": int
    }
  ]
}
```

| 参数名 | 说明 | 示例 |
|-----|---------|--------|
| pid | 对局唯一标识，若为int类型会自动转为string | 10001 |
| three_landlord_cards | 三张地主牌 | "444" |
| ai_amount | AI玩家数量，为1或2 | 2 |
| player_data | AI玩家数据，AI玩家数为1时只会读取数组中的第一个元素，内容元素见下方 | \ |
| model | AI玩家使用的模型 | "WP" |
| hand_cards | AI玩家的手牌 | "333444456789TJQKA2XD" |
| position_code | AI玩家的位号 **0-地主上家，1-地主，2-地主下家** | 1 |

<details>
<summary>完整示例</summary>

```json
{
  "action": "init",
  "data": {
    "three_landlord_cards": "444",
    "pid": 10001,
    "ai_amount": 2,
    "player_data": [
      {
        "model": "WP",
        "hand_cards": "333444456789TJQKA2XD",
        "position_code": 1
      },
      {
        "model": "WP",
        "hand_cards": "3555666777888999T",
        "position_code": 2
      }
    ]
  }
}
```

</details>

### 游戏进程上报

也就是上报玩家出牌

```json
{
  "pid": string / int,
  "player": int,
  "cards": string
}
```

| 参数名 | 说明 | 示例 |
|-----|---------|--------|
| pid | 对局唯一标识，若为int类型会自动转为string | 10001 |
| player | 出牌玩家位号 | 0 |
| cards | 玩家出的牌，为空字符串则表示不要 | "TJQKA" |

> 程序并不会检测玩家所出的牌是否遵循规则，请保证上报数据准确

<details>
<summary>完整示例</summary>

```json
{
  "action": "play",
  "data": {
    "pid": 10001,
    "player": 0,
    "cards": "TJQKA"
  }
}
```

</details>

## API响应

```json
{
  "type": string,
  "action": string,
  "status": string,
  "msg": string,
  "data": object
}
```
| 参数名 | 说明 |
|-----|---------|
| type | 响应类型，与上报数据的`action`相对应，上报"play"时为"step" |
| action | 响应AI操作，尚未轮到AI出牌时为"receive"，AI出牌时为"play" |
| status | 响应状态，为"ok"/"fail" |
| msg | 错误信息，响应状态为"fail"时不为空 |
| data | 响应数据，见下方 |

响应数据：

```json
{
  "pid": pid,
  "game_over": boolen,
  "play": [
    {
      "cards": array,
      "confidence": string
    },
    {
      "cards": array,
      "confidence": string
    }
  ]
}
```

| 参数名 | 说明 |
|-----|---------|
| pid | 对应对局标识 |
| game_over | 对局是否结束，结束则为`true` |
| play | AI出牌数据，action为"receive"时无此元素，元素内容见下方 |
| cards | AI出的牌，为数组类型 |
| confidence | AI的胜率估计 |



# Connect 4 Client WebSocket Manual

## reload
現在の状態を取得する

send 
```json
{"call": "reload"}
```
receive
* ログイン0人の状態
```json
{
    "call": "reload",
    "stdin": null
}
```
+ ログイン1人の状態
```json
{
    "label": "demo-a",
    "stdin": null,
    "round_id": 3,
    "user0": "user_name",
    "user1": null,
    "call": "reload"
}
```
+ ゲーム進行中の場合
```json
{
    "label": "demo-a",
    "stdin": "7 6 1\n.......\n.......\n.......\n.......\n.......\n..0....\n",
    "round_id": 3,
    "user0": "手動操作: hogehoge",
    "user1": "手動操作: foo",
    "call": "reload",
    "player": 1
}
```

## login
ログイン

send
```json
{"call": "login", "user0": "手動操作: hogehoge"}
```
receive
+ ゲーム参加者1人の状態
```json
{
    "call": "reload",
    "user0": "手動操作: hogehoge",
    "message": "先攻: 手動操作: hogehoge さんがログインしました"
}
```
+ ゲーム参加者2人の状態
```json
{
    "call": "step",
    "user1": "foo",
    "message": "後攻: 手動操作: foo さんがログインしました",
    "user0": "手動操作: hogehoge",
    "ready": true,
    "player": 0
}
```

## send
send data

send
```json
{"call": "step", "stdout": 2, "stdin": "7 6 0\n.......\n.......\n.......\n.......\n.......\n.......\n\n", "player": 0}
```
receive
+ ゲーム進行
```json
{
    "call": "step",
    "stdout": 2,
    "stdin": "7 6 1\n.......\n.......\n.......\n.......\n.......\n..0....\n",
    "player": 1,
    "message": "",
    "before": "7 6 0\n.......\n.......\n.......\n.......\n.......\n......."
}
```

+ ゲーム終了
```json
{
    "call": "end",
    "stdout": 6,
    "stdin": "7 6 0\n.......\n.......\n.......\n.......\n.1.0.0.\n0001111\n",
    "player": 1,
    "message": "1 の勝利",
    "before": "7 6 1\n.......\n.......\n.......\n.......\n.1.0.0.\n000111.",
    "win": "1"
}
```

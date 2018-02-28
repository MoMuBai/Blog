# 空数据格式规范

## 请求为200的返回结果

### 返回的result为一个数组，并且有数据时期望的结果
```json
{
  "code": 200,
  "data": {
    "result": [
      {
        "house_id": 36,
        "area": "\u6c5f\u5e72\u533a"
      }
    ],
    "totals": 1 //不一定有
  },
  "message": ""
}
```

### 返回的result为一个数组，并且没有数据时期望的结果
```json
{
  "code": 200,
  "data": {
    "result": [], //返回一个空的数组
    "totals": ""
  },
  "message": ""
}
```

### 返回的result为一个对象，并且有数据时期望的结果
```json
{
  "code": 200,
  "data": {
    "result": {
      "user_id": 4711141,
      "mobile": "18757555795"
    }
  },
  "message": "\u67e5\u8be2\u6210\u529f\uff01"
}
```

### 返回的result为一个对象，并且没有数据时期望的结果
```json
{
  "code": 200,
  "data": {
    "result": null //返回一个空的对象
  },
  "message": "\u67e5\u8be2\u6210\u529f\uff01"
}
```


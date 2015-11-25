# PatientsClub前后台接口

## 1 帐户管理

​**接口:** http://www.patientsclub.com/we/cgi-bin/account.cgi

**说明:** 本节定义了所有和用户帐户相关的接口。**除了注册和登录外其它所有接口都需要附带注册或是登录返回的的鉴权token和user参数。**

### 1.1 注册

**功能:**  注册一个全新帐号。

**方法:** post

**参数:**

| 参数名      | 类型     | 描述                  |
| -------- | ------ | ------------------- |
| action   | string | enroll              |
| phone    | string | 为手机号，比如：13912345678 |
| password | string | 密码，md5后的字符串         |

**返回值：**

``` 
code: 0,                     // 返回码， 0 表示成功， -1 系统异常， 1 用户已存在
message: '注册成功',          // 与返回码对应的文字
data : {
  'user' : 'xxxxxxxx',         // 用户标识，非手机号，非昵称，系统生成 
  'token': 'Qasdjkio123xie32', // 返回的token, 后面所有登录后的请求需带上此token供后台鉴权
}
```

### 1.2 登录

​**功能:** 使用帐号登录app。

​**方法:** post

​**参数:**

| 参数名      | 类型     | 描述                 |
| -------- | ------ | ------------------ |
| action   | string | login              |
| phone    | string | 手机号，比如：13912345678 |
| password | string | 密码，md5加密后的字符串      |

**返回值：**

``` 
code: 0,                     // 返回码， 0 表示成功， -1 系统异常， 1 帐号或密码错误
message: '登录成功',          // 与返回码对应的文字
data: {
  'user' : 'xxxxxxxx',         // 用户标识，非手机号，非昵称，系统生成 
  'token': 'Qasdjkio123xie32', // 返回的token, 后面所有登录后的请求需带上此token供后台鉴权
}
```

### 1.3 注销

​**功能：**  注销一个正在使用的帐号。

​**方法：** post

​**参数：**

| 参数名    | 类型     | 描述     |
| ------ | ------ | ------ |
| action | string | logout |

​**返回值：**

``` 
code: 0,                     // 返回码， 0 表示成功， -1 系统异常
message: '注销成功',          // 与返回码对应的文字
```

### 1.4 修改密码

​**功能：**  使用原密码鉴权置新密码

**方法：** post

​**参数：**

| 参数名    | 类型     | 描述         |
| ------ | ------ | ---------- |
| action | string | mod_pass   |
| o_pass | string | 旧密码  md5加密 |
| n_pass | string | 新密码 md5加密  |

**返回值：**

``` 
code: 0,                     // 返回码， 0 表示成功， -1 系统异常， 1 旧密码错误
message: '修改成功',          // 与返回码对应的文字
```

### 1.5 重置密码

​**功能：**  使用验证码鉴权置新密码

**方法：** post

​**参数：**

| 参数名    | 类型     | 描述         |
| ------ | ------ | ---------- |
| action | string | reset_pass |
| n_pass | string | 新密码        |
| phone  | string | 手机号        |

​**返回值：**

``` 
code: 0,                     // 返回码， 0 表示成功， -1 系统异常
message: '设置成功',          // 与返回码对应的文字
data: {
  'user': 'xxxxxx',           // 用户名
  'token': 'xdafdqeqsadaf',   // 签名
}
```

### 1.6 发送验证码

​**功能：**  请求后端对指定手机号发送验证码

​**方法：** post

​**参数：**

| 参数名    | 类型     | 描述        |
| :----- | ------ | --------- |
| action | string | send_code |

**返回值：**

``` 
code: 0,                     // 返回码， 0 表示成功， -1 发送失败
message: '发送成功',          // 与返回码对应的文字
```

### 1.7 验证验证码

**功能：**  验证接收的验证码

​**方法：** post

​**参数：**

| 参数名    | 类型     | 描述          |
| ------ | ------ | ----------- |
| action | string | verify_code |
| code   | string | 接收的验证码      |

​**返回值：**

``` 
code: 0,                     // 返回码， 0 表示成功， -1 系统异常
message: '验证OK',            // 与返回码对应的文字
```

## 2  关系链管理

​**接口:** http://www.patientsclub.com/we/cgi-bin/follow.cgi

​**说明:** 本节定义了所有与关系链相关的接口。

### 2.1 关注/取消关注一个用户

​**功能：**  关注一个用户或取消关注一个用户

​**方法：** post

​**参数：**

| 参数名         | 类型     | 描述                  |
| ----------- | ------ | ------------------- |
| action      | string | add: 添加关注，del: 删除关注 |
| target_user | string | 被关注者用户              |

​**返回值：**

``` 
code: 0,                     // 返回码， 0 表示成功， -1 系统异常， 1 对方把你加入黑名单
message: '成功关注',          // 与返回码对应的文字
```

### 2.2 查询关注人列表

​**功能：** 查询指定用户的关注列表和被关注列表

​**方法：** get

​**参数：**

| 参数名         | 类型     | 描述                                      |
| ----------- | ------ | --------------------------------------- |
| action      | string | follow_list: 关注列表, followed_list: 被关注列表 |
| target_user | string | 想要查询用户的用户                               |

​**返回值：**

``` 
code: 0,                     // 返回码， 0 表示成功， -1 系统异常
message: '查询成功',          // 与返回码对应的文字
data: [
  {
    'user': '139xxxxxxxx',                // 帐号
    'nickname': '田伯光'，                 // 昵称 
    'head_url': 'http://xxxxx/head.jpg',  // 头像缩略图
    'signatue': 'enjoy every day',        // 签名
    'follow': 'yes',                      // yes 已关注, no 未关注
  },
  ...
]
```

### 2.3 查询关注状态

**功能：** 查询一个用户是否已经关注另一个用户

**方法：** get

**参数：**

| 参数名         | 类型     | 描述                      |
| ----------- | ------ | ----------------------- |
| action      | string | follow_status           |
| target_user | string | 查询user是否已经关注target_user |

**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， -1 系统异常
message: '查询成功',             // 与返回码对应的文字
data: {'follow': 'yes' | 'no'}  // yes 已关注， no 未关注
```

### 2.4 拉黑/解除拉黑

**功能：** 拉黑一个不喜欢的用户，或是对一个已经拉黑的用户解除拉黑 

​**方法：** post

​**参数：**

| 参数名         | 类型     | 描述                     |
| ----------- | ------ | ---------------------- |
| action      | string | block 拉黒， unblock 解除拉黑 |
| target_user | string | 目标用户                   |

**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， -1 系统异常
message: '拉黑成功',             // 与返回码对应的文字
```

## 3 记录管理

​**接口：** http://www.patientsclub.com/we/cgi-bin/record.cgi

​**说明：** 本节定义所有和用户分享记录相关的接口。

### 3.1 发表记录

​**功能：** 用户发布新记录

​**方法：** post

​**参数：**

| 参数名     | 类型     | 描述             |
| ------- | ------ | -------------- |
| action  | string | create         |
| weather | string | 天气标签图标对应url    |
| mood    | string | 心情标签图标对应url    |
| status  | string | 状态标签图标对应url    |
| text    | string | 文本内容           |
| picture | int    | 图片个数           |
| public  | string | yes: 公开，no: 私密 |
| address | string | 用户输入的地址信息      |

**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， -1 系统异常 
message: '发布成功',             // 与返回码对应的文字
data: {
  'sign': 'xxxxxdafdafasfas',   // 上传图片鉴名
  'img_conf': [
     {
       'appid': 'xxxx',             // 参考万象优图
       'bucket': 'xxxx',            // 
       'fileid' : 'xxxx',           //
       'callback': 'magic context', // 万象优图回调数据
     },
     ...
   ]
}
```

### 3.2 删除记录

​**功能：** 删除一条记录

​**方法：** post

​**参数：**

| 参数名    | 类型     | 描述   |
| ------ | ------ | ---- |
| action | string | del  |
| id     | int    | 记录id |

​**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， -1 系统异常
message: '删除成功',             // 与返回码对应的文字
```

### 3.3 查看单条记录内容

​**功能：** 查看一条记录

​**方法：** get

​**参数：**

| 参数名    | 类型     | 描述           |
| ------ | ------ | ------------ |
| action | string | query_detail |
| id     | int    | 记录id         |

**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， -1 系统异常
message: '查询成功',             // 与返回码对应的文字
data: {
  'user': {
    'user': 'xxxxxx',
    'nickname': 'xxxxx',
    'head': 'http://xxxxx/1.jpg',
  },
  'record': {
    'id': ‘1234’,                              // 记录id(字符串，非整数)
    'time': 1447988674                         // 发表时间(长整型，timestamp)
    'weather': 'http://xxxxxx/sunshine.jpg',   // 天气
    'mood': 'http://xxxxxxx/happy.jpg',        // 心情
    'status': 'http://xxxxx/work.jpg',         // 状态
    'text': 'today is a good day',             // 内容
    'address': 'china.hunan',                  // 地址
    'public': 'yes',                           // 是否公开
    'picture': ['http://xxxx/1.jpg', ...]      // 图片列表
  },
  'interact': {
     'view': 28,
     'like': 6
     'comment': {
        'count': 6,
        'list': [
           {
              'user': 'xxxxxx',
              'nickname': 'xxxxx',
              'id': '1213131',
              'time': 121412313,
              'text': 'hello comment', 
              're_nickname': 'xxxxx',    // 被回复评论用户名
           },
           ...,
        ]         // list
     }            // comment
  }               // interact
}                 // data
```

### 3.4 查看记录列表v1

​**功能：** 返回记录id列表

​**方法：** get

​**参数：**

| 参数名    | 类型     | 描述                                       |
| ------ | ------ | ---------------------------------------- |
| action | string | query_list_v1                            |
| type   | string | follow: 动态页，recommand: 推荐页，time: 最近更新页， home: 用户主页 |
| user   | string | 当type为home时，被查看主页的用户名，其它type忽略该参数        |
| page   | int    | 第几页，每责固定拉取20条信息                          |

**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， 其它失败
message: '查询成功',             // 与返回码对应的文字
data: {
  'page': 1,                       // 第几页信息
  'ids': ['1234', '1235', ...]     // 记录id列表
}
```

### 3.5 查看记录列表v2

​**功能：** 返回记录详细信息列表

​**方法：** get

​**参数：**

| 参数名    | 类型     | 描述                                  |
| ------ | ------ | ----------------------------------- |
| action | string | query_list_v2                       |
| type   | string | follow: 动态页，time: 最近更新页， home: 用户主页 |
| user   | string | 当type为home时，被查看主页的用户名，其它type忽略该参数   |
| page   | int    | 第几页，每责固定拉取20条信息                     |

​**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， -1 系统异常， 1 没有更多内容
message: '查询成功',             // 与返回码对应的文字
data: {
  'page': 0,                    // 第几页
  'records': [
     {..},                      // 记录内容定义见3.3节的返回的data定义                
     ...
   ]
}
```

### 3.6修改记录属性

**功能：** 修改一条记录的公开属性

**方法：** post

**参数：**

| 参数名    | 类型     | 描述              |
| ------ | ------ | --------------- |
| action | string | alt_privacy     |
| id     | int    | 记录id            |
| public | string | yes: 公开，no: 不公开 |

**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， -1 系统异常
message: '删除成功',             // 与返回码对应的文字
```

## 4 互动管理

**接口：** http://www.patientsclub.com/we/cgi-bin/interact.cgi

​**说明：** 本节定义了用户在patientsclub的互动相关的接口

### 4.1 增加评论/赞

​**功能：** 对一条记录进行评论或是点赞

​**方法：** post

​**参数：**

| 参数名        | 类型     | 描述                   |
| ---------- | ------ | -------------------- |
| action     | string | add                  |
| type       | string | comment: 评论，like: 点赞 |
| record_id  | int    | 记录id                 |
| comment_id | int    | 评论id,  供评论评论使用，点赞置空  |
| comment    | string | 评论内容，点赞置空            |

​**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功，-1 系统异常
message: '评论成功',             // 与返回码对应的文字
```

### 4.2 删除评论/赞

​**功能：** 删除一条评论或是一个点赞

​**方法：** post

​**参数：**

| 参数名        | 类型     | 描述                   |
| ---------- | ------ | -------------------- |
| action     | string | del                  |
| type       | string | comment: 评论，like: 点赞 |
| record_id  | int    | 记录id                 |
| comment_id | int    | 评论id, 删除点赞置空         |

​**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， -1 系统异常
message: '删除成功',             // 与返回码对应的文字
```

### 4.3 查评论列表，赞列表，浏览次数

​**功能：** 查看一条记录的评论列表，赞列表，浏览次数

​**方法：** get

​**参数：**

| 参数名       | 类型     | 描述                                    |
| --------- | ------ | ------------------------------------- |
| action    | string | select                                |
| type      | string | comment: 评论，like: 点赞，view: 浏览，all: 所有 |
| record_id | string | 记录id                                  |

**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功，-1 系统异常
message: '评论成功',             // 与返回码对应的文字
------------下面为 type = comment 时返回的内容 ------
data: [
  {
    'id': '12345',                    // 评论id
    'time': 1447988674,               // 评论时间
    'user': '12134212',               // 评论用户
    'nickname': 'user name',          // 评论用户昵称
    'head': 'http://xx/head.jpg'      // 评论用户头像
    'comment': 'this is a comment',   // 评论内容
    'comment_id': 'xxxx'              // 被评论id, 对非评论的评论此参数置0
  }, 
  ...
]

------------下面为 type = like 时返回的内容----------------
data: [
  {
    'user': '12345',                     // 赞用户id
    'time': 1447988674,                  // 赞时间
    'nickname': 'nickname',              // 赞用户昵称
    'head': 'http://xxxxx/head.jpg',     // 赞用户头像
  }, 
  ...
]

------------下面为 type = view 时返回的内容-------------
data: 123                               // 被浏览次数

------------ 下面为 type = all 时返回的内容 ------------
data: {
  'comment' : [
    {
      'id': '12345',                    // 评论id
      'time': 1447988674,               // 评论时间
      'user': '12134212',               // 评论用户
      'nickname': 'user name',          // 评论用户昵称
      'head': 'http://xx/head.jpg'      // 评论用户头像
      'comment': 'this is a comment',   // 评论内容
      'comment_id': 'xxxx'              // 被评论id, 对非评论的评论此参数置0
    }, 
    ...
  ],
  'like': [
    {
      'user': '12345',                     // 赞用户id
      'time': 1447988674,                  // 赞时间
      'nickname': 'nickname',              // 赞用户昵称
      'head': 'http://xxxxx/head.jpg',     // 赞用户头像
    }, 
    ...
  ],
  'view': 123,                              // 被浏览次数
}
```

## 5 个人资料管理

**接口:** http://www.patientsclub.com/we/cgi-bin/profile.cgi

​**说明:** 本节定义用户资料相关接口。

### 5.1 修改个人资料

​**功能：** 修改个人资料，需提供除头像外的完整个人资料profile

**方法：** post

​**参数：**

| 参数名     | 类型     | 描述                                       |
| ------- | ------ | ---------------------------------------- |
| type    | string | alt                                      |
| profile | string | 完整或是部分gprofile的json串，json串的格式见查看资料接口, 不涉及头像修改，头像修改见下面cgi. |

​**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， 其它失败
message: '修改成功',             // 与返回码对应的文字
```

### 5.2 修改个人头像

​**功能：** 修改头像图片

​**方法：** post

​**参数：**

| 参数名  | 类型     | 描述       |
| ---- | ------ | -------- |
| type | string | alt_head |

**返回值：**

``` 
code: 0,                             // 返回码， 0 表示成功， -1 系统异常
message: '修改成功',                  // 与返回码对应的文字
data: {
  'appid': 'xxxxx',                  // 参考万象优图
  'bucket': 'xxxxx',                 //
  'fileid': 'xxxxxx',                //
  'sign': 'xxxxxxxx',                // 签名
  'callback': 'magic context',       // 万象优图回调数据
}
```

### 5.3 查看个人资料

​**功能：** 查看个人资料，返回完整个人资料profile

​**方法：** get

​**参数：**

| 参数名         | 类型     | 描述          |
| ----------- | ------ | ----------- |
| type        | string | query       |
| target_user | string | 待查询个人资料的用户名 |

​**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功，-1 系统异常
message: '查询成功',             // 与返回码对应的文字
data: {
  'user': '181xxxxxxxx',
  'nickname': '田伯光'，
  'head_url': 'http://xxxxxxx/田伯光_head.jpg',
  'agender': 'male',
  'birthday': 1447988674,
  'address': '国家/省/市',
  'signature': '快乐田伯光，万里独行田伯光'，
  'disease': '癌症',
  'role': '家属'，
  'start_time': 1447988674,
}
```

### 5.4 查看个人社区信息

**功能：** 查看一个用户在社区活动的整体统计情况，比如加入时间，发表记录次数，follow用户个数，被follow用户个数等

​**方法：** get

​**参数：**

| 参数名         | 类型     | 描述          |
| ----------- | ------ | ----------- |
| type        | string | stat        |
| target_user | string | 待查询个人资料的用户名 |

​**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， 其它失败
message: '查询成功',             // 与返回码对应的文字
data: {
  'user': '181xxxxxxxx',         // 用户id
  'enroll_time': 1447988674,     // 注册时间 
  'follow_count': 6,             // 关注人数
  'followed_count': 28,          // 被关注人数
  'record_count': 496,           // 记录数
  'login_count': 6,              // 登录次数
  'comment_count': 28,           // 评论次数
  'commented_count': 496,        // 被评论次数
  'like_count': 6,               // 赞次数
  'liked_count': 28,             // 被赞次数
}
```

## 6 推荐列表

**接口:**  http://www.patientsclub.com/we/cgi-bin/recommand.cgi

**功能:**  本接口供用户向patientsclub反馈问题

**方法:**  get

**参数:**

| 参数名  | 类型   | 描述   |
| ---- | ---- | ---- |
|      |      |      |

​**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， -1 系统异常
message: '查询成功',             // 与返回码对应的文字
data: {
  'page': 0,
  'users': [
     {
        'user': 'xxxxx',
        'nickname': 'xxxxx',
        'head': 'http://xxxxx/head.jpg',
        'disease': 'xxxx',
        'role': '家属',
        'img': 'http://xxxxxx/recommand.jpg',
        'text': 'let\'s light the future, together',
        'follow': 'yes' | 'no',        // yes 已关注， no 未关注
     },
     ...
  ]
}
```

## 7 用户反馈

**接口：** http://www.patientsclub.com/we/cgi-bin/feedback.cgi

**功能:** 本接口供用户向patientsclub反馈问题

**方法：** post

**参数：**

| 参数名    | 类型     | 描述     |
| ------ | ------ | ------ |
| conent | string | 反馈问题内容 |

​**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， 其它失败
message: '查询成功',             // 与返回码对应的文字
```

## 8 业务数据管理

**接口:** http://www.patientsclub.com/we/cgi-bin/business.cgi

**说明:** 本节定义了所有与业务数据相关的接口，业务数据可能随时间推移可能随时发生变更。通过本接口把这些变动限制在后台，前端自动适配。

### 8.1 查询疾病列表

​**功能：** 返回当前app支持的疾病列表供用户设置个人资料时选择

​**方法：** get

​**参数：**

| 参数名  | 类型     | 描述      |
| ---- | ------ | ------- |
| type | string | disease |

​**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， 其它失败
message: '查询成功',             // 与返回码对应的文字
data: [
  '器官移植',
  '心脑血管',
  ...
]
```

### 8.2 查询地址列表

​**功能：** 查询地址信息列表。

​**方法：** get

​**参数：**

| 参数名          | 类型     | 描述                  |
| ------------ | ------ | ------------------- |
| type         | string | address             |
| address_code | string | 地址偏码 查全国的省列表固定传‘00’ |

​**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， 其它失败
message: '查询成功',             // 与返回码对应的文字
// 查全国的省， address_code为'00', 返回数据情况示例如下
data: {
  '01': '北京',
  '02': '上海',
  '25': '湖南',
  ...
  }
// 查湖南的市， address_code为'25', 返回数据情况示例如下 
data: {
  '2501': '长沙',
  '2502': '湘潭',
  '2503': '株洲',
  ...
  }
```

### 8.3 查询标签列表

**功能：** 查询标签列表。

​**方法：** get

​**参数：**

| 参数名       | 类型     | 描述                                  |
| --------- | ------ | ----------------------------------- |
| type      | string | tag                                 |
| tag_class | string | 标签分类，可选: weather, mood, status, all |

**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， 其它失败
message: '查询成功',             // 与返回码对应的文字
data: {
  'weather': ['http://xxxxxxx/sunshine.jpg', 'http://xxxxxxx/rain.jpg', ...],
}

--------上面是当class为weather或mood或status的返回，当class为all时如下返回---------------
data: {
  'weather': ['http://xxxxx/sunshine.jpg', 'http://xxxxx/rain.jpg', ...],
  'mood': ['http://xxxx/happy.jpg', 'http://xxxxx/sad.jpg', ...],
  'status': ['http://xxxx/work.jpg', 'http://xxxx/travel.jpg', ...],
}
```

## 9 系统通知

**说明:**  系统推送通知那特定一群用户，给单个用户提醒有新内容，新评论等。这部分内容将基于腾讯信鸽服务来做，具体接口后续再定。

## 10 万象优图回调接口

**接口：** http://www.patientsclub.com/we/cgi-bin/img_callback.cgi

**功能:** 本接口供腾讯云图片服务后台回调

**方法：** post

​**参数：**

| 参数名  | 类型   | 描述   |
| ---- | ---- | ---- |
|      |      |      |

**返回值：**

``` 
code: 0,                        // 返回码， 0 表示成功， 其它失败
message: '处理成功',             // 与返回码对应的文字
```
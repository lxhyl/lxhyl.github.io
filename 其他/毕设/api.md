# 菜单
 
 * url `/menu`   
 * method `get`

# 登录

* url `/login`    

* method `post`

* body   
```js
{
   account  //账号
   encryInfo // 将账号密码拼接的字符串使用sha1加密 
}
```
* eg  
```js
{
    account:'zhangpengfan',
    encryInfo:'fcc6d0f3130d2434a0f66b9d3e0a2de80b297ad7'
}
```

# 日志  
* url `/log`   
* method `get`   
* params  
```js
{
    psd:'9cba38553598029d6ce73126a0682ec98e5a0496',   // 身份验证，写死
    logName:'年份-月份'
}
```

* eg.
```js
{
    psd:'9cba38553598029d6ce73126a0682ec98e5a0496', 
    logName:'2021-2'
}
```


# 账户管理   

## 获取所有账户   

* url `/accountList`   
* method `get`
* response 
```js
{
"code": 200,
"data": [
{
"name": "傅宇辉",  
"level": 100,   // 职位
"account": "fyh123",
"id": "d4h68i1p2-6gmpwhwiyffxlt1ds/k*j05e/z0hei9&4d80*em/", // 账号id
"password": "fyh123"
},
{
"name": "测试",
"level": 1,
"account": "test",
"id": "pk+c0hsgp4awtyz*i1tfr9&hia4@$j$a&kwo1lht9gay3f/1ii",
"password": "test"
},
{
"name": "张鹏帆",
"level": 100,
"account": "lxhyl",
"id": "ykj09lnh3a1d$cyh-*03hksrgocqvas2&grj8tyezzmx*w$c3r",
"password": "lxhyl"
}
],
"msg": "成功"
}
```



## 创建账号    

* url `/signup`  
* method `post`   
* data 
```js
{
account: "test"  //账号
level: 1      // 职位枚举值
name: "测试"  // 姓名
password: "test" // 密码
}
```

## 获取当前账号信息
* url `/getUserInfo`  
* method `get`   


## 编辑账号信息
* url `/editAccount`  
* method `post`   
* data 
```js
{
account: "test"  
id: "pk+c0hsgp4awtyz*i1tfr9&hia4@$j$a&kwo1lht9gay3f/1ii"  // 账号id
level: 1
name: "测试"
password: "test"
}
```

## 删除账号
* url `/deleteAccount`  
* method `post`   
* data 
```js
{
 accountId: "q$oq/ppxcsphua9u/jm$&c0bfcbk*74wft897rtinauwmbd-p0" // 要删除的账号id
}
```

## 退出登录
* url `/logout`  
* method `get`


# 枚举值接口  
* url `enums`   
* method `get`
* params  
```js
{
    enmusKey: // 要获取的枚举值 所有值如下 
}
```

* enumsKey:level  职位
* enumsKey:status 任务状态


#  上传文件  
* url `/files`
* method `post`
* headers 
```js
{
    Content-Type: multipart/form-data;boundary=----WebKitFormBoundaryBDOt8RHQWMk5B52t
}
```
* response 
```js
{
    code:200,
    msg:'成功'
    data:{
        fileId:10   // 返回的文件id，在创建项目时需要把此id传过去
    }
}
```


# 任务相关

##  创建任务

* url `/createTask`   
* method `post`   
* body 
```js
{
    title // 任务名称  必须
    startDate // 任务开始时间  必须
    endDate // 任务结束时间  必须
    cost // 成本  必须
    responsePerson // 负责人
    phone // 联系方式  
    address // 联系地址 
    description // 描述
    fileIds // 文件ID 多个使用','分隔
    tags: text1,type1;text2,type2  // 每个标签用';'分隔,标签内容和类型用','分隔
    comment // 备注,
    fatherTask // 如果是创建子任务，需要把父任务的id传回来
    quantity // 数量  number类型
    flow // 工作流  多个工作流使用';'分隔
}
```

## 获取任务列表  
* url `/taskList`  
* method `get`   
* params 
```js
{

    status: 1 //待办列表, 2 进行中,3已完成,10 全部
}
```

## 获取所有任务
* url `/task/allList`   
* method `get`

## 任务详情
* url `/task/detail`   
* method `get`  
* params  
```js
{
    taskId //任务ID
}

* response 
```js
{
"code": 200,
"msg": "成功",
"data": {
"taskId": "y744hno2qxg930elqbcgd72qa3dj97",
"title": "ceshiwrenwu1",
"startDate": "1619107200000",
"endDate": "1619193600000",
"cost": "124",
"responsePerson": "chuonyqt83un79$n/hzddgf*jl-h$h+7pu7zf7*4-h4y77yh//",
"phone": "214",
"address": "24",
"tags": "124,danger",
"comment": "124",
"description": "描述",
"status": 1,
"createPerson": "等级4",
"createDate": "1618995701113",
"responsePersonName": "等级4",
"quantity": 1000,
"flow": "购买材料;质检材料;切割;加工;打磨;质检"
"nowFlow":"购买材料" 
}
}
```


## 改变任务状态

* url `/task/changTaskStatus`
* method `post`   
* params 
```js
{
    status:2 // 改变为进行中
    taskId //要改变的任务
}
```
## 改变进行中工作流
* url `/task/ing/changeFlow`  
* method `post`  
* params  
```js
{
    taskId //任务ID
    flow // 要变更的状态名称
}
```

## 更改任务的负责人
* url `/task/changeResPerson`
* method `post`
* data
```js
{
    taskId // 任务的id
    newPerson // 新负责人的id
}
```


# 备注 任务进度 相关

## 添加任务备注
* url `/task/changeLog`
* method `post`  
* data 
```js
{
  taskId  // 任务id
  logtext // 要添加的内容
}
```

## 获取任务备注

* url `/task/getChangeLogs` 
* method `get`
* params 
```js
{
    taskId //任务Id
}
```




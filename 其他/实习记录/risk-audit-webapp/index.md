# 9-8
9-8日要做一个项目，项目开发环境配置的时候各种问题频出0.0！小米用的自建的gitlab作为代码库，拉取项目到本地的时候一直跑不起来。  


首先安装nginx，作为代理转发服务器，以解决跨域问题。代理一直无效。最后配置了windows的host文件才有效。  
转发逻辑大概是这样： 通过vue在小米内部的url启动服务。本地的nginx服务器会监听到这个url（默认的80端口），再代理到本地的127.0.0.1（一直不生效，最后修改host文件才生效）
 
   
好不容易生效后，修改代码，视图却不更新，以为热更新有问题，手动刷新浏览器也没用，反正各种找问题测试。这个问题一直搞得晚上8：30，最后才发现是浏览器有缓存！一直访问的都是原来的数据！

# 9-9
早上看文档学习，下午开发折腾！

# 9-10

今天早上搞了菜单栏的第一栏，还有优化了反复引用同一个组件的问题。结果dialog的蒙层会遮挡住内容，最后通过dialog的append-to-body解决。

学到的东西

?> mixin 抽取公告逻辑真的很方便  

?> element的表格组件一般通过插槽来获取或者加filter来修改数据

# 9-17
开发了几天，现在看来前端mock数据真的很重要，就不用慢慢等后端接口，阻碍前端的进度。
搜索了一下，发现mockjs这个插件可以做到。通过拦截axios的请求，来自己模拟接口返回数据，很大程度降低 mock的数据和代码的耦合度。下个项目一定要用！这个项目现在有点像烂摊子了，搞得很烦。

# 9-18
  今天搞了半天的菜单数据，菜单是独立的系统配置的，这个系统会为不同角色分配不同菜单权限，好复杂啊。。。同时等待后端接口文档中...   

?> 下午2点的时候，数据部门的前端做了一次分享  

第一个是关于组件的复用封装，他们部门根据业务需求，由于要大量使用表单，所以对表单做了封装，根据json数据来生成表单。其他部门的前端也共同讨论了利与弊。我写的这个项目中一个组件中有很多input,select,radio等等，其实自己也简单抽象了一下表单。只不过只有这一个页面会用到，所以没有封装为组件，后续可以再学习下。    

同时也启发了一些思考，比如还可以通过slot去增加表单的灵活性。  

> 小总结
封装组件或者抽取公共方法数据再用mixin，都是为了简化代码，降低代码重复度，但也要适可而止，根据业务来判断使用哪种方式！ 


# 9-21  
今天早上部署的时候出现了问题，线上编译打包再部署，特别慢，于是改为本地打包直接部署，要修改.gitignore，将dist目录也上传至代码库，然后修改部署脚本，忽略编译，直接部署。但是出了点问题，直接部署dist目录会出现路径问题，解决方案是`vue.config.js`中增加`publicPath: process.env.NODE_ENV === "production" ? "./" : "/"`。再重新部署，问题解决！


# 9-22   
对了一天接口，总是各种问题频出，还好陈哥来帮我了，不然前端我一个人还真搞不定。感觉后端压力超级大，各种权限流程控制，太复杂了。   

今天要说学到的东西，就是git的一些命令了,git还是得多用多练！  
代码中一些字段，展示的值可能是汉字，但后端要的可能是数字，这时候可以用对象的健值对来对数字和汉字做一个映射，然后格式化时就会很方便。


# 9-24
搞了一天，终于把项目提测了，前后端都漏出了久违的笑容。


# 9-30  

最近几天一直改bug，都是些业务流程，接口字段不明确引起的，对业务的理解真的很重要。  

还有，就是自己代码写的太垃圾了，只是实现了功能，应该还有很多待优化的地方。
也学到挺多的。element的select，radio等标签，应该格式一致才能相匹配。

想学习下flutter，可是搞了半天环境都没部署好！收假去再搞搞！

# 10-19  

好多天没记录了，今天来记录下。  
1.代码整洁，易读的重要性，找了本代码整洁之道看了看。
2.需求是不断变化的，业务可能会很复杂，所以，组件的复用，应该视业务来判断   
3.遇到的挑战就是点亮菜单栏，虽然项目整体不大，但逻辑很复杂，要根据流程判断点亮那个导航栏，由于组件的复用，所以最初搞得有点复杂。最后是通过vuex存储要点亮的菜单，然后在导航守卫里面进行判断，给要点亮的菜单赋值。



# 10-30

最近项目不忙，写了个任务管理玩玩。todo类项目可真适合练手啊,见[todo](https://github.com/lxhyl/todo)


# 11-5

风控中间平台2.0终于部署上线了。
2.0.1的需求也做完上到测试环境了，记录下学到的vue自定义指令的运用
参见[自定义指令](前端/文章/2020/index?id=输入框千分位)

# 11-10  

今天学了下函数式编程感觉很有趣，[参见](/fun/index.md).
vuex这种单向数据流，就有点函数式的意味啊    

element的表单校验中,NaN是判定为非空的。因为要对数据转换，我没有判断是否为空就使用了parseInt函数,导致表单校验一直失败。以后要多加注意！



# 11-16     
创建项目，和回退重新创建复用一个页面，而当请求项目详情数据回显时，后端会返回此项目的完整数据，然后再提交时，就会把很多无用数据也发给后端。所有需要对数据进行过滤。  
也就是请求到的数据和发给后端的请求体求交集。


# 11-26  

有个需求，要根据后端返回数据去判断路由到那个页面。  
我的解决方案是，先建立一个index页面，在这个index页面中处理相关逻辑，文件结构如下
```js
|——detail
   |——index.vue
   |——组件A
   |——组件B
   |——组件C
   |——mock.js
   |——mixin.js
```

`index.vue`中请求到数据，再去根据数据判断加载那个组件,通过prop将数据传入组件   
`mock.js`中引入mock数据   
`mixin.js`中写组件的公共逻辑，同时引入格式化数据的mixin   







# 12-1    

获取并格式化日期时间，使用`date.toLocaleString()`很方便 


**语法**
```js
const date = new Date();

/**
 *  @param {String || Array} locales 语言环境
 *  @param {Object} options  配置返回的字符串
 */
date.toLocaleString([locales[,options]])
```

> 例1      
```
new Date().toLocaleString('zh-cn')
// 2020/12/1 下午4:34:24
```   

**options** 参数

默认都为numeric  

> 例：  
```js
new Date(2020,11,01,16,45).toLocaleString()
//  2020/12/1 下午4:45:00
```
  
  
下面表格使用`2020-12-1/16:45 星期二`为例

|       |year  |month|day|hour    |minute|weekday|
|:--:   |:---  |:---:|:-:|:--:    |:----:|:-----:|
|numeric|2020年|12月 |1日|下午4时  |45    |二     |
|2-digit|20年  |12月 |01日|下午04时|45    |   |
|narrow|       |12   |  
|short|        |12月|     |       |      |周二|
|long|         |十二月|    |      |      |星期二|


> 例2 
```js
const options = {
  year:'numeric',
  month:'short',
  day:'2-digit',
  hour:'2-digit',
  minute:'numeric',
  weekday:'long',
}
new Date(2020,11,01,16,45).toLocaleString('zh-cn',options)
// 2020年12月01日星期二 下午04:45


**补充** 
时区默认和'zh-cn'都会显示出上下午，使用'cn'则不会...
```


# 12-8  
今天遇到一个问题,后端需要自定义请求头传递参数,我一想这不是很简单吗?请求中加个headers参数不就完事了吗?  
啪一下代码就敲出来了。
```js
request({
    url: ``,
    method: '',
    headers: {
        idempotent_token: idempotent_token || '',
    },
    data,
});
```  
这时自然是打开页面测试,按传统码德来讲，我请求发出去了啊，请求头中有`idempotent_token`属性了啊.   
可返回的不是200，我大意了啊，没有管，因为现在是开发时间，以为是后端正在开发的原因。后端开发完成后，我再测还是有问题，后端说没问题，`staging`环境试了两次了，都没问题.我说有问题，控制台已经看到属性且有值了, 返回的还是系统异常。后端说我开发环境有问题。我就再测。   
仔细一想，开发环境和`staging`环境的区别是我本地多了`nginx`代理。一个谷歌搜索打出去，发现，`nginx`会将带`_`的自定义请求头过滤。   
解决方法是在`nginx.conf`中http部分添加如下配置  
```js
underscores_in_headers on;
```
好你个`nginx`，来!欺负我一个没有经验的年轻人，希望年轻人好自为之，好好学习码德.



# 12-9   

很多页面都要文件上传，所以可以将文件类型提取出来。  
新增文件类型文件,并导出为数组格式。   
```js
export default [
  "pdf",
  "jpg",
  "jpeg",
  "jpe",
  "png",
  "rar",
  "tar",
  "zip",
  "doc",
  "docx",
  "xls",
  "xlsx",
  "ppt",
  "pptx",
];
```   
然后判断文件类型是否符合条件时就可以使用`array.includes`，或者`array.indexOf`.    

也可供提示使用,如下：    
![fileTypes](https://raw.githubusercontent.com/lxhyl/lxhyl.github.io/master/files/img/fileTypes.jpg)



# 12-15   

学习到了个解决bug的方法. 

vue项目,使用的是element-ui,当数据更新页面不更新时,要查看数据是否设置了`getter`和`setter`.如果没有，说明vue没有监听到此数据。解决方法是使用`Vue.set`给赋值，而不是直接`=`赋值。   

**语法**     
`Vue.set( target, propertyName/index, value )`



写个了demo测试了下
```html
 <input v-model="test.input" />
  <div v-for="radio in radioOption" :key="radio.value">
      <input
        type="radio"
        v-model="test.radio"
        name="testradio"
        :value="radio.value"
      />
      <label>{{ radio.label }}</label>
  </div>
  <el-input v-model="test.inputel"></el-input>
  <el-radio-group v-model="test.radioel">
      <el-radio
        v-for="radio in radioOption"
        :key="radio.value"
        :label="radio.label"
        :value="radio.value"
      ></el-radio>
  </el-radio-group>
```

```js
 data() {
    return {
      test: {},
      radioOption: [
        { label: "test1", value: 1 },
        { label: "test2", value: 2 },
      ],
    };
  },
  created() {
    this.test.input = null;
    this.test.radio = null;
    // this.test.inputel = null;
    this.$set(this.test, "inputel", null);
    this.test.radioel = null;
  },
```

发现，原生是可以正常工作的。elment-ui在初始化时不可以直接设置为null.必须通过`Vue.set`才可以正常工作。   


# 12-18  

知道为啥vue强调单向数据流了，周五因为数据流混乱导致后端找了俩小时bug。

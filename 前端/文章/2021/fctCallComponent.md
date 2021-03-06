> 前言     

一个vue项目中有大量要使用弹窗的情况，一些很小的二次确认产品为了让设计统一也让使用弹窗组件，而每个弹窗都要维护其相应的状态，所以有的场景就很麻烦。 而函数式调用就很方便。   

项目使用的是ant-design-vue

## 目录结构 
```
|--Dialog
   |--src
      |--index.js  // 实例化组件
      |--dialog.js  // 组件的dom
   |--index.js  // 挂载组件
```

## 组件的dom
```js
export default {
  data() {
    return {
      visible: true
    }
  },
  computed: {
    // 传入content 的类型
    type() {
      return typeof this.props.content   
    },
    // 如果传入了个组件，可以获取其vNode 调用其上的方法或者获取其数据
    contentNode() {
      if (this.type === 'object') {
        return this.renderContent().componentInstance
      }
    }
  },
  methods: {
    close() {
      this.visible = false
    },
    open() {
      this.visible = true
    },
    renderContent() {
      if (!this.props.content) return ''
      if (this.type === 'string' || this.type === 'object') return this.props.content
      return this.props.content()
    }
  },
  render() {
    return <a-modal
      visible={this.visible}  // 控制打开关闭
      props={{ ...this.props.attr }} // 属性
      on={{ ...this.props.methods }} // 方法
    >
      {this.renderContent()}
    </a-modal>
  }
}
```

很简单，只有一个`visible`用于维护弹窗的关闭打开。 其他参数只是相当于在此做了转发,最后都会传入`a-modal`组件里.  

`content`用于渲染内容，可以使用字符串，方法,jsx，或者直接传入一个组件   

而`title`,`footer`等和平常使用`a-modal`一样。  

## 实例化组件
```js
import Vue from 'vue'
import Dialog from './dialog'

const Node = Vue.extend(Dialog)

const createCom = options => {
  return new Node({
    el: document.createElement('div'), // 挂载节点
    data: () => ({ props: options })  // 此data会传入dialog组件中
  })
}

export default createCom
```

## 挂载组件  

`index.js`  
```js
import component from './src/index.js'

export default {
  install(Vue) {
    Vue.prototype.$dialog = component
  }
}
```


`main.js`  
```js
import Dialog from './components/Dialog/index'
Vue.use(Dialog)
```
Vue.use会调用上面的install方法将组件挂载到vue原型上


## 使用
```js
 import DeleteConfirm from './deleteConfirm'
 const confirmNode = this.$dialog({
        // 传入一些属性
        attr: {
          destroyOnClose: true,
          title: `title`
        },
        methods: {
          cancel() {
            confirmNode.close()
          },
          ok() {
            // 调用DeleteConfirm 组件上的submit方法
            confirmNode.contentNode.submit() 
          }
        },
        content: <DeleteConfirm data={{id:'test'}}/>
      })
 }
```
content是可以直接传入一个组件的,所以复杂场景也能hold住。  

`this.$dialog({})`会返回组件的vNode，可以直接调用上面的`close`,`open`等方法

封装其他组件也是同理的
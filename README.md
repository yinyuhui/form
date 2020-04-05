# i-form

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).


## 1.1 表单构成

表单由三部分构成

- form（绑定表单数据，校验规则，提供validate方法）
- formItem（进行数据校验，展示错误提示）
- Input等（数据双向绑定，数据变化时通知formItem校验）

## 1.2 Input 实现

数据双向绑定 

-  `:value="value"` 绑定 value

- ` @input` 监听 input 事件，并将输入值 emit 出去即实现数据双向绑定

输入内容变化时通知 formItem 进行校验

input 中 `this.$parent.$emit("validate", value)`

## 1.3 FormItem 实现

formItem 主要做的就是对数据进行校验以及展示提示信息，校验借助`async-validator`库

### 1.3.1 校验原理

通过从form中获取的校验规则，从input中获取的输入值，自身上绑定的prop值三者，实现校验逻辑

- 获取rules：可以通过`this.$parent` 获取rules， 也可以通过`provide`，`inject`获取rules
- 获取输入值：input 中已经emit 出来了，只需要在formItem 的 created 中`$on`响应即可，`this.$on('validate', value)`
- prop值是通过props直接传递进来的，用于读取rules的关键字

### 1.3.2 validate 方法实现

借助`async-validator`库，我们需要

1. 实例化一个描述器，用来表明校验关键字及校验规则
2. 实例化一个schema，作为校验器，执行它的validate方法
3. 传入校验内容和校验之后执行的回调，回调中根据校验状态修改是否展示提示信息，修改样式等

由于需要在form实例中暴露一个validate方法，用于判断整个表单是否校验通过，因此需要额外处理一下，将上面方法体包在Promise中，resolve校验结果

在组件 created 钩子中， 发现该表单项需要校验，则将整个this对象暴露给父组件

## 1.4 Form 实现

接收两个参数，一个是rules，用于formItem中获取校验规则，另一个是modal，用于整个表单校验时获取表单数据

在formItem中暴露了需要校验的实例对象，在form中接收这些对象，并存储起来

在form中也要实现一个validate方法，用于校验所有需要校验的表单项，并将校验结果暴露出去

1. 首先我们拿到了所有需要校验的实力对象，遍历并执行他们的校验方法之后可以得到所有的promise对象
2. promise.all 执行这些 promise 对象，可以得到校验结果数组
3. 对全部通过时通过校验，否则校验失败。执行回调函数，把校验结果通过回调参数暴露出去

## 1.5 代码实现

完成简单示例，表单项仅实现input，仓库地址：

https://github.com/yinyuhui/form

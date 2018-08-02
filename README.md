# GentleFunBlog/client
My Blog react采矿记录，持续更新中...喜欢请Star~ 有误的地方欢迎指正~
## react-router-dom 
    
- 在 index.js中加入 `BrowserRouter` 或者 `HashRouter` ,子路由使用Route标签，嵌套路由的实现需要在子页面添加Route标签；
- 在App.js中使用Switch 表示此页面只允许渲染一个子路由；
- 在Route中添加exact表示精确匹配路由；


## 引入scss

- ` npm i sass-loader node-sass -D ` 安装解析器
- 在 `/node_modules/react-scripts/config/webpack.config.dev.js ` 和 ` node_modules/react-scripts/config/webpack/prod.js ` 中添加以下代码：

```js
    {
        test:/.\scss$/,
        loader:['style-loader','css-loader','sass-loader']
    },
    {
        exclude:[/.\(js|jsx|jsm)$/,/\.html$/,/\.scss/],
        loader:require.resolve('file-loader'),
        options:{
            name:'static/media/[name].[hash:8].[ext]',
        }
    }
```
## 引入国际化 react-intl-universal 

- `npm i react-intl-universal --save`
- 新建国际化文件 en-US.js 格式类似于：
```js
    module.exports={
        title:'Title'
    }
```
- 在 `App.js`中引用 `import intl form 'react-intl-universal' `，定义` state={initDone:true}` 并在`render()` 对组件进行条件渲染：
```js
    render(){
        return(
            this.state.initDone &&
            <Switch>
                ···
            </Switch>
        )
    }
```
- 引用国际化文件：
```js
    const locales={
        'zh-CN':require('./lang/zh-CN.js'),
        'en-US':require('./lang/en-US.js')
    }
    
```

- 在`componentDidMount`中初始化`intl` :
  
```js
   intl.init({
       currentLocale:'en-US',
       locales:locales
   }).then(()=>{
       this.setState({initDone:true})
   }).catch(()=>{
       this.setState({initDone:true})
   })
```
> 在上面的代码中，`intl.init`方法使用了Promise获取了放在CDN上的日期国际化文件，如果是内网环境，修改 `react-intl-universal` 中的`init`方法代码或者将 `COMMON_LOCALE_DATE_URLS`中的地址改为内网即可。使用方法如`intl.get('title')` ,更多使用方法可参考官方说明[alibaba/react-intl-universal](https://github.com/alibaba/react-intl-universal?spm=a2c4e.11153940.blogcont72332.5.55185d3dFUeNsB)

## 引入ant-design

- `npm i ant-design --save`
- 在`index.css`中引用样式文件`@import '~antd/dist/antd.css'`
- 设置本地引用`iconfont`，首先下载资源文件 [iconfont](https://ant.design/docs/spec/download-cn)，将文件解压到`node_modules/ant-design/iconfont` 修改文件`node_modules/antd/dist/antd.css`和`node_modules/antd/lib/style/index.css`中 `@font-face`下对应的路径改为本地文件路径`../iconfont/...` 即可

## antd的自定义表单验证

- 新建`validatorItem`对象
```js
    const validatiorItem={
        rules:[{
            validator(rule,values,callback){
                //判断values，有误则使用回调输出错误信息,正确则直接callback()
                callback('输入有误！'）
            }
        }]
    }
```
- 使用时 
```js
    <FormItem>
        {
            getFiledDecorator('Account',validatorItem)(
                <Input />
            )
        }
    </FormItem>
```

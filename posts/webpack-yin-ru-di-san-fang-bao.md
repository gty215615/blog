---
title: 'webpack引入第三方包'
date: 2019-11-08 19:29:29
tags: [webpack,自动化工具]
published: true
hideInList: true
feature: https://tva3.sinaimg.cn/large/0072Vf1pgy1foxkiak921j31kw0w0nnl.jpg
isTop: false
---
> 使用webpack引入第三方包，可以使用webpack内部自带的插件ProvidePlugin来实现

<!-- more -->

### 1.使用webpack.ProvidePlugin来实现

webpack.ProvidePlugin会将引入的第三方包加载到每一个模块内，所以当此时调用wendow.$时会出现undefined，只需要在webpack.config.js文件中引入webpack再在plugins中添加即可
			
```javascript
const webpack = require("webpack");
	module.exports = {
    ...
    plugins: [
        new webpack.ProvidePlugin({
            $: 'jquery',
            jQuery: 'jquery'
        })
    ],
		  ...
}
```

## fis-command-webfont

---


## 背景与简介

目前移动端webfont字体使用越来越广泛，由于缺少比较好的自动化工具，开发者在修改字体图标时需要在2个平台进行转换(svg转ttf再转woff2,woff2普遍不支持)才能完成字体生成工作。

经过调研，基于[grunt-webfont](https://github.com/sapegin/grunt-webfont)改造成fis插件，实现一键转换svg图标为svg,oet,ttf,woff,woff2的功能。

由于开源模块大量使用了异步调用，所以暂未改造成fis release 插件，先用command命令来实现自动化需求。

您可以通过第三方平台来了解字体生成与转换过程: https://icomoon.io/app/#/select/font 、 https://everythingfonts.com/#

## 开始使用

### 安装插件

执行 `npm install -g fis-command-webfont` 全局安装

### 配置插件

在fis-conf.js里面添加配置：


```javascript

fis.config.set("webfont",{
    'src'       : '/static/fonts/icons',//图标目录
    'dest'      : '/static/fonts',  //产出字体目录
    'fontname'  : 'zuoye_font', //产出字体名称
    'order'     : 'name' //name或者time //图标按名称还是按修改时间排序，默认按名称排序
});

```

`注意`：每个图标都会递增生成对应的uinicode，为了避免更新后编码变动，建议icon按字母顺序加前缀并按名称排序

您也可以通过命令行方式传递参数，具体见`fis webfont -h`。

### 生成字体

在fis模块根目录执行`fis webfont`即可完成字体转换工作，字体将生成更新到dest目录里(原有同名字体将删除)。


## WOFF2问题

WOFF2作为新一代字体标准比woff有明显的优势(大小减小30%)，但浏览器支持度低，具体见[caniuse](http://caniuse.com/#search=woff2)

目前woff2的字体转换工具很少，没有相应的npm模块。Google提供的[方案](https://github.com/google/woff2)也在开发中且不支持windows，如果您想生成woff2字体，需要在本机(mac或linux)安装Google的转换工具：

```
git clone https://github.com/google/woff2.git
cd woff2
git submodule init
git submodule update
make clean all
```

注意：编译完之后会在目录内产生`woff2_compress`和`woff2_decompress`，请将文件copy到PATH目录(mac上为`/usr/local/bin`)或者将当前目录添加到PATH中。

如果您不需要产出woff2字体，请在fis-conf.js配置里面添加一项指定产出字体:

```
types : 'svg,eot,woff,ttf'

```



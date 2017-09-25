---
layout: post
title:  "Compass入门"
categories: CSS
date:   2017-09-25 15:48:05
author: Logan
tags:  compass
---

* content
{:toc}

# compass安装

`gem install compass`

***

# 项目初始化及编译

**创建一个Compass项目**

`compass create myProject`

创建完之后进入该目录

`cd myProject`

- `config.rb` ： 项目的配置文件
- `sass` ： sass源文件存放目录
- `css` ： css源文件存放目录

**编译**

- Compass的编译命令是
    `compass compile`
- Compass编译生产环境需要压缩后的css文件
    `compass compile --output-style compressed`
- Compass只编译发生变动的文件，如果要重新编译未变动的文件
    `compass compile --force`
- Compass自动编译命令
    `compass watch`

> `.scss`文件转换为`.sass`文件<br>
> `sass-convert main.scss msin.sass`





***

# Compass 核心模块

- `reset`模块： 重置样式，可用`normalize`代替
- `layout`模块： 页面布局控制能力，使用较少
- `CSS3`模块： 提供跨浏览器的CSS3能力
- `Typography`模块： 修饰文本样式
- `Utilities`模块： 提供某些不属于其他模块的功能，多为`mixin`

> 以下两个模块需要指定引入<br>
> `@import "compass/reset"`<br>
> `@import "compass/layout"`<br>

## CSS3模块

**引入**

```sass
@import "compass/css3";
```

**调用**

```sass
// 边框阴影
@include box-shadow(1px 1px 5px #000);
// 动画
@include animation(myani 10s ease-in-out 2s infinite alternate);
// 背景绘制区域
@include background-clip(padding-box);
// 背景图像定位
@include background-origin(padding-box);
// 边框圆角
@include border-radius(3px);
// 透明度
@include opacity(.5);
// 转换
@include transform(translate(20px,50px));
// 过渡
@include transition(all 2s ease-in-out 1s);
// 行内区块
@include inline-block;
// 渐变
@include background(linear-gradient( #333, #0c0));
// placeholder
input[type="text"] {
  @include input-placeholder {
    color: #bfbfbf;
    font-style: italic;
  }
}
```


## Browser Support 模块

**引入**

```sass
@import "compass/support";
```

**配置**

```sass
// 支持的浏览器
$supported-browsers: chrome, firefox, ie, opera, safari;
// 支持浏览器的最低版本
$browser-minimum-versions: ("ie": "8","chrome": "12");
```

## Typography 模块

**引入**

```sass
@import "compass/typography";
```

**修改不同状态下链接颜色**

语法： `link-colors($normal, $hover, $active, $visited, $focus);`

```sass
a {
    @include link-colors(#00c, #0cc, #c0c, #ccc, #cc0);
}
```

## Utilities 模块

### 精灵图合图

- 第一步： 把所有需要合图文件放在`images`文件夹下的新建`icons`文件夹内
- 第二步： 创建一个`_icons.scss`文件并引入`screen.scss`文件内
    
    ```sass
    @import "icons";
    ```

- 第三步： 在`_icons.scss`文件中引入`sprites`模块

    ```sass
    @import "compass/utilities/sprites";
    ```

- 第四步： 在`_icons.scss`文件中引入`icons`文件夹内的所有`png`图片

    ```sass
    @import "icons/*.png";
    ```

    > 此时会在icons文件夹同级生成一个精灵图

- 第五步： 调用`mixin` `all-icons-sprites()`

    ```sass
    @include all-icons-sprites();
    ```

    > 此时会在编译后的css文件内自动生成与文件名相关的类名并设置好`background-position`

    > 在命令行输入`compass sprite "icons/*.png"` 可以看到合图时生成的`.scss`文件

- 第六步： 在想设置精灵图的类名下调用`mixin` `icons-sprite("imgName")`

    ```sass
    .icons__item{
        @include icons-sprite("alipay");
    }
    ```

- 说明： 文件命名时如果存在`xxx.png`和`xxx_active.png`和`xxx_hover.png`，则会自动生成hover和active状态下的css

### 变量配置

> 变量配置设置要在`_icons.scss`最顶部进行，为了重置默认变量

- `$icons-sprite-dimensions` : 设置变量`$icons-sprite-dimensions`值为`true`，则在相应类名下生成相应图片宽高，默认为`false`

    ```sass
    $icons-sprite-dimensions: true;
    ```

- `$icons-layout` : 设置合图方向，值有`vertical`和`horizontal`和`smart`，默认为`vertical`

    ```sass
    $icons-layout: smart;
    ```

- `$disable-magic-sprite-seletors` : 设置是否不智能生成`:hover`和`:active`伪类，值有`false`和`true`，默认为`false`

    ```sass
    $disable-magic-sprite-seletors: true;
    ```

## Helps函数

- `image-width()`函数：返回图片的宽
- `image-height()`函数：返回图片的高
- `inline-image()`函数：将图片转为data协议的数据，消除HTTP请求，对小图像来说是性能优化，但生成的CSS文件更大

```sass
$width: image-width('alipay.png');
$height: image-height('alipay.png');
.ali {
    background-image: inline-image('alipay.png');
    width: $width;
    height: $height;
}
```

***

# 图标适配retina屏幕

- 第一步： 在`sass`文件夹内放入`_retian-sprites.scss`文件
- 第二步： 在`_icons.scss`中引入`_retian-sprites.scss`文件

    ```sass
    @import "retina-sprites";
    ```

- 第三步： 在`images`文件夹下新建`retina-version`文件夹
- 第四步： 在`retina-version`文件夹下新建`sprites`文件夹和`sprites-vertion`文件夹
- 第五步： 在相应文件夹内放入相应图片
- 第六步： 在`_icons.scss`中配置变量

    ```sass
    $sprites: sprite-map("retina-version/sprite/*.png");
    $sprites2x: sprite-map("retina-version/sprite-retina/*.png");
    ```

- 第七步： 在`_icons.scss`中调用`mixin`

    ```sass
    .icon-alipay{
        @include retina-sprite(alipay[,true,true]);
    }
    ```

    > 第二个参数为可选参数，设置为`true`则会生成`:hover`样式<br>
    > 第三个参数为可选参数，设置为`true`则会生成`:active`样式
---
title: CSS梳理
date: 2022-08-10 19:53:23
tags: CSS
categories: 前端基础
cover: /img/03.jpg
---
# CSS：层叠样式表
    由选择符和声明组成

## 样式引入
    内部样式：<style></style>标签，一般在head标签内
        副作用：
    外部样式：(结构样式分离，推荐使用)
        link标签：
            rel：'stylesheet'
            type：'text/css'
            href：路径

            区别：
                属于XHTML标签
                link引用的CSS会同时被加载
        style标签：
            @import url("路径")

            区别：
                CSS提供的一种方式
                全部下载完再被加载
                出来的时间有点老，只在IE5以上才能识别

    行内样式：
        style作为属性直接写在标签后面

    样式优先级(就近原则)
        内联样式最优先；外部和内部一样，看谁覆盖，一般先写外部再写内部
        ！important：开挂加权重

## 选择器
    标签选择器（element选择器）
        eg：div{属性名：属性值}
        以文档语言对象作为选择器，即使用结构总中元素名称作为选择符
    类选择器
        eg：.class名{属性：属性值}

        <style>
            a{
                color:red
            }
            b{
                color:yellow
            }
        </style>
        <div class="a b"></div>
        标签里的属性可设置多个，样式优先级看style里的属性位置
    id选择器
        #id名{属性：属性值}
        唯一性
    通配符选择器
        *{属性：属性值}
        一般用来格式化内外边距
    群组选择器
        , 合并选择器，节约代码量
    后代选择器
        空格
    伪类选择器
        :link{属性：属性值} 初始状态
        :visited{属性：属性值} 被访问后状态
        :hover{属性：属性值} 鼠标悬浮状态
        :active{属性：属性值} 激活时状态，即鼠标按下状态
        书写顺序：link-visited-hover-active
    选择器的权重
        !important      10000
        内联            1000
        id              100
        class           10
        元素(标签)       1
        包含选择符      权重之和比较，但不会越级

    规则
        当不同选择符的样式设置有冲突的时候，高权重选择符的样式会覆盖低权重选择符的样式
        相同权重的选择符，样式遵循就近原则：那个选择符最后定义，就采用那个选择符样式

## 文本属性：font
        font-size：字体大小;单位是px，浏览器默认是16px，设计图常用字号是12px
        font-family：字体;
        color:字体颜色
        font-weight：加粗
        font-style：倾斜 italic(倾斜)/oblique(倾斜的字体)/normal（正常）
        text-align：文本水平对齐
            left
            right
            center
            justify
        line-height：行高
        text-indent:首行缩进，只对第一行起作用
        letter-spacing：字间距
        word-spacing:词间距
        text-decoration：文本修饰
        text-transform：文本大小写转换
        font：文字简写
            #font-style  #font-weight font-size/line-height font-family（加#可省略）

## 列表属性：list-style
    list-style-type：定义列表符合样式
        disc，circle，square，none
    list-style-image：将图片设置为列表符合样式
        url();
    list-style-position：设置列表项放置位置是否在li标签内
        outside
        inside
    list-style：简写
          list-style-type list-style-image  list-style-position;顺序无所谓

## 背景属性：background
    background-color：背景颜色
    background-image：背景图片
        url()
    background-repeat：背景图片的平铺
        repeat/norepeat/repeat-x/repeat-y
    background-position：背景图片的定位
        上 右
    background-size：图片大小
        cover把背景图形扩展至足够大，以使背景图形完全覆盖背景区域，有可能会裁掉一部分
        contain：把图像等比例放置最大
    background-attachment：背景图片的固定方式   一般用于视觉差
        fixed：相对于浏览器视口固定
        scroll：相对于网页整体固定
    background:color image repeat position attachment;顺序无所谓，可单独设置某一个值

## 浮动属性：float
    属性
        left
        right
        none
    作用
        定义网页中其他文本如何环绕该元素显示
        就是让竖着的东西横着来
        脱离默认文档流
        见缝插针
    副作用
        高度塌陷：由于子元素脱离默认文档流而撑不起来父元素
    解决方案：
        1. 父元素写死宽高
        2. overflow：hidden
            引发BFC，让浮动元素重新计算高度
        3. 清除浮动
            设置于浮动元素之后
            clear：none/both/left/right

## 盒子模型
    W3C盒模型
        上下左右
        上下 左右
        上 左右 下
        上 右 下 左

        内边距
            padding
            不支持负数
        边框
            border
                border-width
                border-color
                border-style
        外边距
            margin
            支持负数
            子盒子不知道父元素的边距
            1.子元素的margin给父盒子的padding，注意高度计算
            2.给父元素设置边框
            3.加浮动，父子加皆可
            4.overflow：hidden BFC

## 溢出属性
    overflow
        visible：默认值，溢出内容显示在元素之外
        hidden：溢出内容隐藏
        scroll：滚动
        auto：如果溢出加滚动条，没有溢出正常显示
        inherit：规定应该遵从父元素继承overflow属性的值
        overflow-x：X轴溢出
        overflow-y：Y轴溢出
    white-space
        normal：忽略空白
        nowrap：不折行
        pre：保留标签内文本间隔样式
        pre-wrap：保留标签内折行
        pre-line：不保留空格
        inherit：继承
    text-overflow
        clip：默认，不显示省略号
        ellipsis：省略号




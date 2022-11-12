---
title: HTML梳理
date: 2022-08-01 19:53:23
tags: HTML
categories: 前端基础
cover: /img/02.jpg
---

**HTML 全称：'Hyper Text Markup Language'(超文本标记语言)**

HTML的基本语法：  
    1.常规标记  
    2.空标记

# HTML语义化
1. 样式丢失时还能保持结构
2. 利于爬虫SEO搜索
3. 方便阅读维护


# DOCTYPE
文档声明标签   
`<!DOCTYPE html>`

# html标签
`<html lang='en'></html>`  
'en':英文; 'zh-CN':中文; 'ja-jp':日文

# meta标签
`<meta charset='***'>`
UTF-8

# 常用标签

1. 文本标签 \<hn>\</hn>
2. 段落标签 \<p>\</p> 
3. 换行标签 \<br/>
4. 水平线标签 \<hr/> <hr/>
5. 文字加粗标签 
    \<b>\</b> <b>b标签</b>
    \<strong>\</strong> <strong>strong标签</strong> 
6. 倾斜标签
    \<em>\</em> <em>em标签</em>
    \<i>\</i> <i>i标签</i> 
7. 删除线标签
    \<s>\</s> <s>s标签</s>
    \<del>\</del> <del>del标签</del> 
8. 扩展
    \<u>\</u> <u>下划线</u>
    \<sup>\</sup> <sup>上标</sup> 
    \<sub>\</sub> <sub>下标</sub> 

# 特殊符号
    &lt;   :  <
    &gt;   :  >
    &nbsp; :  空格 宽度受字体影响
    &emsp; :  空格 宽度为一个中文的宽度
    &copy; :  版权
    &trade :  商标
    &reg;  :  商标
    
# div和span
    div：块级元素
        没有具体含义，用来划分页面的区域，独占一行
        自上而下排列
        可定义宽高
        eg：div p ul li ol dl dt dd h1-6
    span：行内元素
        没有实际意义，主要用在文本样式修改时候，内容有多宽就占用多宽的空间距离
        不可定义宽高
        margin、padding的left、right可以设置
        eg：a b em i span strong等
    行内元素：
        eg： img input


# 列表
    有序列表(order list)
        ol>li
        ol里面可以随意放标签，但是ol里面只能放置li
        数字是自动生成的
        属性
            type:1,a,A,i,I
            start:取值只能是一个数字
    无序列表(Unordered list)
        ul>li
        ul里面只能放li，li里面可以放其他标签
        默认是黑色的实心圆
        属性：
            type：disc，circle，square，none
    自定义列表
        dl>dt|dd

# 图片标签的路径
    img标签
    src
        绝对路径是指文件在硬盘上真正存在的路径
        相对路径就是相对于自己的目标文件位置

# 图片标签的属性
    src：路径信息
    title：鼠标悬停上去的提示信息
    alt：图片加载失败的提示信息
    width：宽度
    height：高度

# 超链接标签
    实现不同页面的跳转
    属性
        href:跳转链接
        title：鼠标悬浮提示
        target：打开方式
            _self:默认值，本页面打开
            _blank:新窗口打开

# table表格
     tr:行(table row)
     td:列
    表格属性
        width：宽度
        height：高度
        border：边框
        bordercolor：边框颜色
        bgcolor：背景颜色
        align：水平对齐
        cellspacing：单元格之间的间距
        cellpadding：单元格内边距
    tr属性(width不生效)
        height：高度
        bgcolor：背景颜色
        align：文字水平对齐
        valign：文字垂直对齐
    td属性
        width：宽度 （影响一整列）
        height：高度 （影响一整行）
        bgcolor：背景颜色
        align：文字水平对齐
        valign：文字垂直对齐
    表格合并
        colspan：合并列数，必须是td
        rowspan：合并行数，必须是td
    表单标签
        form标签
            action：传输数据地址
            method：方法post、get
                get
                    是从服务器上获取数据
                    把参数数据队列加到提交表单的ACTION所指的URL中，值和表单内字段一一对应，在URL中可以看到。
                    Request.QueryString获取
                    传输数据量小，不能大于2KB
                    安全性低，效率好
                post
                    是向服务器传送数据
                    通过HTTP post机制，将表单内各个字段与其内容放置在HTML HEADER内一起传送到ACTION所指的URL地址，用户看不见整个过程。
                    Request.Query获取
                    数据量大，最大80KB
                    安全性高
            input标签
                type：
                    text:文本框
                    submit：提交框
                    button：按钮框
                    reset：重置框
                    password：密码框
                placeholder：描述输入字段预期值的简短的提示信息兼容到IE8以上
                name：必须设置，后端获取的key值
                value：按钮显示值

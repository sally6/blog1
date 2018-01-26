---
layout: post
title: "Google Maps JavaScript API"
date: 2018-01-23
excerpt: "调用Google地图API方式；标记类聚；获取静态地图"
tags: [Google Maps, 静态地图]
comments: true
---

###### API接口说明文档

- [Google Maps JavaScript API帮助文档](https://developers.google.cn/maps/documentation/javascript/tutorial)

- [菜鸟教程 Google 地图API](http://www.runoob.com/googleapi/)

google地图在web网页中的应用在帮助文档中已经有具体说明，这里不再多说。接下来主要讲解下在调用google 地图过程中需要注意的地方：

###### 在国内调用API方式

```html
<script src="//maps.google.cn/maps/api/js?key=你的秘钥&language=zh-cn" type="text/javascript"></script>
```
文档中使用的API链接在国内无法访问的，可以将域名改为`maps.google.cn`即可正常访问

---
###### 标记类聚
在有些地图API中也叫点聚合，类似效果如图
![Google maps 标记类聚](/images/map/map.png)

GitHub 上的 [Google 地图存储区](https://github.com/googlemaps/js-marker-clusterer/blob/gh-pages/src/markerclusterer.js)提供了 `MarkerClusterer` 的 JavaScript 内容库和图片文件。将 GitHub 上的下列文件下载或复制到一个应用可访问的位置：
-  [markerclusterer.js](https://github.com/googlemaps/js-marker-clusterer/blob/gh-pages/src/markerclusterer.js)
-  [m1.png](https://github.com/googlemaps/js-marker-clusterer/blob/gh-pages/images/m1.png)
-  [m2.png](https://github.com/googlemaps/js-marker-clusterer/blob/gh-pages/images/m2.png)
-  [m3.png](https://github.com/googlemaps/js-marker-clusterer/blob/gh-pages/images/m3.png)
-  [m4.png](https://github.com/googlemaps/js-marker-clusterer/blob/gh-pages/images/m4.png)
-  [m5.png](https://github.com/googlemaps/js-marker-clusterer/blob/gh-pages/images/m5.png)

示例网站：[奥玛物流仓储联盟-地图找仓](http://www.oym56lm.com/ckgy/map.html)

---

###### 获取静态地图
以下内容来源于[csdn博客](http://blog.csdn.net/u011393661/article/details/14130109)
，API域名稍作调整

API接口：http://maps.google.cn/maps/api/staticmap

参数详解

参数分为几类：

- 位置参数：如center, zoom，没有标记（markers）时为必填。
- 地图参数：如size（必填）, format, maptype, language。
- 特征参数：如markers, path, visible, style。
- 报告参数：如seneor（是否使用传感器定位用户）。

<kbd>center</kbd>

定义地图中心，到地图各边缘的距离等。格式可以是{纬度,经度}或字符串地址，可以唯一标识地球表面的具体位置。

纬度和精度可以精确到6位小数，纬度的范围是[-90, 90]，经度的范围是[-180, 180]。经度值基于到英国格林威治（本初子午线所在地）的位置。

字符串地址的格式如 City Hall,New York,NY 的形式。发送请求前需使用字符串转义，使之编码为如 City+Hall,New+York,NY 的形式。

例如显示以北京为中心的一个静态地图：

![北京地图](/images/map/map_1.png)

代码：
```html
<img src="http://maps.google.cn/maps/api/staticmap?center=Beijing&zoom=9&size=256x256&maptype=roadmap&language=zh-CN&sensor=false">
```
<kbd>zoom</kbd>

定义地图缩放级别，指定当前试图的分辨率，取值范围是[0,21+]，0表示最低缩放，在地图上可见整个世界，21+可以看到建筑物个体。

<kbd>size</kbd>

指定地图图片矩形尺寸，使用 widthxheight 的形式。大小限制为 640x640。

<kbd>format</kbd>

指定图片格式，默认用PNG图像，可选格式包括：png8/png（默认）、png32, gif, jpg, jpg-baseline（非渐进式JPEG压缩格式）。

png/gif是无损压缩，jpg/jpg-baseline是有损压缩。大多数JPEG图像采用渐进式载入，即先载入较为粗糙的图像，再随着更多数据的传入而提高图像分辨率，这样可以快速加载网页。但JPEG的某些应用，例如打印要求非渐进式（基线）图片，此时需要选择jpg-baseline格式。

<kbd>maptype</kbd>

定义地图类型，其值可以是：

- `roadmap`（默认）指定标准路线图，如Google Maps网站的默认显示。
- `satellite` 指定卫星图片。
- `terrain` 指定自然地形地图，显示地形和植被。
- `hybrid` 指定卫星和路线图的混合图片，在卫星图片上显示主要街道和地址名称的透明层。

<kbd>language</kbd>

指定地图上标记的显示语言。

<kbd>markers</kbd>

在指定位置添加标记。可以有多个markers。一个markers中的多个参数用&#124;（%7C）分隔。多个标记只要样式相同，就可以放置在同一个markers参数中。如果指定了markers，则可以无需指定center/zoom参数。

例如以故宫的经纬度获取地图：

![标记故宫](/images/map/map_2.png)
![标记故宫](/images/map/map_3.png)

代码：
```
http://maps.google.cn/maps/api/staticmap?zoom=13&size=256x256&markers=39.917110,116.396971&maptype=roadmap&language=zh-CN&sensor=false
 
http://maps.google.cn/maps/api/staticmap?zoom=13&size=256x256&markers=color:blue%7Clabel:G%7C39.917110,116.396971&maptype=roadmap&language=zh-CN&sensor=false
```

<kbd>path</kbd>

定义图片上叠加层的两个或多个连接点的单条路径。格式为：

path=pathStyles|pathLocation1|pathLocation2|...
其中pathStyles的参数为：

- `weight` 指定路径宽度（以像素为单位），默认5个像素。
- `color` 以24位或32位十六进制指定颜色，或从集合 {black, brown, green, purple, yellow, blue, gray, orange, red, white} 中指定一种颜色。使用32位十六进制，最后2个字符指定8位的Alpha透明值，范围从00（完全透明）到FF（完全不透明）。pathStyles支持透明度，但markers不支持。
- `fillcolor` 闭合区域的填充色。
例如划一道从北京到成都的线：

![北京到成都](/images/map/map_4.png)

代码（以经纬度和字符串形式绘制）：
```
http://maps.google.cn/maps/api/staticmap?zoom=4&size=256x256&path=39.917110,116.396971%7C30.665629,104.064978&maptype=roadmap&language=zh-CN&sensor=false
 
http://maps.google.cn/maps/api/staticmap?zoom=4&size=256x256&path=Beijing%7CChengdu&maptype=roadmap&language=zh-CN&sensor=false
```
北京-成都-上海的连线：

![北京-成都-上海的连线](/images/map/map_5.png)
![北京-成都-上海的连线](/images/map/map_6.png)

代码（分别用默认样式与自定义样式绘制）：
```
http://maps.google.cn/maps/api/staticmap?zoom=4&size=256x256&path=Beijing%7CChengdu&path=Chengdu%7CShanghai&path=Shanghai%7CBeijing&maptype=roadmap&language=zh-CN&sensor=false
 
http://maps.google.cn/maps/api/staticmap?zoom=4&size=256x256&path=color:red%7CBeijing%7CChengdu&path=color:purple%7CChengdu%7CShanghai&path=Shanghai%7CBeijing&maptype=roadmap&language=zh-CN&sensor=false
```

使用填充色。例如绘制上海的经济辐射区域：

Google静态地图API详解 - ShaneJhu - 逆风的方向 更适合飞翔

![绘制上海的经济辐射区域](/images/map/map_7.png)

代码：
```
http://maps.google.com/maps/api/staticmap?zoom=6&size=256x256&path=fillcolor:yellow%7CShanghai%7CYangzhou%7CNanjing%7CHangzhou%7CShanghai&maptype=roadmap&language=zh-CN&sensor=false
```

<kbd>visible</kbd>

指定一个位置，即使不显示标记或其他指示器也应该在地图上保持可见。用此参数确保在静态地图上显示某些特征或地图位置。

<kbd>style</kbd>

用于自定义样式以更改地图的特定地图项（如道路、公园等）的显示方式。可以添加多个style参数。格式：

style=feature:featureArgument|element:elementArgument|rule:ruleArgument...
style的参数：
- feature 设置地图项
- element 设置地图项子集
- 规则 定义更细致的显示样式

`feature`的设置（完整列表见Google Maps Javascript API）：
- feature:all （默认） 所有地图项
- feature:road 所有道路
- feature:landscape 所有背景地貌（通常为道路间的区域）

`element`的设置：
- element:all （默认）选择该地图项的所有元素
- element:geometry 只选择地理元素
- element:labels 只选择与该地图项关联的文本标签

样式规则设置：

- hue（0xRRGGBB格式的RGB十六进制字符串），选择项的基本颜色。
- lightness（[-100, 100]间的浮点值），亮度变化百分比。负值增加暗度（-100为黑），正值增加亮度（+100为白）。
- saturation（[-100, 100]间的浮点值），颜色色度变化百分比。
- gamma（[0.01, 10.0]间的浮点值，1.0表示不应用任何校正），灰度校正值。灰度系数以非线性方式修改色相亮度，而不会影响白色值或黑色值。灰度系数通常用于修改多个元素的对比度。可以通过gamma值来提高（<1.0）或降低（>1.0）元素边缘与内部间的对比度。
- inverse_lightness:true 颠倒现有的亮度。
visibility（on, off, simplified）用于表示元素是否在地图上出现及其出现方式。

<kbd>sensor</kbd>

sensor（必填）是否使用传感器确定用户位置，可以设置为true或false。







---
layout:     post
title:      "3D Earth to Show statistics"
subtitle:   "全球大数据可视化显示"
date:       2014-12-21 5:23
author:     "Jianfei"
header-img: "img/gaodelbs.png"
tags:        [Robot]
category:   Robot
---

<h2 class="section-heading">3D 地球</h2>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;前段时间去上海参加hackathon的时候还听周神和策神跟我说数据可视化，是大数据时代一个热点。这次我在查询高德地图API时发现了一张“<a href="http://lbs.amap.com/gps/">全球定位热力图</a>”，十分酷炫，是根据各地区的地图api访问次数而生成的地图图像！看了下其引用库，是最基础的JS下的3D库THREE.js，而对于global的数据分布则可以直接使用WebGL Global，开源的搭建好的地球视图，只需要提供json格式的经纬度数据，就可以描绘出美轮美奂的定制版数字地球啦！</p>

<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;另外，3D的编程现在不仅局限于openGL了，前段时间有使用过vpython，也是非常好的3D工具，这里也粗略的学习了一下一个3D场景的基本元素。其中包括场景、渲染器、相机、物体、光源，在编程中会慢慢体会到这些元素的实际设定。</p>

<h2 class="section-heading">Demo</h2>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;闲话不多说了，看下面的demo，全球人口信息在3年中的变化。</p>

<iframe frameborder="0" marginheight="0" marginwidth="0" border="0" scrolling="no" height="600px" width="100%" src="http://marsrock.net/globe/"></iframe>


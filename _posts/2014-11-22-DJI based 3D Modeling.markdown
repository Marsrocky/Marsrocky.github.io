---
layout:     post
title:      "DJI based 3D Modeling"
subtitle:   "飞行器项目开启"
date:       2014-11-22 14:30
author:     "Jianfei"
header-img: "img/post-bg-01.jpg"
tags:        [Android, DJI, 3D Modeling]
category:   Android
---

<h2 class="section-heading">总体谈谈</h2>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;先说说这个项目的前世吧，机缘巧合，学CS的我暑假可以到萌生在HKUST的创业公司DJI实习。DJI，中文名大疆创新，一家旨在做飞行器影像系统的年轻公司，据资料显示第五年的销售额已达80亿，并且拿到了红杉资本的投资，还是非常有前途的公司。我在实习中负责的是一个智能车基于视觉的识别，基本原理什么的都是近乎从零看起，但让我叩响了工程领域计算机视觉的大门，也让我对飞行器和计算机视觉结合的开发充满了兴趣。在开学后经过一段时间的调查和思考，终于有了一点点想法，于是我一鼓作气将这个想法细化为一份Proposal。</p>

<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;恰逢DJI的应用创新开发大赛，我的Proposal得到了认可并收到了研发用的<a href="http://www.dji.com">Phantom Vision 2+</a>，非常开心也备受鼓舞，决定今天开始做整个框架和模块的设计。</p>

<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;我要做的大体可以分为以下部分：</p>
<ul class="post-list">
	<li>Android 社交分享平台（包括IM，Share，Capture）</li>
	<li>3D Modeling based on OPENCV</li>
	<li>后台服务器（处理建模过程和社交平台）</li>
	<li>DJI SDK利用飞行器进行图像捕捉</li>
</ul>

<h2 class="section-heading">Android 社交分享平台</h2>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这里是一个<u>图片和视频</u>的分享平台，访问方式是通过地图LBS，将发布的图片选择并定位，图片拍摄的地理信息会在飞行器拍摄时获得。所以用户可以直观的通过拉伸地图看到好友发布图片和视频分享的地方，并进行访问。</p>
<cite>Technology: LBS, Android, IM message.</cite>

<h2 class="section-heading">3D Modeling based on OPENCV</h2>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这里用OPECV将2D图片转化成3D图像。具体技术难度有待考察，不过根据很多paper的demo看起来效果还是很不错的。问题在于语言的使用，如何与后台服务器进行对接，后台服务器是python实现而这里是C++，这里应该如何解决。</p>
<cite>Technology: 3D Modeling, Communication</cite>

<h2 class="section-heading">后台服务器</h2>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;后台服务器处理的是用户数据库，用户数据库包括基本信息和用户的分享信息。没有什么难的问题。</p>

<h2 class="section-heading">DJI SDK 图像捕捉和图像信息获取</h2>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;正在阅读。具体用到SDk的使用会在后面给出。</p>

<a href="#">
    <img class="img-responsive" src="{{ site.baseurl }}/img/post-bg-02.jpg" alt="">
</a>
<span class="caption text-muted">好啦，扬帆，启航！</span>

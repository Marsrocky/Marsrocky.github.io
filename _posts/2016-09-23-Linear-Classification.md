---
layout:     post
title:      "Linear Classification"
subtitle:   "线性分类笔记"
date:       2016-09-23 15:00
author:     "Jianfei"
header-img: "img/post-bg-lc.jpg"
tags:        [CS231n]
category:   Course
---

测试  

测试mathjax$x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$

{% highlight python %}
import scrapy

from dbspider.items import DbspiderItem
from scrapy.selector import Selector
from scrapy.http import HtmlResponse
from scrapy.http import Request

class DbSpider(scrapy.spiders.Spider):
	name = "dbmovie"
	allow_domains = ["douban.com"]
	start_urls = [
		"https://movie.douban.com/subject/20438962/comments"
	]

	def parse(self, response):
		sel = Selector(response)
		self.log('Url crawled: %s' % response.url)
		item = DbspiderItem()
		
		# Grade of movie
		item['grade'] = sel.xpath('//div[@class="comment"]/h3/span[@class="comment-info"]/span[contains(@class, "allstar")]/@title').extract()
		# Time of comment
		item['time'] = sel.xpath('//div[@class="comment"]/h3/span[@class="comment-info"]/span[@class=""]/text()[1]').extract()
		# Approval of comment
		item['approval'] = sel.xpath('//div[@class="comment"]/h3/span[@class="comment-vote"]/span[contains(@class, "votes")]/text()[1]').extract()
		# Comment of movie
		item['comment'] = sel.xpath('//div[@class="comment"]/p[@class=""]/text()[1]').extract()

		with open('data_result.txt', 'a') as f:
			for i in range(20):
				# f.write('num: %s\n' % str(i+1))
				f.write('time: %s\n' % item['time'][i].strip().encode('utf-8'))
				f.write('approval: %s\n' % item['approval'][i].encode('utf-8'))
				f.write('comment: %s\n' % item['comment'][i].strip().encode('utf-8'))

				# Due to the possible lost data, we don`t use this data
				# f.write('grade: %s\n' % item['grade'][i].encode('utf-8'))		

				f.write('\n')
			f.close()

		yield item

		next_page = '//div[@id="paginator"]/a[@class="next"]/@href'
		if response.xpath(next_page):
			next_url = 'https://movie.douban.com/subject/20438962/comments' + response.xpath(next_page).extract()[0]
			request = scrapy.Request(next_url, callback = self.parse)
			yield request

{% endhighlight %}
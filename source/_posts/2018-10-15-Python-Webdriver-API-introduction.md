---
title: Python Webdriver API introduction
date: 2018-10-15 17:17:36
tags:
 - python自动化
 - selenium
 - Webdriver
categories:
 - python自动化
 - selenium
 - Webdriver
---
首先，简要介绍下什么是WebDriver，WebDriver是selenium RC的替代品，考虑到selenium的兼容性，selenium RC并没有被彻底抛弃，如果你使用selenium开发一个新的自动化测试项目，强烈建议你使用WebDriver，那么WebDriver和selenium RC区别在哪里呢？

selenium RC 在浏览器中运行 JavaScript（seleniumCore，一堆JS的函数集合） 应用，使用浏览器内置的 JavaScript 翻译器来翻译和执行selenese 命令（selenese 是 selenium 命令集合）并把浏览器的代理设置为 Selenium Server 的
Http Proxy。（也就是说selenium RC可以看成是由这三部分组成的：client, selenium core, Http Proxy）; 而WebDriver 通过原生浏览器支持或者浏览器扩展直接控制浏览器。WebDriver 针对各个浏览器而开发，取代了嵌入到被测 Web 应用中的 JavaScript。与浏览器的紧密集成支持创建更高级的测试，避免了JavaScript 安全模型导致的限制。除了来自浏览器厂商的支持，WebDriver 还利用操作系统级的调用模拟
用户输入。

可以这么认为，selenium是一个浏览器自动化测试框架，它主要是用于 Web 应用程序的自动化测试（实际应该远不止于Web应用程序的测试）。而Selenium WebDriver提供了各种语言环境的API来支持更多控制权和编写符合标准软件开发实践的应用程序。通常我们认为selenium包含selenium IDE（主要用于Firefox 浏览器的自动化脚本录制），selenium Grid（利用 Grid，可以很方便地同时在多台机器上和异构环境中并行运行多个测试事例）和 WebDriver（替代selenium RC）

<!--MORE-->
下面重点梳理下WebDriver的常用操作实现

**浏览器操作**
<font color='green'>1. 最大化浏览器
#coding=utf-8
from selenium import webdriver
driver = webdriver.Ie()
driver.get("www.sina.com")
#driver = webdriver.Firefox()
#driver.get("http://www.sina.com")
print "浏览器最大化"
driver.maximize_window() #将浏览器最大化显示
driver.quit()

<font color='green'>2. 浏览器前进后退实现
#coding=utf-8
from selenium import webdriver
import time
driver = webdriver.Ie()
driver.get("www.sina.com")
#访问sina首页
first_url= 'http://www.sina.com'
print "now access %s" %(first_url)
.get(first_url)
time.sleep(2)
#访问sina页面
second_url='http://news.sina.com'
print "now access %s" %(second_url)
driver.get(second_url)
time.sleep(2)
#返回（后退）到sina首页
print "back to %s "%(first_url)
driver.back()
time.sleep(1)
#前进到新闻页
print "forward to %s"%(second_url)
driver.forward()
time.sleep(2)
driver.quit()

**测试对象操作**
一般来说，webdriver 中比较常用的操作对象的方法有下面几个
   click 点击对象
   send_keys 在对象上模拟按键输入
   clear 清除对象的内容，如果可以的话
   submit 清除对象的内容，如果可以的话
   text 用于获取元素的文本信息
对于这些常用对象的脚本设计，这里我给出几个实例，以加深印象，事实上大部分的实际应用和这里的用法大同小异，另外在实际使用这些对象时，要习惯查阅WebDriver手册，如此反复才能做到得心应手。下面以百度搜索为例，看这些对象是如何在代码中被使用的。
from selenium import webdriver
import time
driver = webdriver.Firefox()
driver.get("http://www.baidu.com")
#通过 clear()、send_keys() 来操作
driver.find_element_by_id("kw").clear()
driver.find_element_by_id("kw").send_keys("selenium")
time.sleep(2)
#通过 submit() 来操作
driver.find_element_by_id("su").submit()
time.sleep(3)
#id = cp 元素的文本信息
data=driver.find_element_by_id("cp").text
print data #打印信息
time.sleep(3)
#通过 click() 来操作
driver.find_element_by_id("login").click()
#也可定位登陆按钮，通过 enter（回车）代替 click()
driver.find_element_by_id("login").send_keys(Keys.ENTER)

**定位测试元素**
TBC...

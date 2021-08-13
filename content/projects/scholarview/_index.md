---
# Title, summary, and page position.
linktitle: ScholarView
summary: 
weight: 1
icon: book-reader
icon_pack: fas

# Page metadata.
title: ScholarView
date: "2021-08-10T00:07:26Z"
type: book  # Do not modify.
---

谷歌学者数据可视化系统
## Demo of the Project
Please find the video demo of the project here: https://www.youtube.com/watch?v=qVbehPwiN7Y

## 主旨
通过展示UIUC教授的谷歌学者数据，帮助人们更好的理解学者的学术成果和学术历史和学术影响力等等信息
![image info](./images/vis.JPG)

## 意义
1.	学习了HTML/CSS的基础，和JS基础，页面的布局，怎么使用charts
2.	学习了前后端的连接，如何使用云端数据库，怎么样把网站部署到云端，怎么写SQL
3.	利用前端和后台数据库，创建了一个账户管理系统

## 难点
### 1. 产品上的
#### 怎么样设计数据可视化系统才能让用户更好地了解一个教授的学术历史？用什么数据？如何处理数据？用什么图表
a. 比如为了展示教授的研究方向，我们把教授发表的paper的标题用网络爬虫爬取出来，然后分解成一个个的单词，去掉无意义的单词比如冠词，代词，连词等等，剩下的单词按照出现频率来用word cloud来展示\
![image info](./images/word_cloud.JPG)
b. 再比如为了展示教授在学术圈内的关系，我们统计了教授的每个合作作者的共享引用次数，然后用力导引图展示了共享引用次数前十名的合作作者
![image info](./images/force_diagram.JPG)

### 2. 技术上的
主要有三个技术方面的难题:
#### 爬取数据
难点：谷歌学者上面的数据要用网络爬虫来爬取，转化成合适的格式才能保存在数据库里面\
解决：
1. 用Python Scrapy写了一个小型程序来爬取数据，然后把数据保存成JSON
2. 谷歌学者对单位时间内的访问次数有限制，这个不太好解决，只能多出点人力
![image info](./images/scholar_info.JPG)
#### 用户管理系统
难点：从零开始写了一个用户管理系统，它的逻辑比较复杂\
解决： 
1. 登出：登出时登录状态改成已登出，提示用户成功登出
2. 登录：登陆时和数据库查验是否已注册，提示用户成功登入
3. 注册：注册时和数据库查验是否已注册，如果已注册那提示用户，如果没有写入数据库然后提示用户用户名已存在或者成功注册
![image info](./images/user_system.JPG)
#### 快速计算
难点：后端对数据进行计算，要求高效率的算法，不然会网页加载卡顿的问题\
解决：一个例子是建立词云的时候，需要用到“最常见的K个单词”算法
![image info](./images/word_cloud.JPG)
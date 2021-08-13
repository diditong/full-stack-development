---
title: Parsing the OpenAPI Definitions
linktitle: Parsing the OpenAPI Definitions
type: book
date: "2021-08-12T07:20:00+08:00"
# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---
## Overview
Swagger是接口设计的工具，它设计的接口可以导出json，目前swagger有两个标准： OpenAPI Specification 2（OSA2）和OpenAPI Specification 3（OSA3），3比2的规范更细致更全面，前面两周的开发主要集中在OSA2。

开发Tolstoy的上传功能，主要有两个资源可以利用：
1. Swagger的官方文档
2. 类似工具如yapi的swagger上传功能：

## Solution
### Function 1：Parsing an infinite-loop structure
前一版的swagger上传功能无法成功解析properties, schema和$ref，无法成功解析parameters和responses（分别对应请求和响应）中的无限循环结构（无限$ref），这一版重新写了四个解析函数：getResponse, getRequest, getRespChildrenFromProps, getReqChildrenFromDef

这四个函数用到的逻辑几乎是相同的，因此未来可以在此处优化，之所以对相似的功能写了四个函数是因为
	1. Tolstoy先前对请求体和响应体使用了相似但不相同的数据结构。（从修改最少原代码的原则出发，我延用了这些没有统一的数据结构。）
	2. swagger中请求parameters和responses字段的结构也有所不同。

目前准备先开发功能，再优化代码，优化思路是
	1. 首先统一request和response两个字段的数据结构，该数据结构应该可以包容两个字段既有的差异。
	2. 统一数据结构后，由于getResponse和getRequest的功能非常相似，可以将二者合一（须注意parameters和responses字段结构的差异）
	3. getRespChildrenFromProps, getReqChildrenFromDef相当于是前两者的辅助函数，作用是获取请求和响应的children，因此也可以将二者合一

### Function 2: Parsing and presenting non-200 responses
前一版的解析功能只能展示swaggerjson中响应字段为“200”（成功）的情况：
![image info](../images/upload_JSON/1.png)

但是不能展示其他返回码的情况，例如：
![image info](../images/upload_JSON/2.png) 

解决方案是扩充了响应的数据结构，加入了’retCode’和’message’并传递给展示层
![image info](../images/upload_JSON/3.png)

并且新写了一个Console组件（黑底白字+特殊字体），用于展示message
![image info](../images/upload_JSON/4.png)


### Function 3: Auto categorizing interfaces upon uploading
前一版的上传功能和分类功能没有打通，只能在新增接口时分类，这一版可以根据swagger的最外层的tags属性和每一个接口内部的tags属性来自动分类

### Function 4: Parsing and presenting header, query, and path parameters
这一功能与功能2的类似，可以采用相似的办法处理，结果如下

（非请求体）参数的展示
![image info](../images/upload_JSON/5.png)

（非请求体）参数的编辑
![image info](../images/upload_JSON/6.png) 
 
## New Features 
目前已经开发出来的功能，和对应的测试用例如下

功能1-解析properties，schema和$ref        test0-test10
功能2-支持非200字段的解析与展示             test6-8
功能3-支持接口上传自动分类                 test0-test10


## Issues
先满足大多数的上传需求，可根据用户需求细化/强化功能，以下是一些目前发现的优先级比较低的小问题，
1. 支持allOf字段：有的swaggerjson里面会使用allOf字段
2. 支持返回的header字段和以及files类型：https://swagger.io/docs/specification/2-0/describing-responses/
3. 支持返回400时解析schema参数（通常def的名字是Error）
4. 支持解析swagger示例“example”字段




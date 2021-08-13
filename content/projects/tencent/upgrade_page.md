---
title: 升级接口页面
linktitle: 升级接口页面
type: book
date: "2021-08-12T07:20:00+08:00"
# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---
## 存在的问题

在开发Tolstoy swaggerjson上传功能时发现：目前Tolstoy“前端自定义”的各个功能，只支持了与请求体（request body）参数相关的增删查改，而不支持非请求体参数（query，header，path），因此需要将升级到更全面的接口功能。

由于“协议明细”，“编辑”，“Mock”，“用例”四个功能使用的是同样的只包含请求体信息的数据结构，必须首先把这个数据结构扩充，使之包含非请求体参数的信息，然后逐一升级这四个功能。

## 开发计划
### 第一步：重新定义统一的Request Type和Response Type
原因是：
1. 以前相似的数据结构使用了不同的Type定义，修改Type时需要逐一修改，不仅造成开发过程中的思想负担，而且降低了代码质量
2. 之前的Request Type只包括了请求体参数，而没有包含其他参数（query，header，path）的定义，必须先扩充数据结构然后才能开发相关功能（比如编辑非请求体的参数）

影响：
会影响到interface页面下的其他功能，包括mock，用例和编辑。可以在保留原来分散的Type的基础上先定义出统一的Type，然后再把每个功能的Type迁移到统一的Type上面

### 第二步：升级编辑功能
编辑请求的功能只支持请求体参数，如下图
![image info](../images/upgrade_page/1.png)

目标是和YApi对齐，使之支持非请求体参数类型的编辑，如下图
![image info](../images/upgrade_page/2.png)

同样，编辑响应的功能只支持响应体参数，需要支持非200返回数据的设置，如下图
![image info](../images/upgrade_page/3.png)

### 第三步：修改Mock和用例功能，对接统一的数据结构




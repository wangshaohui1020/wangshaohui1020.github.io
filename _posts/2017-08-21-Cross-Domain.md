---
layout: post_layout
title: his系统遇到的问题及解决方案
time: On Monday, August 21, 2017
location: BeiJing
pulished: true
excerpt_separator: "```"
---


- 1、表单提交，数据量很大，不能采取之前的jsonp的方式
经历：
    - ①一直用的就是它，这次提交出错，找原因，发现不能提交量大的数据，会有数据丢失
    - ②jsonp只是get方式提交，即使写成post，也是get方式提交，这是jsonp内部机制的问题
    - ③后端处理：
    ```java
	//不用@ResponseBody，采用post接收：
	method = RequestMethod.POST
	//解决跨域问题
    response.setHeader("Access-Control-Allow-Origin", "*");
	//最后把json数据写出去：
	response.getWriter().write(json);
	```
  前端处理：
	ajax请求，post提交
    - ④传参：多个同类数据，传json数组串，后台用阿里巴巴的包转换
    ```java
    List<ConCheckResultBean> checkResultList =
    com.alibaba.fastjson.JSONArray.parseArray(resultJson, ConCheckResultBean.class);
    ```
（之前用了别的包，转换出问题了，普通的可以转，里面再套list其他类型时出错）
- 2、注册总结（做了好多次，应该总结一下）
    - 注册后，把注册后的用户信息返回出去，以便直接登录使用
    - 注册步骤：①手机号、图片验证码、短信验证码、②用户信息完善 两个接口，一个是注册接口，一个是更新接口
- 3、详情接口
    - 进入详情，除了返回详情外，更新阅读数（错误犯了不止一次了）
    - 用户是否点过赞
- 4、点赞接口
    - 查询用户是否对本篇文章点过赞
    - 用户未点过，添加点赞，查询本文章，并对其进行点赞数量的更新
    - 用户点过，返回已点过
- 5、取消点赞接口
    - 查询用户是否对本篇文章点过赞
    - 用户点过，删除点赞，查询本文章，并对其进行点赞数量的更新
    - 返回未点过
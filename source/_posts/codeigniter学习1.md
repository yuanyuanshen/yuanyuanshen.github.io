---
title: codeigniter学习1
date: 2017-02-04 15:52:34
tags: [php codeigniter]
categories: php学习
comments: true
---

### CI框架学习篇(1)

#### 关于CI

* 特点：轻量(2.2M)、快速(用到哪些进行手动的加载)、功能强大
* 基于MVC模型
* 能够生成干净的URL，便于SEO优化
* 扩展性强
* 不需要模板引擎(写原生速度更快、不需要重新编译模板)

#### MVC框架

模型：提供增、删、改查数据库这些功能
视图：负责给用户展示页面功能
控制器：连接视图和模型，是模型和视图以及其他处理的中介

#### URL片段

CI是访问单入口来执行的其中的功能 访问index.php来操作controller

index.php/home/index home/index就是URL片段 类名/方法 index是默认的方法

localhost/ci/index.php/home 会直接索引到home下的index方法

#### CI的一些操作

1. 配置默认控制器
2. 载入视图文件 $this->load->view('file_name') php文件不需要写后缀，其他文件需要后缀名
3. 给视图传递数据
~~~
$data['title'] = '标题';
$this->load->view('file_name1'，$data);
$this->load->view('file_name2');
//file_name2可以使用$data的数据，其他视图使用也只需要加载一次
~~~

<!-- more -->

4. 载入辅助函数

* 手动加载

~~~
//常用url辅助函数，将其放在自动加载中
$this->load->helper('url');//url辅助函数
echo site_url();//访问控制器方法名来删除
echo '<hr/>';
echo base_url();//一些css的路径等
redirect('类名/方法名');//直接跳转
~~~

* 自动全局加载

~~~
//application-config-autoload.php
$autoload['helper'] = array('url');
~~~

5. 自定义函数

~~~
//system-core-Common.php自动加载、全局使用
function p($arr){
    echo '<pre>';
    print_r($arr);
    echo '</pre>';
}
~~~

6. 表单验证类

* 载入验证类
```
$this->load->library('form_validation');
```
* 设置规则
```
$this->form_validation->set_rules('name值','标签名称','规则');
```
* 执行验证（返回bool值）
```
$this->form_validation->run()
```
* 表单验证辅助函数
```
$this->load->helper('form');
set_value('name')//充填数据
form_error('name','<span>','</span>')//显示错误
set_select()
set_checkbox()
set_radio()
```




# layuimini-thinkphp

## 说明

框架主要使用ThinkPHP6.0 + layuimini，拥有完善的权限的管理模块以及敏捷的开发方式。

特意注明：项目来源 EasyAdmin  http://easyadmin.99php.cn

## 安装

安装项目依赖包：
```
composer install --no-plugins --optimize-autoloader --no-dev
```

nginx伪静态配置：
```
    location / {
        index  index.html  index.htm  index.php;
        if (!-e $request_filename) {
               rewrite  ^(.*)$  /index.php?s=/$1  last;
               break;
         }
    }
```

访问域名，进行项目初始化安装，包括数据库。

将 .example.env 文件复制为 .env 文件，进行配置。

## 项目特性

* 快速CURD命令行
    * 一键生成控制器、模型、视图、JS文件
    * 支持关联查询、字段设置等等
* 基于`auth`的权限管理系统
    * 通过`注解方式`来实现`auth`权限节点管理
    * 具备一键更新`auth`权限节点，无需手动输入管理
    * 完善的后端权限验证以及前面页面按钮显示、隐藏控制
* 完善的菜单管理
    * 分模块管理
    * 无限极菜单
    * 菜单编辑会提示`权限节点`
* 完善的上传组件功能
    * 本地存储
    * 阿里云OSS`建议使用`
    * 腾讯云COS
    * 七牛云OSS
* 完善的前端组件功能
   * 对layui的form表单重新封装，无需手动拼接数据请求
   * 简单好用的`图片、文件`上传组件
   * 简单好用的富文本编辑器`ckeditor`
   * 对弹出层进行再次封装，以极简的方式使用
   * 对table表格再次封装，在使用上更加舒服
   * 根据table的`cols`参数再次进行封装，提供接口实现`image`、`switch`、`list`等功能，再次基础上可以自己再次扩展
   * 根据table参数一键生成`搜索表单`，无需自己编写
* 完善的后台操作日志
   * 记录用户的详细操作信息
   * 按月份进行`分表记录`
* 一键部署静态资源到OSS上
   * 所有在`public\static`目录下的文件都可以一键部署
   * 一个配置项切换静态资源（oss/本地）
* 上传文件记录管理
* 后台路径自定义，防止别人找到对应的后台地址

## EasyAdmin 相关

* 官方网站：[http://easyadmin.99php.cn](http://easyadmin.99php.cn)

* 文档地址：[http://easyadmin.99php.cn/docs](http://easyadmin.99php.cn/docs)

* 演示地址：[http://easyadmin.99php.cn/admindemo](http://easyadmin.99php.cn/admindemo)
（账号：admin，密码：123456。备注：只有查看信息的权限）

* GitHub地址：[https://github.com/zhongshaofa/easyadmin](https://github.com/zhongshaofa/easyadmin)

* Gitee地址：[https://gitee.com/zhongshaofa/easyadmin](https://gitee.com/zhongshaofa/easyadmin)




## 参考

LAYUI MINI 官网：http://layuimini.99php.cn/

Laravel 官网：https://laravel.com/

Layui 官网： https://layui.dev/

Laravel 6 中文文档：https://laravelacademy.org/books/laravel-docs-6
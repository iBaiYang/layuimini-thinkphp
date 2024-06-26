# layuimini-thinkphp

## 说明

该框架是一个后台管理系统，主要使用layuimini + ThinkPHP6.0，拥有完善的权限的管理模块以及敏捷的开发方式。

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

项目入口目录`public`。

将 `.example.env` 文件复制为 `.env` 文件，进行配置。

访问域名，进行项目初始化安装，包括数据库创建。

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

## 数据准备

上面访问域名时，会对项目进行初始化安装，包括数据库的创建，这里只是作为参考。

数据表信息：

```
-- ----------------------------
-- Table structure for lt_mall_cate
-- ----------------------------
DROP TABLE IF EXISTS `lt_mall_cate`;
CREATE TABLE `lt_mall_cate` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `title` varchar(20) NOT NULL COMMENT '分类名',
  `image` varchar(500) DEFAULT NULL COMMENT '分类图片',
  `sort` int DEFAULT '0' COMMENT '排序',
  `status` tinyint unsigned DEFAULT '1' COMMENT '状态(1:禁用,2:启用)',
  `remark` varchar(255) DEFAULT NULL COMMENT '备注说明',
  `create_time` int DEFAULT NULL COMMENT '创建时间',
  `update_time` int DEFAULT NULL COMMENT '更新时间',
  `delete_time` int DEFAULT NULL COMMENT '删除时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `title` (`title`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb3 ROW_FORMAT=COMPACT COMMENT='商品分类';

-- ----------------------------
-- Records of lt_mall_cate
-- ----------------------------
INSERT INTO `lt_mall_cate` VALUES ('1', '手机', '/static/common/images/logo.png', '0', '1', '', '1589440437', '1589440437', null);

-- ----------------------------
-- Table structure for lt_mall_goods
-- ----------------------------
DROP TABLE IF EXISTS `lt_mall_goods`;
CREATE TABLE `lt_mall_goods` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `cate_id` int DEFAULT NULL COMMENT '分类ID',
  `title` varchar(20) NOT NULL COMMENT '商品名称',
  `logo` varchar(500) DEFAULT NULL COMMENT '商品logo',
  `images` text COMMENT '商品图片 以 | 做分割符号',
  `describe` text COMMENT '商品描述',
  `market_price` decimal(10,2) DEFAULT '0.00' COMMENT '市场价',
  `discount_price` decimal(10,2) DEFAULT '0.00' COMMENT '折扣价',
  `sales` int DEFAULT '0' COMMENT '销量',
  `virtual_sales` int DEFAULT '0' COMMENT '虚拟销量',
  `stock` int DEFAULT '0' COMMENT '库存',
  `total_stock` int DEFAULT '0' COMMENT '总库存',
  `sort` int DEFAULT '0' COMMENT '排序',
  `status` tinyint unsigned DEFAULT '1' COMMENT '状态(1:禁用,2:启用)',
  `remark` varchar(255) DEFAULT NULL COMMENT '备注说明',
  `create_time` int DEFAULT NULL COMMENT '创建时间',
  `update_time` int DEFAULT NULL COMMENT '更新时间',
  `delete_time` int DEFAULT NULL COMMENT '删除时间',
  PRIMARY KEY (`id`),
  KEY `cate_id` (`cate_id`)
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8mb3 ROW_FORMAT=COMPACT COMMENT='商品列表';

-- ----------------------------
-- Records of lt_mall_goods
-- ----------------------------
INSERT INTO `lt_mall_goods` VALUES ('1', '1', '落地-风扇', '/static/common/images/logo.png', '/static/common/images/logo.png|/static/common/images/logo.png|/static/common/images/logo.png|/static/common/images/logo.png', '<p>76654757</p>\n\n<p><img alt=\"\" src=\"/static/common/images/logo.png\" style=\"height:689px; width:790px\" /></p>\n\n<p><img alt=\"\" src=\"/static/common/images/logo.png\" style=\"height:877px; width:790px\" /></p>\n', '599.00', '368.00', '0', '594', '0', '0', '675', '1', '', '1589454309', '1589567016', null);
INSERT INTO `lt_mall_goods` VALUES ('2', '1', '电脑', '/static/admin/images/loginbg.png', '/static/common/images/logo.png', '<p>477</p>\n', '0.00', '0.00', '0', '0', '115', '320', '0', '1', '', '1589465215', '1589476345', null);

-- ----------------------------
-- Table structure for lt_system_admin
-- ----------------------------
DROP TABLE IF EXISTS `lt_system_admin`;
CREATE TABLE `lt_system_admin` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `auth_ids` varchar(255) DEFAULT NULL COMMENT '角色权限ID',
  `head_img` varchar(255) DEFAULT NULL COMMENT '头像',
  `username` varchar(50) NOT NULL DEFAULT '' COMMENT '用户登录名',
  `password` char(40) NOT NULL DEFAULT '' COMMENT '用户登录密码',
  `phone` varchar(16) DEFAULT NULL COMMENT '联系手机号',
  `remark` varchar(255) DEFAULT '' COMMENT '备注说明',
  `login_num` bigint unsigned DEFAULT '0' COMMENT '登录次数',
  `sort` int DEFAULT '0' COMMENT '排序',
  `status` tinyint unsigned NOT NULL DEFAULT '1' COMMENT '状态(0:禁用,1:启用,)',
  `create_time` int DEFAULT NULL COMMENT '创建时间',
  `update_time` int DEFAULT NULL COMMENT '更新时间',
  `delete_time` int DEFAULT NULL COMMENT '删除时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `username` (`username`) USING BTREE,
  KEY `phone` (`phone`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb3 ROW_FORMAT=COMPACT COMMENT='系统用户表';

-- ----------------------------
-- Records of lt_system_admin
-- ----------------------------
INSERT INTO `lt_system_admin` VALUES ('1', null, '/static/admin/images/head.jpg', 'admin', 'ed696eb5bba1f7460585cc6975e6cf9bf24903dd', null, '', '1', '0', '1', '1718199715', '1718199865', null);

-- ----------------------------
-- Table structure for lt_system_auth
-- ----------------------------
DROP TABLE IF EXISTS `lt_system_auth`;
CREATE TABLE `lt_system_auth` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `title` varchar(20) NOT NULL COMMENT '权限名称',
  `sort` int DEFAULT '0' COMMENT '排序',
  `status` tinyint unsigned DEFAULT '1' COMMENT '状态(1:禁用,2:启用)',
  `remark` varchar(255) DEFAULT NULL COMMENT '备注说明',
  `create_time` int DEFAULT NULL COMMENT '创建时间',
  `update_time` int DEFAULT NULL COMMENT '更新时间',
  `delete_time` int DEFAULT NULL COMMENT '删除时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `title` (`title`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8mb3 ROW_FORMAT=COMPACT COMMENT='系统权限表';

-- ----------------------------
-- Records of lt_system_auth
-- ----------------------------
INSERT INTO `lt_system_auth` VALUES ('1', '管理员', '1', '1', '测试管理员', '1588921753', '1589614331', null);
INSERT INTO `lt_system_auth` VALUES ('6', '游客权限', '0', '1', '', '1588227513', '1589591751', '1589591751');

-- ----------------------------
-- Table structure for lt_system_auth_node
-- ----------------------------
DROP TABLE IF EXISTS `lt_system_auth_node`;
CREATE TABLE `lt_system_auth_node` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `auth_id` bigint unsigned DEFAULT NULL COMMENT '角色ID',
  `node_id` bigint DEFAULT NULL COMMENT '节点ID',
  PRIMARY KEY (`id`),
  KEY `index_system_auth_auth` (`auth_id`) USING BTREE,
  KEY `index_system_auth_node` (`node_id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=17 DEFAULT CHARSET=utf8mb3 ROW_FORMAT=COMPACT COMMENT='角色与节点关系表';

-- ----------------------------
-- Records of lt_system_auth_node
-- ----------------------------
INSERT INTO `lt_system_auth_node` VALUES ('1', '6', '1');
INSERT INTO `lt_system_auth_node` VALUES ('2', '6', '2');
INSERT INTO `lt_system_auth_node` VALUES ('3', '6', '9');
INSERT INTO `lt_system_auth_node` VALUES ('4', '6', '12');
INSERT INTO `lt_system_auth_node` VALUES ('5', '6', '18');
INSERT INTO `lt_system_auth_node` VALUES ('6', '6', '19');
INSERT INTO `lt_system_auth_node` VALUES ('7', '6', '21');
INSERT INTO `lt_system_auth_node` VALUES ('8', '6', '22');
INSERT INTO `lt_system_auth_node` VALUES ('9', '6', '29');
INSERT INTO `lt_system_auth_node` VALUES ('10', '6', '30');
INSERT INTO `lt_system_auth_node` VALUES ('11', '6', '38');
INSERT INTO `lt_system_auth_node` VALUES ('12', '6', '39');
INSERT INTO `lt_system_auth_node` VALUES ('13', '6', '45');
INSERT INTO `lt_system_auth_node` VALUES ('14', '6', '46');
INSERT INTO `lt_system_auth_node` VALUES ('15', '6', '52');
INSERT INTO `lt_system_auth_node` VALUES ('16', '6', '53');

-- ----------------------------
-- Table structure for lt_system_config
-- ----------------------------
DROP TABLE IF EXISTS `lt_system_config`;
CREATE TABLE `lt_system_config` (
  `id` int unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(30) NOT NULL DEFAULT '' COMMENT '变量名',
  `group` varchar(30) NOT NULL DEFAULT '' COMMENT '分组',
  `value` text COMMENT '变量值',
  `remark` varchar(100) DEFAULT '' COMMENT '备注信息',
  `sort` int DEFAULT '0',
  `create_time` int DEFAULT NULL COMMENT '创建时间',
  `update_time` int DEFAULT NULL COMMENT '更新时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `name` (`name`),
  KEY `group` (`group`)
) ENGINE=InnoDB AUTO_INCREMENT=88 DEFAULT CHARSET=utf8mb3 ROW_FORMAT=COMPACT COMMENT='系统配置表';

-- ----------------------------
-- Records of lt_system_config
-- ----------------------------
INSERT INTO `lt_system_config` VALUES ('41', 'alisms_access_key_id', 'sms', '填你的', '阿里大于公钥', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('42', 'alisms_access_key_secret', 'sms', '填你的', '阿里大鱼私钥', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('55', 'upload_type', 'upload', 'local', '当前上传方式 （local,alioss,qnoss,txoss）', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('56', 'upload_allow_ext', 'upload', 'doc,gif,ico,icon,jpg,mp3,mp4,p12,pem,png,rar,jpeg', '允许上传的文件类型', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('57', 'upload_allow_size', 'upload', '1024000', '允许上传的大小', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('58', 'upload_allow_mime', 'upload', 'image/gif,image/jpeg,video/x-msvideo,text/plain,image/png', '允许上传的文件mime', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('59', 'upload_allow_type', 'upload', 'local,alioss,qnoss,txcos', '可用的上传文件方式', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('60', 'alioss_access_key_id', 'upload', '填你的', '阿里云oss公钥', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('61', 'alioss_access_key_secret', 'upload', '填你的', '阿里云oss私钥', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('62', 'alioss_endpoint', 'upload', '填你的', '阿里云oss数据中心', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('63', 'alioss_bucket', 'upload', '填你的', '阿里云oss空间名称', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('64', 'alioss_domain', 'upload', '填你的', '阿里云oss访问域名', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('65', 'logo_title', 'site', 'LayTap', 'LOGO标题', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('66', 'logo_image', 'site', '/favicon.ico', 'logo图片', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('68', 'site_name', 'site', 'LayTap后台系统', '站点名称', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('69', 'site_ico', 'site', '填你的', '浏览器图标', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('70', 'site_copyright', 'site', '填你的', '版权信息', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('71', 'site_beian', 'site', '填你的', '备案信息', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('72', 'site_version', 'site', '2.0.0', '版本信息', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('75', 'sms_type', 'sms', 'alisms', '短信类型', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('76', 'miniapp_appid', 'wechat', '填你的', '小程序公钥', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('77', 'miniapp_appsecret', 'wechat', '填你的', '小程序私钥', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('78', 'web_appid', 'wechat', '填你的', '公众号公钥', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('79', 'web_appsecret', 'wechat', '填你的', '公众号私钥', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('80', 'txcos_secret_id', 'upload', '填你的', '腾讯云cos密钥', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('81', 'txcos_secret_key', 'upload', '填你的', '腾讯云cos私钥', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('82', 'txcos_region', 'upload', '填你的', '存储桶地域', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('83', 'tecos_bucket', 'upload', '填你的', '存储桶名称', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('84', 'qnoss_access_key', 'upload', '填你的', '访问密钥', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('85', 'qnoss_secret_key', 'upload', '填你的', '安全密钥', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('86', 'qnoss_bucket', 'upload', '填你的', '存储空间', '0', null, null);
INSERT INTO `lt_system_config` VALUES ('87', 'qnoss_domain', 'upload', '填你的', '访问域名', '0', null, null);

-- ----------------------------
-- Table structure for lt_system_log_202406
-- ----------------------------
DROP TABLE IF EXISTS `lt_system_log_202406`;
CREATE TABLE `lt_system_log_202406` (
  `id` int unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `admin_id` int unsigned DEFAULT '0' COMMENT '管理员ID',
  `url` varchar(1500) NOT NULL DEFAULT '' COMMENT '操作页面',
  `method` varchar(50) NOT NULL COMMENT '请求方法',
  `title` varchar(100) DEFAULT '' COMMENT '日志标题',
  `content` text NOT NULL COMMENT '内容',
  `ip` varchar(50) NOT NULL DEFAULT '' COMMENT 'IP',
  `useragent` varchar(255) DEFAULT '' COMMENT 'User-Agent',
  `create_time` int DEFAULT NULL COMMENT '操作时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=631 DEFAULT CHARSET=utf8mb3 ROW_FORMAT=COMPACT COMMENT='后台操作日志表 - 202406';

-- ----------------------------
-- Records of lt_system_log_202406
-- ----------------------------
INSERT INTO `lt_system_log_202406` VALUES ('630', null, '/admin/login/index.html', 'post', '', '{\"username\":\"admin\",\"password\":\"***********\",\"keep_login\":\"0\"}', '192.168.56.1', 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36', '1718199865');

-- ----------------------------
-- Table structure for lt_system_menu
-- ----------------------------
DROP TABLE IF EXISTS `lt_system_menu`;
CREATE TABLE `lt_system_menu` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `pid` bigint unsigned NOT NULL DEFAULT '0' COMMENT '父id',
  `title` varchar(100) NOT NULL DEFAULT '' COMMENT '名称',
  `icon` varchar(100) NOT NULL DEFAULT '' COMMENT '菜单图标',
  `href` varchar(100) NOT NULL DEFAULT '' COMMENT '链接',
  `params` varchar(500) DEFAULT '' COMMENT '链接参数',
  `target` varchar(20) NOT NULL DEFAULT '_self' COMMENT '链接打开方式',
  `sort` int DEFAULT '0' COMMENT '菜单排序',
  `status` tinyint unsigned NOT NULL DEFAULT '1' COMMENT '状态(0:禁用,1:启用)',
  `remark` varchar(255) DEFAULT NULL,
  `create_time` int DEFAULT NULL COMMENT '创建时间',
  `update_time` int DEFAULT NULL COMMENT '更新时间',
  `delete_time` int DEFAULT NULL COMMENT '删除时间',
  PRIMARY KEY (`id`),
  KEY `title` (`title`),
  KEY `href` (`href`)
) ENGINE=InnoDB AUTO_INCREMENT=254 DEFAULT CHARSET=utf8mb3 ROW_FORMAT=COMPACT COMMENT='系统菜单表';

-- ----------------------------
-- Records of lt_system_menu
-- ----------------------------
INSERT INTO `lt_system_menu` VALUES ('227', '99999999', '后台首页', 'fa fa-home', 'index/welcome', '', '_self', '0', '1', null, null, '1573120497', null);
INSERT INTO `lt_system_menu` VALUES ('228', '0', '系统管理', 'fa fa-cog', '', '', '_self', '0', '1', '', null, '1588999529', null);
INSERT INTO `lt_system_menu` VALUES ('234', '228', '菜单管理', 'fa fa-tree', 'system.menu/index', '', '_self', '10', '1', '', null, '1588228555', null);
INSERT INTO `lt_system_menu` VALUES ('244', '228', '管理员管理', 'fa fa-user', 'system.admin/index', '', '_self', '12', '1', '', '1573185011', '1588228573', null);
INSERT INTO `lt_system_menu` VALUES ('245', '228', '角色管理', 'fa fa-bitbucket-square', 'system.auth/index', '', '_self', '11', '1', '', '1573435877', '1588228634', null);
INSERT INTO `lt_system_menu` VALUES ('246', '228', '节点管理', 'fa fa-list', 'system.node/index', '', '_self', '9', '1', '', '1573435919', '1588228648', null);
INSERT INTO `lt_system_menu` VALUES ('247', '228', '配置管理', 'fa fa-asterisk', 'system.config/index', '', '_self', '8', '1', '', '1573457448', '1588228566', null);
INSERT INTO `lt_system_menu` VALUES ('248', '228', '上传管理', 'fa fa-arrow-up', 'system.uploadfile/index', '', '_self', '0', '1', '', '1573542953', '1588228043', null);
INSERT INTO `lt_system_menu` VALUES ('249', '0', '商城管理', 'fa fa-list', '', '', '_self', '0', '1', '', '1589439884', '1589439884', null);
INSERT INTO `lt_system_menu` VALUES ('250', '249', '商品分类', 'fa fa-calendar-check-o', 'mall.cate/index', '', '_self', '0', '1', '', '1589439910', '1589439966', null);
INSERT INTO `lt_system_menu` VALUES ('251', '249', '商品管理', 'fa fa-list', 'mall.goods/index', '', '_self', '0', '1', '', '1589439931', '1589439942', null);
INSERT INTO `lt_system_menu` VALUES ('252', '228', '快捷入口', 'fa fa-list', 'system.quick/index', '', '_self', '0', '1', '', '1589623683', '1589623683', null);
INSERT INTO `lt_system_menu` VALUES ('253', '228', '日志管理', 'fa fa-connectdevelop', 'system.log/index', '', '_self', '0', '1', '', '1589623684', '1589623684', null);

-- ----------------------------
-- Table structure for lt_system_node
-- ----------------------------
DROP TABLE IF EXISTS `lt_system_node`;
CREATE TABLE `lt_system_node` (
  `id` int unsigned NOT NULL AUTO_INCREMENT,
  `node` varchar(100) DEFAULT NULL COMMENT '节点代码',
  `title` varchar(500) DEFAULT NULL COMMENT '节点标题',
  `type` tinyint(1) DEFAULT '3' COMMENT '节点类型（1：控制器，2：节点）',
  `is_auth` tinyint unsigned DEFAULT '1' COMMENT '是否启动RBAC权限控制',
  `create_time` int DEFAULT NULL COMMENT '创建时间',
  `update_time` int DEFAULT NULL COMMENT '更新时间',
  PRIMARY KEY (`id`),
  KEY `node` (`node`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=69 DEFAULT CHARSET=utf8mb3 ROW_FORMAT=COMPACT COMMENT='系统节点表';

-- ----------------------------
-- Records of lt_system_node
-- ----------------------------
INSERT INTO `lt_system_node` VALUES ('1', 'system.admin', '管理员管理', '1', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('2', 'system.admin/index', '列表', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('3', 'system.admin/add', '添加', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('4', 'system.admin/edit', '编辑', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('5', 'system.admin/password', '编辑', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('6', 'system.admin/delete', '删除', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('7', 'system.admin/modify', '属性修改', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('8', 'system.admin/export', '导出', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('9', 'system.auth', '角色权限管理', '1', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('10', 'system.auth/authorize', '授权', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('11', 'system.auth/saveAuthorize', '授权保存', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('12', 'system.auth/index', '列表', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('13', 'system.auth/add', '添加', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('14', 'system.auth/edit', '编辑', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('15', 'system.auth/delete', '删除', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('16', 'system.auth/export', '导出', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('17', 'system.auth/modify', '属性修改', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('18', 'system.config', '系统配置管理', '1', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('19', 'system.config/index', '列表', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('20', 'system.config/save', '保存', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('21', 'system.menu', '菜单管理', '1', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('22', 'system.menu/index', '列表', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('23', 'system.menu/add', '添加', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('24', 'system.menu/edit', '编辑', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('25', 'system.menu/delete', '删除', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('26', 'system.menu/modify', '属性修改', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('27', 'system.menu/getMenuTips', '添加菜单提示', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('28', 'system.menu/export', '导出', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('29', 'system.node', '系统节点管理', '1', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('30', 'system.node/index', '列表', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('31', 'system.node/refreshNode', '系统节点更新', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('32', 'system.node/clearNode', '清除失效节点', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('33', 'system.node/add', '添加', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('34', 'system.node/edit', '编辑', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('35', 'system.node/delete', '删除', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('36', 'system.node/export', '导出', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('37', 'system.node/modify', '属性修改', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('38', 'system.uploadfile', '上传文件管理', '1', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('39', 'system.uploadfile/index', '列表', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('40', 'system.uploadfile/add', '添加', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('41', 'system.uploadfile/edit', '编辑', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('42', 'system.uploadfile/delete', '删除', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('43', 'system.uploadfile/export', '导出', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('44', 'system.uploadfile/modify', '属性修改', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('45', 'mall.cate', '商品分类管理', '1', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('46', 'mall.cate/index', '列表', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('47', 'mall.cate/add', '添加', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('48', 'mall.cate/edit', '编辑', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('49', 'mall.cate/delete', '删除', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('50', 'mall.cate/export', '导出', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('51', 'mall.cate/modify', '属性修改', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('52', 'mall.goods', '商城商品管理', '1', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('53', 'mall.goods/index', '列表', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('54', 'mall.goods/stock', '入库', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('55', 'mall.goods/add', '添加', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('56', 'mall.goods/edit', '编辑', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('57', 'mall.goods/delete', '删除', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('58', 'mall.goods/export', '导出', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('59', 'mall.goods/modify', '属性修改', '2', '1', '1589580432', '1589580432');
INSERT INTO `lt_system_node` VALUES ('60', 'system.quick', '快捷入口管理', '1', '1', '1589623188', '1589623188');
INSERT INTO `lt_system_node` VALUES ('61', 'system.quick/index', '列表', '2', '1', '1589623188', '1589623188');
INSERT INTO `lt_system_node` VALUES ('62', 'system.quick/add', '添加', '2', '1', '1589623188', '1589623188');
INSERT INTO `lt_system_node` VALUES ('63', 'system.quick/edit', '编辑', '2', '1', '1589623188', '1589623188');
INSERT INTO `lt_system_node` VALUES ('64', 'system.quick/delete', '删除', '2', '1', '1589623188', '1589623188');
INSERT INTO `lt_system_node` VALUES ('65', 'system.quick/export', '导出', '2', '1', '1589623188', '1589623188');
INSERT INTO `lt_system_node` VALUES ('66', 'system.quick/modify', '属性修改', '2', '1', '1589623188', '1589623188');
INSERT INTO `lt_system_node` VALUES ('67', 'system.log', '操作日志管理', '1', '1', '1589623188', '1589623188');
INSERT INTO `lt_system_node` VALUES ('68', 'system.log/index', '列表', '2', '1', '1589623188', '1589623188');

-- ----------------------------
-- Table structure for lt_system_quick
-- ----------------------------
DROP TABLE IF EXISTS `lt_system_quick`;
CREATE TABLE `lt_system_quick` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `title` varchar(20) NOT NULL COMMENT '快捷入口名称',
  `icon` varchar(100) DEFAULT NULL COMMENT '图标',
  `href` varchar(255) DEFAULT NULL COMMENT '快捷链接',
  `sort` int DEFAULT '0' COMMENT '排序',
  `status` tinyint unsigned DEFAULT '1' COMMENT '状态(1:禁用,2:启用)',
  `remark` varchar(255) DEFAULT NULL COMMENT '备注说明',
  `create_time` int DEFAULT NULL COMMENT '创建时间',
  `update_time` int DEFAULT NULL COMMENT '更新时间',
  `delete_time` int DEFAULT NULL COMMENT '删除时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=24 DEFAULT CHARSET=utf8mb3 ROW_FORMAT=COMPACT COMMENT='系统快捷入口表';

-- ----------------------------
-- Records of lt_system_quick
-- ----------------------------
INSERT INTO `lt_system_quick` VALUES ('1', '管理员管理', 'fa fa-user', 'system.admin/index', '0', '1', '', '1589624097', '1589624792', null);
INSERT INTO `lt_system_quick` VALUES ('2', '角色管理', 'fa fa-bitbucket-square', 'system.auth/index', '0', '1', '', '1589624772', '1589624781', null);
INSERT INTO `lt_system_quick` VALUES ('3', '菜单管理', 'fa fa-tree', 'system.menu/index', '0', '1', null, '1589624097', '1589624792', null);
INSERT INTO `lt_system_quick` VALUES ('6', '节点管理', 'fa fa-list', 'system.node/index', '0', '1', null, '1589624772', '1589624781', null);
INSERT INTO `lt_system_quick` VALUES ('7', '配置管理', 'fa fa-asterisk', 'system.config/index', '0', '1', null, '1589624097', '1589624792', null);
INSERT INTO `lt_system_quick` VALUES ('8', '上传管理', 'fa fa-arrow-up', 'system.uploadfile/index', '0', '1', null, '1589624772', '1589624781', null);
INSERT INTO `lt_system_quick` VALUES ('10', '商品分类', 'fa fa-calendar-check-o', 'mall.cate/index', '0', '1', null, '1589624097', '1589624792', null);
INSERT INTO `lt_system_quick` VALUES ('11', '商品管理', 'fa fa-list', 'mall.goods/index', '0', '1', null, '1589624772', '1589624781', null);

-- ----------------------------
-- Table structure for lt_system_uploadfile
-- ----------------------------
DROP TABLE IF EXISTS `lt_system_uploadfile`;
CREATE TABLE `lt_system_uploadfile` (
  `id` int unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `upload_type` varchar(20) NOT NULL DEFAULT 'local' COMMENT '存储位置',
  `original_name` varchar(255) DEFAULT NULL COMMENT '文件原名',
  `url` varchar(255) NOT NULL DEFAULT '' COMMENT '物理路径',
  `image_width` varchar(30) NOT NULL DEFAULT '' COMMENT '宽度',
  `image_height` varchar(30) NOT NULL DEFAULT '' COMMENT '高度',
  `image_type` varchar(30) NOT NULL DEFAULT '' COMMENT '图片类型',
  `image_frames` int unsigned NOT NULL DEFAULT '0' COMMENT '图片帧数',
  `mime_type` varchar(100) NOT NULL DEFAULT '' COMMENT 'mime类型',
  `file_size` int unsigned NOT NULL DEFAULT '0' COMMENT '文件大小',
  `file_ext` varchar(100) DEFAULT NULL,
  `sha1` varchar(40) NOT NULL DEFAULT '' COMMENT '文件 sha1编码',
  `create_time` int DEFAULT NULL COMMENT '创建日期',
  `update_time` int DEFAULT NULL COMMENT '更新时间',
  `upload_time` int DEFAULT NULL COMMENT '上传时间',
  PRIMARY KEY (`id`),
  KEY `upload_type` (`upload_type`),
  KEY `original_name` (`original_name`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8mb3 ROW_FORMAT=COMPACT COMMENT='上传文件表';

-- ----------------------------
-- Records of lt_system_uploadfile
-- ----------------------------
INSERT INTO `lt_system_uploadfile` VALUES ('1', 'alioss', 'image/jpeg', '/static/common/images/logo.png', '', '', '', '0', 'image/jpeg', '0', 'jpg', '', '1573612437', null, null);
INSERT INTO `lt_system_uploadfile` VALUES ('2', 'alioss', 'image/jpeg', '/static/common/images/github.png', '', '', '', '0', 'image/jpeg', '0', 'jpg', '', '1573612437', null, null);
INSERT INTO `lt_system_uploadfile` VALUES ('3', 'alioss', 'image/x-icon', '/static/common/images/favicon.ico', '', '', '', '0', 'image/x-icon', '0', 'ico', '', '1573612437', null, null);
```

## 备注

体验分支：exp

## EasyAdmin 相关

该框架基于的框架的官方名称是 EasyAdmin，建议在EasyAdmin的基础上进行开发，本框架只是一个示例，下面是相关信息。

* 官方网站：[http://easyadmin.99php.cn](http://easyadmin.99php.cn)

* 文档地址：[http://easyadmin.99php.cn/docs](http://easyadmin.99php.cn/docs)

* 演示地址：[http://easyadmin.99php.cn/admindemo](http://easyadmin.99php.cn/admindemo)
（账号：admin，密码：123456。备注：只有查看信息的权限）

* GitHub地址：[https://github.com/zhongshaofa/easyadmin](https://github.com/zhongshaofa/easyadmin)

* Gitee地址：[https://gitee.com/zhongshaofa/easyadmin](https://gitee.com/zhongshaofa/easyadmin)

## 参考

* LAYUI MINI 官网：[http://layuimini.99php.cn](http://layuimini.99php.cn)

* Layuimini：[https://github.com/zhongshaofa/layuimini](https://github.com/zhongshaofa/layuimini)

* Layui 官网： [https://layui.dev](https://layui.dev)

* Layui：[https://github.com/sentsin/layui](https://github.com/sentsin/layui)

* ThinkPHP：[https://github.com/top-think/framework](https://github.com/top-think/framework)

* Annotations：[https://github.com/doctrine/annotations](https://github.com/doctrine/annotations)

* Jquery：[https://github.com/jquery/jquery](https://github.com/jquery/jquery)

* RequireJs：[https://github.com/requirejs/requirejs](https://github.com/requirejs/requirejs)

* CKEditor：[https://github.com/ckeditor/ckeditor4](https://github.com/ckeditor/ckeditor4)

* Echarts：[https://github.com/apache/incubator-echarts](https://github.com/apache/incubator-echarts)



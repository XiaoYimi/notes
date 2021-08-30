# Thinkphp5.1

## 下载

### 检查配置

- PHP >= 5.6.0

- PDO PHP Extension

- MBstring PHP Extension

  

- Composer 是否全局安装



### Phpstudy

#### 下载

- 包地址: https://www.xp.cn/



#### 安装

- 一直 next 步骤
- 选择存在在 D 盘位置,不占 C 盘空间.



#### 测试

- 模式切换 (WAMP, WNMP ...)

- 我们采用 WAMP 模式.



### Composer

#### 下载

- 包地址: https://getcomposer.org/download/



#### 安装

- 一直 next 步骤
- Choose the command-line PHP you want to use:  选择能执行命令的 PHP 程序.



#### 检测

- 命令行检测:  composer -V



#### 配置

- 由于通过 composer 命令下载/安装的包源可能来自国外,可能导致下载很慢,甚至网络中断.

- 为了解决这一弊端,我们切换 composer 的包源地址,可执行以下命令:

  ```js
  composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
  ```



### 创建 TP 应用

- 切换到 Web 根目录下执行以下命令:

  ```js
  composer create-project topthink/think=5.1.* tp5
  ```

- 其中, th5 是项目根目录名,可自行更改.



### 配置 TP 应用

#### 使用 phpstudy 来构建 web 站点

domain: api.xiaoyimi.com

http(s): 80

root folder: self setting (TP 项目根目录/public)

sync hosts: ok

error page: no create



#### 在浏览器访问

- 在浏览器地址栏输入: www.xiaoyimi.com

如果可以看到 'ThinkPHP V5.1' 就配置成功了.



#### 重写(隐藏)应用入口文件

##### Apache

- 在 TP 项目根目录/public 下,打开  .htaceess 文件,修改成如下：

```php
<IfModule mod_rewrite.c>
  Options +FollowSymlinks -Multiviews
  RewriteEngine On

  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteRule ^(.*)$ index.php?/$1 [QSA,PT,L]
</IfModule>
```



##### Nginx

- 通过 nginx.conf 文件配置,修改成如下:

```php
location / { // …..省略部分代码
   if (!-e $request_filename) {
   		rewrite  ^(.*)$  /index.php?s=/$1  last;
    }
}
```



- 对于 二级应用目录配置

```php
location /youdomain/ {
    if (!-e $request_filename){
        rewrite  ^/youdomain/(.*)$  /youdomain/index.php?s=/$1  last;
    }
}
```



- 根据 以上进行配置(隐藏入口文件)

原来的访问 URL:

```php
http://serverName/index.php/模块/控制器/操作/[参数名/参数值...]
```

设置后的访问 URL:

```php
http://serverName/模块/控制器/操作/[参数名/参数值...]
```



## TP 应用配置 (TP 根目录/config)

### 应用信息配置 (app.php)

- app_name  ----  应用名称
- app_host  ----  应用地址(网站访问地址)
- app_debug  ----  应用调试模式(上线时请及时关闭)



### 数据库配置 (database.php)

- type  ----  数据库类型 (建议使用 mysql)

- hostname  ---- 服务器(数据库)地址 (本地 127.0.0.1)
- database  ----  数据库名
- username  ----  用户名 (默认: root)
- password  ---- 密码 (默认: root)
- hostport  ----  端口 (默认 3306)
- resultset_type  ----  数据集返回类型 (默认 array, 建议 collection => 更好地实现模型关联)



## 开发前的思考

### 开发的原则

- 安全性

- 可用性

- 可维护性

- 代码简洁

- 性能



*注意: 一定要基于长期维护的角度去开发,并对接口实现可拓展性.*



### 实现版本控制

- 由于项目版本的更迭,访问的接口可能有所变化.为了实现项目版本更迭前后的用户都能够访问,就必须考虑接口的版本控制.



### 接口返回数据的格式问题

#### 一般情况下,接口中返回的数据格式大致有2种情况

- 有数据返回

```js
{
    msg: '接口返回提示',
    data: '接口返回的数据',
    errorCode: '自定义的错误代码标识'
}
```



- 无数据返回

```js
{
    msg: '接口返回提示',
    data: '接口返回的数据',
    errorCode: '自定义的错误代码标识'
}
```


#### 对数据返回的方法进行封装

- 由于项目的所有接口都会对接口进行返回接口提示或数据,属于公共模块.故我们一般将它放在应用目录下的公共模块下定义目录 controller.
- 封装实现 (详见封装数据统一格式返回)



### 验证层的场景考虑

- 对于验证层的场景,对于同一项目的不同模块同样适用.并且在控制器中一步到位的适用.

```php
(new ControllerValidate)->goCheck('scene');
```



### 对于模型层的设计考虑

- 由于模型层的操作是关于数据表的行为,不同的模块可对进行操作,属于公共模块.故我们一般将它放在应用目录下的公共模块下定义目录 model.



### 对于控制器的代码简短的疑问

- 在控制器下的方法,一般会考虑几个步骤

  - 对页面传参进行验证 (场景验证)

  - 对控制器的方法进行业务逻辑代码编写

  - 对接口提示或数据的返回



- 根据 MVC 设计模式,我们的业务逻辑代码都在模型层中编写,另外验证层根据场景来验证.

  - M 业务逻辑代码编写

  - V 返回静态页面或页面数据
  - C 对模型层的方法进行调用



- 在编辑接口时,一般用到的只有 M, C 层.



### 对于程序中断的处理

- 程序中断 (2 中情况)
  - 开发中的调试
  - 程序运行出错



- 中断程序的实现
  - 通过抛出异常即可



- 封装实现 (详见封装异常处理)





## 对于概念的理解

### 注入依赖

- 所谓的注入依赖,可以理解为 use ***(类).
- 同级目录可以不 use ***,不在同级目录必须使用 use 注入依赖.



### 模型关联的方法

#### hasOne (一对一)

hasOne('关联模型', '关联模型的键值(默认关联模型名 + '_id')', '当前模型主键(默认自动获取)');



#### hasMany (一对多)

hasMany ('关联模型', '关联模型的键值(默认关联模型名 + '_id')', '当前模型主键(默认自动获取)');



#### belongsTo (一对一)

belongsTo('关联模型', '关联模型的键值(默认关联模型名 + '_id')', '当前模型主键(默认自动获取)');



#### belongsToMany (多对多)

('关联模型', '中间表(默认当前模型名_关联模型名)', '关联模型的键值(默认关联模型名 + '_id')', '当前模型主键(默认自动获取)');



### json(data, status, header, options)

- json() 是 Thinkphp 提供返回数据的函数

- 函数中的形式参数理解

  - data  ----  请求返回的数据结果

  - status  ----  请求的状态码

  - header  ----  请求头

  - options  ----  参数



### Cache

- 获取缓存信息

```php
Cache:get('key');
```



- 设置缓存信息

```php
Cache::set('key', 'value', 'timestemp');
```



- 删除缓存信息

```php
Cache::rm('key');
```



- 获取并删除缓存信息

```php
Cache::pull('key');
```



- 清空缓存信息

```php
Cache::clear();
```



- 助手函数 - 缓存

```php
// 设置缓存
cache('key', 'value', 'timestemp');

// 获取缓存
cache('key');

// 删除缓存
cache('key', NULL);
```

*注意:  timestemp = 0, 则表示永久缓存.*



### limit

- limit(row)    ----    获取 row 行记录.
- limit(row, step)    ----    从 row 行记录开始搜索 step 条记录.



### page

page(currentpage, row)    ----    获取 currentpage 页的行数据,且该页至多 row

 条记录.



### 验证内置规则 print

支持 Thinkphp 5.1.17+. (项目 5.1.39)



#### 报错: 规则错误

- 解决方案





### 用户 Token

- 用户 token 是系统函数随机生成并加密的长字符串.

```php
/*
* sha1(prefix) — 计算字符串的 sha1 散列值
* md5(prefix) — 计算字符串的 MD5 散列值
* uniqid(prefix, true|false) — 生成一个唯一ID,如若想生成唯一ID请配合 md5 函数
* microtime — 返回当前 Unix 时间戳和微秒数
*/

$token = sha1(md5(uniqid(md5(microtime(true)), true)));
```



- 在用户登录成功时,会将所生成的 token 作为 key, 用户信息表记录作为 value 添加到缓存(Cache)中.

- 在路由鉴权中,通过将(登录成功后的)用户 token 作为信息头参数,通过缓存在获取当前用户信息,并将关键信息保存在 request() 中,以便后期获取或使用.



### think 常用命令

- 在指定/公共模块创建控制器

```php
php think make:controller [module/Controller] | [Controller] 
```



- 在指定/公共模块创建模型层

```php
php think make:model [module/Model] | [Model] 
```



- 在指定/公共模块创建验证层

```php
php think make:validate [module/Validate] | [Validate] 
```



- 在指定/公共模块创建视图层

```php
php think make:view [module/View] | [View] 
```





## 封装

### 封装异常处理

- 由于 TP 应用程序的异常处理是返回静态代码的错误提示,而我们对于产品上线后的异常处理只需要返回 json 格式的数据.故我们采用自定义封装异常类处理.

封装前 (debug = true 模式)

```php
[0] RouteNotFoundException in Route.php line 897
当前访问路由未定义或不匹配
......
Environment Variables
......
ThinkPHP Constants
......

ThinkPHP V5.1.39 LTS { 十年磨一剑-为API开发设计的高性能框架 }
```



封装前 (debug = false 模式)

```php
页面错误！请稍后再试～
ThinkPHP V5.1.39 LTS { 十年磨一剑-为API开发设计的高性能框架 }
```



封装后 (预期)

```js
{
	msg: '接口访问异常',
	errorCode: 10000
}
```



#### 实现封装

- 在应用目录 application 下创建目录 lib.
- 在目录 lib 下创建目录 exception,用于存放封装的异常类.
- 在目录 exception 创建类文件 ExceptionHandler.php,内容如下:

```php
<?php
namespace app\lib\exception;

use Exception;
use think\exception\Handle;
use app\lib\exception\BaseException;

class ExceptionHandler extends Handle
{
    // 定义私有变量(状态码, 请求提示, 错误码)
    public $code;
    public $msg;
    public $errorCode;

    // 重写方法 render 
    public function render(Exception $e)
    {
        // 校验是否来自 BaseException
        if ($e instanceof BaseException) {
            // 主动抛出异常 (参数验证失败/程序出错)
            $this->code = $e->code;
            $this->msg = $e->msg;
            $this->errorCode = $e->errorCode;
        } else {

            // 如果在开发中开启 debug 模式, 直接抛出默认异常(未封装)
            if (config('app.app_debug')) { return parent::render($e); }

            // 未知的异常处理
            $this->code = 500;
            $this->msg = '服务器异常';
            $this->errorCode = 999;
        }
        $res = [
            'msg' => $this->msg,
            'errorCode' => $this->errorCode,
        ];
        return json($res, $this->code);
    }
}
```



- 由于我们是自定义封装异常类处理,所以应在配置目录 config 下的文件 app.php 修改配置 exception_handle,如下:

```php
'exception_handle' => '\app\lib\exception\ExceptionHandler',
```



- 在目录 exception 创建类文件 BaseException.php,内容如下:

```php
<?php
namespace app\lib\exception;

use Exception;

class BaseException extends Exception
{
    // 定义私有变量,并默认赋值
    public $code = 400;
    public $msg = '访问异常';
    public $errorCode = 999;

    // 重写方法 __construct, 且默认接收数组类型参数
    public function __construct($params = [])
    {
        if (array_key_exists('code', $params)) { $this->code = $params['code']; }
        if (array_key_exists('msg', $params)) { $this->msg = $params['msg']; }
        if (array_key_exists('errorCode', $params)) { $this->errorCode = $params['errorCode']; }
    }
}
```



#### 使用封装的异常处理

- 通过依赖注入 BaseException.
- 通过 throw new BaseException() 来抛出异常;



```php
use BaseException;

public function controller_name
{
	// 不带参数
	throw new BaseException();
	// 指定参数 (code, msg, errorCode)
	throw new BaseException(['code' => 200, 'msg' => '请求成功!'， 'errorCode' => 0]);
}
```





### 封装验证层

#### 根据场景校验参数

- 在目录 application/common 下创建场景判断验证文件 BaseValidate.php ,内容如下:

```php
<?php
namespace app\common\validate;

use think\Validate;
use app\lib\exception\BaseException;

class BaseValidate extends Validate
{
    /**
     * 验证场景
     *
     * @param boolean $scene
     * @return void
     */
    public function goCheck($scene = false)
    {
        // 获取页面参数
        $params = request()->param();
        // 场景值为 false 时进行全局参数校验,其它值则根据场景值校验参数
        $check = $scene ? $this->scene($scene)->check($params) : $this->check($params);
        // 参数校验
        if (!$check) { throw new BaseException(['msg' => $this->getError(), 'errorCode' => 10000 ]); }
        return true;
    }

}
```



#### 使用命令式创建验证规则文件

```php
php think make:validate TestValidate
```



#### 配置验证层需验证的字段以及规则

```php
<?php

namespace app\common\validate;

use think\Validate;

use app\common\validate\BaseValidate;

class TestValidate extends BaseValidate
{
    /**
     * 定义验证规则
     * 格式：'字段名'	=>	['规则1','规则2'...]
     *
     * @var array
     */	
	protected $rule = [
        'username' => 'require',
        'email' => 'require|email'
    ];
    /**
     * 定义错误信息
     * 格式：'字段名.规则名'	=>	'错误信息'
     *
     * @var array
     */	
    protected $message = [
        'username.require' => '用户名不能为空',
        'email.require' => '邮箱不能为空',
        'email.email' => '邮箱格式错误'
    ];
    /**
     * 定义验证场景
     * 格式: '场景' => ['field', 'field', ...]
     *
     * @var array
     */
    protected $scene = [
        'test' => ['email']
    ];
}

```



#### 使用封装好的验证层

- 某一控制器代码,内容如下:

```php
use TestValidaye;

public function controller_name
{
	// 全部参数校验
	(new Test())->goCheck();
	// 指定场景验证指定字段
	(new Test())->goCheck('test');
}

```





### 封装数据统一格式返回

- 由于每次接口请求后都会返回请求提示或数据,且这些返回数据格式都基本一致.故我们统一封装成一定的格式返回.

- 请求结果返回大致有 2 中形式(有无数据返回).

  - 有数据返回 showDataResult()

  - 无数据返回 showNoDataResult()



- 使用命令式创建基类控制器 BaseController

```
php think make:controller BaseController
```



在基类控制器 BaseController,编辑 2 个静态方法,内容如下:

```php
<?php

namespace app\common\controller;

use think\Controller;
use think\Request;

class BaseController extends Controller
{
    /**
     * showDataResult 统一返回带数据的结果
     *
     * @param string $msg 请求信息提示
     * @param array $data 请求返回的数据
     * @param integer $code 请求的状态码
     * @return void
     */
    static public function showDataResult($msg = 'ok', $data = [], $code = 200)
    {
        $res = [ 'msg' => $msg, 'data' => $data ];
        return json($res, $code);
    }

    /**
     * showNoDataResult 统一返回不带数据的结果
     *
     * @param string $msg 请求信息提示
     * @param integer $code 请求的状态码
     * @return void
     */
    static public function showNoDataResult($msg = 'ok', $code = 200)
    {
        return self::showDataResult($msg, [], $code);
    }
}

```



#### 在控制器中使用基层基类控制的静态方法

- 某一控制器代码,内容如下:

```php
public function controller_name
{
    return self::showDataResult('请求提示', '请求数据结果', '请求状态码');
    return self::showNoDataResult('请求提示', '请求状态码');
}
```





项目开发启动



## 项目开发启动

### TP 框架的文件目录删减

#### application 目录如下

```js
// 删减后的目录
++ application

  ++ common

​    ++ controller

​      -- BaseController.php

​    ++ validate

​      -- BaseValidate.php

  ++ lib

​    ++ exception

​      -- BaseException.php

​      -- ExceptionHandler.php

  -- .htaccess

  -- command.php

  -- common.php

  -- provider.php

  -- tags.php
```



### 对路由进行配置

- 配置文件 config/app.php 关于路由项

  - url_route_must  ---- 是否强制使用路由(建议 true)

  - route_complete_match  ----  路由是否完全匹配 (建议 true)



- 修改文件 route/route.php
  - 对文件 route.php 进行内容删除所有路由.



### 模块目录

#### 使用命令式创建模块

```php
/**
* api => 模块名称
* v1 => 版本控制(接口版本)
* User => 控制器(必须首字母大写)
*/

php think make:controller api/v1/User
```



#### 实现所创建的模块控制器继承基类控制器

- 打开文件 application/api/v1/User.php ,并修改内容如下:

```php
<?php

namespace app\api\controller\v1;

use think\Controller;
use think\Request;

use app\common\controller\BaseController;

class User extends BaseController
{
    // 发送验证码
    public function sendCode()
    {
        return self::showNoDataResult('发送成功!');
    }
}
```



### 路由定义

- 由于控制器 User 已定义 sendCode 作为发送验证码的接口,且在配置文件中已设置 '强制使用路由', ‘路由完全匹配’,故需要在路由配置文件中重新定义路由访问接口.

```php
/**
* 以控制器 User 的发送验证码 sendCode 为例
* :version 匹配参数(版本控制)
*/
Route::post('api/:version/user/sendcode', 'api/v1.User/sendCode');
```



### Postman 接口测试

#### 创建项目接口存放目录

- 点击 New Collection.
- 填写项目接口存放目录名称 Name.
- 填写项目接口存放目录描述 Description.
- 点击 Create.(创建成功)



#### 定义接口环境变量

- 点击setting 齿轮图标.
- 点击 Add.
- 填写接口环境名称  Environment Name.
- 添加接口环境变量.

| variable(变量) | intial value(初始值)           | current value(当前值)          |
| -------------- | ------------------------------ | ------------------------------ |
| devurl         | http://api.xiaoyimi.com/api/v1 | http://api.xiaoyimi.com/api/v1 |
| weburl         | http://api.xiaoyimi.com        | http://api.xiaoyimi.com        |

​	*devurl 为开发环境*

​	*weburl 为生产环境*

- 点击 Add. (创建成功)



#### 接口测试

- 选择请求方式 (Get, Post).

- 输入请求地址.

  - 不使用环境变量

    http://api.xiaoyimi.com/api/v1/user/sendcode

  - 使用环境变量

    {{devurl}}/user/sendcode

- 点击 Send.

- 查看下方请求返回结果.

- 点击 Save,保存至指定接口环境.





### 控制器基本逻辑代码

- 正则校验页面参数
- 调用模型层方法
- 返回请求结果或提示



### 模型层基本代码

- 获取页面参数
- 进行业务逻辑开发
- 返回数据或 true



### 错误码

| 错误码 (errorCode) | 错误码信息提示                            |
| ------------------ | ----------------------------------------- |
|                    |                                           |
| 0                  | OK                                        |
| 1                  | 请勿重新操作                              |
| 10                 | 通用参数错误                              |
| 20                 | 资源未找到                                |
|                    |                                           |
| 30                 | 参数 token 未定义,禁止操作                |
| 31                 | 用户 token 已过期,请重新登录              |
| 32                 | 你已经退出了(备注:退出操作获取不到 token) |
|                    |                                           |
| 40                 | 请选择上传的图片                          |
| 41                 | 上传失败(图片张数\|文件扩展名限制)        |
| 42                 |                                           |
|                    |                                           |
| 50                 | 绑定类型冲突                              |
| 51                 | 该手机号已被绑定                          |
| 52                 | 该邮箱已被绑定                            |
| 53                 | 该第三方登录已被绑定                      |
|                    |                                           |
| 90                 | 阿里大于(短信服务)关闭                    |
| 91                 | 阿里大于(短信服务ClientException)异常     |
| 92                 | 阿里大于(短信服务ServerException)异常     |
|                    |                                           |
| 100                | 验证码频繁获取,请稍后再来                 |
| 101                | 验证码发送失败                            |
| 102                | 验证码日发送量限制                        |
| 103                | 请重新获取验证码                          |
| 104                | 验证码不一致,请重新输入                   |
|                    |                                           |
| 1000               | 该用户不存在                              |
| 1001               | 该用户已存在                              |
| 1002               | 请填写用户名                              |
| 1003               | 该用户已被绑定                            |
| 1004               | 用户名格式错误                            |
| 1005               | 该用户已被禁用                            |
| 1006               | 该用户权限不足                            |
|                    |                                           |
| 1100               | 无效(非法)的手机号                        |
| 1101               | 该手机号已存在                            |
| 1102               | 请填写手机号                              |
| 1103               | 该手机号已被绑定                          |
| 1104               | 手机号格式错误                            |
| 1105               | 未绑定手机号码                            |
|                    |                                           |
|                    |                                           |
| 1200               | 该邮箱不存在                              |
| 1201               | 该邮箱已存在                              |
| 1202               | 请填写邮箱                                |
| 1203               | 该邮箱已被绑定                            |
| 1204               | 邮箱格式错误                              |
|                    |                                           |
| 1300               | 密码输入错误                              |
|                    |                                           |
| 1400               | 该话题分类不存在                          |
|                    |                                           |
|                    |                                           |
|                    |                                           |
|                    |                                           |
|                    |                                           |
|                    |                                           |
|                    |                                           |
|                    |                                           |
|                    |                                           |
|                    |                                           |
|                    |                                           |
|                    |                                           |
|                    |                                           |
|                    |                                           |
|                    |                                           |
|                    |                                           |



## 对接阿里大于 SDK

### 创建项目配置文件

- 在配置目录 config 下创建配置项目文件 api.php,内容如下:

```php
<?php

/* 项目配置文件 */
return [
    // 用户 token 有效时长(0 => 永久)
    'token_expire' => 0,

    // 阿里大于(短信服务)配置
    'aliSMS' => [
        // 是否开启短信验证
        'isopen' => false,

        // 短信验证有效时长
        'expire' => 60,

        // 产品服务名称
        'product' => 'Dysmsapi',

        // 产品服务版本
        'version' => '2017-05-25',

        'regionId' => 'cn-hangzhou',

        // 短信签名名称
        'SignName' => '<SignName>',

        // 短信模板ID 
        'TemplateCode' => '<TemplateCode>',

        // 访问阿里云API的密钥
        'accessKeyId' => '<accessKeyId>',
        'accessSecret' => '<accessSecret>'
    ]
];
```



### 前往阿里云搜索短信服务文档

- 阿里云-短信服务文档地址

https://help.aliyun.com/document_detail/59210.html?spm=5176.11065259.1996646101.searchclickresult.44007b6aXOg8Lh



### 安装 PHP SDK

#### 前提条件

- PHP 开发环境(系统要求).
- 注册阿里云账号并生成访问密钥 (AccessKey).
- 已安装 composer



#### 安装步骤

- 使用 Composer 安装依赖.

```php
composer require alibabacloud/client
```



- 检测是否安装成功
  - 在项目根目录下的目录 vendor 查看是否存在目录 alibabacloud



- 安装完成,前往 OpenAPI Explorer 查询 SendSms,并生成相关 API 的 demo,详见控制台的PHP示例代码.

https://api.aliyun.com/?spm=a2c4g.11186623.2.12.45804e6adntuLm#/?product=Dysmsapi&version=2017-05-25&api=SendSms&tab=DEMO&lang=PHP



#### 重要的参数获取

- 前提条件

  - 登录阿里云

  - 搜索'短信服务'



##### 短信签名名称 (SignName)

- 在 阿里云 => 短信服务 => 国内消息 => 签名管理 => 签名名称

- 注意

  *点击 '国内消息' ,就可以看到 '签名名称' 了.*

  *签名名称: 一般填写为 应用名称*

  *申请审核通过才可使用*



##### 短信模板ID (TemplateCode)

- 在 阿里云 => 短信服务 => 国内消息 => 模板管理 => 模板CODE

- 注意

  *点击 '国内消息' ,就可以看到 '模板管理' 了.*

  *申请审核通过才可使用*



##### 验证码参数变量 (TemplateParam)

- TemplateParam 接收的是一个 JSON 类型的参数值
- 由于模板当中有变量 $code,故在 TemplateParam 传值时必传字段 code.
-  TemplateParam 传参实例 demo,如下:

```json
{ 'code': '验证码值' }
```



##### 阿里云API访问密钥 -- AccessKey  (妥善保管)

- 鼠标移至账户头像,在悬浮弹出层点击 'AccessKey  管理'



###### AccessKey ID

- LTAI4FuvyeeRwTUN7TEKaRqk



###### Access Key Secret

- 9oiF1cv8bFJ6mGxcTZMC7A7drTqwLV 



#### 公共控制器 AliSMSController

- 使用命令式创建公共控制器 AliSMSController

```php
php think make:controller AliSMSController
```



- 公共控制器 AliSMSController 内容如下:

```php
<?php

namespace app\common\controller;

// 注入相关依赖 (SDK)
use AlibabaCloud\Client\AlibabaCloud;
use AlibabaCloud\Client\Exception\ClientException;
use AlibabaCloud\Client\Exception\ServerException;
// 引入异常类
use app\lib\exception\BaseException;

class AliSMSController
{
    static public function sendCode($phone, $code)
    {
        AlibabaCloud::accessKeyClient(config('api.aliSMS.accessKeyId'), config('api.aliSMS.accessSecret'))
            ->regionId(config('api.aliSMS.regionId'))
            ->asDefaultClient();

        try {
            $options = [
                'query' => [
                    'RegionId' => config('api.aliSMS.regionId'),
                    'PhoneNumbers' => $phone,
                    'SignName' => config('api.aliSMS.SignName'),
                    'TemplateCode' => config('api.aliSMS.TemplateCode'),
                    'TemplateParam' => "{\"code\": " . $code . "}",
                ],
            ];
            $result = AlibabaCloud::rpc()
                ->product(config('api.aliSMS.product'))
                // ->scheme('https') // https | http
                ->version(config('api.aliSMS.version'))
                ->action('SendSms')
                ->method('POST')
                ->host('dysmsapi.aliyuncs.com')
                ->options($options)
                ->request();
            return $result->toArray();
        } catch (ClientException $e) {
            throw new BaseException(['msg' => $e->getErrorMessage() . PHP_EOL, 'code' => 200, 'errorCode' => 91 ]);
        } catch (ServerException $e) {
            throw new BaseException(['msg' => $e->getErrorMessage() . PHP_EOL, 'code' => 200, 'errorCode' => 92 ]);
        }
    }
}
```



- 在控制器|模型层中使用阿里大于(短信服务)发送短信,内容如下:

```php
// 注入依赖
use AliSMSController;

public function controller_name
{
    $phone = request()->param('phone');
    $code = random_int(1000, 9999);
    // 发送手机验证码
    $res = AliSMSController::sendSMS($phone, $code);
    
    if ($res['Code'] == 'OK') { return Cache::set($phone, $code, 60); }
    
	if ($res['Code'] == 'isv.MOBILE_NUMBER_ILLEGAL') {
        throw new BaseException(['code' => 200, 'msg' => '无效(非法)的手机号', 'errorCode' => 1100]); 
    }

    if ($res['Code'] == 'isv.DAY_LIMIT_CONTROL') {
        throw new BaseException(['code' => 200, 'msg' => '验证码日发送量限制', 'errorCode' => 102]); 
    }

    throw new BaseException(['code' => 200, 'msg' => '验证码发送失败', 'errorCode' => 101]);
}
```



## 路由分组与版本控制

### 路由分组 (一般分组 | 鉴权分组)

- 将路由访问的前半部分(相同的)路径或通过路由鉴权将某些路由归为一组.

```php
// 案例 -- 路由访问的前半部分(相同的)路径归为一组 (分组前)
Route:get('api/:version/user/sendcode', 'api/:version.User/sendCode');
Route:get('api/:version/user/phonelogin', 'api/:version.User/phoneLogin');

// 案例 -- 路由访问的前半部分(相同的)路径归为一组 (分组后)
Route::group('api/:version/', function () {
   Route::get('user/sendcode', 'api/:version.User/sendCode');
   Route:get('user/phonelogin', 'api/:version.User/phoneLogin');
});


// 案例 -- 路由鉴权 (分组前)
Route:post('api/:version/user/logout', 'api/:version.User/Logout')->middleware(['ApiUserAuth']);
Route:post('api/:version/user/bindphone', 'api/:version.User/bindPhone')->middleware(['ApiUserAuth']);

// 案例 -- 路由鉴权 (分组后)
Route::group('api/:version/', function() {
    Route:post('user/logout', 'api/:version.User/Logout');
    Route:post(user/bindphone', 'api/:version.User/bindPhone');
})->middleware(['ApiUserAuth']);


```





### 版本控制

- 通过路由访问所携带的版本参数来识别地访问指定版本下的路由.

*修改前*

```php
Route::post('api/:version/user/sendcode', 'api/v1.User/sendCode');
```

当项目接口版本有所更迭,该路由只能访问 v1 版本的路由.



*修改后*

```php
Route::post('api/:version/user/sendcode', 'api/:version.User/sendCode');
```

当项目接口版本有所更迭,该路由可根据版本参数 version 的值来识别地访问指定版本下的路由.



- 版本控制的好处
  - 不需要对新版本重写所有接口,只需要复制上一版本的代码,并更改命名空间即可,修改修改局部接口即可.
  - 不需要定义太多的路由.





## 自定义验证规则

### 自定义验证规则方法的形式参数理解

$value    ----    验证规则的字段值

$rule    ----    

$data    ----    页面参数

$field    ----    



### 公有|私有的验证规则

#### 验证规则属于公有与私有的区别

- 验证规则定义的方法位置不同,所使用的范围也不同.

  - 基类验证层 (公共)

  - 当前验证层 (私有)



- 共有验证规则方法定义 (application/common/BaseValidate.php)

```php
...
protected function isPefectCode($value, $rule = '', $data = '', $filed = '')
{
    // 验证逻辑 (demo -- 验证码是否一致)
    $beforeCode = cache($data['phone']);
    if (!$beforeCode) { throw new BaseException(['msg' => '请重新获取验证码', 'errorCode' => 103, 'code' => 200]); }
    if ($beforeCode != $data['code']) { throw new BaseException(['msg' => '验证码不一致,请重新输入', 'errorCode' => 103, 'code' => 200]); }
    return true;
}
...
```



- 私有验证规则方法定义 (application/common/UserValidate.php)

```php
...
protected function isPefectCode($value, $rule = '', $data = '', $filed = '')
{
    // 验证逻辑 (demo -- 验证码是否一致)
    $beforeCode = cache($data['phone']);
    if (!$beforeCode) { throw new BaseException(['msg' => '请重新获取验证码', 'errorCode' => 103, 'code' => 200]); }
    if ($beforeCode != $data['code']) { throw new BaseException(['msg' => '验证码不一致,请重新输入', 'errorCode' => 103, 'code' => 200]); }
    return true;
}
...
```





## 中间件的使用

### 详解中间件

- 在访问路由的过程中,如果该路由已使用中间件,则会优先执行中间件.
- 在执行中间件的过程中,如果执行到 $next($request); 则继续下一个中间件执行.
- 当所有的中间件都执行完毕,再访问指定路由.



### (命令式)创建中间件

```php
php think make:middleware ApiUserAuth
```



### 中间件内部逻辑

- 中间件函数 handle 的形式参数理解

  - $request    ----    请求信息(信息头/参数/系统配置 ...)

  - $next    ----    中间件的回调函数(参数接收 $request)



```php
// 路由鉴权 - token
<?php

namespace app\http\middleware;

use app\lib\exception\BaseException;

class ApiUserAuth
{
    public function handle($request, \Closure $next)
    {
        // 获取信息头
        $header = request()->header();
        
        // 用户 token 不存在,提示非法 token
        if (!array_key_exists('token', $header)) { throw new BaseException(['msg' => '参数 token 未定义,禁止操作', 'errorCode' => 30, 'code' => 200]); }

        // 用户 token 存在
        $token = $header['token'];
        // 根据用户 token 获取(数据表 user | user_bind)用户信息
        $user = cache($token);

        // 缓存中不存在用户信息,用户 token 过期
        if (!$user) { throw new BaseException(['msg' => '用户 token 已过期,请重新登录', 'errorCode' => 31, 'code' => 200]); }

        // 缓存中存在用户信息,则将存储在 request 请求中(关键点)
        $request->userToken = $token;
        $request->userId = array_key_exists('type', $user) ? $user['user_id'] : $user['id'];
        $request->userTokenUserinfo = $user;
        
        // 执行下一个中间件
        return $next($request);
    }
}

```



### 在配置中定义中间件

- 中间件配置文件: config/middleware.php,内容如下:

```php
<?php
    
return [
    // 路由鉴权 - token
    'ApiUserAuth' => app\http\middleware\ApiUserAuth::class,
];  
```



### 在路由中使用中间件

- 在路由配置文件 route/route.php, 内容如下:

```php
// 单路由使用中间件
Route::get('api/:version/user/info', 'api/:version.User/info')->middleware(['ApiUserAuth']);

// 在路由分组中使用中间件(内部路由都会经过中间件过滤)
Route::group('api/:version/', function() {
    Route::get('user/info', 'api/:version.User/info');
})->middleware(['ApiUserAuth']);
```





## 上传图片

### 基本了解

- 文档: https://www.kancloud.cn/manual/thinkphp5_1/354121



### 具体逻辑

- 获取页面上传的图片文件
- 校验图片文件大小,文件扩展名(优化),并定义数据返回格式.
- 将图片文件移动到项目根目录/public/uploads.
- foreach 遍历判断上传图片状态,并写入数据表.
- for 遍历对图片文件的路径获取完整访问路径



### 基本案例 -- 上传图片

- 公共函数层: application/common.php

```php
<?php
    
// 获取文件完整路径 url
function getFileUrl($url = '') {
    if (!$url) { return  false; }
    return url($url, '', false, true);
}
```



- 长传文件的基类控制器层: application/common/controller/FileController.php

```php
<?php

namespace app\common\controller;

use think\Controller;
use think\Request;

class FileController extends Controller
{
    /**
     * 上传单文件
     *
     * @param [type] $file 图片信息
     * @param string $size 图片大小 (3M = 3145728)
     * @param string $ext 图片文件扩展名 (jpg|png|gif)
     * @param string $path 图片存放路径 (项目根目录/public/uploads)
     * @return void ['data' => '文件路径', '上传状态']
     */
    static public function UploadEvent($file, $size = '3145728', $ext = 'jpg,png,gif', $path = 'uploads')
    {
        $info = $file->validate(['size' => $size, 'ext' => $ext])->move($path);
        return [
            'data' => $info ? $info->getPathname() : $info->getError(),
            'status' => $info ? true : false
        ];
    }
}

```





- 上传图片模型层: application/common/model/Image.php

```php
<?php

namespace app\common\model;

use app\common\controller\FileController;
use app\lib\exception\BaseException;
use think\Model;

class Image extends Model
{
    // 自动时间戳
    protected $autoWriteTimestamp = true;

    // 上传多图
    public function uploadMore()
    {
        // 获得上传成功后的图片信息
        $images = $this->upload(request()->userId, 'imglist');

        // 图片张数
        $imgCount = count($images);

        // 遍历循环获取完整的图片路径
        for ($i=0; $i<$imgCount; $i++) {
            $images[$i]['url'] = getFileUrl($images[$i]['url']);
        }

        return $images;
    }


    // 上传图片
    public function upload($userid = '', $field = '')
    {
        // 获取上传图片的文件
        $files = request()->file($field);

        // (多图)图片文件上传接收的数据为数组类型
        if (is_array($files)) {
            
            // 存储上传成功的结果
            $arr = [];
            // 开始上传图片
            foreach($files as $file) {
                $res = FileController::UploadEvent($file);
                // 上传成功
                if ($res['status']) {
                    $arr[] = [
                        'url' => $res['data'],
                        'user_id' => $userid
                    ];
                }
            }
            // 一次性写入数据表 image
            return $this->saveAll($arr);
        }
        
        // 单文件上传
        if (!$files) { throw new BaseException(['msg' => '请选择上传的图片', 'errorCode' => 40, 'code' =>200]); }

        $file = FileController::UploadEvent($files); var_dump($file);
        // 上传失败
        if (!$file['status']) { throw new BaseException(['msg' => '上传失败(图片张数|文件扩展名限制)', 'errorCode' => 41, 'code' =>200]); }
        
        return self::create(['url' => $file['data'], 'user_id' => $userid]);
    }
}
```



## WebSocket 通信

*注意:  由于我们使用的是 Thinkphp 5.1, 故在项目根目录安装时请安装 2.0.x 版本的 WebSocket.*



### 安装步骤

- 文档详解: Thinkphp 5.1 手册 => 扩展库 => Workerman
- 使用 composer 命令安装

```php
composer require topthink/think-worker=2.0.*
// 或者
composer require topthink/think-worker 2.0.x
```

- 安装成功,可在项目目录 config 看到多了 3 个文件.

  - gateway_worker.php

  - worker_server.php

  - worker.php

- 在本项目的通讯服务功能中,我们只会使用到文件 gateway_worker.php.



### 原理图解

```js
// 图解流程 (m, n 为随机数字)
client_m => gateway_n => worker_n => gateway_m => client_n

1. 客户端 client_m 打开连接并发送数据到 gateway_n 进程.
2. worker_n 进程接收来自 gateway_n 进程的数据.
3. 再将数据经过 gateway_m 进程转发给客户端 client_n.


Client => HTTP(GET|POST) => Web站点(TP框架,会校验用户身份/路由/参数等的合法性) => (单向进入)GateWayWorker => Websocket(单向返回) => Client
```



### Worker 优点

- 简单实现客户端之间的通信.

- 从原理可知, gateway 与 worker 给予 socket 长连接通信. gateway 与 worker 可以部署在不同服务器上(分布式部署).
- gateway 进程只负责网路IO，业务逻辑交给 worker 处理.
- 可以 reload Worker 进程，实现在不影响用户的情况下完成代码热更新.



### 配置及使用

- Workerman 官网: https://www.workerman.net/

- GatewayWorker 手册: http://doc2.workerman.net/
- 由于本项目只用到 gateway_worker.php, 故我们看 GatewayWorker 手册。

详见视频 205



- 使用 websocket，请先把项目打包发送到服务器环境
  - 切换至项目根目录,选择所有文件和文件夹,右键添加至压缩文件(.ZIP).
  - 进入 Navicat,选择项目 sql 数据库,右键转存为 SQL 文件.
  - 进入宝塔面板 => '网站' => '添加站点',填写配置(域名(example.dishait.cn)...).
  - 没有域名,请前往阿里云解析设置 dishait.cn,添加记录 (主机记录(域名 example), 记录值(IP)).
  - 进入宝塔面板 => '文件' => www.root => example.dishait.cn,可检测访问 example.dishait.cn.
  - 上传已打包的项目.ZIP后并解压,进入'网站' => 找到 example.dishait.cn => 配置 => 网站目录 => 运行目录(/publicj.
  - 进入宝塔面板 => '网站' => 找到 example.dishait.cn => 配置 => 伪静态(设置成 (thinkphp).
  - 进入宝塔面板 => ‘数据库’ => example.dishait.cn => 导入(已转存为 SQL 的文件).
  - 进入宝塔面板 => ‘文件’ => config/database.php (打开修改 database 为 example.dishait.cn, 用户名及密码).
  - 测试访问接口API, 网址输入 example.dishait.cn/api/v1/postclass.可查看到返回数据.
  - 进入宝塔面板 => ‘文件’ => 找到'终端'图标,点击并进入 => 切换为当前项目 => 测试(php think).
  - 打开 websocket,执行命令 php think worker:geteway.可见 websocket://0.0.0.0:1234.
  - 复制宝塔面板(网站的IP),以及 websocket 的端口号(1234),百度搜索 websocket 在线连接,输入 ws://IP:port 进行测试.(可能访问不到,就是安全组未开放(端口)).
  - 进入宝塔面板 => ‘文件’ => geteway_worker.php 进行修改 port(12345最好大一些,防止端口冲突).
  - 进入宝塔面板 => ‘安全’ => 填写放行端口(12345),以及备注.
  - 进入阿里云 => 云服务器 ECS => 实例 => 更多 => 网络和安全组 => 安全组配置 => 配置规则 => 添加安全组规则 => 填写端口(12345)以及描述.
  - 在 websocket 在线连接的网站中,输入 ws://IP:port 再次进行测试.可见连接成功后的 client_id



### Workerman 官网手册

#### Gateway 类提供的接口

sendToClient(client_id, send_data)    =>    向客户端发送消息

bindUid(client_id, uid)    =>    绑定用户ID,实现多端推送.

sendToUid(uid, message)    =>    发送消息给用户下所绑定的客户端.

isUidOnline(uid)    =>    检测用户 uid 是否在线(需配合 bindUid(client_id, uid)  使用, 返回 1 | 0).



### 为处理兼容小程序(wss和隐藏端口号)

#### 特别注意

- 小程序要求使用 wss 协议,而且必须隐藏端口号.

- GateWayWorker 可以实现 wss 协议,但是无法隐藏端口号.



#### 解决方案

- 下载 GatewayWorker
  - 地址: https://www.workerman.net/download
- 将下载的 GatewayWorker 进行解压,可见 GatewayWorker  根目录下内容:

```js
+ GatewayWorker
  + Applications
  + vendor
  - composer.json
  - start_for_win.bat
  - start.php
  ...
```



- 将整个 GatewayWorker   根目录剪切到项目根目录下的目录 extend 下.
- 在项目根目录下的目录 vendor 的目录 workerman 删除内部文件.(建议压缩备份).



#### 产生闲置报废的文件

- /application/lib/socket/Event.php

- /config/gateway_worker.php



#### 产生的配置处理(需要对照 gateway_worker.php 进行对扩展配置处理)

*注意: GatewayWorker 方法文档 http://workerman.net/gatewaydoc/*

- 进入目录 /extend/GatewayWorker/Applications/YourApp.
- 修改文件  /extend/GatewayWorker/Applications/YourApp/Events.php.

```php
// 修改客户端连接返回的数据格式

<?php

use \GatewayWorker\Lib\Gateway;

class Events {
	// 客户端连接时触发
    public static function onConnect($client_id)
    {
        // // 向当前client_id发送数据 (demo)
        // Gateway::sendToClient($client_id, "Hello $client_id\r\n");
        // // 向所有人发送 (demo)
        // Gateway::sendToAll("$client_id login\r\n");

        // 发送当前客户端 ID 给用户,返回 json 格式
        //sendToCurrentClient($string)
        return Gateway::sendToCurrentClient(json_encode([
            'type' => 'bind',
            'client_id' => $client_id    
        ]));
    }
    
    // 客户端发送消息时触发
    public static function onMessage($client_id, $message)
   {
   }
   
   // 客户端断开连接时触发
   public static function onClose($client_id)
   {
   }
    
}
```



- 修改文件  /extend/GatewayWorker/Applications/YourApp/start_businessworker.php.

```php
// 修改 bussinessWorker进程数量(核数的2倍)
// 服务注册地址(端口同服务器 websocket 端口)

<?php

use \Workerman\Worker;
use \Workerman\WebServer;
use \GatewayWorker\Gateway;
use \GatewayWorker\BusinessWorker;
use \Workerman\Autoloader;

// 自动加载类
require_once __DIR__ . '/../../vendor/autoload.php';

// bussinessWorker 进程
$worker = new BusinessWorker();
// worker名称
$worker->name = 'YourAppBusinessWorker';
// bussinessWorker进程数量(核数的2倍)
$worker->count = 4;
// 服务注册地址
$worker->registerAddress = '127.0.0.1:1238';

// 如果不是在根目录启动，则运行runAll方法
if(!defined('GLOBAL_START'))
{
    Worker::runAll();
}
```



- 修改文件  /extend/GatewayWorker/Applications/YourApp/start_gateway.php.

```php
<?php

use \Workerman\Worker;
use \Workerman\WebServer;
use \GatewayWorker\Gateway;
use \GatewayWorker\BusinessWorker;
use \Workerman\Autoloader;

// 自动加载类
require_once __DIR__ . '/../../vendor/autoload.php';

// gateway 进程，这里使用Text协议，可以用telnet测试
// $gateway = new Gateway("tcp://0.0.0.0:8282");
$gateway = new Gateway("websocket://0.0.0.0:8282");

// gateway名称，status方便查看
$gateway->name = 'YourAppGateway';
// gateway进程数
$gateway->count = 4;
// 本机ip，分布式部署时使用内网ip
$gateway->lanIp = '127.0.0.1';
// 内部通讯起始端口，假如$gateway->count=4，起始端口为4000
// 则一般会使用4000 4001 4002 4003 4个端口作为内部通讯端口 
$gateway->startPort = 2900;
// 服务注册地址
$gateway->registerAddress = '127.0.0.1:1238';

// 心跳间隔
$gateway->pingInterval = 30;
// 心跳数据
$gateway->pingData = '{"type":"ping"}';

if(!defined('GLOBAL_START'))
{
    Worker::runAll();
}


```



- 修改文件  /extend/GatewayWorker/Applications/YourApp/start_register.php.

```php
<?php

use \Workerman\Worker;
use \GatewayWorker\Register;

// 自动加载类
require_once __DIR__ . '/../../vendor/autoload.php';

// register 必须是text协议
$register = new Register('text://0.0.0.0:1238');

// 如果不是在根目录启动，则运行runAll方法
if(!defined('GLOBAL_START'))
{
    Worker::runAll();
}
```



- 在项目根目录下 /public/index.php 引入文件 /extend/GatewayWorker/vendor/autoload.php

```php
<?php

// [ 应用入口文件 ]
namespace think;

// 加载基础文件
require __DIR__ . '/../thinkphp/base.php';

// 引入 gatewayworker 文件
require __DIR__ . '/../extend/GatewayWorker/vendor/autoload.php';

// 解决跨域问题
header("Access-Control-Allow-Origin: *");

// 支持事先使用静态方法设置Request对象和Config对象

// 执行应用并响应
Container::get('app')->run()->send();

```



- 启动 websocket 服务

进入宝塔面板  => 文件 => 终端,进入 /extend/GatewayWorker/ ,执行以下命令:

```cmd
# -d 手动进程启动
php start.php start -d
```



- 测试 websocket 连接

在 websocket 在线测试中输入 wss://example.com/wss ,检测是否成功.





#### WebSocket 配置的理解

```php
<?php

// 自定义 websocket 配置

return [

    'worker' => [
        // worker 名称
        'name' => 'YourAppBusinessWorker',
        // bussinessWorker进程数量(核数的2倍) (自定义配置)
        'count' => 4,
        // 服务注册地址 (自定义配置端口号)
        'registerAddress' => '127.0.0.1:1238'
    ],
    
    'gateway' => [
        // websocket 访问地址 (自定义配置,端口号为宝塔安全组配置放行的端口)
        'url' => 'websocket://0.0.0.0:8282',
        // gateway名称，status方便查看
        'name' => 'YourAppGateway',
        // gateway进程数(跟 worker 进程数一致) (自定义配置)
        'count' => 4,
        // 本机ip，分布式部署时使用内网ip (自定义配置)
        'lanIp' => '127.0.0.1',
        // 内部通讯起始端口，假如$gateway->count=4，起始端口为4000
        // 则一般会使用4000 4001 4002 4003 4个端口作为内部通讯端口 (端口占用时就修改)
        'startPort' => 2900,
        // 服务注册地址(跟 worker 服务注册地址一致)
        'registerAddress' => '127.0.0.1:1238',
        // 心跳间隔
        'pingInterval' => 30,
        // 心跳数据
        'pingData' => '{"type":"ping"}',
    ],

    // register 必须是text协议(端口号跟服务器注册地址的端口号一致)
    'register' => 'text://0.0.0.0:1238'

];
```





## 部署 HTTPS

- 进入阿里云官网.
- 搜索 ssl, 点击 SSL 证书.
- 购买 SSL 证书 (有一个免费版测试用的).
-  证书申请 (证书绑定域名-项目网址, ...).
- 在域名解析那里查看是否生成记录值,在证书申请点击验证.
- 验证成功,点击提交审核.
- 根据对应的服务器类型(Apache, Nginx)下载对应的 SSL 证书.
- 解压证书压缩包(.key, .pem).
- 进入宝塔面板 => 网站 => 项目 => 设置 => SSL => 其它证书
- 打开 .key, .pem 文件,复制里面全部内容到密钥(key), 证书(pem) 进行粘贴保存,并且强制使用 HTTPS.
- 进入浏览器网址输入域名进行访问(http => https).





## 隐藏端口号

进入宝塔面板 => 网站 => 设置 => 配置文件,内容如下:

```
...

// 自行添加以下这段代码,(访问由 ws://IP:port => wss://Domain/wss)
// wss://Domain/wss 后的 wss 来自于以下 location /wss

location /wss
{
	proxy_pass: '127.0.0.1:(安全组放行的端口)';
	proxy_http_version: 1.1;
	proxy_set_header: Upgrade $http_upgrade;
	proxy_set_header: Connection "Upgrade";
	proxy_set_header: X-Real-IP $remote_addr;
}



location ~^/(\.user.ini....) {
	return 404;
}
...
```







## 跨域处理

### 所有模块中应用

在入口文件中 public/index.php 处理,内容如下:

```php
<?php

// [ 应用入口文件 ]
namespace think;

// 加载基础文件
require __DIR__ . '/../thinkphp/base.php';

// 支持事先使用静态方法设置Request对象和Config对象

// 解决跨域
header("Access-Control-Allow-Origin: *");

// 执行应用并响应
Container::get('app')->run()->send();

```



### 单个模块中使用

在模块公共文件夹 common 下创建 文件夹 bahavior, 并在目录 behavior 下创建 文件 Cors.php,内容如下:

```php
// Cors.php

<?php

namespace app\common\behavior;
use think\Exception;
use think\Response;

class Cors {

    // 解决跨域处理
    public function run ($dispatch) {
        $host_name = isset($_SERVER['HTTP_ORIGIN']) ? $_SERVER['HTTP_ORIGIN'] : "*";
        $headers = [
            "Access-Control-Allow-Origin" => $host_name,
            "Access-Control-Allow-Credentials" => 'true',
            "Access-Control-Allow-Headers" => "X-Token,x-uid,x-token-check,x-requested-with,content-type,Host"
        ];

        if($dispatch instanceof Response) {
            $dispatch->header($headers);
            // var_dump('if');
        } else if($_SERVER['REQUEST_METHOD'] === 'OPTIONS') {
            // var_dump('else if');
            $dispatch['type'] = 'response';
            $response = new Response('', 200, $headers);
            $dispatch['response'] = $response;
        }
    }
}
```



在 文件 application/tags.php 中

```php
// tags.php
<?php

// 应用行为扩展定义文件
return [
    // 应用初始化
    'app_init'     => [],
    // 应用开始
    'app_begin'    => [
    	'app\\common\\behavior\\Cors'
    ],
    // 模块初始化
    'module_init'  => [],
    // 操作开始执行
    'action_begin' => [],
    // 视图内容过滤
    'view_filter'  => [],
    // 日志写入
    'log_write'    => [],
    // 应用结束
    'app_end'      => [
    	'app\\common\\behavior\\Cors'
    ],
];
```



### 单文件使用

```php
// 某文件.php

header("Access-Control-Allow-Origin: *");
```





## PostMan 导出接口文档

- 点击设置,进行环境配置(线上 API 接口地址).
- 点击 API 接口文件夹的更多(···),点击 export.
- 进入网址设置环境...
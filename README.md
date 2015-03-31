# 如何实现利用免费资源打造公有库以及私有库

开发规范的包文件，并将这些包发布到`github` or `bitbucket`打造公有以及私有库

> * 开发规范的library包文件
> * 创建公有包(github)
> * 创建私有包(bitbucket)
> * 如何使用[https://packagist.org/](https://packagist.org/)
> * 项目中引入公共包
> * 项目中引入私有包





### 1. 如何开发规范的library包

参考教程：[https://packagist.org/about](https://packagist.org/about)

* 创建组织架构

    ###### 组织架构如下: 
        irpackagist                                     //根目录(irpackagis这个无所谓随便起)
            |---README.md                               //帮助文档
            |---composer.json                           //固定格式文件(必须)
            |---lib                                     //类库包目录(一般设为src或者lib)
                 |---Iran                               //目录
                       |---Func                         //目录
                             |---StringHelp.php         //类库文件1
                             |---StringHelp2.php        //类库文件2

* 编写类功能文件

    ###### 示例`StringHelp.php`如下:
        <?php
        namespace Iran\Func;
        class StringHelp{
            static public function dd(){
                echo 'dd test';
            }
        }



* 创建`composer.json`文件: 

    ###### **下面是composer.json示例**:
        { 
            "name": "iran/test",                                        //包名字(必须)
            "type": "library" ,
            "description": "my first test", 
            "keywords": ["iran", "iranw"], 
            "homepage": "http://www.phpno.com", 
            "license": "MIT",
            "authors": [
                {"name": "Iran Wang", "email": "wang_wenguan@yeah.net"}
            ],
            "require": {
                "php": ">=5.3.2"
            },
            "require-dev": {
                "phpunit/phpunit": ">=3.7"
            },
            "autoload": {
                "psr-0": { "Iran\\Func\\": "lib/" }                     //自动加载
            }
        }
    ###### `composer.json`参数参考文档 [https://getcomposer.org/doc/04-schema.md](https://getcomposer.org/doc/04-schema.md)


自此，一个标准的php包就算开发完毕^^ (装逼完成)---


### 2. 上传到`Github`公有化

抱着开放，开源，屌丝的气质，我们将此类包公有化。造福大众，恩泽天下(话说那个sb会用呢???)

* 创建仓库

    ###### 直接登录github，web端创建仓库[https://github.com/new](https://github.com/new) (如果你不会 我只能哈哈了)，这里我们创建`irpackagist`  url:[https://github.com/iranw/irpackagist](https://github.com/iranw/irpackagist)

* 上传本地代码->仓库
    
    ######  代码上传流程:
        $cd /f/git-package/irpackagist
        $git clone https://github.com/iranw/irpackagist
        $git add --all                                              //将刚才开发的高性能包添加到git
        $git commit -m "test my private packagist"                  //注释
        $git config --global user.name "iranw"                      //记得替换你的用户名
        $git push origin master                                     //推送到github服务器
        $git tag -a v1.0 -m "my version 1.0"                        //创建版本标志
        $git push origin v1.0                                       //推送到特定版本标志

自此我们开发的包就已经在互联网上流行开来(如果主够nb的话 可能还会有人来挖你哦)


### 3. 上传到`bitbucket`私有化

有时候我们的包非常敏感，或者说我们非常自私。不允许别人来查看以及使用我们的包。那么,我们可以使用免费的[bitbucket](https://bitbucket.org/dashboard/overview)服务来服务我们的私有化包。
具体`bitbucket`是什么，自行google

具体上传步骤类似`github`,都是基于git的，不再详述


### 4. 包开放到`packagist.org`

将我们的公有包添加到[packagist.org](https://packagist.org/)索引，以便别人更好的搜索。

只要在[https://packagist.org/packages/submit](https://packagist.org/packages/submit)页面将我们的github公共包url地址(例如[https://github.com/iranw/irpackagist](https://github.com/iranw/irpackagist))
添加到输入框即可。之后你就可以在`packagist.org`的搜索框里搜索到你上传的包啦(^_^......)

注：添加的时候回验证你的composer.json正确性，所以严格按照要求填写。可以参考第三方包的书写格式


### 5. 如何引入公有包

* 创建composer.json文件

在项目根目录创建composer.json文件 如下:

    {    
        "require": {
            "iran/test": "1.0"      #这个包就是刚才我们的上传的包
        }
    }

* 执行composer update

前提你的电脑需要安装composer工具，教程参考[https://getcomposer.org/doc/00-intro.md](https://getcomposer.org/doc/00-intro.md)

* 入口布局

    ###### 框架目录结构如下:     
        yafroot
            |---composer.json
            |---public
                   |---index.php
            |---app
            |---vendor
                   |---iran 
                         |---test
                               |---lib
                                    |---Iran
                                          |---Func
                                                |---String.php

* 如何使用
    
    ######直接在入口文件使用样例:
        <?php
        require '../vendor/autoload.php';
        \Iran\Funcc\StringHelp::dd();

### 6. 如何引入私有包
    
私有包和公有包引入的的规则基本一致，只要在`composer.json`文件稍作修改即可

* 私有包引入composer.json文件格式
    
    ###### 样例:
        {    
            "require": {
                "iran/test": "1.0",                                     //公有库
                "iran/irpackagist3": "1.0"                              //私有库
            },
            "repositories": [
                {
                    "type": "vcs",                                      //私有库类型
                    "url": "https://bitbucket.org/iranw/test"           //私有库地址
                }
            ]
        }

注：在`composer update`的过程中需要输入私有库的用户密码

* 如何使用
    
    ######直接在入口文件使用样例:
        <?php
        require '../vendor/autoload.php';
        \Iran\Funcc\StringHelp::dd();           //公有库函数调用
        \Iran\Funccc\StringHelp::dd();          //私有库函数调用


--- 自此 整个公有库&私有库调用教程制作完毕 ---

创建私有库的其他途径(自建镜像库，Sais,Artifact)

[https://getcomposer.org/doc/05-repositories.md#hosting-your-own](https://getcomposer.org/doc/05-repositories.md#hosting-your-own)









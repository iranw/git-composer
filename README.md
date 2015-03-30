# 如何实现利用免费资源打造共有库以及私有库

开发规范的包文件，并将这些包发布到github or bitbucket打招共有以及私有库

> * 开发规范的library包文件
> * 创建共有包(github)
> * 创建私有包(bitbucket)
> * 如何使用[https://packagist.org/](https://packagist.org/)

#### 1. 如何开发规范的library包

参考教程：[https://packagist.org/about](https://packagist.org/about)

* 创建composer.json文件    

``
    {
        "name": "iran/test",
        "type": "library",
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
            "phpunit/phpunit":         ">=3.7"
        },
        "autoload": {
            "psr-0": { "Iran\\Func\\": "lib/" }
        }
    }
``
/* 使用引用 blockquote*/
>Code Share
>> Code Share

sadfsad







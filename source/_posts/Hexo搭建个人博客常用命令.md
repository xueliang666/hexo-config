title: Hexo搭建个人博客常用命令
author: 学亮
date: 2019-04-28 14:53:05
tags:
---
> 快速上手篇所以一切都配置好之后（按照个性化配置好你的_config.yml文件），你发布一篇博客到网上只需要三步：
> 
> 第一步：创建博文在你的博客主目录下通过git bash键入 hexo new 博客标题
> 第二步：按照hexo的渲染方式将你的博文渲染成静态文件继上一步之后， hexo g（完整命令是hexo generate，即生成文件的意思）
> 
> 第三步：按照_config.yml里配置的发布方式，将public里的静态文件上传至你的线上代码仓库（Github或Coding）hexo
> d（完整命令是hexo deploy，即发布的意思）仅此而已，这时打开浏览器，输入符合你pages的域名，看看自己的博客是不是显现出来了。
# HexoKidimos

#### 介绍
用于存储Kidimos简单得可怜的博客

#### 软件架构
软件架构说明
本博客的代码部分存储在gitee上，静态文件则在github.io生成。
可以访问Kidimos.github.io查看博客.


#### 安装教程

1.  xxxx
2.  xxxx
3.  xxxx

#### 使用说明

1.  当把hexo博客项目clone到本地的时候，我们无法直接进行hexo clean/hexo g/hexo s等命令
2.  原因是我们在git push的过程中，并没有将node_modules文件夹一起push到仓库中，这是对的。
3.  为了能够在本地成功地运行，我们需要在根目录下运行npm install hexo-deployer-git --save命令，这条命令可以帮助我们在本地生成node_modules文件夹及内容，运行完这条命令我们就可以在本地运行hexo博客系统了！
4.  以下是几条常用命令的作用
    1.  hexo clean —— 清除缓存文件和已生成的静态文件
    2.  hexo d —— 自动生成网站静态文件，并部署到设定的仓库
    3.  hexo g —— 生成网站静态文件到默认设置的public文件夹
    4.  hexo s —— 启动本地服务器，用于预览主题
    5.  hexo new "xxx" —— 新建一篇标题为xxx的文章

#### 参与贡献

1.  Fork 本仓库
2.  新建 Feat_xxx 分支
3.  提交代码
4.  新建 Pull Request


#### 特技

1.  使用 Readme\_XXX.md 来支持不同的语言，例如 Readme\_en.md, Readme\_zh.md
2.  Gitee 官方博客 [blog.gitee.com](https://blog.gitee.com)
3.  你可以 [https://gitee.com/explore](https://gitee.com/explore) 这个地址来了解 Gitee 上的优秀开源项目
4.  [GVP](https://gitee.com/gvp) 全称是 Gitee 最有价值开源项目，是综合评定出的优秀开源项目
5.  Gitee 官方提供的使用手册 [https://gitee.com/help](https://gitee.com/help)
6.  Gitee 封面人物是一档用来展示 Gitee 会员风采的栏目 [https://gitee.com/gitee-stars/](https://gitee.com/gitee-stars/)

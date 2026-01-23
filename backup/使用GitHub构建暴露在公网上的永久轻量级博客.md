[Gmeek](https://github.com/Meekdai/Gmeek)
---
>本项目来源于https://github.com/Meekdai/Gmeek
>一个博客框架，超轻量级个人博客模板。完全基于Github Pages 、 Github Issues 和 Github Actions。不需要本地部署，从搭建到写作，只需要18秒，2步搭建好博客，第3步就是写作。
 - [Demo页面](http://meekdai.github.io/)
 - [Gmeek更新日志](https://meekdai.github.io/post/Gmeek-geng-xin-ri-zhi.html)
 - [Gmeek快速上手](https://blog.meekdai.com/post/Gmeek-kuai-su-shang-shou.html)
 
## 部署博客
1.[注册ggithub](https://github.com/)
2.前往项目[Gmeek](https://github.com/Meekdai/Gmeek)
3.点击这里[通过模板创建仓库](https://github.com/new?template_name=Gmeek-template&template_owner=Meekdai)或；
![](http://momo-1-img.ao1160301aila.workers.dev/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-04-12%20191353.png)

## 修改全局文件 config.json
``` config.json
 {
     "title":"秋", #标题
     "subTitle":"秋的国内笔记" #摘要/简介
     "avatarUrl":"http://momo-1-img.ao1160301aila.workers.dev/momobako.png", #头像/图标
     "GMEEK_VERSION":"last"
 }
```
![](http://momo-1-img.ao1160301aila.workers.dev/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-04-12%20192756.png)
**对应修改**
![](http://momo-1-img.ao1160301aila.workers.dev/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-04-12%20193231.png)

## 发布文章
在github仓库的issues->点击New issues进行编辑
![](http://momo-1-img.ao1160301aila.workers.dev/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-04-12%20193516.png)

## 发布内容 （使用Makrdown写作语言编写）
1.Add a title=标题
2.Add a description=编辑文章
3.**`注意Labels要选择documentation`**
4.点击Create发布文章

![](http://momo-1-img.ao1160301aila.workers.dev/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-04-12%20194207.png)


域名访问
---
>github提供的域名还是蛮长的不好记，可以替换成自己的域名
>>在仓库的Settings中Pages->Custom domain填写自己的域名，建议二级域名(去域名提供商添加CNAME记录、内容'子域名'、目标``XXX.github.io``)[添加域名规则](https://docs.github.com/pages/configuring-a-custom-domain-for-your-github-pages-site)->输入添加好的子域->Save完成设置
![](http://momo-1-img.ao1160301aila.workers.dev/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-04-12%20195536.png)
`一会儿就可以访问了`

### 教程结束
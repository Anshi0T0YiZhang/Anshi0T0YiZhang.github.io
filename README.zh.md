# Hux blog ģ��

### [�ҵĲ��������� &rarr;](http://huxpro.github.io)


### �����յ�"Page Build Warning"��email

����jekyll������3.0.x,��ԭ����pygments�����������֧�֣���ֻ֧��һ��-rouge����������Ҫ�� `_config.yml`�ļ����޸�`highlighter: rouge`.���⻹��Ҫ��`_config.yml`�ļ��м���`gems: [jekyll-paginate]`.

ͬʱ,����Ҫ������ı���jekyll����.

ʹ��`jekyll server`��ͬѧ��Ҫ������

1. `gem update jekyll` # ����jekyll
2. `gem update github-pages` #���������İ�

ʹ��`bundle exec jekyll server`��ͬѧ�ڸ���jekyll����Ҫ����`bundle update`�����������İ�.

�ο��ĵ���[using jekyll with pages](https://help.github.com/articles/using-jekyll-with-pages/) & [Upgrading from 2.x to 3.x](http://jekyllrb.com/docs/upgrading/2-to-3/)


## ����ģ��(beta)

�ҵĲ��Ͳֿ⡪��`huxpro.github.io`���Ǿ����޸ĵģ����һ����������ύ���룬��˸��������һ���ȶ����ģ�塣��ҿ���ֱ��forkģ�塪��`huxblog-boilerplate`,Ҫ�ĵĵط��Ҷ�˵���ˡ����߿���ֱ������zip�������Լ�ȥ�޸ġ�

```
$ git clone git@github.com:Huxpro/huxblog-boilerplate.git
```

**[������Ԥ��ģ�� &rarr;](http://huangxuan.me/huxblog-boilerplate/)**

## ���汾����

##### New Feature (V1.5.2)

* ����fork���ҵĲֿ�֮�󣬻�Ҫɾ������Ĺ����ҵ��ĵ��ǲ��Ǹе��Է����أ�**Boilerplate** ģ�彫��������ٿ�ʼ������ϲ�����¡�
* `-apple-system`����ӵ���������������ˣ����������ʽ�ܽ�iOS9Ĭ�ϵ�������**San Francisco**���ֵķǳ�Ư����
* ����˴�������Զ����е�bug,�滻Ϊ������������������[issue#15](https://github.com/Huxpro/huxpro.github.io/issues/15)

###### ������ʷ�汾���˾���û�б�Ҫ�˽⣬����Ӣ�ľ����ˡ�



## ֧��

<<<<<<< HEAD
* ��������ɵ�fork��������ܽ��������ߺ� github �ĵ�ַ���������ҳ��ײ����ҽ��ǳ���л�㡣
* �����ϲ���ҵ��������ģ�壬����`huxpro.github.io`���repository����ޡ������Ͻ�**star**һ�¡�
=======
* 你可以自由的fork。如果你能将主题作者和 github 的地址保留在你的页面底部，我将非常感谢你。
* 如果你喜欢我的这个博客模板，请在`huxpro.github.io`这个repository点个赞——右上角**star**一下。
>>>>>>> refs/remotes/Huxpro/master

## ˵���ĵ�

* ��ʼ
	* [����Ҫ��](#environment)
	* [��ʼ](#get-started)
	* [дһƪ����](#write-posts)
* ���
	* [�����](#sidebar)
	* [���������](#mini-about-me)
	* [�Ƽ���ǩ](#featured-tags)
	* [��������](#friends)
	* [HTML5 ��ʾ�ĵ�����](#keynote-layout)
* ������ Google/Baidu Analytics
	* [����](#comment)
	* [��վ����](#analytics) 
* �߼�����
	* [�Զ���](#customization)
	* [�����ͼ](#header-image)
	* [����չʾ����-ͷ�ļ�](#seo-title)

#### Environment

����㰲װ��jekyll������ֻ��Ҫ������������`jekyll serve`�����ڱ��������Ԥ�����⡣�㻹��������`jekyll serve --watch`���������Ա��޸ı��Զ������޸ĺ���ļ���

�� [@BrucZhaoR](https://github.com/BruceZhaoR)�Ĳ��ԣ�������������ǿ��Ե��Զ������޸ĺ���ļ��ģ�ˢ�º����ʵʱԤ�����ٷ��ļ��ǽ��鰲װbundler���������ڱ��ص�Ч���͸���github������һ���ġ�����������https://help.github.com/articles/using-jekyll-with-pages/#installing-jekyll


#### Get Started

�����ͨ���޸� `_config.yml`�ļ������ɵĿ�ʼ��Լ��Ĳ���:

```
# Site settings
title: Hux Blog             # ��Ĳ�����վ����
SEOTitle: Hux Blog			# �ں������ϸ̸��
description: "Cool Blog"    # ���˵�㣬����һ��

# SNS settings      
github_username: huxpro     # ���github�˺�
weibo_username: huxpro      # ���΢���˺ţ��ײ����ӻ��Զ����µġ�

# Build settings
# paginate: 10              # һҳ��׼���ż�ƪ����
```

Jekyll�ٷ���վ���кܶ�Ĳ������Ե��������������µ�������ʽ...��ַ�����[Jekyll - Official Site](http://jekyllrb.com/) ���İ�������[Jekyll����](http://jekyllcn.com/).

#### write-posts

Ҫ���������һ����markdown�ĸ�ʽ��������`_posts/`����ֻҪ������ƪģ�������������������׸�������á�

yaml ͷ�ļ�������:

```
---
layout:     post
title:      "Hello 2015"
subtitle:   "Hello World, Hello Blog"
date:       2015-01-29 12:00:00
author:     "Hux"
header-img: "img/post-bg-2015.jpg"
tags:
    - Life
---

```

#### SideBar

���ұ�:
![](http://huangxuan.me/img/blog-sidebar.jpg)

�������� `_config.yml`�ļ������`Sidebar settings`�ǿ顣
```
# Sidebar settings
sidebar: true  #��Ӳ����
sidebar-about-description: "�򵥵�����һ�����Լ�"
sidebar-avatar: /img/avatar-hux.jpg     #��Ĵ�ͷ������ʹ�þ��Ե�ַ.
```

���������Ӧʽ���ֵģ�����Ļ�ߴ�С��992px��ʱ�򣬲�����ͻ��ƶ����ײ����������bootstrapդ��ϵͳ <http://v3.bootcss.com/css/>


#### Mini About Me

Mini-About-Me ���ģ�齫�����ͷ�����棬չʾ�����е��罻�˺š����Ҳ����Ӧʽ���֣�����Ļ��Сʱ�򣬻Ὣ���ƶ���ҳ��ײ���ֻ��������΢�е�С�仯�������뿴���롣

#### Featured Tags

���������վ [Medium](http://medium.com) ���Ƽ���ǩ�ǳ����ſᣬ�����ҽ������˽�����
���ģ�������Ƕ����ģ����Գ���������ҳ�棬������ҳ�ͷ����ÿһƪ���±����ͷ�ϡ�

```
# Featured Tags
featured-tags: true  
featured-condition-size: 1     # A tag will be featured if the size of it is more than this condition value
```

Ψһ��Ҫע�����`featured-condition-size`: ���һ����ǩ�� SIZE��Ҳ����ʹ�øñ�ǩ�����������������趨������ֵ�������ǩ�ͻ�����ҳ�ϱ��Ƽ���
 
�ڲ���һ������ģ�� `{% if tag[1].size > {{site.featured-condition-size}} %}` ��������ɸѡ���˵�.


#### Friends

�������Ӳ��֡������ȫ��ҳ����ʾ��

�������� `_config.yml`�ļ������`Friends`�ǿ飬�Լ��Ӱɡ�

```
# Friends
friends: [
    {
        title: "Foo Blog",
        href: "http://foo.github.io/"
    },
    {
        title: "Bar Blog",
        href: "http://bar.github.io"
    }
]
```


#### Keynote Layout

HTML5�õ�Ƭ���Ű棺

![](http://huangxuan.me/img/blog-keynote.jpg)

�ⲿ��������ռ��html��ʽ�Ļõ�Ƭ�ģ�һ���õ����� Reveal.js, Impress.js, Slides, Prezi �ȵ�.����Ϊһ���ִ����Ĳ�����ô�����˷�html�õƵĹ�����~

����Ҫԭ�������һ�� `iframe`������������ⲿ���ӡ������ֱ��д��ͷ�ļ�����ȥ��������������yamlͷ�ļ���д����

```
---
layout:     keynote
iframe:     "http://huangxuan.me/js-module-7day/"
---
```

iframe�ڲ�ͬ���豸�У������Զ��ĵ�����С�������ڱ߾���Ϊ�����ֻ��û��������»������Լ���Ӹ�������ݡ�


#### Comment

���Ͳ���֧�ֶ�˵[Duoshuo](http://duoshuo.com)����ϵͳ��Ҳ֧��[Disqus](http://disqus.com)����ϵͳ��

`Disqus`�ŵ��ǣ����ʱȽ����У�����Ҳ�ܴ�������飬����������ۣ�����ʵʱ֪ͨ��ֱ�ӻظ�֪ͨ���ʼ������ˣ�ȱ���ǣ����۱���Ҫȥע��һ��disqus�˺ţ�����һ��ֻ��Facebook��Twitter��������ǽ�ڼ����ٶ�������һ�㡣��Ҫ֪����ɶ�������Կ���ǰ�İ汾��[����](http://brucezhaor.github.io/about.html) ������Ϳ��Կ�����

`��˵` �ŵ��ǣ�֧�ֹ��ڸ������罻���(΢����΢�ţ����꣬QQ�ռ� ...)һ������ť���ܣ������½�ȽϷ��㣬�������Ҳ�Ǵ����ĵģ������disqusȫӢ�ĵ�Ҫ���ײ���һЩ��ȱ���ǣ����ǽ������һ�㡣
��Ȼ���ǿ����Զ�������css�ģ������뿴��˵�������ĵ� http://dev.duoshuo.com/docs/5003ecd94cab3e7250000008 ��

**����**������Ҫȥע��һ���˺ţ�������disqus���Ƕ�˵�ġ�**��Ҫֱ��ʹ���ҵİ���**

**���**����ֻ��Ҫ�������yamlͷ�ļ�������һ�¾Ϳ����ˡ�

```
duoshuo_username: _����û���_
# ����
disqus_username: _����û���_
```

**���**��˵��֧�ַ���ģ�����㲻��������������ã�`duoshuo_share: false`�������ͬʱʹ����������ϵͳ���������˸о��ֵֹġ�

#### Analytics

��վ����������֧�ְٶ�ͳ�ƺ�Google Analytics����Ҫȥ�ٷ���վע��һ�£�Ȼ�󽫷��ص�code�������棺

```
# Baidu Analytics
ba_track_id: 4cc1f2d8f3067386cc5cdb626a202900

# Google Analytics
ga_track_id: 'UA-49627206-1'            # ����Google�˺�ȥע��һ���ͻ����һ��������id
ga_domain: huangxuan.me			# Ĭ�ϵ��� auto, ���������Զ����˵������������û���Լ�����������Ҫ�ĳ�auto��
```

#### Customization

�����ϲ�����ڣ������ȥ�Զ����ҵ����ģ��� code��[Grunt](gruntjs.com)�Ѿ�Ϊ��׼�����ˡ�����л Clean Blog��

JavaScript ��ѹ��������Less �ı��롢Apache 2.0 ���ͨ�������� watch ����Ķ�����Щ�����������С��򵥵��������������� `grunt` �Ϳ���ִ��Ĭ�����������㹹���ļ��ˡ���������һ�� JavaScript �� Less �Ļ���`grunt watch` ���������ġ�

**����������� `_include/` �� `_layouts/`�ļ����µĴ��루�������������沼�ֵĵط�������Ϳ���ʹ�� Jekyll ʹ�õ�ģ������ [Liquid](https://github.com/Shopify/liquid/wiki)���﷨ֱ���޸�/��Ӵ��룬�����и��д�����Զ����������**

#### Header Image

�����ͼ�ǿ����Լ�ѡ�ģ�������ƪʾ��post���֪����������ˡ���
  [issue #6 ](https://github.com/Huxpro/huxpro.github.io/issues/6) ���ұ��ʵ�����ô�������ñ����ͼ�ÿ��أ�
  
�����ͼ��ѡȡ��ȫ�ǿ����˵������ˣ���Ҳ�ﲻ���㡣ÿһƪ���¿����в�ͬ�ĵ�ͼ�������ʲô�ͷ�ʲô�������Ҫ������С��Ҫ̫�󣬷������������

������Ҫע����Ǳ�ģ��ı�����**��ɫ**�ģ����Ա���ɫҪ����Ϊ**��ɫ**����**��ɫ**����֮��ɫϵ�Ͷ��ˡ���Ȼ�㻹�����Զ����޸�������ɫ����֮����github pages���ǿ�����ȫ�ĸ��Զ����Լ��Ĳ��͡�

#### SEO Title

�ҵĲ��ͱ����� **��Hux Blog��** ��������Ҫ��������ʱ����ʾ **�������Ĳ��� | Hux Blog��** ���������ҪSEO Title�������ˡ�

��ʵ���SEO Title���Ƕ�����<head><title>����</title></head>�������Ķ����Ͷ�˵����ı��⣬����������޸ĵġ�

## ��л

1. ���ģ���Ǵ�����[IronSummitMedia/startbootstrap-clean-blog-jekyll](https://github.com/IronSummitMedia/startbootstrap-clean-blog-jekyll)  fork �ġ� ��л�������
2. ��л[@BrucZhaoR](https://github.com/BruceZhaoR)�����ķ��� 

3. ��л Jekyll��Github Pages �� Bootstrap!




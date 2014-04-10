---
layout: post
title: PocketGuide掌上导游项目开发第3篇-后台项目架构与数据库设计
author: dengbiao
categories:
- develop
- python
keywords: 掌上导游,安卓,景点导游,掌上导游后台项目架构,数据库设计,android,python,flask,flask-sqlalchemy,sqlalchemy,基于android的手机景点导航
description: 掌上导游项目后台项目架构与数据库设计,基于android的手机景点导航

---

前面两篇博文介绍了掌上导游这个项目的开发背景、功能模块和开发环境的搭建。第一次写这样的博文，感觉还是挺难的。不过既然开头了，就写完吧，万事开头难，能坚持到最后的才是赢家不是么。


### 项目结构
后台是用python开发的使用的是flask框架。犹豫项目部署到了新浪的SAE上面，所以代码结构和普通的python项目不太一样，不过改动也不会特别大，这里就不恢复成本地版本了，毕竟SAE还是不错的，也推荐大家将自己项目部署上去，这样你的应用就不仅仅只能在局域网使用了，而是一个实实在在的互联网应用。 ***很高大上有木有*** 

<!--more-->


下面这幅图是整个项目的结构图，截取的vim在NERDTree下的目录结构。


![项目结构图][1]


项目的最外层是index.wsgi、config.yaml和myapp目录,前面文件都是新浪SAE默认需要用到的，其中index.wsgi是项目入口，config.yaml是项目的配置文件。可以配置站点信息。 


index.wsgi文件内容  

{% highlight python %}
import sae
from myapp import app      

application = sae.create_wsgi_app(app)
{% endhighlight %}

config.yaml文件内容参考  

{% highlight python %}
name: attractions          
version: 1                 

handlers:
- url: /static/            
  static_path: myapp/static
{% endhighlight %}


myapp目录下主要有mdoels.py、views.py、api.py以及static目录、templates目录。  如下图：   

![myapp目录结构][2]

    models.py  数据模型定义
    views.py   管理后台界面
    api.py     接口
    static     资源文件目录
    templates  视图文件目录  


除了上面这些文件，剩下还有一些其他的py文件，是geohash模块代码，关于`geohash`前面有介绍过，这里就不在描述了。

###数据库结构

数据库结构对应models.py文件中的model定义。   

![model类][3]


    User类          用户类
    Area类          地区类
    Image类         图片类
    Category类      景点分类
    Attraction类    景点类
    SuggestionAttr  景点推荐类


具体几个类的定义如下：

{% highlight python %}
#用户表
class User(db.Model):
    __tablename__ = 'user'
    id = db.Column(db.Integer, primary_key = True) #id 
    name = db.Column(db.String(100), unique = True) #用户名
    nickname = db.Column(db.String(100)) #昵称
    password = db.Column(db.String(200)) #密码
    email = db.Column(db.String(120)) #邮箱
    status = db.Column(db.Integer) #用户状态 0:未激活 1:激活 2:禁用
    visits = db.relationship('Attraction', secondary=visits, backref=db.backref('visit_tourists', lazy='dynamic')) #用户到过的景点
    favorites = db.relationship('Attraction', secondary=favorites , backref=db.backref('fav_tourists', lazy='dynamic')) #用户收藏的景点
    praises = db.relationship('Attraction', secondary=praises, backref=db.backref('praise_tourists', lazy='dynamic')) #用户点赞的景点
{% endhighlight %}


{% highlight python %}
#地区表
class Area(db.Model):
    __tablename__ = 'area'
    id = db.Column(db.Integer, primary_key = True) #id 
    name = db.Column(db.String(100), unique = True) #地区名
    word = db.Column(db.String(100)) #单词首字母
    attractions = db.relationship('Attraction', backref='area', lazy='dynamic')
{% endhighlight %}


{% highlight python %}
#景点图片表
class Image(db.Model):
    __tablename__ = 'image'
    id = db.Column(db.Integer, primary_key = True) #id
    name = db.Column(db.String(200)) #图片名称
    url = db.Column(db.String(500)) #图片路径
    thumb = db.Column(db.String(500)) #缩略图
    f_type = db.Column(db.Integer) #图片类型  1.image-list 2.x-logo 3.m-logo 4.s-logo   图片列表  大图 中图  小图
    attraction_id = db.Column(db.Integer, db.ForeignKey('attraction.id'))
{% endhighlight %}


{% highlight python %}
#景点类型表
class Category(db.Model):
    __tablename__ = 'category'
    id = db.Column(db.Integer, primary_key = True) #id
    name = db.Column(db.String(200)) #类型名称
    attractions = db.relationship('Attraction',uselist = False, backref='category',lazy='dynamic') #景点类型下的所有景点
{% endhighlight %}


{% highlight python %}
#景点表
class Attraction(db.Model):
    __tablename__ = 'attraction'
    id = db.Column(db.Integer, primary_key = True) #id
    name = db.Column(db.String(200), unique = True) #景点名称
    address = db.Column(db.String(500)) #景点地址
    icon = db.Column(db.String(500)) #icon
    categoryid = db.Column(db.Integer, db.ForeignKey('category.id')) #景点类型id
    areaid = db.Column(db.Integer, db.ForeignKey('area.id')) #景区所属地区
    geohash = db.Column(db.String(100)) #景点的geohash值  对应地理位置 解码可得经纬度
    latitude = db.Column(db.Float) #经度
    longitude = db.Column(db.Float) #纬度
    traffic = db.Column(db.String(500)) #景点公交情况
    summary = db.Column(db.Text) #景点简述
    detail = db.Column(db.Text) #详细描述
    avgmark = db.Column(db.Float) #平均分
    favcount = db.Column(db.Integer) #收藏数
    cmtcount = db.Column(db.Integer) #评论数
    checkincount = db.Column(db.Integer) #到此一游数
    sharecount = db.Column(db.Integer) #分享数目
    cityname = db.Column(db.String(500)) #城市名
    recmdseason = db.Column(db.String(500))  #推荐季节
    recmdfestival = db.Column(db.String(500)) #推荐节日
    spendtime = db.Column(db.String(500)) #所需时间
    price = db.Column(db.Integer) #价格
    parprice = db.Column(db.String(500)) #价格
    parpricenote = db.Column(db.String(500)) #价格备注
    phone = db.Column(db.String(200)) #电话号码
    opentime = db.Column(db.String(500)) #开放时间
    opentimenote = db.Column(db.String(1000)) #开放时间备注
    order_num = db.relationship('SuggestionAttr', uselist=False, backref='attraction')
    visit_users = db.relationship('User', secondary=visits, backref=db.backref('visit_users', lazy='dynamic')) #到过景点的用户
    fav_users = db.relationship('User', secondary=favorites , backref=db.backref('fav_users', lazy='dynamic')) #收藏景点的用户
    praise_users = db.relationship('User', secondary=praises, backref=db.backref('praise_users', lazy='dynamic')) #点赞景点的用户

{% endhighlight %}

{% highlight python %}
#推荐景点表
class SuggestionAttr(db.Model):
    __tablename__ = 'suggestion_attr'
    id = db.Column(db.Integer, primary_key = True) #id
    attraction_id = db.Column(db.Integer, db.ForeignKey('attraction.id')) #外键 景点id
    order_num = db.Column(db.Integer,default=0) #排序号 越大优先级越高
{% endhighlight %}

{% highlight python %}
#观光表
visits = db.Table('visits',
    db.Column('id',db.Integer),
    db.Column('user_id', db.ForeignKey('user.id')), #用户id 外键
    db.Column('attraction_id', db.ForeignKey('attraction.id')) #景点id 外键
)

#收藏表
favorites = db.Table('favorites',
    db.Column('user_id', db.ForeignKey('user.id')), #用户id 外键
    db.Column('attraction_id', db.ForeignKey('attraction.id')) #景点id 外键
)

#点赞表
praises = db.Table('praises',
    db.Column('user_id', db.ForeignKey('user.id')), #用户id 外键
    db.Column('attraction_id', db.ForeignKey('attraction.id')) #景点id 外键
)

{% endhighlight %}


字段后面都有说明，就不介绍了，需要注意的是后面的三个是关联表，和其他的class定义上不同。


下一章继续介绍API接口声明。  


[1]:/assets/uploads/QQ20140410-1.png  "项目结构图" 
[2]:/assets/uploads/QQ20140410-3.png  "myapp目录结构" 
[3]:/assets/uploads/QQ20140410-4.png  "model类" 


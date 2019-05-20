## django课堂知识总结
### 项目创建
进入虚拟环境：安装django   pip install django==2.0.7
虚拟环境内创建项目：django-admin startproject 项目名
应用创建，项目内:Python manage.py startapp 应用名
启动项目：Python manage.py runserver [0.0.0.0:8000]ip端口可选

### setting的配置
数据库配置：
- DATABASES = {mysql引擎、库名、用户名、密码、ip、端口}
- pip install pymysql   5.7以上版本
- 项目__init__.py中 import pymysql    pymysql.install_as_MySQLdb()

模板配置
静态文件配置
缓存配置
多媒体配置
日志配置
rest_framework 配置
	分页配置
	过滤配置
	renderer配置

###　数据库迁移
生成迁移：Python manage.py makemigrations   [应用名]
执行迁移：python manage.py migrate   映射模型对应的数据库表

### 后台的使用admin
创建超级管理员：python manage.py createsuperuser 管理员名
关联数据库与后台管理 应用admin.py：
- 导入模型 from 应用名.models import 模型名
- admin.site.register(模型名)


### pycharm功能
全局搜索：Ctrl + shift + f
全局替换：Ctrl + shift + r

> 定义模型models，指定路由path，访问方法view

### 定义模型类
定义： class 模型名  继承models.Model
             字段 = models.字段类型（限制）
定义元选项相关class Meta:   db_table='表名'

### 路由的使用
- 项目路由 ：path('应用/', include(('应用名.urls','应用名'),namespace='命名空间名'))
- 应用路由：
  - 无参：path('方法名/'，views.方法或者方法，name='别名用于反向解析')
    有参：路由写法path('路由名/<转换器:参数名>如：<int:id>/',方法名，name='别名用于反向解析')  这里字符串用str表示。

  - re_path 

    re_path('路由名/（\d+）/(\d+)/') 
    re_path('路由名/(?P<参数名>\d+)/(?P<参数名>\d+)/(?P<参数名>\d+)/')

    > 参数起名和不起名（path和flask的写法很像，re_path正则匹配和1.11版本很像）

### 视图函数的使用
响应函数：
- HttpResponse('响应字符串')
- HttpResponseRedirect、redirect   跳转

新增数据（通过函数将数据存入数据库）: 
- 对象.save() 或者 （flask-sqlalchemy=>db.session.add(对象)）
- 模型.objects.create(字段1=值1， 字段2=值2)         (flask=> 模型.query)
  删除数据：对象.delete()
  修改数据：查询结果.update()   save()
  查询数据：
- 模型.objects.filter()   .first()  .all()     查询结果集
- 模型.objects.get(条件=值)   条件成立且结果唯一，否则报错
- 模型.objects.exclude(条件=值)   过滤掉满足条件的数据   .all()查询结果集
- 模型.objects.exists(条件=值)     结果返回布尔值
- 模型.objects.all().count()    返回数据总数
- 模型.objects.all()    返回所有队形集合
- 模型.objects.all().values()    提高查询效率 ， 返回所有数据的键值对
- 模型.objects.all().order_by(条件)    排序取值，条件加个减号是降序
- 模型.objects.filter(字段__contains='值').all()  包含
- 模型.objects.filter(字段__startswith='值').all()  前缀
- 模型.objects.filter(字段__endswith='值').all()   后缀
- 模型.objects.filter(字段__in=[]).all()
- 模型.objects.all() .aggregate(Avg('字段'))    聚合函数 平局值
- 模型.objects.all() .aggregate(Sum('字段'))    聚合函数 求和
- 模型.objects.all() .aggregate(Max('字段'))    聚合函数 最大值
- 模型.objects.all() .aggregate(Min('字段'))    聚合函数 最小值
- 模型.objects.all() .aggregate(Count('字段'))    聚合函数 计数
- 模型.objects.filter(字段1__gt=F('字段2')).all()    同表两个字段比较 _ _gt= 大于
- 模型.objects.filter(字段1=值，字段2=值).all()   且
- 模型.objects.filter(字段1=值).filter(字段2=值)   链式  且
- 模型.objects.filter(Q（字段1=值 ）| Q（字段2=值）).all()    或
- 模型.objects.filter(~Q（字段 条件 值）).all()    非   也可以是filter换条件查询和exclude的反向使用

###  数据关系模型

一对一：onetoonefield
    从表（两个表谁都可以）声明关系  字段(外键名)=models.OneToOneFild(关联模型类，on_delete=models.CASCADE，null=True)  注意主键删除时外键是否删除了。
    - 主表添加数据（和普通模型添加数据的方法一样）
        - 分配级联关系：
      获取从表对象
      从表对象.外键字段_id（此字段明面不存在） = 主键值（就是主表对象.主键字段  （外键字段_id是可以取到主键字段的值得））  这种写法拿到的是外键的值
      从表对象.外键字段 = 主表对象    这种写法拿到的是主表的信息，两种写法还是有差别的。
        - 通过从表找主表的内容：先获取从表对象，获取主表信息：以上两种方法
        - 通过主表找从表的内容：反向取值（隐式访问）>>先获取主表对象，获取从表信息：主表对象.从表类名小写/定义了related_name参数，主表对象.related_name的参数

一对多：外键声明
	从表，多的一方声明关系：字段（外键名）= models.ForeiginKey(主表类名，on_delete=models.CASCADE,null=True)
    （主表添加数据，分配级联关系-给主表添加从表的数据-获取从表对象，获取主表对象，从表对象属性赋值给主表对象或者属性，产生关联）
    通过主表查找从表对象：主表对象.从表小写_set.all()/主表对象.从表类名小写/定义了related_name参数，主表对象.related_name的参数
    通过从表查找主表对象:从表对象.外键字段_id（此字段明面不存在） = 主键值/从表对象.外键字段 = 主表对象 

多对多：manytomanyfiled
    从表声明关系（在哪都行）： 字段=models.ManyToManyField(主表类名)  => 生成中间表（可以手动生成，便于维护，可以把中间表当做主表使用）
    通过从表查询主表的数据：
        从表对象.关联字段.all()/filter()
    通过主表查询从表的数据：
        主表对象.从表1小写_set/定义了related_name参数时，主表对象.related_name的参数
    操作中间表：获取相关对象，主表对象和从表对象，增add删remove 

### 模板语法

继承    ｛% extends '路径/文件' %｝

挖坑    ｛% block 坑名 %｝  ｛% endblock %｝

坑继承  ｛｛ block.super ｝｝

视图传参给模板 ｛‘键’：值，。。。｝

变量    ｛｛ 变量名/键名 ｝｝

标签    ｛% 标签名 %｝

循环    ｛% for语句 %｝｛% endfor %｝

​             循环编号｛｛ forloop.counter/counter0/revcounter/revcounter0/first/last  ｝｝

条件    ｛% if 语句/解析变量值%｝  ｛% endif %｝ 

条件相等｛% ifequal  变量1 变量2 %｝  ｛% endifequal %｝  中间是空格，不要打等号，变量值加引号

过滤器 ｛｛变量 | safe｝｝   很多按需求查询

静态文件加载方式：

直接写：< link rel='stylesheet' href='路径'>

第二种：｛% load static %｝ 

​                 <link rel='stylesheet' href='{% static "路径" %}'>

### 反向解析

条件：项目路由命名空间，应用路由命名，实现重定向（比硬编码的方式更灵活）

- 后端（跳转到哪个方法就是哪个方法路由的命名）（有无参数是指想要跳转的函数是否有参数）

  无参情况  **HttpResponseRedirect（reverse(‘命名空间名：命名’)）**注意导包是django.urls下的 

  有参情况（flask中 url_for('蓝图第一个参数.函数名'， 参数名=值。。。。)）

  **HttpResponseRedirect（reverse(‘命名空间名：命名’，kwargs=｛‘参数名’：参数值｝)）**   参数名是定义路由时转换器对应的参数名  下面的args的写法也行

  re_path下无名的**HttpResponseRedirect（reverse(‘命名空间名：命名’，args=（参数值，）)）  *逗号是必须有的*

  re_path下有名的**HttpResponseRedirect（reverse(‘命名空间名：命名’，args=（参数值，）)）

  > 有名字的用kw的  没名字的就用args，**跳转的时候的参数用的是路由里面的参数**

- 前端（html页面中解析）硬编码也是可以的

  <a href='｛% url '命名空间：命名'  %｝'>    无参情况   </a>

  有参情况

  <a href='｛% url '命名空间：命名'  参数值。。。。 %｝'>    无参情况   </a>  参数值直接写就好

### 请求和响应、五种请求方式

获取请求参数：

get请求    request.GET.get()   getlist()

post请求   request.POST.get()   getlist()

获取文件         FILES

请求地址        path

请求cookie值      COOKIES

请求session数据  session

请求user对象      默认是匿名用户

响应前端的数据：

HTTPResponse（数据）

render（页面）

HttpResponseRedirect（地址）

JsonResponse（json数据）



### 登陆注册

- form表单校验（自带user相关表与方法）
演示流程：register.html定义form标签-input传给后端的值通过name属性获取，应用下urls.py定义路由,创建视图函数定义注册的方法-判断请求方法是get还是post-get返回页面，post方法接收参数（常规方法request.POST.get()），
【表单验证】使用流程，应用下创建一个form.py文件
  -引入from django import forms
  -定义校验参数的类继承自forms.Form: class RegisterForm(forms.Form):
  -获取前端传给后端的参数：参数名=forms.字段类型（校验条件。。。）  重构错误信息 error_message作为校验条件
  -views.py中：form = RegisterForm(request.POST)   获取到form表单提交的参数，form就是对三个字段的校验，.is_valid()方法返回的布尔值代表是否通过  form.errors获取校验失败原因
  
  -失败原因返回值返给前端：{{errors.字段}}
【表单校验的基础上使用clean方法】
form.py中 类字段下面直接调用def clean(self): 
    做出判断，判断的是校验的数据如 ：参数1 = self.cleaned_data.get('校验过的参数')
    判断条件不成立的 抛出错误   raise forms.ValidationError('参数'：‘自定义异常声明’)
    参数2=.。。。类似参数1 的操作
返回值目前固定 return self.cleaned_data
【校验通过后】
保存：两个方法（create_user/ create_superuser）
username = form.cleaned_data.get('username')
password = form.cleaned_data.get('pwd1')
模型User.objects.create_user(username=username,password=password)   取的值是经过form验证过的

注册成功后跳转到登录地址 return HttpResponseRedirect(reverse('user:login'))

【登录】表单校验账号密码
创建登录页面login.html 
创建路由
定义方法，判断请求方法是get还是post-get返回页面，登录页面获取信息，post方法接收参数，表单校验post传递的参数  form = LoginForm（request.POST） 同样是用is_valid()方法判断，form.errors返回校验失败的信息
定义登录校验类，获取post传递的参数，定义条件
定义clean方法   def clean(self):   判断用户是否注册、密码是否相同（可以用获取用户名exists（）方法验证是否存在，校验的是用户名）

需要注意的是密码 所有先用自带的方法验证： user = auth.authenticate(username=username, password=password)   校验的是用户名或者密码

同样return的是self.cleaned_data

【登陆】 

views.py中     user = auth.authenticate(username=form.cleaned_data.get('username'), password=form.cleaned_data.get(password) )    formis_

 auth.login(request,user)

auth.logout()

- form表单校验（自定义user相关表与方法）cookie+token方法

  -定义模型 User，执行迁移

  -定义路由register、login、logout

  -定义注册视图方法：get请求返回页面，post请求接收参数

  -定义form校验类方法（这一步应该写在post方法前面）

  -然后视图函数中

  ​      获取表单对象 form = RegisterForm(request.form)对象

  ​      调用is_valid()方法判断 

  ​      username = form.cleaned_data.get('username')

  ​      password = make_password(form.cleaned_data.get('password'))

  ​      密码加密需要make_password(orm.cleaned_data.get('password'))方法加密

  ​       校验成功保存： 模型.objects.create(username=username,

  ​                                                                 password=password)

  ​       保存成功：重定向登录  return HttpResponseRedirect(reverse('user:login'))

  ​       失败返回错误信息errors= form.errors 重新render页面，并返回错误信息

  -定义登陆路由（登陆涉及token的获取与存储）

  -定义登视图方法get请求返回页面，

  -定义form校验类方法

  ​	类方法继承自forms.Form

  ​	定义验证字段

  ​	调用def clean(self):方法

  ​	或者针对校验字段  def clean_username(self):方法    或者  def clean_password(self):方法    先获取前端传递过来的相关参数，然后从数据库取相对应的值，然后做相应逻辑判断，不存在抛异常，返回值就是相对应的字段  return   self.clean_data.get('对应字段') 

  ​	判断密码：自定义的模型类一定注意用这种方法验证def clean_password(self) 

  -post请求接收参数（成功在cookie中存储token值，失败返回错误参数）

  ​	获取form=LoginForm（request.POST）  对象

   	    调用is_valid()方法做判断

  ​	    取值：用户名校验成功

  ​	username=form.cleaned_data['usrname']

  ​	password= form.cleaned_data['password']

  ​	         获取用户对象user     user = MyUser.objects.get(username=username)    取值唯一且存在，表单校验过了

  ​	         校验密码是否一致：

  ​	校验密码需要调用check_password(password,user.password)解码

  ​	if not check_password(password,user.password)：就返回错误信息和登陆页面，   成功使用cookie+token进行登陆身份标识，创建res对象对应下面res

  -定义登陆成功后，重定向页面的路由，比如index首页

  -定义返回页面的视图函数

  -然后去login的视图函数中添加res=HttpResponseRedirect(reverse('user:index'))

  -设置cookie   res.set_cookie('token','token_value',max_age=60*60*24)

  -生成随机token值，

   	定义python工具包utils

  ​	定义funtions.py的公共方法文件

  ​	定义获取唯一且随机token的方法：

  ​	import uuid

  ​	def get_token():

  ​		token=uuid.uuid4().hex  (这个获取随机不重复的uuid值)

  ​		return token

  -回到视图函数调用token    res对象下面传入token    修改为res.set_cookie('token',token,max_age=60*60*24) 

  -后端保存token值，用于下次访问进行判断

  ​	可以用mysql，models中定义一个token模型类

  ​	生名与用户的关联关系，token表示从表 （示登陆与电脑的关系决定表关系），定义on_delete字段，主表数据删除，从表也删除

  ​        执行迁移

  -回到视图函数：过期时间相关操作：注意时区问题 outtime= datetime.urcnow() +timedelta(时或分或妙如 days=1) 

  -mytoken = 模型.objects.filter(user_id=user) 如果my_token存在，修改mytoken.token=token   .mytoken.outtime=outtime.   调用mytoken.save()

  -不存在调用create方法    token模型.objects.create(token=token,user=user，out_time=outtime)都是自定义的字段
  -【注销】调用方法：res.delete_cookie('token)

- session方法
和上面差不多就是校验登录成功以后，res响应对象后面，设置session，
向session中存储user_id：request.session['user_id'] =user.id   （1、这一步向cookie中存储键值对，key是sessionid，value是唯一的 2、向django_session表中存储session值）
设置过期时间：request.session.set_expiry(60*60*24) 
【注销-三种】
可以直接调用（用这个就行了） del request.session['user_id']    #auth.logout(user作为匿名用户,清空session的数据) 
也可以用 reques.session.flush()直接清空
也可以在django_session表中 获取session_key= request.session.session_key   删除session_key  request.session.delete(session_key)

### 中间件与五个处理函数  AOP面向切面编程
      process_request方法     process_view                process_template_response      process_response
请求地址      -|->     路由      -|->   视图   -->models调用数据库   -|->     渲染模板         -|->        响应页面
      校验通过才会匹配路由     执行方法之前调用                  默认不调用                  响应过后调用
        登录状态校验                                                                     日志、同步刷新数据
还有一个 process_exception方法  默认也是不是调用，跑出异常的时候调用

中间件使用流程：
- utils中的创建想对应的middleware文件,定义中间件类函数，继承自MiddlewareMixin
- 定义相应的函数方法  request函数的参数是self,request    response函数的参数是self,request,response
- 添加至settings中，自带中间件后面，utils.middleware.自定义中间件名 
- 中间件的process_request和process_response执行流程u型操作，return为None的情况：先从上到下执行request，然后从下到上执行response 1221，某个方法return操作有返回值，以后就不会再往下走。
- 执行方法之前的process_view  参数self,view_func,view_args,view_kwargs
- 中间件从上到下执行request，然后从上到下再执行view ， 然后从下往上执行response
### 装饰器与中间件登录状态的校验
- 装饰器(cookie+token形式)
functions.py文件中
 def is_login():
   @wraps(func)
   def check(request,*args, **kwargs):
	#token的校验,是否存在，过期时长
	#获取token
	token = request.COOKIES.get('token')
	mytoken = token模型.objects.filter(token=token).first()
	if not mytoken:
	    #找不到，返回登录页面，重新登录
	    return HttpResponseRedirect(reverse('user:login'))
	#获取存活时间,并去除时区
	if mytoken.outtime.repalce(tzinfo=None)< datetime.utcnow():


	    return HttpResponseRedirect(reverse('user:login'))						


​	
​	return func(request,*args, **kwargs)
   return check
- 中间件middleware.py中，定义功能，并添加到settings中
   class 登录中间件类名（继承自MiddlewareMixin）：
	def process_request(self,request):
		path = request.path
		if path in ['/user/register/','/user/login/']:
			return None    #这三行代码通过指定路径，过滤掉登录注册
		user_id = request.session.get('user_id')
		if not user_id:
			return HttpResponseRedirect(reverse('user:login'))

			user = MyUser.objects.get(pk=user_id)
			request.user = user   匿名用户赋值为登录系统的用户
> 重定向次数过多就是中间件有问题，在这里要把登录注册排除在这个中间件之外，登录注册不需要校验，其他方法才需要
### 上传图片（头像上传为例）

前端配置form标签中添加属性enctype=“multipart/form-data”

后端配置

​	创建upload文件夹

​	定义模型字段models.ImageField（uploiad_to='upload',null=True）

​	定义路由注册

​	添加注册页面form表单定义头像input标签type=file 

​	定义视图函数，get返回页面，post接收参数，（图片从哪取，获取表单对象的参数request.POST,request.FILES）

settings媒体文件配置：	

​	MEDIA_URL='/media/'

​	MEDIA_ROOT=os.path.join(BASE_DIR,‘media’)
项目路由 ： urlpatterns += static(MEDIA_URL, document_root=MEDIA_ROOT)

### 分页对象

定义路由

定义方法：get请求，传递对象到前端页面

后端操作

前端使用

### RBAC用户角色权限表的设计
### csrf的使用-403

前端form标签下面加上｛% csrf_token %｝

### 日志的使用
### django—restframework的一般流程与增删该查对应的方法、接口
### 分页
### 过滤
###






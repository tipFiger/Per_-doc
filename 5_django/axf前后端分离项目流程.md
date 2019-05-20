1、项目分析
2、创建虚拟环境
3、进入虚拟环境，安装项目依赖
4、创建项目 django-admin startproject axf
5、关联项目与环境
6、创建应用
7、构建项目结构：项目目录、应用目录、日志目录logs、配置目录utils、启动文件manage.py、接口文档目录docs、依赖文件requirement.txt、备份数据库目录sqldir
8、前后端分离，页面在前端渲染所以，不需要静态文件、模板文件等，如果综合前后端分离与不分离开发，就需要静态文件、模板文件
9、设置项目配置  settings、init文件
- 访问主机配置
- 安装应用配置
- 数据库配置
- 缓存配置
- 日志配置

- rest-framework配置（分页配置、过滤、renderer配置）
- 跨域配置
  - 添加应用 'corsheaders',
  - 添加中间件 'corsheaders.middleware.CorsMiddleware', 在中间件'django.middleware.common.CommonMiddleware',之前
  - 允许跨域的请求头，可以使用默认值，默认的请求头为:
    CORS_ALLOW_HEADERS = (
    'XMLHttpRequest',
    'X_FILENAME',
    'accept-encoding',
    'authorization',
    'content-type',
    'dnt',
    'origin',
    'user-agent',
    'x-csrftoken',
    'x-requested-with',
    'Pragma',
    )

  - 跨域请求时，是否运行携带cookie，默认为False
    CORS_ALLOW_CREDENTIALS = True
  - 允许所有主机执行跨站点请求，默认为False
  - 如果没设置该参数，则必须设置白名单，运行部分白名单的主机才能执行跨站点请求
    CORS_ORIGIN_ALLOW_ALL = True

10、项目路由-分发
11、子路由资源注册
12、资源序列化
13、创建视图函数返回给前端对应的数据

1、DoesNotExist at /sel_stu/
Student matching query does not exist.

错误原因：get查询，主键不存在时候就会报错，条件查询的结果唯一
解决方法：注意get方法的局限性

2、MultipleObjectsReturned at /ssbc/
get() returned more than one Course -- it returned 7!

错误原因：get 获取的数据不是唯一的
解决方法：注意查询字段的唯一性

3、goods.Goods.goods_front_image: (fields.E210) Cannot use ImageField because Pillow is not installed.
        HINT: Get Pillow at https://pypi.org/project/Pillow/ or run command "pip install Pillow".
错误原因：模型字段有图片类型
解决方法： 安装pillow库pip install Pillow

4、TypeError: argument of type 'function' is not iterable
错误原因：子路由书写过程中，name=index  忘了加‘’
解决原因：回想改动与出错区间，加上引号以后成功解决

5、TypeError at /app/add_art/
render() missing 1 required positional argument: 'template_name'
错误原因：render第一个参数request没写
解决方法：添加第一个参数request

6、ValueError: The view app.views.add_art didn't return an HttpResponse object. It returned None instead.
错误原因：视图函数post方法没有写完整（猜测post方法要有存储点），没有接收post请求
解决方法：写完post方法的代码

7、TypeError: module.__init__() takes at most 2 arguments (3 given)
错误原因：过滤文件filters.py 的类继承出错
解决方法：继承自正确的父类

8、TypeError: object of type 'type' has no len()
错误原因：settings中的rest_framework，renderer设置中少了一个逗号，猜测是元素的属性问题检测不到正确的类型
解决方法：检查项目是否能正常运行，检查代码的正确性，路由的正确路径是否正常，最后项目没有进来可能就是settings中配置的问题，找到那个renderer设置，加上元组的逗号。

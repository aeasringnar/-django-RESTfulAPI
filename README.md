# drfAPI

基于 Django 3.2 的 RESTfulAPI 风格的项目模板，用于快速构建高性能的服务端。

## 技术栈

- **框架选择**：基于 Django 3.2 + django-rest-framework 3.12
- **数据模型**：基于 MySQL 存储，使用 mysqlclient 作为驱动，测试也可使用内置 sqlite3
- **授权验证**：基于 JWT 鉴权，来自 PyJWT，并做了单点登录的优化。
- **内置功能**：自定义命令、代码生成、文件处理、用户系统、异常处理、异步处理、全文检索、动态权限、接口返回格式化、日志格式化、分页、模糊查询、过滤、排序、缓存、分布式锁等

## 快速入门

如需进一步了解，参见 [Django 文档](https://docs.djangoproject.com/zh-hans/3.2/)。

### 本地开发

```bash
pip install -r requirements.txt
python manage.py makemigrations
python manage.py migrate
python manage.py loaddata apps/user/user.json
python manage.py ruserver 0.0.0.0:8000
open http://localhost:8000/swagger/
```

### 如何创建新的 App
1、使用自定义命令创建 App
```bash
python manage.py startmyapp your_app_name
```
2、将 App 加入到 Settings 中
```bash
INSTALLED_APPS = [
   ...
   'apps.your_app_name',
   ...
]
```
3、编写您的 models、serializers、views、urls

一般来说，您的正常开发流程如下：
1. 根据业务编写您的模型文件 models.py
2. 根据模型和您的需求编写您的序列化器 serializers.py
3. 根据模型、序列化器、权限来编写您的视图文件 views.py
4. 根据视图文件编写您的路由文件 urls.py
5. 最后将您的路由添加到入口路由文件中，默认它位于 ./drfAPI/urls.py

4、本项目提供了对于 CRUD 代码的自动生成命令

首先需要创建好需要的模型文件

然后将需要生成的信息更新到生成代码的json文件中，它需要遵循以下的逻辑，它的位置在./configs/generateCode.json

注意：json 文件中的 key 是不能更改的

```bash
[
  {
    "app_name": "public", # 标明你要生成代码的AppName
    "models": [ # 标明你要生成代码的模型
      {
        "model_name": "ConfDict", # 具体的模型类名
        "verbose": "系统字典", # 模型的中文标识
        "searchs": [ # 标明这个模型生成的代码需要的搜索字段，如果不需要可以删除
          "dict_key",
          "dict_value"
        ],
        "filters": [ # 标明这个模型生成的代码需要的过滤字段，如果不需要可以删除
          "dict_type"
        ]
      }
    ]
  }
]
```

执行生成代码的命令

```bash
python manage.py generatecode
```

然后查看你的 App，再进行自定义的微调。

### 线上部署

```bash
bash server.sh start  # 获取帮助：bash sever.sh help 默认启动端口为 8001
```
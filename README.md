# REST API设计原则

- 协议 & 数据返回格式---- API 与用户的通信协议: Https & JSON

- Endpoint ---- API的具体网址
  - 每种网址代表一种资源，不能有动词，只能有名词
  - 一般情况下，名词与数据库表名对应
  - 数据库的表是同种记录的集合，故API中的名词应该使用复数
  - **不要大写**
  - Root Endpoint ---- https:{hostname}:{port}/api/{resources}/{id?}

- Http动词 & 返回结果
  - 常用动词
    - GET：从服务器取出资源（一项或多项）
      - /collection：返回资源对象的列表（数组）
      - /collection/resource：返回单个资源对象
    - POST：在服务器新建一个资源
      - /collection：返回新生成的资源对象
    - PUT：在服务器更新资源（客户端提供改变后的完整资源）
      - /collection/resource：返回完整的资源对象
    - PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）
      - /collection/resource：返回完整的资源对象
    - DELETE（DELETE）：从服务器删除资源。
      - /collection/resource：返回一个空文档
    - 不常用动词
      - HEAD：获取资源的元数据
      - OPTIONS: 获取信息，关于资源的那些属性是客户端可以改变的

- Filtering ---- API提供参数，过滤返回结果，常见参数如下
  - ?limit=10：指定返回记录的数量
  - ?offset=10：指定返回记录的开始位置。
  - ?page=2&per_page=100：指定第几页，以及每页的记录数。
  - ?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。
  - ?animal_type_id=1：指定筛选条件

- 状态码 ---- 服务器向用户返回的状态码和提示信息,常见的如下
  - 200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
  - 201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
  - 202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
  - 204 NO CONTENT - [DELETE]：用户删除数据成功。
  - 400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
  - 401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
  - 403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
  - 404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
  - 406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
  - 410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
  - 422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
  - 500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。

- 认证 Oauth

- 域名 应该将API部署在专属域名下

- Version - API的版本号放在URL中
    ```C#
    [ApiVersion("1.0")]
    [Route("api/v{version:apiVersion}/[controller]")]

    ```

- Hypermedia API - 即返回结果中提供链接，连向具体的API方法。使得用户不查询文档，也知道下一步应该如何。

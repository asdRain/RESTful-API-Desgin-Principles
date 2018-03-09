1.	协议
总是使用https协议

2.	域名
应该尽量部署在专用的域名之下
https://myleo.rp.edu.sg/cm/api 

3.	版本控制
版本号置于url中
https://myleo.rp.edu.sg/api/cm/v1 
随着系统发展，总有一些API失效或者迁移，失效返回404 not found或410 gone;对于迁移的API，返回301重定向

4.	资源路径
a.	RESTful构架中每个路径代表资源，所以路径中只能有名词
https://myleo.rp.edu.sg/api/cm/v1/cars
b.	尽量为资源对象定义ID，创建便于阅读的URI
无论它代表着单一的“数据项”
http://example.com/products/4554
还是数据项集合等等
http://example.com/products?color=green
http://example.com/orders/2017/5
c.	使用子资源表达关系
如果一个资源与另外一个资源有关系，使用子资源：
GET /cars/711/drivers 返回 car 711的所有司机
GET /cars/711/drivers/4 返回car 711的4号司机

5.	Verbs
对于REST 操作，常用有以下五种：
-	GET	取出资源
-	POST	创建资源
-	PUT	更新资源，请求者提供改变后的完整资源 
-	PATCH	更新资源，请求者提供改变的属性
-	DELETE	删除资源
以及不常用的
-	HEAD	获取资源元数据
-	OPTIONS	获取信息，关于请求者可以对资源做什么操作的信息

6.	交互原则
a.	查询、过滤条件使用query string
i.	使用limit和offset实现分页，缺省返回记录数量limit=20, 缺省返回记录的开始位置offset=0
GET /cars?limit=10&limit=5	
ii.	指定筛选条件
GET /cars?car_mode=1
iii.	排序
GET /cars?sort=-id,+color
iv.	经常使用的复杂的查询标签化
GET /cars?car_mode=1&sort=-id,+color
	GET /cars#recent_mode1_cars
b.	用来描述数据或者请求的元数据放在header中
i.	描述请求：使用X-Result-Fields header过滤字段
X-Result-Fields: id, color
ii.	描述数据：使用X-Total-Count header 说明显示数据总数
X-Total-Count: 300
c.	Content body 仅仅用来传输数据，不要多余包装
d.	数据要做到拿来就可用的原则，不需要拆箱过程。
e.	数据里的子对象不应该被提取，除非用户指定需要提取的子对象						
此处应该定义为
{
	“trade_id”: “1234051”,
	“order”: “http://~/$v/orders/987654312”
}
如果需要提取子对象，需要在请求头生命X-Expansion-Fields
7.	使用HATEOAS（Hypermedia as the Engine of Application State）
使用Web Linking的方式来描述程序的状态信息，使状态流程控制都在应用程序的后台完成，如此需要处理的是理解状态表述，数据收集和结果显示。以下简单描述HATEOAS的分页使用场景： 
Request	GET http://~/$v/cars?limit=20&offset=20
Response	
link: <http://~/$v/cars?limit20&offset=0>;rel=”last”, <http://~/$v/cars?limit=2-&offset=40>;rel=”next”
8.	状态码&错误处理
注意所有的响应状态应该包含在响应头之中，以下列举常见的状态码
Code	HTTP Operation	Body Contents	Description
102 Processing	GET,POST,PUT,DELETE,PATCH	处理状态的信息	当前请求正在处理
200 OK	GET,PUT	资源	操作成功
201 Created	POST,PUT	资源，元数据	对象创建成功
202 Accepted	POST,PUT,DELETE,PATCH	处理信息	请求已经被接受
204 No Content	DELETE,PUT,PATCH	n/a	操作成功，但没有返回数据
301 Moved permanently	GET	Link	资源已经被移除
303 See other	GET	Link	重定向
304 Not Modified	GET	n/a	资源没有被修改
400 Bad Request	GET,POST,PUT,DELETE,PATCH	错误提示	参数列表错误(缺少,格式不匹配)
401 Unauthorized	GET,POST,PUT,DELETE,PATCH	错误提示	未授权
403 Forbidden	GET,POST,PUT,DELETE,PATCH	错误提示	访问受限,授权过期
404 Not Found	GET,POST,PUT,DELETE,PATCH	错误提示	资源、服务未找到
405 Method Not Allowed	GET,POST,PUT,DELETE,PATCH	错误提示	不允许的http方法
406 Not Acceptable	GET,POST,PUT,DELETE,PATCH	错误提示	媒体内容不符合要求
408 Request Timeout	GET,POST,PUT,DELETE,PATCH	错误提示	请求超时
409 Conflict	GET,POST,PUT	错误提示	资源冲突，重复的资源
415 Unsupported Media Type	GET,POST,PUT,DELETE,PATCH	错误提示	不支持的数据（媒体）类型
422 Unprocessable Entity	GET,POST,PUT,PATCH	错误提示	请求格式正确，但有语法错误，无法响应
423 Locked	GET,POST,PUT,DELETE,PATCH	错误提示	
429 Too Many Requests	GET,POST,PUT,DELETE,PATCH	错误提示	请求过多被限制
500 Internal Server Error	GET,POST,PUT,DELETE,PATCH	错误提示	系统内部错误
501 Not Implemented	GET,POST,PUT,DELETE,PATCH	错误提示	接口未实现
http容器的状态码不应该使用
Code	HTTP Operation	Body Contents	Description
303	GET	Link	静态资源被移除，应用限制使用
503	GET,POST,PUT,DELETE,PATCH	Text body	服务器宕机

错误提示的信息格式:
i.	错误标题 message
ii.	错误代码 error code
iii.	错误信息 error message
iv.	资源 resource, 可选
v.	属性field, 可选
vi.	文档地址 document, 可选
{
		“message”: “Message title”,
		“errors”: [
			{
				“code”: “rate_limit_exceeded”,
				“message”: “Too Many Requests. API rate limit exceeded”,
				“document”: “https://dev.githut.com/v3/gists/”
}
		]
}
9.	安全
a.	调用限制	为保证服务的可用性应对服务进行调用过载保护
Response headers:
X-RateLimit-Limit: 3000			调用量的最大限制
X-RateLimit-Reset: 1403162176510 调用限制重置时间
X-RateLimit-Remaining: 299		 剩余的调用量
b.	安全验证
使用OAuth2.0进行调用授权，使用http请求头Authorization设置授权码；必须使用User-Agent是设置客户端信息，无UserAgent请求头的请求会被拒绝访问。
10. Basic Principles
a.  结果集中的子资源请尽量使用其它API的link替代
b.	所有参数和返回值的属性名首字母小写
c.	结果集请返回资源模型数组，方便统一分页格式
d.	API表格中的API Name列对应Controller name，并不是API描述名
e.	并不需要在参数中定义当前用户，Common会提供通用接口取得当前用户信息
f.	时间格式
	使用统一的时间格式作为API的参数
	2009-06-15T13:45:30 
	C# code
	DateTime.ToString("s")
	Ref<https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-date-and-time-format-strings>
	以下简略好处
	- 忽略地域差异统一时间格式
 	- 方便前端语言解析（如javascript）使用。
	 API 资源实体中的时间属性可以使用 基本字符串时间格式及ticks格式，如
	start_time 2009-06-15T13:45:30
	start_time_ticks  636328335055574976
g.  使用标准方法GET/POST/PUT/DELETE来操作资源实体。资源名称不能以动词开头，以名词复数结尾。如：
	资源实体：Topic
	动作类型		Method		Url						request body								return content
	创建			POST		http://host/api/topics		{ name: “topic1”, content: “content” }		{ id: 1, name:”topic1”, content: “content” }
	更新			PUT			http://host/api/topics/1	{ id:1, name:”topic2”, content: “content” }	{ id: 1, name:”topic2”, content: “content” }
	删除			DELETE		http://host/api/topics/1	
	查询			GET			http://host/api/topics/1											{ id:1, name:”topic2”, content: “content” }
	查询			GET			http://host/api/topics/											[{id:1 … }, {id:2 … }]
	对于本例，Controller的名称为TopicsController， 其中包含 Post\Put\Delete\Get四种Action方法。
	-	对于查询，可以定义多种参数不同的Get方法，并且分为返回单一资源及批量资源的两种API。注意本例中未列举请求参数。	      
	-	对于删除，删除成功不需要返回资源对象。但是删除失败的话应该返回统一格式的错误信息。错误信息的类型common这里会提供.
	-	对于创建/更新，操作成功后返回资源对象，不需要操作成功的提示信息。操作失败则返回统一错误信息。
	返回资源对象可以不包含全部属性，但必须要保持属性名一致及整体格式一致。
	比如
	修改资源 Topic, PUT http://host/api/topics/	，请求内容 {  id: 1, name: “topic2”, content: ”haha” }
	不能返回结果集 {result: "b6ec39a8-2d7d-4983-af63-c920399b626d" }
	或["b6ec39a8-2d7d-4983-af63-c920399b626d"]
	或"b6ec39a8-2d7d-4983-af63-c920399b626d"
    可以返回{ id: "b6ec39a8-2d7d-4983-af63-c920399b626d" }





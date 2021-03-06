# Interface Document

## Updates

- Marks

	- **M** Modify

	- **N** New

	- **D** Delete

	- **O** Other

- 170814 [Link](https://719daze.me/JP_Thinktank/170814.html)

- 170831 [Link](https://719daze.me/JP_Thinktank/170831.html)

	- **M** Mail Check

	- **M** File Upload

	- **M** Search Document

	- **N** Major Search

	- **N** Course Search

	- **N** File Modify

	- **N** User info modify

- 170901 [Link](https://719daze.me/JP_Thinktank/170901.html)

	- **N** Get College List

- 170903 [Link](https://719daze.me/JP_Thinktank/170903.html)
 
 	- **N** File Download

 	- **N** User Logout

- 170910 [Link](https://719daze.me/JP_Thinktank/170910.html)

	- **M** Search Document

	- **M** Register

	- **M** File Upload

	- **D** Document Detail

	- **D** File Modify

	- **N** Get Download List

	- **N** Get Upload List

	- **N** Delete Document

	- **O** Add notes

- 170911 [Link](https://719daze.me/JP_Thinktank/170911.html)

	- **M** Get Download List

	- **M** Get Upload List

	- **O** Recover 170903

- 170914 [Link](https://719daze.me/JP_Thinktank/170914.html)

	- **N** Get User Info

	- **N** Get User List

	- **N** Get File List

	- **N** Get User Log

	- **N** Get File Log

## Global
```
{
	"status":200,		//3 digit integer.
	"message":"OK",		//Short sentence to explain.
	"data":{}			//Requiring data.
}
```
** Time format : `2017-12-31 23:59:59` ** 

host : _`hostname`_ 

appname : `jpidea`

url = `"https://" + host + "/" + appname + interfaceRoute`

authority :

```
Visitor	//Unregisted
Member	//Registed
Manager	//Administrator (not used)
```

## Requring information
```
//Successful return
	"status" : 200
	"message" : "OK"
//Failure return
	"status" : 300
	"message" : "FAIL"
//No permission
	"status" : 400
	"message" : "Permission denied"
```
## Requests
### No permission requests
#### Login

> 前端在用户登录页面使用

> 提交用户名与密码 返回是否成功登录

url : `/login`

method : `POST`

request :
```
{"username":"id","password":"password"}
``` 
responce :
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data":{}
}
{
	"status" : 300 ,
	"message" : "Username or password wrong" ,
	"data":{}
}
```
#### Register

> 前端在用户注册页面使用

> 提交用户名 昵称 密码 邮箱 手机 学院专业

> 后台写入数据

`Update 170910`

url : `/register`

method : `POST`

request
```
{
	"username" : "",
	"nickname" : "",
	"password" : "",
	//"avator" : "",				//Delete 170910
	"mail" : "",
	"phone" : "",
	//"qq" : "",					//Delete 170910
	"xid" : 1,						//前端利用Get College List接口和Major Search接口
	"mid" : 1,
}
```
responce
```
{
	"status" : 200,
	"message" : "OK",
	"data" : {}
}
{
	"status" : 300,
	"message" : "Username has been used",
	"data" : {}
}
{
	"status" : 300,
	"message" : "Mail has been used",
	"data" : {}
}
```
#### Mail Check

> 后台发送验证邮件使用

> 发送的链接中含有用户信息和key 

> 验证链接是否合法或超时

url : `/mailcheck`

method : `POST`

request
```
{
	"user" : "",					//Update 170831
	"key" : "",
}
```
responce
```
{
	"status" : 200,
	"message" : "OK",
	"data" : {}
}
{
	"status" : 300,
	"message" : "Invalid mailcheck link",
	"data" : {}
}
{
	"status" : 300,
	"message" : "Mailcheck link time out",
	"data" : {}
}
```

### Permission requests
#### File Upload

> 前端上传文件界面中使用

> 前端提供文件名 文件 老师 课程 是否原创 简介

> 后端写入数据库并保存文件

`Update 170910`

url : `/upload`

method : `POST`

request
```
{
	"name" : "" ,
	//"module" : 1 ,				//Delete 170910
	"file" : 0 , 					//Update 170831
	"teacher" : "" ,  				//Update 170831
	"course" : 100101 , 			//Update 170831
	//"docformat" : -1 ,			//Delete 170910
	//"fileformat" : "" ,			//Delete 170910
	"upusername" : 0 ,				//Update 170910
	"origin" : 0 ,
	"desc" : ""
}
```
responce
```
{
	"status" : 200,
	"message" : "OK"
}
{
	"status" : 300,					//Update 170910 添加文件过大(>100MB)提醒
	"message" : "File too large"
}
```

#### Search Document

> 前端搜索文档时用 包括上端搜索课程框与下面的专业下拉框

> 前端利用瀑布流发送获取方式(1课程搜索框 2专业下拉框)或者对应的课程 分页

> method选择了其中一个之后 与之无关的信息置为0

> 后台每次返回5条内容 包括文件相关信息与下载地址

`Update 170910`

url : `/document/search`

method : `GET`

request 
```
{									//Update 170831
	"method" : 0 , 					//1:course 2:major
	"course" : 1 , 					//cid
	"college" : 1 , 				//xid
	"major" : 1 , 					//mid
	"page" : 1,						//Update 170910
}
```
responce
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" : 
	[
		{
			"fid" : 0 ,
			"upuid" : 0 ,
			"fileinfo" : 
			{
				"name" : "" ,
				"module" : 0 ,
				"course" : 1 ,
				"docformat" : 0 ,
				//"fileformat" : "" ,
				"origin" : 0 ,
				"uptime" : 2017-12-31 23:59:59 ,
				"desc" : "" ,
			}
			"upperinfo" :
			{
				"nickname" : "" ,
				//"xid" : 1 ,		//Delete 170831
				"xname" : "" ,
				//"mid" : 1 ,		//Delete 170831
				"mname" : "" ,
			}
		}
	]
}
{									//Update 170831
	"status" : 300 ,
	"message" : "Invalid course" ,
	"data" : {}
}
{
	"status" : 300 ,
	"message" : "Invalid major" ,
	"data" : {}
}
```

#### ~~Document Detail~~			

`Delete 170910`

url : `/document/detail/${fid}`

method : `GET`

request `{}`

responce
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" : 
	[
		{
			"fid" : 1 ,
			"fileinfo" : 
			{
				"name" : "" ,
				"module" : 0 ,
				"course" : 1 ,
				"docformat" : 0 ,
				//"fileformat" : "" ,
				"origin" : 0 ,
				"uptime" : 2017-12-31 23:59:59 ,
				"desc" : "" ,
				"dncnt" : "" ,
			}
			"uid" : 1 ,
			"upperinfo" :
			{
				"nickname" : "" ,
				"xid" : 1 ,
				"xname" : "" ,
				"mid" : 1 ,
				"mname" : "" ,
			}
		}
	]
}
{
	"status" : 300 ,
	"message" : "File not exist" ,
	"data" : {}
}
{
	"status" : 300 ,
	"message" : "File has been locked" ,
	"data" : {}
}
```

#### Major Search 

> 前端的专业下拉框使用

> 在选定前面的学院学院框后发送学院xid

> 返回的是专业信息 包括mid和专业名

`Update 170831`

url : `/document/coltomajor`

method : `GET`

request 
```
{
	"xid" : 0 ,
}
```

responce
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" :
	[
		{
			"mid" : 0 ,
			"mname" : "" ,
		}
	]
}
{
	"status" : 300 ,
	"message" : "Wrong college" ,
	"data" : {}
}
```

#### Course Search

> 前端在上面的课程搜索框里使用

> 前端发送输入框的内容 接收匹配的课程与cid

> 用于以后的搜索

`Update 170831`

url : `/document/coursesearch`

method : `GET`

request 
```
{
	"keyword" : "" ,
}
```

responce
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" :
	[
		{
			"cid" : 0 ,
			"cname" : "" ,
		}
	]
}
{
	"status" : 300 ,
	"message" : "No result" ,
	"data" : {}
}
```

#### ~~File Modify~~

`Update 170831`

`Delete 170910`

url : `/document/modify/${fid}`

method : `POST`

request
```
{
	"name" : "" ,
	"module" : 1 ,
	"teacher" : "" ,
	"course" : 100101 ,
	"docformat" : -1 ,
	"fileformat" : "" ,
	"upuid" : 0 ,
	"origin" : 0 ,
	"desc" : ""
}
```
responce
```
{
	"status" : 200,
	"message" : "OK"
}
```

#### User Info Modify

> 自定义用户信息页面使用

> 前端提交相关信息表单 后台进行修改

> 出现密码错误等不合法问题时返回错误 否则正常修改

`Update 170831`

`Update 170910`

url : `/usermodify/${username}` `Update 170910`

method : `POST`

request
```
{									//0 or "" = No change
	"nickname" : "",
	"oldpwd" : "",					//All operation required
	"newpwd" : "",					//Pwd change required
	"avator" : "",
	"mail" : "",
	"phone" : "",
	"qq" : "",
	"xid" : 0,
	"mid" : 0,
}
```
responce
```
{
	"status" : 200,
	"message" : "OK",
	"data" : {}
}
{
	"status" : 300,
	"message" : "Wrong password",
	"data" : {}
}
```
#### Get College List

> 搜索专业的下拉框中使用 作用于第一个下拉框

> 加载页面的时候加载这部分信息

> 显示在第一个下拉框中 选中之后执行Course Search

`Update 170901`

url : `/document/getcollege`

method : `GET`

request `{}`

responce
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" :
	[
		{
			"xid" : 0 ,
			"xname" : "" ,
		}
	]
}
```

#### File Download

> 发起下载请求时由后台返回url地址

`Update 170903`

url : `/filedl/${fid}`

method : `GET`

request `{}`

responce
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" :
	{
		"path" : "",
	}
}
```

#### User Logout

> 用户退出登录操作

`Update 170903`

url : `/logout`

method : `POST`

request `{}`

responce
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" : {}
}

```

#### Get Download List

> 用户个人页中使用 显示用户所有曾经下载过的文件

> 进入用户个人页面对应页面时加载

> 返回所有下载过的文件的相关信息

`Update 170910`

url : `/getdld`

method : `GET`

request
```
{
	"username" : "" ,
	"page" : "" ,					//Update 170911
}
```

responce
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" : 
	[
		{
			"fid" : 0 ,
			"fileinfo" : 
			{
				"name" : "" ,
				"module" : 0 ,
				"course" : 1 ,
				"docformat" : 0 ,
				"origin" : 0 ,
				"uptime" : 2017-12-31 23:59:59 ,
				"desc" : "" ,
			}
		}
	]
}
```

#### Get Upload List

> 用户个人页中使用 显示用户所有上传过的文件

> 进入用户个人页面对应页面时加载

> 返回所有上传过的文件的相关信息 提供删除操作

`Update 170910`

url : `/getupld`

method : `GET`

request
```
{
	"username" : "" ,
	"page" : "" ,					//Update 170911
}
```

responce
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" : 
	[
		{
			"fid" : 0 ,
			"fileinfo" : 
			{
				"name" : "" ,
				"module" : 0 ,
				"course" : 1 ,
				"docformat" : 0 ,
				"origin" : 0 ,
				"uptime" : 2017-12-31 23:59:59 ,
				"desc" : "" ,
			}
		}
	]
}
```

#### Delete Document

> 用户删除自己的上传文件时使用

> 提供fid 返回删除成功信息

> 后台将其标注为已删除

`Update 170910`

url : `\del\${fid}`

method : `POST`

request : ` `

responce : 
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" : {}
}
{
	"status" : 300 ,
	"message" : "File not exist or deleted" ,
	"data" : {}
}
```

#### Get User Info

> 索求用户名 发送用户详细信息

`Update 170914`

url : `userinfo\$username`

method : `GET`

request :
```
{
	"page" : 1 ,
}
```

responce : 
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" : 
	[
		{
			"uid" : 1 ,
			"username" : "" ,
			"nickname" : "" ,
			"avator" : "" ,
			"sign" : "" ,
			"coin" : 0 ,
			"mail" : "" ,
			"phone" : "" ,
			"qq" : "" ,
			"xname" : "" ,
			"mname" : "" ,
			"auth" : "" ,
			"status" : "" ,
			"uploadcount" : 0 ,
			"downloadcount" : 0 ,
			"collectcount" : 0 ,
		}
	]
}
```

#### Get User List

> 后台视图用于用户详细信息 列表形式显示

> 对于前端，提供一定的查询条件 不需要的条件置空

> 全空为列举所有用户信息

> 后端提供符合查询条件的用户的全部所需信息

`Update 170915`

url : `admin\user\list`

method : `GET`

request :
```
{
	"username" : "" ,				//用户名
	"nickname" : "" ,				//昵称 后台需支持模糊查询
	"xid" : 0 ,						//所属学院
	"mid" : 0 ,						//所属专业
	"status" : -1 ,					//状态
	"low_upcnt" : 0 ,				//上传数 不少于的数值 eg.查上传数为4~7个的用户 "low_upcnt" = 4 包含边界4
	"top_upcnt" : 0 ,				//上传数 不多于的数值 eg.查上传数为4~7个的用户 "top_upcnt" = 7 包含边界7
	"low_downcnt" : 0 ,				//下载数
	"top_downcnt" : 0 ,
	"low_collectcnt" : 0 ,			//收藏数
	"top_collectcnt" : 0 ,
	"low_registtime" : 0 ,			//注册时间 不早于的数值
	"top_registtime" : 0 ,			//注册时间 不晚于的数值
	"low_lastlogintime" : 0 ,		//最后登录时间 不早于的数值
	"top_lastlogintime" : 0 ,		//最后登录时间 不晚于的数值

	"page" : 1 ,					//页码
	"res_per_page" : 20 ,			//每页结果数 提供10 20 50 100四个选项
	"sort_method" : "" ,			//排序依据 一个单词(见下)
	"sort_ref" : 1					//输出顺序 字符为按照字典序 1升序(1 2 3) 2降序(3 2 1)

}
```

> 排序依据

> uid&emsp;&emsp;username&emsp;&emsp;nickname&emsp;&emsp;coin&emsp;&emsp;mid(实际为xid与mid双关键字排序)

> unlocktime&emsp;&emsp;mailtime&emsp;&emsp;upcnt&emsp;&emsp;downcnt&emsp;&emsp;collecttime

> registtime&emsp;&emsp;lastlogintime

responce : 
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" : 
	{
		"rescount" : 0 ,			//总结果数
		[
			{
				"uid" : 0 ,
				"username" : "" ,
				"nickname" : "" ,
				"coin" : 0 ,
				"mail" : "" ,
				"phone" : "" ,
				"qq" : "" ,
				"xname" : "" ,		//学院名
				"mname" : "" ,		//专业名
				"status" : 0 ,		//状态
				"unlktime" : "" ,	//status封禁时的解封时间 status正常为空
				"mailtime" : "" ,	//未激活时显示邮件失效时间 status正常为空
				"upcnt" : "" ,
				"downcnt" : "" ,
				"collectcnt" : "" ,
				"registtime" : "" ,
				"lastlogintime" : "" ,
			}
		]
	}
}
{
	"status" : 300 ,				//存在结果且页码超出结果数上界时返回
	"message" : "Page Upperbound Overflow" ,
}
```

#### Get File List

> 后台视图用于文档详细信息 列表形式显示

> 对于前端，提供一定的查询条件 不需要的条件置空

> 全空为列举所有文档信息

> 后端提供符合查询条件的文档的全部所需信息

`Update 170915`

url : `admin\file\list`

method : `GET`

request :
```
{
	"fid" : 0 ,
	"fname" : "" ,					//文件名 需支持模糊查询
	"upuid" : "" ,					//上传者
	"upnickname" : "" ,				//上传者昵称 需支持模糊查询
	"xid" : 0 ,						//学院xid 前端利用Get College List接口
	"mid" : 0 ,						//专业mid 前端利用Major Search接口
	"cid" : 0 ,						//课程cid 前端利用Course Search接口
	"status" : -1 ,					//提供特定状态文件查询
	"origin" : -1 ,					//是否原创
	"teacher" : 0 ,					//教师 需支持模糊查询 origin为1时无效
	"low_downcnt" : 0 ,				//下载数 不少于的数值 eg.查下载数为4~7个的用户 "low_downcnt" = 4 包含边界4
	"top_downcnt" : 0 ,				//下载数 不多于的数值 eg.查下载数为4~7个的用户 "top_downcnt" = 7 包含边界7
	"low_collectcnt" : 0 ,			//收藏数
	"top_collectcnt" : 0 ,
	"low_uptime" : 0 ,				//上传时间 不早于的数值
	"top_uptime" : 0 ,				//上传时间 不晚于的数值

	"page" : 1 ,					//页码
	"res_per_page" : 20 ,			//每页结果数 提供10 20 50 100四个选项
	"sort_method" : "" ,			//排序依据
	"sort_ref" : 1					//输出顺序 字符为按照字典序 1升序(1 2 3) 2降序(3 2 1)
}
```

> 排序依据

> fid&emsp;&emsp;fname&emsp;&emsp;upuid&emsp;&emsp;upusername&emsp;&emsp;nickname

> mid(实际为xid与mid双关键字排序)&emsp;&emsp;cid&emsp;&emsp;status&emsp;&emsp;teacher&emsp;&emsp;dldcnt

> colcnt&emsp;&emsp;uptime

responce :
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" : 
	{
		"rescount" : 0 ,
		[
			"fid" : 0 ,
			"fname" : "" ,
			"upuid" : 0 ,
			"upusername" : "" ,
			"upnickname" : "" ,
			"xname" : "" ,			//学院
			"mname" : "" ,			//专业
			"cname" : "" ,			//课程
			"status" : 0 ,
			"teacher" : "" ,
			"dldcnt" : 0 ,
			"colcnt" : 0 ,
			"uptime" : "" ,
			"origin" : 0 ,
		]
	}
}
{
	"status" : 300 ,
	"message" : "Page Upperbound Overflow" ,
}
```

## Get User Log

> 后台视图用于用户操作记录查询 列表形式显示

> 对于前端，提供一定的查询条件 不需要的条件置空

> 全空为列举所有记录信息

> 后端提供符合查询条件的记录的全部所需信息

`Update 170918`

url : `admin\user\log`

method : `GET`

request :
```
{
	"targetusername" : "" ,
	"opusername" : "" ,
	"optype" : "" ,					//操作
	"low_optime" : "" ,				//操作时间
	"top_optime" : "" ,
	"low_endtime" : "" ,			//封禁结束时间
	"top_endtime" : "" ,

	"page" : 1 ,					//页码
	"res_per_page" : 20 ,			//每页结果数 提供10 20 50 100四个选项
	"sort_method" : "" ,			//排序依据
	"sort_ref" : 1					//输出顺序 字符为按照字典序 1升序(1 2 3) 2降序(3 2 1)
}
```

> 排序依据

> opuid&emsp;&emsp;optargetuid&emsp;&emsp;optime&emsp;&emsp;opip&emsp;&emsp;endtime

responce :
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" : 
	{
		"rescount" : 0 ,
		[
			"opusername" : "" ,
			"targetusername" : "" ,
			"optime" : "" ,
			"opip" : "" ,
			"type" : "" ,
			"endtime" : "" .
		]
	}
}
{
	"status" : 300 ,
	"message" : "Page Upperbound Overflow" ,
}

```

## Get File Log

> 后台视图用于文件操作记录查询 列表形式显示

> 对于前端，提供一定的查询条件 不需要的条件置空

> 全空为列举所有记录信息

> 后端提供符合查询条件的记录的全部所需信息

`Update 170918`

url : `admin\file\log`

method : `GET`

request :
```
{
	"targetusername" : "" ,
	"opusername" : "" ,
	"optype" : "" ,					//操作
	"low_optime" : "" ,				//操作时间
	"top_optime" : "" ,

	"page" : 1 ,					//页码
	"res_per_page" : 20 ,			//每页结果数 提供10 20 50 100四个选项
	"sort_method" : "" ,			//排序依据
	"sort_ref" : 1					//输出顺序 字符为按照字典序 1升序(1 2 3) 2降序(3 2 1)
}
```

> 排序依据

> opuid&emsp;&emsp;optargetuid&emsp;&emsp;optime&emsp;&emsp;opip&emsp;&emsp;

responce :
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" : 
	{
		"rescount" : 0 ,
		[
			"opusername" : "" ,		//后台注意在数据库加入opuid这一项
			"targetusername" : "" ,
			"optime" : "" ,
			"opip" : "" ,
			"type" : "" ,
			"desc" : "" .
		]
	}
}
{
	"status" : 300 ,
	"message" : "Page Upperbound Overflow" ,
}

```


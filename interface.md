# Interface Document

## Updates

- Marks

	- **M** Modify

	- **N** New

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
url : `/register`

method : `POST`

request
```
{
	"username" : "",
	"nickname" : "",
	"password" : "",
	"avator" : "",
	"mail" : "",
	"phone" : "",
	"qq" : "",
	"xid" : 1,
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
url : `/upload`

method : `POST`

request
```
{
	"name" : "" ,
	"module" : 1 ,
	"file" : 0 , 					//Update 170831
	"teacher" : "" ,  				//Update 170831
	"course" : 100101 , 			//Update 170831
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

#### Search Document
url : `/document/search`

method : `GET`

request 							//Update 170831
```
{
	"method" : 0 , 					//1:course 2:major
	"course" : 1 , 					//cid
	"college" : 1 , 				//xid
	"major" : 1 , 					//mid
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

#### Document Detail
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

#### File Modify

`Update 170831`

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
`Update 170831`

url : `/usermodify/${uid}`

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
		"path" : "" ,
	}
}
```

#### User Logout
`Update 170903`

url : `/logout/${uid}`

method : `POST`

request `{}`

responce
```
{
	"status" : 200 ,
	"message" : "OK" ,
	"data" :{}
}
```
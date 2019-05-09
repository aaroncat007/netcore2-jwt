# ASP.NET Core 2.2 - JWT Authentication API
基於.NET Core 2 建立 JWTs (_JSON Web Tokens_) 授權驗證 

## Settings ##
IMPORTANT: 將 appsettings.json 內的 "JWT"."Secret" 屬性替換為您自己的亂數 ，這將用來生成JWT tokens金鑰

## Routes ##

/users/authenticate 
> 取得授權
- method: POST 
- body: username , password
- return: If the username and password are correct , return user datails and JWT token

/api/books
> 取得書籍列表
- method: GET

## Running the project ##

> 以下範例將使用 [Postman](https://www.getpostman.com/) 進行

1. 透過 Visual Studio 2017 執行 或 透過 CLI 執行命令 `dotnet run`
2. 執行 _Postman_ 發送下列 GET request:

```
    GET http://localhost:63939/api/books HTTP/1.1
    cache-control: no-cache
    Accept: */*
    Host: localhost:63939
    accept-encoding: gzip, deflate
    Connection: keep-alive
```
如果正常運作，應該返回 401 HTTP 狀態碼 (_Unauthorized_)

3. 發送下列 POST request :

```
    POST http://localhost:63939/users/authenticate HTTP/1.1
    cache-control: no-cache
    Content-Type: application/json
    Accept: */*
    Host: localhost:63939
    accept-encoding: gzip, deflate
    content-length: 39
    Connection: keep-alive
    
    {username: "test", password: "test"}
```

如果正常運作，將返回 JSON object 如下:

```
{
    "id": 1,
    "firstName": "Test",
    "lastName": "User",
    "username": "test",
    "password": null,
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1bmlxdWVfbmFtZSI6IjEiLCJiaXJ0aGRhdGUiOiIxOTkyLTAxLTAxIiwibmJmIjoxNTU3Mzc3ODc0LCJleHAiOjE1NTc5ODI2NzQsImlhdCI6MTU1NzM3Nzg3NH0.tHmzKFfGQTiUOfOF3wcBTrwXK9T7XW4rh_haB9wgkOw",
    "birthdate": "1992-01-01T00:00:00"
}
```

4. 發送下列 GET request:

```
    GET http://localhost:63939/api/books HTTP/1.1
    cache-control: no-cache
    Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1bmlxdWVfbmFtZSI6IjEiLCJiaXJ0aGRhdGUiOiIxOTkyLTAxLTAxIiwibmJmIjoxNTU3Mzc3ODc0LCJleHAiOjE1NTc5ODI2NzQsImlhdCI6MTU1NzM3Nzg3NH0.tHmzKFfGQTiUOfOF3wcBTrwXK9T7XW4rh_haB9wgkOw
    Accept: */*
    Host: localhost:63939
    accept-encoding: gzip, deflate
    Connection: keep-alive
```

如果運作正常，應返回資料:

```
	[
	    {
	        "author": "Ray Bradbury",
	        "title": "Fahrenheit 451",
			"ageRestriction": false
	    },
	    {
	        "author": "Gabriel García Márquez",
	        "title": "One Hundred years of Solitude",
			"ageRestriction": false
	    },
	    {
	        "author": "George Orwell",
	        "title": "1984",
			"ageRestriction": false
	    },
	    {
	        "author": "Anais Nin",
	        "title": "Delta of Venus",
			"ageRestriction": true
	    }
	]
```

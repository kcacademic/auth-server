Start Auth Server
=================
mvn spring-boot:run
mvn spring-boot:run -Dspring.profiles.active="jwt"

Auth Server - Implicit Flow
===========================
//Login and Approve
http://localhost:8080/oauth/authorize?client_id=clientapp&response_type=token&scope=read_profile_info
//Copy Access Token from Redirected URL
http://localhost:8081/login#access_token=f0275bef-efdf-44ac-b8bb-773ff8fa6a24&token_type=bearer&expires_in=119
//Use the Access Token to access protected resource
curl -X GET http://localhost:8081/api/users/me -H "authorization: Bearer f0275bef-efdf-44ac-b8bb-773ff8fa6a24"
//Response from the Resource Server
{"name":"user","email":"user@sapient.com","authorities":"[ROLE_USER]"}

Auth Server - Authorization Code Grant
======================================
//Login and Approve
http://localhost:8080/oauth/authorize?client_id=clientapp&response_type=code&scope=read_profile_info
//Copy Auth Code from Redirected URL
http://localhost:8081/login?code=REbu9j
//Exchange the Auth Code fpr Access Token
curl -X POST --user clientapp:123456 http://localhost:8080/oauth/token -H "content-type: application/x-www-form-urlencoded" -d "code=REbu9j&grant_type=authorization_code&redirect_uri=http://localhost:8081/login&scope=read_profile_info"
//Response from the Auth Server with Access Token
{
    
	"access_token": "21980cf1-a233-4790-9d4d-3038884f6194",
    
	"token_type": "bearer",
    
	"refresh_token": "0974112f-7f40-4537-8f64-9ed47174b99a",
    
	"expires_in": 119,
    
	"scope": "read_profile_info"

}
//Use the Access Token to access protected resource
curl -X GET http://localhost:8081/api/users/me -H "authorization: Bearer 21980cf1-a233-4790-9d4d-3038884f6194"
//Response from the Resource Server
{"name":"user","email":"user@sapient.com","authorities":"[ROLE_USER]"}

Add New Class -> Models Folder -> RefreshToken.cs

```C#
public class RefreshToken
{
	public string Token {get; set;} = string.Empty;
	public DateTime Created {get; set;} = DateTime.Now;
	public DateTime Expires {get; set;}
}
```


Readd in the User Model

```C#
public class User
{
	public string Username {get; set;} = string.Empty;
	public byte[] PasswordHash {get; set;}
	public byte[] PasswordSalt {get; set;}
	public string RefreshToken {get; set;} = string.Empty;
	public DateTime TokenCreated {get; set;}
	public DateTime TokenExpires {get; set;}
}
```

```C#
[HttpPost("login")]
public async Task<ActionResult<string>> Login(UserDTO request)
{
	if(user.Username != request.Username)
	{
		return BadRequest("User not found.");
	}
	if( !VerifyPasswordHash(request.Password, user.PasswordHash, user.PasswordSalt) )
	{
		return BadRequest("Password didn't match!");
	}
	string token = CreateToken(user);

	//ADDED THESE
	var refreshToken = GenerateRefreshToken();
	SetRefreshToken(refreshToken)

	return Ok(token);
}

```

```C#
private RefreshToken GenerateRefreshToken()
{
	var refreshToken = new RefreshToken
	{
		Token = Convert.ToBase64String(RandomNumberGenerator.GetBytes(64)),
		Expires = DateTime.Now.AddDays(7)
	};
	return refreshToken;
}

private void SetRefreshToken(RefreshToken newRefreshToken)
{
	var cookieOptions = new CookieOptions
	{
		HttpOnly = true,
		Expires = newRefreshToken.Expires,
	};
	Response.Cookies.Append("refreshToken", newRefreshToken.Token, cookieOptions);
	user.RefreshToken = newRefreshToken.Token;
	user.TokenCreated = newRefreshToken.Created;
	user.TokenExpires = newRefreshToken.Expires;
}

```C#
[HttpPost("refresh-token")]
public async Task<ActionResult<string>> RefreshToken()
{
	var refreshToken = Request.Cookies["refreshToken"];
	if(!user.RefreshToken.Equals(refreshToken))
	{
		return Unauthorized("Invalid Refresh Token");
	}else if(user.TokenExpires < DateTime.Now)
	{
		return Unauthorized("Token expired.")
	}
	string token = CreateToken(user);
	var newRefreshToken = GenerateRefreshToken();
	SetRefreshToken(newRefreshToken)

	return Ok(token);
}
```
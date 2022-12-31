

###### Create UserDTO

```C#
public class UserDTO{
	public string Username {get; set;} = string.Empty;
	public string Password {get; set;} = string.Empty;
}
```

###### Create User

```C#
public class User
{
	public string Username {get; set;} = string.Empty;
	public byte[] PasswordHash {get; set;}
	public byte[] PasswordSalt {get; set;}
}
```

###### Create API Controller Empty

```C#
public static User user = new User();

[HttpPost("register")]
public async Task<ActionResult<User>> Register(UserDTO request)
{
	CreatePasswordHash(request.Password, out byte[] passwordHash, out byte[] passwordSalt);
	user.PasswordHash = passwordHash;
	user.PasswordSalt = passwordSalt;
	return Ok(user);
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
	return Ok("My Crazy Token!");
}
```

```C#
private bool VerifyPasswordhash(string password, byte[] passwordHash, byte[] passwordSalt )
{
	using (var hmac = new HMACSHA512(passwordSalt))
	{
		var computeHash = hmac.ComputeHash(System.Text.Encoding.UTF8.GetBytes(password));
		return computedHash.SequenceEqual(passwordHash);
	}
}

```

```C#
private void CreatePasswordHash(string password, out byte[] passwordHash, out byte[] passwordSalt)
{
	using(var hmac = new HMACSHA512())
	{
		passwordSalt = hmac.Key;
		passwordHash = hmac.ComputeHash(System.Text.UTF8.GetBytes(password))
	}
}
```

### CreateToken Method

```C#
private string CreateToken(User user)
{
	List<Claim> claims = new List<Claim>
	{
		new Claim(ClaimTypes.Name, user.Username),
		new Claim(ClaimTypes.Role, "Admin")
	};
	
	//install Microsoft.IdentityModel.Tokens
	//passing secret key from appsettings
	//Init IConfiguration from the constructor of the controller
	var key = new SymmetricSecurityKey(System.Text.Encoding.UTF8.GetBytes(
		_configuration.GetSection("AppSettings:Token").Value));
	))

	var cred = new SigningCredentials(key, SecurityAlgorithms.HmacSha512Signature);
	var token = new JwtSecurityToken(
		claims: claims,
		expires: DateTime.Now.AddDays(1),
		signingCredentials: creds);
	var jwt = new JwtSecurityTokenHandler().WriteToken(token);

	return jwt;
}
```

```C#
//appsettings.json
//ensure key is larger than 16 characters
"AppSettings": {
	"Token": "my top secret key";
}
```

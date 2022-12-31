
Add New Class -> RefreshToken.cs

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
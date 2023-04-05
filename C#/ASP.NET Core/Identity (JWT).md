### Nuget
.proj
``` xml
<ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.Authentication.JwtBearer" Version="3.1.32" />
</ItemGroup>
```
### appsettings.json
``` js
"Identity": {
    "Jwt": {
        "IssuerSigningKey": "",
        "Issuer": "",
        "Audience": "",
        "Lifetime": 270000
    }
}
```
### Startup.cs
ConfigureServices
``` cs
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.IncludeErrorDetails = true;
        options.TokenValidationParameters = new TokenValidationParameters()
        {
            ValidateIssuerSigningKey = true,
            IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(Configuration.GetValue<string>("Identity:Jwt:IssuerSigningKey"))),
            ValidateIssuer = true,
            ValidIssuer = Configuration.GetValue<string>("Identity:Jwt:Issuer"),
            ValidateAudience = true,
            ValidAudience = Configuration.GetValue<string>("Identity:Jwt:Audience"),
            ValidateLifetime = true
        };
    });
```
### IdentityController.cs
``` cs
[ApiController]
[Route("api/[controller]")]
public class IdentityController : ControllerBase
{
    private readonly IConfiguration _configuration;

    public IdentityController(IConfiguration configuration)
    {
        _configuration = configuration;
    }

    [HttpPost]
    [Route("login")]
    public async Task<IActionResult> Login([FromBody] Login.Request request)
    {
        if (!ModelState.IsValid)
        {
            return BadRequest(ModelState);
        }

        // TODO: 驗證帳號密碼

        var utcNow = DateTime.UtcNow;

        var claims = new List<Claim>();
        claims.Add(new Claim(JwtRegisteredClaimNames.Sub, userId));
        claims.Add(new Claim(JwtRegisteredClaimNames.UniqueName, request.Username));
        claims.Add(new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString()));
        claims.Add(new Claim(JwtRegisteredClaimNames.Iat, utcNow.ToString()));

        // TODO: 加入帳號權限的 Claims

        var securityTokenHandler = new JwtSecurityTokenHandler();
        var securityToken = securityTokenHandler.CreateToken(new SecurityTokenDescriptor()
        {
            SigningCredentials = new SigningCredentials(
                new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_configuration.GetValue<string>("Identity:Jwt:IssuerSigningKey"))),
                SecurityAlgorithms.HmacSha256Signature),
            Issuer = _configuration.GetValue<string>("Identity:Jwt:Issuer"),
            Audience = _configuration.GetValue<string>("Identity:Jwt:Audience"),
            Subject = new ClaimsIdentity(claims),
            NotBefore = utcNow,
            Expires = utcNow.AddSeconds(_configuration.GetValue<int>("Identity:Jwt:Lifetime"))
        });
        var token = securityTokenHandler.WriteToken(securityToken);

        return Ok(token);
    }

    [HttpGet]
    [Route("user")]
    public async Task<IActionResult> GetUserName()
    {
        return Ok(User.Identity.Name);
    }
}
```
### Login.cs
``` cs
public class Login
{
    public class Request
    {
        [Required]
        public string Username { get; set; }

        [Required]
        public string Password { get; set; }
    }
}
```
### 以 curl 測試
取得 token
```
curl -H "Content-Type: application/json" -d "{\"Username\":\"{username}\",\"Password\":\"{password}\"}" http://localhost:5000/api/identity/login
```
查看帳號名稱
```
curl -H "Authorization: Bearer {token}" http://localhost:5000/api/identity/user
```

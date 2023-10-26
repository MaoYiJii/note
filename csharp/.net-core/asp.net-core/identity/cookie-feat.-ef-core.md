# Cookie feat. EF Core

#### Nuget

.proj

```xml
<ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.Authentication.JwtBearer" Version="3.1.32" />
</ItemGroup>
```

#### Startup.cs

ConfigureServices

```cs
services
    .AddEntityFrameworkSqlServer()
    .AddDbContext<DefaultDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("Default")));

services
    .AddIdentity<ApplicationUser, ApplicationRole>()
    .AddEntityFrameworkStores<DefaultDbContext>();

services.Configure<IdentityOptions>(options =>
{
    // Password settings.
    options.Password.RequireDigit = true;
    options.Password.RequireUppercase = true;
    options.Password.RequireLowercase = true;
    options.Password.RequireNonAlphanumeric = true;
    options.Password.RequiredLength = 6;
    options.Password.RequiredUniqueChars = 1;

    // Lockout settings.
    options.Lockout.DefaultLockoutTimeSpan = TimeSpan.FromMinutes(5);
    options.Lockout.MaxFailedAccessAttempts = 5;
    options.Lockout.AllowedForNewUsers = true;

    // User settings.
    options.User.AllowedUserNameCharacters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+";
    options.User.RequireUniqueEmail = false;
});
```

Configure

```cs
app.UseAuthentication();
app.UseAuthorization();
```

#### IdentityController.cs

```cs
[ApiController]
[Route("api/[controller]")]
public class IdentityController : ControllerBase
{
    private readonly IConfiguration _configuration;

    public IdentityController(IConfiguration configuration, UserManager<User> userManager, SignInManager<User> signInManager)
    {
        _configuration = configuration;
    }

    [HttpPost]
    [Route("login")]
    public async Task<IActionResult> Login([FromBody] Login.Request request)
    {
        await _signInManager.PasswordSignInAsync(request.Username, request.Password, true, true);
        return Ok();
    }

    [HttpGet]
    [Route("user")]
    public async Task<IActionResult> GetUserName()
    {
        return Ok(User.Identity.Name);
    }
}
```

#### Login.cs

```cs
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

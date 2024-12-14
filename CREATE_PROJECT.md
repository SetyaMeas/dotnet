## .NET web api with controller base

### Create project

```
dotnet new webapi -n MyWebApi
```

---

### Folder structure

```
MyWebApi
├── appsettings.Development.json
├── appsettings.json
├── obj/
├── Program.cs
├── Properties/
├── MyWebApi.csproj
├── MyWebApi.http
└── Controllers/ /* manually create */
    └── TestController.cs
```

---

### Modify Program.cs

```csharp
// Program.cs
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

builder.Services.AddControllers(); // manually add

var app = builder.Build();

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();

app.MapControllers(); // manually add

app.Run();
```

---

### Create Controller

```csharp
// Controllers/TestController.cs
using Microsoft.AspNetCore.Mvc;
namespace MyWebApi.Controllers
{
    [Route("api/{controller}")]
    [ApiController]
    public class TestController : ControllerBase
    {
        [HttpGet]
        public IActionResult GetMethod()
        {
            return Ok("This is GET method");
        }

        [HttpPost]
        public IActionResult PostMethod()
        {
            return Ok("This is POST method");
        }

        [HttpPut]
        public IActionResult PutMethod()
        {
            return Ok("This is PUT method");
        }

        [HttpDelete]
        public IActionResult DeleteMethod()
        {
            return Ok("This is Delete method");
        }
    }
}
```

---

### Running program

```
dotnet run
```

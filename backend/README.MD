## Development environment
### Visual Studio guide for C# projects
https://learn.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code?pivots=dotnet-7-0

P.S. gitignore generation for .NET: `dotnet new gitignore --force`

## Init instructions
1. From https://metanit.com/sharp/grpc/1.1.php only need `dotnet new grpc`
2. gRPC transcoding: https://learn.microsoft.com/ru-ru/aspnet/core/grpc/json-transcoding?view=aspnetcore-7.0
Even newer article: https://learn.microsoft.com/en-us/aspnet/core/grpc/json-transcoding?view=aspnetcore-7.0
Important thing to learn: config of `appsettings.json`
```
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
```
3. TODO add Swagger UI as well, articles:
  - https://learn.microsoft.com/en-us/aspnet/core/grpc/json-transcoding-openapi?view=aspnetcore-7.0
  - https://learn.microsoft.com/en-us/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-7.0&tabs=visual-studio


## Howto add Swagger
Two articles
1. https://learn.microsoft.com/en-us/aspnet/core/grpc/json-transcoding-openapi?view=aspnetcore-7.0
2. https://learn.microsoft.com/en-us/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-7.0&tabs=visual-studio

Key notes:
1. Add this to backend.csproj:
```
  <PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
  </PropertyGroup>
```
2. How to generate swagger config:
  1. Launch app for the first time
  2. In build folder (for instance `bin/Debug/net7.0`) there will be generated `backend.xml` because name of the app is `backend`. Thus you put this into swagger gen:
  ```
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1",
        new Microsoft.OpenApi.Models.OpenApiInfo { Title = "gRPC transcoding", Version = "v1" });

    var filePath = Path.Combine(System.AppContext.BaseDirectory, "backend.xml");
    c.IncludeXmlComments(filePath);
    c.IncludeGrpcXmlComments(filePath, includeControllerXmlComments: true);
});
```

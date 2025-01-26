# MegaMan API Documentation

## Contexto
Este projeto é uma API feita em .NET Core para listar os dados dos bosses de MegaMan. O objetivo principal é ser um backend que fornece JSONs no formato abaixo:

```json
{
  "Id": 1,
  "Code": "DLN/DRN-003",
  "Name": "Cutman",
  "HP": 150,
  "Picture": "https://vignette.wikia.nocookie.net/megaman/images/2/22/Cutman.png"
}
```

## Especificações do Projeto

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>netcoreapp3.1</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore" Version="3.1.8" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.1.8">
      <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
      <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="3.1.8" />
    <PackageReference Include="Newtonsoft.Json" Version="12.0.2" />
  </ItemGroup>
</Project>
```

## Dependências

| Dependência | Descrição |
|-------------|-----------|
| [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.1.8) | ORM para .NET Core (veja [artigo](https://docs.microsoft.com/en-us/ef/core/)) |
| [Microsoft.EntityFrameworkCore.Design](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Design/3.1.8) | Ferramentas de design para Entity Framework Core (veja [artigo](https://docs.microsoft.com/en-us/ef/core/cli/dotnet)) |
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/3.1.8) | Provedor de banco de dados SQL Server para Entity Framework Core (veja [artigo](https://docs.microsoft.com/en-us/ef/core/providers/sql-server/)) |
| [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/12.0.2) | Biblioteca para trabalhar com JSON em .NET (veja [artigo](https://www.newtonsoft.com/json/help/html/Introduction.htm)) |

## Endpoints do Projeto

```csharp
namespace Megaman.Controllers
{
    [ApiController]
    [Route("api/v1/robots")]
    public class RobotsController : ControllerBase
    {
        private readonly IRobotServices _services;
        public RobotsController(IRobotServices services)
        {
           _services = services;
        }

        [HttpGet]
        public ActionResult<IEnumerable<RobotReadDTO>> GetAllRobots()
        {
            var robotItems = _services.SearchAll();
            return Ok(robotItems);
        }

        [HttpGet]
        [Route("{id:int}")]
        public object GetCommandById([FromRoute]int id)
        {
            var robot = _services.SearchById(id);

            if(robot != null)
                return Ok(robot);

            return NotFound(new { message = "Nenhum robo encontrado" });
        }

        [HttpPost]
        public ActionResult RobotSend(){
            return Ok();
        }
    }
}
```

## Estrutura do Projeto

```plaintext
.vs
.vscode
bin
Controllers
Database
middlewares
Models
obj
Properties
Services
appsettings.Development.json
appsettings.json  
global.json
MegamanApi.csproj  
MegamanApi.sln
Program.cs
Startup.cs
```

## Técnicas Utilizadas

- **Entity Framework Core**: Utilizado como ORM para interagir com o banco de dados.
- **Dependency Injection**: Injeção de dependências para gerenciar as dependências do projeto.
- **ASP.NET Core**: Framework para construção de APIs web.
- **Newtonsoft.Json**: Biblioteca para manipulação de JSON.


# ABC Hotel – Back End (AbcRest-Final)

ASP.NET Core Web API for the ABC Restaurant / hotel web app. It exposes REST endpoints for **products** (menu items), **table bookings** and **users** (register / login), stores data in SQL Server via Entity Framework Core, and sends booking notification e-mails over SMTP.

The matching Angular client lives in the `ABC-Hotel-FrontEnd` repository.

## Tech stack

- .NET 8 / ASP.NET Core Web API (controllers + Razor Pages enabled)
- Entity Framework Core 8 (SQL Server provider, code-first migrations)
- Swagger / Swashbuckle for API exploration
- `System.Net.Mail` SMTP client for e-mail notifications

## Folder structure

```text
ABC-Hotel-BackEnd/
├── AbcRest-Final/
│   ├── Controllers/
│   ├── Database Context/
│   ├── Interface/
│   ├── Migrations/
│   ├── Model/
│   ├── Properties/
│   ├── Service/
│   ├── AbcRestFinal.csproj
│   ├── appsettings.Development.example.json
│   ├── appsettings.json
│   └── Program.cs
├── .editorconfig
├── .gitattributes
├── .gitignore
├── AbcRest-Final.sln
└── README.md
```

| Folder | Purpose |
| --- | --- |
| `Controllers/` | API controllers: `BookingController`, `ProductsController`, `UsersController` |
| `Database Context/` | `ApplicationDbContext` (EF Core) |
| `Interface/` | Generic repository (`IRepository<T>` / `Repository<T>`) |
| `Model/` | Entity and DTO classes |
| `Service/` | Booking, user and e-mail services |
| `Migrations/` | EF Core migrations (applied automatically on start-up) |

## Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
- SQL Server (Express or LocalDB is fine)
- Optional: Visual Studio 2022 / Rider / VS Code
- Optional: an SMTP account (e.g. a Gmail app password) for booking e-mails

## Configuration

Real credentials must **not** be committed. `appsettings.json` only contains placeholders.

1. Copy the example file and fill in your own values:

   ```bash
   cd AbcRest-Final
   cp appsettings.Development.example.json appsettings.Development.json
   ```

2. Set your SQL Server connection string in `ConnectionStrings:DefaultConnection`.
3. Set the `SmtpSettings` values (server, port, sender, username, app password).

`appsettings.Development.json` is git-ignored, so your local secrets stay on your machine. Alternatively use environment variables (e.g. `SmtpSettings__Password`) or [`dotnet user-secrets`](https://learn.microsoft.com/aspnet/core/security/app-secrets).

## Running locally

```bash
dotnet restore
dotnet build
dotnet run --project AbcRest-Final
```

- On start-up the app runs `Database.Migrate()`, so the database is created/updated automatically.
- In the Development environment Swagger UI is available at `http://localhost:5214/swagger` (see `Properties/launchSettings.json` for the configured ports).
- CORS allows the Angular dev server at `http://localhost:4200`.

To add a new migration after changing models:

```bash
dotnet tool install --global dotnet-ef   # once
dotnet ef migrations add <Name> --project AbcRest-Final
```

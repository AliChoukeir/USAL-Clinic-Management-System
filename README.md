# 🏥 USAL Clinic Management System

A web-based **Clinic Management System** built with **ASP.NET Core MVC**, **Entity Framework Core**, and **SQL Server**.  
This project streamlines clinic operations such as patient management, appointments, medical records, and prescriptions.  
Developed as my **Final Year Project (FYP)**.

---

## 🚀 Features

- Role-based access control (Admin, Doctor, Nurse, Patient)
- Appointment scheduling and management
- Patient and doctor registration
- Electronic Medical Records (EMR)
- Prescription management
- Audit logging and reporting
- Dashboard analytics
- Secure authentication (ASP.NET Identity)
- RESTful API support for mobile integration

---

## 🛠️ Tech Stack

| Category | Technologies |
|-----------|---------------|
| **Frontend** | HTML, CSS, JavaScript, Bootstrap |
| **Backend** | ASP.NET Core MVC, C# |
| **Database** | SQL Server, Entity Framework Core |
| **Security & Identity** | ASP.NET Identity, JWT |
| **Architecture** | Clean Architecture with repository, service & unit-of-work layers |
| **Logging & Testing** | Serilog, xUnit tests |

---

## 🗃️ Project Architecture
```
USAL-Clinic-Management-System/
│
├── UsalClinic.Core/ # Entities and domain models
├── UsalClinic.Application/ # DTOs, services, and business logic
├── UsalClinic.Infrastructure/ # Repositories, EF Core, data access
├── UsalClinic.Web/ # MVC web app (controllers, views)
├── UsalClinic.Api/ # RESTful API (JWT-secured endpoints)
└── UsalClinic.Tests/ # xUnit service tests
```

Dependencies point inward: `Core` references nothing, `Infrastructure` implements the interfaces
`Core` declares, and the two front ends sit on top of `Application`.

---

## ⚡ Getting Started

### Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
- **SQL Server LocalDB** — included with Visual Studio, or install
  [SQL Server Express LocalDB](https://learn.microsoft.com/sql/database-engine/configure-windows/sql-server-express-localdb)

> **Windows only by default.** The connection string targets LocalDB. To run on macOS or Linux,
> point `ConnectionStrings:DefaultConnection` in `appsettings.json` at a SQL Server instance —
> for example one running in Docker:
> ```
> docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Your_password123" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2022-latest
> ```
> ```
> Server=localhost,1433;Database=UsalClinic;User Id=sa;Password=Your_password123;TrustServerCertificate=True
> ```

### Run the web application

```bash
git clone https://github.com/AliChoukeir/USAL-Clinic-Management-System.git
cd USAL-Clinic-Management-System
dotnet run --project UsalClinic.Web
```

Then open **http://localhost:5000**.

**The database is created automatically on first run** — migrations are applied at startup, and
roles plus an administrator account are seeded.

### Default login

| Email | Password |
|---|---|
| `admin@clinic.com` | `Admin@123` |

> Seed credentials for local development only. Change them before deploying anywhere real.

### Run the REST API

The API validates JWTs with a signing key that is **not** stored in the repository. Supply one
before starting it:

```bash
dotnet user-secrets set "JwtSettings:Secret" "replace-with-a-random-string-of-32-plus-characters" --project UsalClinic.Api
```

Then:

```bash
# Windows (PowerShell)
$env:ASPNETCORE_ENVIRONMENT = "Development"
# macOS / Linux
export ASPNETCORE_ENVIRONMENT=Development

dotnet run --project UsalClinic.Api
```

Swagger UI is at **http://localhost:5000/swagger**.

> The `Development` environment is required because .NET only loads user secrets there. Startup
> deliberately **throws** if the signing key is missing, rather than falling back to a default —
> a misconfigured deployment should fail loudly, not run on a predictable key.
>
> In production, supply it as an environment variable instead:
> ```bash
> JwtSettings__Secret="..."      # note the double underscore
> ```

### Run the tests

```bash
dotnet test
```

### Managing migrations manually

Migrations apply automatically, but if you need to run them yourself:

```bash
dotnet tool install --global dotnet-ef
dotnet ef database update --project UsalClinic.Infrastructure --startup-project UsalClinic.Web
```

---

## ⚙️ Configuration

`appsettings.json` holds non-secret configuration only. Secrets are supplied through
[user secrets](https://learn.microsoft.com/aspnet/core/security/app-secrets) in development and
environment variables or a key vault in production.

| Setting | Purpose |
|---|---|
| `ConnectionStrings:DefaultConnection` | SQL Server connection string |
| `JwtSettings:Secret` | JWT signing key — **required by the API**, 32+ characters |
| `EmailSettings:*` | SMTP settings for appointment confirmation emails (optional; the app runs without them) |

---

## 🌱 Future Enhancements

- Telemedicine (video consultations)
- Billing & invoicing
- Mobile app integration
- Real-time notifications
- Advanced analytics dashboard

---

## 👨‍💻 Author

**Ali Choukeir**  
Software Developer | C# ASP.NET Core | 42 Beirut Student | USAL CS Graduate
🔗 [LinkedIn](https://www.linkedin.com/in/ali-choukeir/)  
📧 chkeira664@gmail.com

---

## 📄 License

This project is open source under the **MIT License**.

---

# RaceDay_POE_Part3
# RaceDay - Event Management System

**Course Module:** PROG6212 - Programming 2B  
**Assessment:** Portfolio of Evidence (POE) Part 3  

RaceDay is a full-stack Web Application designed for managing endurance sporting events (running, walking, cycling). Built with ASP.NET Core MVC and ASP.NET Core Web API, the system provides distinct workflows for **Organisers** to manage events and record results, and **Participants** to browse, enrol in races, and track their performance positions.

---

## 🏗 System Architecture

The application is structured into a multi-tier micro-service-ready architecture using the ASP.NET Core Web API backend and ASP.NET Core MVC front-end.

```
                  ┌───────────────────────────────┐
                  │         ASP.NET Core          │
                  │       MVC Front-End           │
                  │       (RaceDay_MVC)           │
                  └──────────────┬────────────────┘
                                 │
                         HTTP / REST API Calls
                                 │
                  ┌──────────────▼────────────────┐
                  │         ASP.NET Core          │
                  │       Web API Back-End        │
                  │       (RaceDay_API)           │
                  └───────┬───────────────┬───────┘
                          │               │
               Entity Framework Core      │  Azure Blob Storage SDK
                          │               │
        ┌─────────────────▼──┐   ┌────────▼──────────────┐
        │  Microsoft SQL     │   │  Azure Blob Storage   │
        │      Database      │   │  (Event Banners)      │
        └────────────────────┘   └───────────────────────┘
```

---

## ✨ Key Features & Functionality

### 1. Role-Based Navigation & Security
- **Authentication & Authorization:** Powered by a custom `SessionAuthorize` filter and ASP.NET Session state management.
- **Organiser Flow:** Automatically redirects logged-in Organisers directly to their **Organiser Dashboard** and administrative routes.
- **Participant Flow:** Redirects Participants to the public **Events Catalogue**, **My Enrolments**, and **My Results** pages.

### 2. Organiser Module
- **Dashboard & Analytics:** Displays metrics for created events, upcoming races, total enrolments across categories, and quick navigation.
- **Event Management:** Create, edit, and delete events. Upload custom banner images directly to **Azure Blob Storage**.
- **Category Management:** Define distance categories, maximum entrant caps, and entry fees per race event.
- **Attendee Tracking & Results Entry:** View participant lists for every event category, assign race bib numbers, and capture finish times/positions.

### 3. Participant Module
- **Events Catalogue:** Advanced searching and multi-parameter filtering (by race type: *Run*, *Walk*, *Cycle*, location, and date range).
- **Event Entry Form:** Select race categories and confirm enrolment with instant bib number generation.
- **My Enrolments:** View upcoming confirmed events with the option to cancel enrolments prior to race day.
- **My Results:** Track overall finish times, positions, and category rankings for completed races.

---

## 🛠 Tech Stack & Dependencies

- **Framework:** .NET 8.0 / ASP.NET Core
- **Front-End:** ASP.NET Core MVC, Razor Views, Bootstrap 5, Bootstrap Icons
- **Back-End API:** ASP.NET Core Web API, Swagger / OpenAPI UI
- **Database:** Microsoft SQL Server, Entity Framework Core 8.0 (Code-First Migrations)
- **Cloud Storage:** Azure Storage Blobs SDK (for event banner images)
- **Containerization:** Docker (`Dockerfile` and multi-stage container support)

---

## 📁 Solution Structure

```
RaceDay/
├── RaceDay_API/                  # Web API Backend Project
│   ├── Controllers/             # Auth, Events, Categories, Enrolments, Results controllers
│   ├── Data/                    # RaceDayContext.cs & EF Core Migrations
│   ├── Models/                  # Database Entity Models
│   └── Program.cs               # API Services & Dependency Injection
│
├── RaceDay_MVC/                  # MVC Front-End Project
│   ├── Controllers/             # Account, Events, Home, Organiser, Participant controllers
│   ├── Filters/                 # SessionAuthorize custom authorization attributes
│   ├── Services/                # ApiService HTTP client wrapper
│   ├── ViewModels/              # Strongly typed view models for Razor views
│   ├── Views/                   # UI Templates (Account, Events, Organiser, Participant)
│   └── appsettings.json         # API Base URL configuration
│
└── RaceDay_API.Tests/            # Unit & Integration Tests Project
```

---

## 🚀 Getting Started

### Prerequisites
- [.NET 8.0 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
- [Visual Studio 2022](https://visualstudio.microsoft.com/) (with *ASP.NET and web development* workload)
- [SQL Server Express](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) or LocalDB

### 1. Database Setup
1. Open `RaceDay_API/appsettings.json` and ensure your SQL Server connection string is configured correctly:
   ```json
   "ConnectionStrings": {
     "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=RaceDayDB;Trusted_Connection=True;MultipleActiveResultSets=true"
   }
   ```
2. Run the Entity Framework migrations to generate the database schema:
   ```bash
   dotnet ef database update --project RaceDay_API
   ```

### 2. Running the Application via Visual Studio
1. Open `WebApplication_prompt_users.sln` in Visual Studio.
2. Right-click the Solution in **Solution Explorer** and select **Properties**.
3. Under **Startup Project**, select **Multiple startup projects**:
   - Set `RaceDay_API` to **Start**
   - Set `RaceDay_MVC` to **Start**
4. Press **F5** to start both projects simultaneously.
   - **Web API Swagger UI:** `https://localhost:7115/swagger`
   - **MVC Application:** `https://localhost:7265`

---

## 🐳 Docker Support

To run the application inside Docker containers, execute the following command from the solution root:

```bash
docker-compose up --build
```

The Web API container exposes port `8080`, and the MVC front-end application exposes port `8000`.

---

## 🛡 License & Credits

Developed as part of the **PROG6212** POE submission.  
© 2026 RaceDay Event Management System.

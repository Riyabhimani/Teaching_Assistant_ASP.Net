# Areas in ASP.NET Core

## What is an Area?

An **Area** in ASP.NET Core is a way to organize a large application into **separate sections**.

Think of an Area like departments in a company:

Admin Department

User Department

Manager Department

Each department has its own **controllers, views, and models**.

## Why do we need Areas?

When a project becomes big:

Too many controllers in one folder

Hard to manage files

Confusing navigation

✅ Areas solve this problem by **grouping related features**.

## Without Area (Small Project)

**Controllers**
 ├── HomeController.cs
 ├── AccountController.cs
 ├── ProductController.cs

## With Area (Large Project)

**Areas**
 ├── **Admin**
 │    ├── Controllers
 │    │     └── DashboardController.cs
 │    ├── Views
 │    │     └── Dashboard
 │    │           └── Index.cshtml
 │    └── Models
 │
 ├── **User**
 │    ├── Controllers
 │    │     └── ProfileController.cs
 │    ├── Views
 │    │     └── Profile
 │    │           └── Index.cshtml

## Real-Life Example

Imagine an **E-Commerce website**:

| Area   | Purpose                        |
| ------ | ------------------------------ |
| Admin  | Manage products, users, orders |
| User   | View products, place orders    |
| Seller | Manage their products          |

## How to Create an Area (Step by Step)

**Step 1: Create Area Folder**

Inside your project:

Areas
 └── Admin
     ├── Controllers
     ├── Views
     └── Models

**Step 2: Create Controller inside Area**

Admin → Controllers → DashboardController.cs

using Microsoft.AspNetCore.Mvc;

namespace MyProject.Areas.Admin.Controllers
{
    [Area("Admin")]
    public class DashboardController : Controller
    {
        public IActionResult Index()
        {
            return View();
        }
    }
}

👉 “This controller belongs to **Admin Area**”

**Step 3: Create View**

Path must be exactly like this:

Areas/Admin/Views/Dashboard/Index.cshtml

<h2>Admin Dashboard</h2>
<p>Welcome to Admin Area</p>

**Step 4: Configure Routing**

In **Program.cs**:

app.MapControllerRoute(
    name: "areas",
    pattern: "{area:exists}/{controller=Home}/{action=Index}/{id?}"
);

app.MapDefaultControllerRoute();

🔹 **{area:exists}** → means area will be used **only if it exists**.

## How to Access Area in Browser

https://localhost:5001/Admin/Dashboard/Index

Breakdown:

**Admin** → Area name

**Dashboard** → Controller

**Index** → Action

## Link to Area from View

<a asp-area="Admin"
   asp-controller="Dashboard"
   asp-action="Index">
   Go to Admin Dashboard
</a>

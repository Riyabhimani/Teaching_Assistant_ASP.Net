# Partial Views in ASP.NET

A **Partial View** is a small, reusable piece of UI that you can use in multiple pages to avoid copying the same code everywhere.

## 🎯 What is a Partial View?

**Partial View = Reusable UI Piece**

Instead of full pages, partial views contain small sections like:
- Header
- Navbar  
- Footer

---


## ❌ Problem Without Partial Views
- Duplicate code everywhere
- Hard to maintain
- Change header? → Update 10+ files!

---

## ✅ Solution: Partial Views

```
Create ONCE:
├── _Header.cshtml
├── _Navbar.cshtml
└── _Footer.cshtml

Use EVERYWHERE:
@Html.Partial("_Header")
@Html.Partial("_Navbar")
@Html.Partial("_Footer")
```

---

## 📁 File Structure for Your Project

```
Views/
├── Shared/
│   ├── _Header.cshtml     ← Your header
│   ├── _Navbar.cshtml     ← Your navbar  
│   └── _Footer.cshtml     ← Your footer
├── Home/
│   └── Index.cshtml
├── Account/
│   └── Login.cshtml
└── _Layout.cshtml
```

**💡 Pro Tip:** Files starting with `_` = Partial Views

---

## 🚀 How to Use Partial Views (3 Easy Methods)

### Method 1: `@Html.Partial()` ⭐ **Recommended**
```html
@Html.Partial("_Header")
@Html.Partial("_Navbar")

<h1>Your Page Content</h1>

@Html.Partial("_Footer")
```

### Method 2: `@Html.RenderPartial()`
```html
@{ Html.RenderPartial("_Header"); }
<!-- Faster but less flexible -->
```

### Method 3: In Layout File (BEST for Header/Navbar/Footer)
**_Layout.cshtml:**
```html
<!DOCTYPE html>
<html>
<body>
    @Html.Partial("_Header")
    @Html.Partial("_Navbar")
    
    @RenderBody()  <!-- Your page content here -->
    
    @Html.Partial("_Footer")
</body>
</html>
```

---

## 💻 Real Code Examples for Your Project

### 1. Header Partial (`_Header.cshtml`)
```html
<header class="main-header">
    <div class="logo">My Awesome Website</div>
    <div class="search-bar">
        <input type="text" placeholder="Search...">
    </div>
</header>
```

**Use it:** `@Html.Partial("_Header")`

### 2. Navbar Partial (`_Navbar.cshtml`)
```html
<nav class="main-nav">
    <a href="/">🏠 Home</a>
    <a href="/About">📄 About</a>
    <a href="/Contact">📞 Contact</a>
    <a href="/Dashboard">📊 Dashboard</a>
</nav>
```

**Use it:** `@Html.Partial("_Navbar")`

### 3. Footer Partial (`_Footer.cshtml`)
```html
<footer class="main-footer">
    <p>&copy; 2026 My Website. All rights reserved.</p>
    <div class="social-links">
        <a href="#">Facebook</a> | 
        <a href="#">Twitter</a> | 
        <a href="#">Instagram</a>
    </div>
</footer>
```

**Use it:** `@Html.Partial("_Footer")`

---

## 🌍 10 Real-World Scenarios

| # | Scenario | Example File | Where Used |
|---|----------|--------------|------------|
| 1 | **Header** | `_Header.cshtml` | All pages |
| 2 | **Navbar** | `_Navbar.cshtml` | All pages |
| 3 | **Footer** | `_Footer.cshtml` | All pages |
| 4 | **Product Card** | `_ProductCard.cshtml` | Ecommerce, Catalog |
| 5 | **Student List** | `_StudentList.cshtml` | Dashboard, Reports |
| 6 | **Login Form** | `_LoginForm.cshtml` | Login page, Modal, Sidebar |
| 7 | **Comment Section** | `_Comments.cshtml` | Blog, News, Articles |
| 8 | **Notification Panel** | `_Notifications.cshtml` | Dashboard, User Panel |
| 9 | **User Profile Card** | `_UserCard.cshtml` | Social media, Directory |
| 10 | **Sidebar Menu** | `_Sidebar.cshtml` | Admin panel, Dashboard |

---

## ⚡ Partial View vs Layout (Quick Comparison)

| Feature | **Layout** (`_Layout.cshtml`) | **Partial View** |
|---------|-------------------------------|------------------|
| **Purpose** | Full page template | Small UI piece |
| **Size** | Entire page structure | Small component |
| **Example** | Header + Body + Footer | Just Header |
| **Used in** | All pages (wraps content) | Inside any view |
| **File name** | `_Layout.cshtml` | `_Header.cshtml` |

---

## 🎉 Advantages (Why Use Partial Views?)

- ✅ **No Duplicate Code**
- ✅ **Easy to Maintain** (Change once = update everywhere)
- ✅ **Clean & Organized**
- ✅ **Faster Development**
- ✅ **Professional Code Structure**
- ✅ **Team Friendly**

---

## 🏗️ Perfect Structure for YOUR Project

```
1. Create these files in Views/Shared/:
   ├── _Header.cshtml
   ├── _Navbar.cshtml
   └── _Footer.cshtml

2. Update _Layout.cshtml:
```
<!DOCTYPE html>
<html>
<head>
    <title>@ViewBag.Title</title>
</head>
<body>
    @Html.Partial("_Header")
    @Html.Partial("_Navbar")
    
    <main class="main-content">
        @RenderBody()
    </main>
    
    @Html.Partial("_Footer")
</body>
</html>
```

**✅ Done! Every page now has Header + Navbar + Footer automatically.**

***
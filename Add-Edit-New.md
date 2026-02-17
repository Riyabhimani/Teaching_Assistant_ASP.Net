# Insert & Update Department Using Single Save Method

---

# ✅ Given Table Structure

```sql
CREATE TABLE MOM_Department (
    DepartmentID INT IDENTITY(1,1) PRIMARY KEY,
    DepartmentName NVARCHAR(100) NOT NULL,
    Created DATETIME DEFAULT GETDATE(),
    Modified DATETIME NOT NULL
);
```

---

# 🎯 Objective

We have:

* Two Stored Procedures (Insert + Update)
* Only ONE controller method:

---

# Insert

## Step 1: Insert Stored Procedure

```sql
CREATE PROCEDURE [dbo].[PR_MOM_Department_Insert]
    @DepartmentName NVARCHAR(100)
AS
BEGIN
    INSERT INTO MOM_Department (DepartmentName, Modified)
    VALUES (@DepartmentName, GETDATE())
END
```

---

## Step 2: Department Model

```csharp
public class DepartmentModel
{
    public int DepartmentID { get; set; }
    public string DepartmentName { get; set; }
}
```

---

## Step 3: Single Save Method (POST) – Without using

```csharp
[HttpPost]
public IActionResult Save(DepartmentModel model)
{
    if (ModelState.IsValid)
    {
        string connectionString = configuration.GetConnectionString("ConnectionString");
        SqlConnection sqlConnection = new SqlConnection(connectionString);
        sqlConnection.Open();
        SqlCommand command = sqlConnection.CreateCommand();
        command.CommandType = CommandType.StoredProcedure;
        command.CommandText = "PR_MOM_Department_Insert";
        command.Parameters.AddWithValue("@DepartmentName", model.DepartmentName);
        command.ExecuteNonQuery();
        sqlConnection.Close();
        return RedirectToAction("list");
    }

    return View("AddEdit", model);
}
```
## command.Parameters.AddWithValue
✅ What it does:
Creates a parameter automatically.
Automatically guesses the SQL type from the value in C#.
### My Question to you in Delete what we used to get parameter??
---

### Better to use Add() method rather than AddWithValue() 
**why?**

* Better performance

* No type guessing

* Avoids unexpected conversion issues

syntax
```csharp
command.Parameters.Add("@DepartmentName", SqlDbType.VarChar).Value = model.DepartmentName;
```

---

### Add another Action method which will bring you to form(view page)



# Step 7: AddEdit View (Razor Form)
```csharp
public IActionResult AddEdit()
{
    return View();
}
```


📁 Views/Department/AddEdit.cshtml

```csharp
@model DepartmentModel

<h2>Add / Edit Department</h2>

<form asp-action="Save" method="post">

    <div>
        <label>Department Name</label>
        <input asp-for="DepartmentName" class="form-control" />
        <span asp-validation-for="DepartmentName" class="text text-danger"></span>
    </div>

    <br />
    <button type="submit" class="btn btn-primary">Save</button>

</form>
```

---

# Update



## Step 1: Update Stored Procedure

```sql
CREATE PROCEDURE [dbo].[PR_MOM_Department_Update]
    @DepartmentID INT,
    @DepartmentName NVARCHAR(100)
AS
BEGIN
    UPDATE MOM_Department
    SET DepartmentName = @DepartmentName,
        Modified = GETDATE()
    WHERE DepartmentID = @DepartmentID
END
```

#### For edit specific department you have to bring that specific department's data to form and that's why you will need "SelectById" sp.

### PR_MOM_Department_SelectByID

```sql
CREATE PROCEDURE PR_MOM_Department_SelectByID
    @DepartmentID INT
AS
BEGIN
    SELECT 
        DepartmentID,
        DepartmentName,
        Created,
        Modified
    FROM 
        MOM_Department
    WHERE 
        DepartmentID = @DepartmentID;
END

```

---
## step 2: Change in controller method "AddEdit" to bring department into form

```csharp
public IActionResult AddEdit(int? id)
{
    

    if (id > 0)
    {
        DepartmentModel model = new DepartmentModel();
        string connectionString = configuration.GetConnectionString("ConnectionString");
        SqlConnection connection = new SqlConnection(connectionString);
        connection.Open();

        SqlCommand command = new SqlCommand("PR_MOM_Department_SelectByID", connection);
        command.CommandType = CommandType.StoredProcedure;
        command.Parameters.Add("@DepartmentID", SqlDbType.Int).Value = model.DepartmentID;

        SqlDataReader reader = command.ExecuteReader();

        if (reader.Read())
        {
            model.DepartmentID = Convert.ToInt32(reader["DepartmentID"]);
            model.DepartmentName = reader["DepartmentName"].ToString();
        }

        connection.Close();
        return View(model);
    }

    return View();
}
```
---

## step 3: Change in controller method "save" to perform update also.

```csharp
// [HttpPost]
// public IActionResult Save(DepartmentModel model)
// {
//     if (ModelState.IsValid)
//     {
//         string connectionString = configuration.GetConnectionString("ConnectionString");
//         SqlConnection sqlConnection = new SqlConnection(connectionString);
//         sqlConnection.Open();
//         SqlCommand command = sqlConnection.CreateCommand();
//         command.CommandType = CommandType.StoredProcedure;
            if (model.DepartmentID == 0)
            {
                // INSERT
                command.CommandText = "PR_MOM_Department_Insert";
                command.Parameters.Add("@DepartmentName", SqlDbType.VarChar).Value = model. DepartmentName;
            }
            else
            {
                // UPDATE
                command.CommandText = "PR_MOM_Department_Update";
                command.Parameters.Add("@DepartmentID", SqlDbType.Int).Value = model.DepartmentID;
                command.Parameters.Add("@DepartmentName", SqlDbType.VarChar).Value = model. DepartmentName;
            }
//         command.ExecuteNonQuery();
//         sqlConnection.Close();
//         return RedirectToAction("list");
//     }

//     return View("AddEdit", model);
// }
```

## step 4: Add hidden input in form

```csharp
@model DepartmentModel

<h2>Add / Edit Department</h2>

<form asp-action="Save" method="post">

    <!-- Hidden field is VERY IMPORTANT for Update -->
    <input type="hidden" asp-for="DepartmentID" />

    <div>
        <label>Department Name</label>
        <input asp-for="DepartmentName" class="form-control" />
    </div>

    <br />
    <button type="submit" class="btn btn-primary">Save</button>

</form>
```


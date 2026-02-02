# Delete operation

**prerequisite**:Ensure 'Delete' stored procedure is created in your database

```sql
CREATE OR ALTER PROC PR_MOM_Department_Delete
    @DepartmentID INT
AS
BEGIN
    DELETE FROM [dbo].[MOM_Department]
    WHERE DepartmentID = @DepartmentID
END
```

## step 1: call the store procedure 'PR_MOM_Department_Delete' in controller's Action method

Add the following code in your controller to handle the deletion of a department. This method connects to the database, executes the delete procedure, and then redirects to the product list page.

```csharp
public IActionResult DeleteDept(int DepartmentID)
        {
            string connectionString = this.configuration.GetConnectionString("ConnectionString");
            SqlConnection sqlConnection = new SqlConnection(connectionString);
            sqlConnection.Open();
            SqlCommand sqlCommand = sqlConnection.CreateCommand();
            sqlCommand.CommandType = CommandType.StoredProcedure;
            sqlCommand.CommandText = "PR_MOM_Department_Delete";
            sqlCommand.Parameters.Add("@DepartmentID", SqlDbType.Int).Value = DepartmentID;
            sqlCommand.ExecuteNonQuery();
            return RedirectToAction("DeptList");
        }
```

**To test if there is any error**:Use try catch and store error in TempData

```csharp
try{
    //code
}
catch (Exception ex)
    {
        TempData["ErrorMessage"] = ex.Message;
        Console.WriteLine(ex.ToString());
    }
```

## step-2: Add delete link on list page

In the list page add delete button that calls DeleteDept method action method.This wil pass the DepartmentID to the method to identify which department to delete

```html
<form method="post" asp-controller="Department" asp-action="DeleteDept" onsubmit="return confirmDelete()">
    <input type="hidden" name="DepartmentID" value="@item.DepartmentID" />
    <button style="width:100px" type="submit" class="btn btn-outline-danger btn-xs mt-1">
        <i class=" bi bi-trash3"> Delete</i>
    </button>
</form>
```

**note**:define script of confirmdelete function

```html
<script>
    function confirmDelete() {
        return confirm("Are you Sure! You want to Delete this record from department-table")
    }
</script>
```

### Another way

Add a route link with using anchor tag with href attribute that points to the DeleteDept action method.

```html
<a href="/Department/DeleteDept?DepartmentID="@item.DepartmentID" class="btn btn-outline-danger btn-xs">
  <i class="bi bi-x"></i>
</a>

```

### using asp-route-

```html
<a asp-controller="Department" asp-action="DeleteDept" asp-route-DepartmentID="@item.DepartmentID" class="btn btn-outline-danger btn-xs">
  <i class="bi bi-x"></i>
</a>
```

**note**:after asp-route- the parameter DepartmentID must match with controller's receiving parameter


## Step 3: Test the Delete Operation

Display Error Message in View Page
```csharp
<span class="text-danger">@TempData["ErrorMessage"]</span>
```
Ensure that the delete functionality works by testing it in the application. Check that the Department is removed from the database and that the list page updates accordingly.
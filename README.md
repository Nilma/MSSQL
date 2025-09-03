# MSSQL on MacBook

## 1. Install Docker Desktop

- Download and install Docker Desktop for macOS from the official Docker website.
- Ensure Docker Desktop is running after installation.

## 2. Download the SQL Server Docker Image

- Open the Terminal application on your Mac.
- Enter the following command to download the latest Microsoft SQL Server image:

```bash
docker pull mcr.microsoft.com/mssql/server:2022-latest
```

## 3. Run the SQL Server Container

- Execute the docker run command to start a new SQL Server container:

```bash
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=YourStrong@Passw0rd" \
  -p 1433:1433 --name sql2022 --hostname sql2022 \
  -d mcr.microsoft.com/mssql/server:2022-latest
```

- Replace `YourStrong@Passw0rd` with a strong password for the `sa` user.  
- In this tutorial, we use: `YourStrong@Passw0rd`

## 4. Connect with Azure Data Studio or VS Code (SQL Server Extension)

- Download and install **Azure Data Studio**, a cross-platform database tool.  
- Open Azure Data Studio and create a new connection.  
- Use `localhost` as the server name and `sa` as the username, then provide the password you set in the docker run command.

### Example connection via VS Code MSSQL extension

- **Server name**: `localhost, 1433`  
- **Trust server certificate**: ✔️  
- **Authentication type**: `SQL Login`  
- **User name**: `sa`  
- **Password**: `YourStrong@Passw0rd`  
- **Save Password**: ✔️  
- **Database name**: (leave blank)  
- **Encrypt**: `Mandatory`  

---

## 5. Create Demo Database for the App

After connecting, create a database for the demo app. Open a new query window and run:

```sql
IF DB_ID(N'DemoDb') IS NULL
BEGIN
  CREATE DATABASE DemoDb;
END
GO

USE DemoDb;
GO

IF OBJECT_ID(N'dbo.People', N'U') IS NULL
BEGIN
  CREATE TABLE dbo.People (
    Id INT IDENTITY PRIMARY KEY,
    Name NVARCHAR(50) NOT NULL,
    CreatedAt DATETIME2 NOT NULL DEFAULT SYSDATETIME()
  );
END
GO

INSERT INTO dbo.People (Name) VALUES (N'Ada'), (N'Grace'), (N'Linus');
GO

SELECT * FROM dbo.People;
```

## 6. Open a New Query Window in VS Code

Now that you are connected to SQL Server through the MSSQL extension, you can run SQL commands directly inside Visual Studio Code.

### Steps to open and run queries

1. **Create a `.sql` file**
   - Go to **File → New File** in VS Code.
   - Save it as `demo.sql` (any file with a `.sql` extension works).

2. **Link the file to your connection**
   - At the top-right of the editor window, you will see a connection selector (a dropdown).
   - Choose your saved connection: `localhost,1433 (SA)`.
   - If the dropdown is missing:
     - Press **F1** → type **MS SQL: Use Database Connection**.
     - Select your connection profile.

3. **Write SQL commands**
   - For example:
     ```sql
     SELECT @@VERSION;
     ```

4. **Run the query**
   - Highlight the SQL you want to run.
   - Press **Cmd+Shift+E** (on Mac) or **Ctrl+Shift+E** (on Windows/Linux).
   - Alternatively, right-click in the editor → **Execute Query**, or click the **▶ Run** button above the query.

5. **View results**
   - The results will appear in a panel at the bottom of VS Code.

---

### Example: Create and test the Demo Database

In your new `demo.sql` file, paste the following:

```sql
IF DB_ID(N'DemoDb') IS NULL
BEGIN
  CREATE DATABASE DemoDb;
END
GO

USE DemoDb;
GO

IF OBJECT_ID(N'dbo.People', N'U') IS NULL
BEGIN
  CREATE TABLE dbo.People (
    Id INT IDENTITY PRIMARY KEY,
    Name NVARCHAR(50) NOT NULL,
    CreatedAt DATETIME2 NOT NULL DEFAULT SYSDATETIME()
  );
END
GO

INSERT INTO dbo.People (Name) VALUES (N'Ada'), (N'Grace'), (N'Linus');
GO

SELECT * FROM dbo.People;



6.	**Run the script
	•	Execute the query (Cmd+Shift+E).
	•	You should see 3 rows returned: Ada, Grace, and Linus.

  © 2025 Nilma Abbas. All rights reserved.

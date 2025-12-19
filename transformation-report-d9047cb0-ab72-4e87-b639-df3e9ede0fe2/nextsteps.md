# Next Steps

## Overview

The transformation appears to be successful with no build errors reported across any of the projects in the solution. All five projects (Bookstore.Data, Bookstore.Domain.Tests, Bookstore.Cdk, Bookstore.Web, and Bookstore.Domain) have compiled without issues.

## Validation Steps

### 1. Verify Target Framework

Confirm that all projects are targeting the appropriate .NET version:

```bash
dotnet list package --framework
```

Review each `.csproj` file to ensure consistent `<TargetFramework>` values across the solution (e.g., `net6.0`, `net7.0`, or `net8.0`).

### 2. Restore and Rebuild

Perform a clean restore and rebuild to ensure all dependencies are correctly resolved:

```bash
dotnet clean
dotnet restore
dotnet build --configuration Release
```

### 3. Run Unit Tests

Execute the test suite to verify functionality:

```bash
dotnet test --configuration Release --verbosity normal
```

Review test results for any failures or warnings that may indicate runtime compatibility issues.

### 4. Check Package Compatibility

Verify that all NuGet packages are compatible with cross-platform .NET:

```bash
dotnet list package --outdated
dotnet list package --deprecated
```

Update any packages that have newer versions available or are marked as deprecated.

### 5. Review Platform-Specific Code

Search for potential platform-specific code patterns:

- Windows-specific APIs (e.g., Registry access, Windows-only file paths)
- P/Invoke declarations that may not be cross-platform
- File path separators (ensure use of `Path.Combine` instead of hardcoded `\` or `/`)

### 6. Test on Target Platforms

Run the application on each target platform:

**Linux:**
```bash
dotnet run --project app/Bookstore.Web/Bookstore.Web.csproj
```

**macOS:**
```bash
dotnet run --project app/Bookstore.Web/Bookstore.Web.csproj
```

**Windows:**
```bash
dotnet run --project app/Bookstore.Web/Bookstore.Web.csproj
```

### 7. Validate Database Connectivity

If Bookstore.Data uses Entity Framework or another ORM, verify database connections work across platforms:

- Test connection strings
- Run migrations: `dotnet ef database update`
- Verify data access operations

### 8. Review Configuration Files

Check that configuration files are properly loaded:

- `appsettings.json` and environment-specific variants
- Environment variables
- User secrets (if applicable)

### 9. Test CDK Deployment

Since the solution includes Bookstore.Cdk, validate the AWS CDK stack:

```bash
cd app/Bookstore.Cdk
dotnet build
cdk synth
```

Review the synthesized CloudFormation template for any issues.

### 10. Performance Testing

Conduct basic performance testing to identify any regressions:

- Measure application startup time
- Test response times for key endpoints
- Monitor memory usage

## Final Steps

### Update Documentation

- Update README files with new build and deployment instructions
- Document any breaking changes from the migration
- Update system requirements to reflect cross-platform support

### Code Review

Conduct a thorough code review focusing on:

- Removed or modified code during transformation
- New warnings introduced by the compiler
- Deprecated API usage

### Create Baseline

Establish a baseline for the migrated application:

- Tag the repository with a migration milestone
- Document the .NET version and SDK used
- Record all package versions for reproducibility

## Deployment Preparation

Once validation is complete:

1. Publish the application for each target platform:
   ```bash
   dotnet publish -c Release -r linux-x64 --self-contained false
   dotnet publish -c Release -r osx-x64 --self-contained false
   dotnet publish -c Release -r win-x64 --self-contained false
   ```

2. Test the published artifacts on their respective platforms

3. Deploy to your target environment using the AWS CDK stack:
   ```bash
   cd app/Bookstore.Cdk
   cdk deploy
   ```

4. Perform smoke testing in the deployed environment

5. Monitor application logs and metrics post-deployment
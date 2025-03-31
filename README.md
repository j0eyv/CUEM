# 1. Custom User Environment Manager

> [!Important]
> The tool is still in development in a private repository. Until (first) release this repository only holds a readme file.

This tool automates the deployment of configurations during logon actions on Windows machines. It connects to Microsoft Graph using Entra ID credentials and performs tasks such as drive mapping, printer mapping and registry key management based on group memberships.

> [!NOTE]
> The original concept for this tool was inspired by a former colleague. Full credit goes to him! After leaving that company, I found myself frequently missing this tool, so I decided to recreate a simplified version on my own.

# 2. Contents
- [1. Custom User Environment Manager](#1-custom-user-environment-manager)
- [2. Contents](#2-contents)
  - [2.1. Core Components](#21-core-components)
  - [2.2. Roadmap](#22-roadmap)
  - [2.3. Functions](#23-functions)
    - [2.3.1. Write-Log](#231-write-log)
    - [2.3.2. Deploy-DriveMappings](#232-deploy-drivemappings)
    - [2.3.3. Deploy-RegistryKeys](#233-deploy-registrykeys)
  - [2.4. Execution](#24-execution)
  - [2.5. Error Handling](#25-error-handling)
  - [2.6. Dependencies](#26-dependencies)
  - [2.7. Installation](#27-installation)
  - [2.8. Contributing](#28-contributing)



## 2.1. Core Components

- **Microsoft Graph Authentication**: The script uses the `Microsoft.Graph` modules to interact with Microsoft Graph APIs.
- **Configuration File**: Reads settings from `Config.xml` located at `C:\ProgramData\CUEM\Config.xml`.
- **Authentication**: Connects to Microsoft Graph using `TenantId`, `AppId`, and `AppSecret` from the configuration file.

## 2.2. Roadmap
- Printer mapping
- Executions (Start-Process)

## 2.3. Functions

### 2.3.1. Write-Log

- Logs messages to a user-specific log file located at `C:\ProgramData\CUEM\Logging\<username>\User.log`.
- Ensures the log directory exists and appends timestamped log entries.

### 2.3.2. Deploy-DriveMappings

- Reads drive mapping configurations from `Config.xml`.
- Retrieves the current user's group memberships using Microsoft Graph.
- Maps or removes network drives based on the user's group memberships and the configuration.
- Logs actions such as adding, removing, or skipping drive mappings.
- Priority handling for situations with conflicting actions (e.g. same drive letters)

### 2.3.3. Deploy-RegistryKeys

- Reads registry key configurations from `Config.xml`.
- Retrieves the current user's group memberships using Microsoft Graph.
- Adds, updates, or removes registry keys based on the user's group memberships and the configuration.
- Logs actions such as creating, setting, or removing registry keys.
- Priority handling for situations with conflicting actions (e.g. same registry key or values)

## 2.4. Execution

The script executes the following tasks sequentially:

1. **Drive Mapping**: Calls `Deploy-DriveMappings` to manage network drives.
2. **Registry Key Management**: Calls `Deploy-RegistryKeys` to manage registry keys.
3. **Process execution**: Calls `Deploy-ProcessExecution` to manage process executions.
4. **Printer mapping**: Calls `Deploy-PrinterMappings` to manage printer mappings.

## 2.5. Error Handling

The script includes error handling for:

- Missing or invalid configuration files.
- Failures in Microsoft Graph API calls.
- Issues with drive mappings or registry key operations.

Errors are logged using the `Write-Log` function.

## 2.6. Dependencies

Requires the following PowerShell modules:

- `Microsoft.Graph.Authentication`
- `Microsoft.Graph.Groups`
- `Microsoft.Graph.Users`

> [!NOTE]
> Installation of these modules is being handled by Cuem-core.ps1 or either an installation method (e.g. MSI). This has yet to be defined.

## 2.7. Installation
To be defined

## 2.8. Contributing
Contributions are welcome! Please open an issue or reach out to me to discuss your ideas.

# 1. Custom User Environment Manager

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
    - [2.3.4. Deploy-Executables](#234-deploy-executables)
    - [2.3.5. Deploy-FileActions](#235-deploy-fileactions)
    - [2.3.6. Deploy-Printers](#236-deploy-printers)
  - [2.4. Execution](#24-execution)
  - [2.5. Dependencies](#25-dependencies)
  - [2.6. Installation](#26-installation)
  - [2.7. Contributing](#27-contributing)



## 2.1. Core Components

- **Microsoft Graph Authentication**: The script uses the `Microsoft.Graph` modules to interact with Microsoft Graph APIs.
- **Configuration File**: Reads settings from `Config.xml` located at `C:\ProgramData\CUEM\Config.xml`.
- **Authentication**: Connects to Microsoft Graph using `TenantId`, `AppId`, and `AppSecret` from the configuration file.

## 2.2. Roadmap
- TBA

## 2.3. Functions

### 2.3.1. Write-Log

- **Purpose:** Logs messages to a user-specific log file located at `C:\ProgramData\CUEM\Logging\<username>\User.log`.

- **Key Features:** Ensures the log directory exists and appends timestamped log entries.


### 2.3.2. Deploy-DriveMappings

**Purpose**: Maps or removes network drives based on configuration.

- **Key Features:**
  - Reads drive mapping configurations from `Config.xml`.
  - Checks user group memberships to determine eligibility.
  - Supports adding and removing drive mappings.
  - Logs success or failure of each operation.
  - Priority handling for situations with conflicting actions (e.g. same drive letters).

### 2.3.3. Deploy-RegistryKeys

**Purpose:**  Adds or removes registry keys based on configuration.

- **Key Features:**
  - Reads registry configurations from `Config.xml`.
  - Checks user group memberships to determine eligibility.
  - Ensures registry keys exist before setting values.
  - Supports adding and removing registry keys.
  - Logs success or failure of each operation.

### 2.3.4. Deploy-Executables

**Purpose:** Starts executables with optional arguments based on configuration.

- **Key Features:**
  - Reads executable configurations from `Config.xml`.
  - Checks user group memberships to determine eligibility.
  - Supports starting executables with or without arguments.
  - Logs success or failure of each operation.


### 2.3.5. Deploy-FileActions

**Purpose:** Performs file operations (copy, delete, rename, move) based on configuration.

- **Key Features:**
  - Reads file action configurations from `Config.xml`.
  - Checks user group memberships to determine eligibility.
  - Supports the following actions:
    - Copy: Copies files to a destination.
    - Delete: Deletes files.
    - Rename: Renames files.
    - Move: Moves files to a new location. 
  - Logs success or failure of each operation.

### 2.3.6. Deploy-Printers

**Purpose:** Adds or removes printers based on configuration.

- **Key Features:**
  - Reads printer configurations from `Config.xml`.
  - Checks user group memberships to determine eligibility.
  - Supports adding and removing printers.
  - Logs success or failure of each operation.


## 2.4. Execution

The script executes the following tasks sequentially:

1. **Drive Mapping**: Calls `Deploy-DriveMappings` to manage network drives.
2. **Registry Key Management**: Calls `Deploy-RegistryKeys` to manage registry keys.
3. **Process execution**: Calls `Deploy-ProcessExecution` to manage process executions.
4. **File Actions**: Calls `Deploy-FileActions` to manage file actions 
5. **Printer mapping**: Calls `Deploy-PrinterMappings` to manage printer mappings.


## 2.5. Dependencies

Requires the following PowerShell modules:

- `Microsoft.Graph.Authentication`
- `Microsoft.Graph.Groups`
- `Microsoft.Graph.Users`

> [!NOTE]
> Installation of these modules is being handled by Cuem-core.ps1 or either an installation method (e.g. MSI). This has yet to be defined.

## 2.6. Installation
To be defined

## 2.7. Contributing
Contributions are welcome! Please open an issue or reach out to me to discuss your ideas.
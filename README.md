# WorkdayApi PowerShell Module

A PowerShell module that provides simple, PowerShell-friendly methods for accessing the Workday SOAP API.

## Description

This PowerShell module simplifies interaction with the Workday SOAP API by providing cmdlets that handle authentication, endpoint management, and common Workday operations. It's designed to make Workday automation tasks straightforward and accessible from the command line.

## Features

* **Easy Configuration**: Set default configuration options and securely save them to the current user's profile
* **Worker Management**: 
  * Get Worker information for one or all workers
  * Get/Set/Update Worker email addresses
  * Get/Set/Update Worker phone numbers
  * Get/Set Worker photos
  * Get/Set Worker documents
  * Get/Set/Update/Remove Worker Other IDs
  * Get Worker National IDs
  * Set Worker User IDs
* **Data Retrieval**:
  * Convert Worker XML to PowerShell objects
  * Get Workday reports
  * Get Workday date/time
  * Worker ID lookup tables
  * Convert Worker data to AD format
* **Integration Management**:
  * Start Workday integrations
  * Retrieve integration event status
* **Flexible API Access**:
  * Submit arbitrary SOAP API calls
  * Support for multiple endpoints (Human Resources, Staffing, Integrations)

## Requirements

* PowerShell 5.1 or later (Desktop or Core editions)
* Access to Workday SOAP API endpoints
* Valid Workday API credentials

#### Manage Worker Phone Numbers

```powershell
# Set a worker's phone number
Set-WorkdayWorkerPhone -WorkerId 123 -WorkerType Employee_ID -Number '+1 (234) 987-6543' -UsageType Work -DeviceType Landline -Primary

# Get worker's phone numbers
Get-WorkdayWorkerPhone -WorkerId 123 -WorkerType Employee_ID | Format-Table

# Update phone number only if different
Update-WorkdayWorkerPhone -WorkerId 123 -WorkerType Employee_ID -Number '+1 (234) 987-6543'
```

#### Manage Worker Email Addresses

```powershell
# Set a worker's email
Set-WorkdayWorkerEmail -WorkerId 123 -WorkerType Employee_ID -Email 'worker@example.com' -Primary

# Get worker's email addresses
Get-WorkdayWorkerEmail -WorkerId 123 -WorkerType Employee_ID

# Update email only if different
Update-WorkdayWorkerEmail -WorkerId 123 -WorkerType Employee_ID -Email 'newemail@example.com'
```

#### Worker Photos

```powershell
# Upload a worker photo
Set-WorkdayWorkerPhoto -WorkerId 123 -WorkerType Employee_ID -FilePath 'C:\Photos\worker123.jpg'

# Get a worker's photo (returns Base64 encoded)
Get-WorkdayWorkerPhoto -WorkerId 123 -WorkerType Employee_ID
```

#### Custom API Requests

```powershell
# Send a custom SOAP request
$response = Invoke-WorkdayRequest -Request '<bsvc:Server_Timestamp_Get xmlns:bsvc="urn:com.workday/bsvc" />' -Uri 'https://SERVICE.workday.com/ccx/service/TENANT/Human_Resources/v26.0'
$response.Xml.Server_TimeStamp
```

#### Integration Management

```powershell
# Start a Workday integration
Start-WorkdayIntegration -IntegrationName 'INT011' -TenantId 'TENANT'

# Get integration event status
Get-WorkdayIntegrationEvent -IntegrationSystemId '12345'
```

## Available Commands

### Configuration Commands
* `Get-WorkdayEndpoint` - Gets the default URI value for all or a particular endpoint
* `Set-WorkdayEndpoint` - Sets the default URI value for a particular endpoint
* `Set-WorkdayCredential` - Sets the default Workday API credentials
* `Save-WorkdayConfiguration` - Saves default Workday configuration to a file in the current user's profile
* `Remove-WorkdayConfiguration` - Removes Workday configuration file from the current user's profile

### Worker Commands
* `Get-WorkdayWorker` - Gets Worker information as Workday XML
* `ConvertFrom-WorkdayWorkerXml` - Converts Workday Worker XML to PowerShell objects
* `Get-WorkdayWorkerByIdLookupTable` - Returns a hashtable of Worker Type and IDs, indexed by ID
* `Get-WorkdayToAdData` - Converts Get-WorkdayWorker output into AD format

### Worker Email Commands
* `Get-WorkdayWorkerEmail` - Returns a Worker's email addresses
* `Set-WorkdayWorkerEmail` - Sets a Worker's email in Workday
* `Update-WorkdayWorkerEmail` - Updates a Worker's email in Workday, only if it is different

### Worker Phone Commands
* `Get-WorkdayWorkerPhone` - Returns a Worker's phone numbers
* `Set-WorkdayWorkerPhone` - Sets a Worker's phone number in Workday
* `Update-WorkdayWorkerPhone` - Updates a Worker's phone number in Workday, only if it is different

### Worker ID Commands
* `Get-WorkdayWorkerNationalId` - Gets Worker National IDs
* `Get-WorkdayWorkerOtherId` - Gets Worker Other IDs
* `Set-WorkdayWorkerOtherId` - Sets a Worker's Other ID in Workday
* `Update-WorkdayWorkerOtherId` - Updates a Worker's Other ID in Workday, only if it is different
* `Remove-WorkdayWorkerOtherId` - Removes a Worker's Other ID from Workday
* `Set-WorkdayWorkerUserId` - Sets a Worker's User ID (username) in Workday

### Worker Document Commands
* `Get-WorkdayWorkerDocument` - Gets Workday Worker Documents
* `Set-WorkdayWorkerDocument` - Uploads a document to a Worker's records in Workday

### Worker Photo Commands
* `Get-WorkdayWorkerPhoto` - Returns a worker's photo (Base64 encoded)
* `Set-WorkdayWorkerPhoto` - Uploads an image file to Workday and sets it as a Worker's photo

### Integration Commands
* `Start-WorkdayIntegration` - Starts a Workday Integration
* `Get-WorkdayIntegrationEvent` - Retrieves the status of a Workday Integration

### Utility Commands
* `Get-WorkdayDate` - Gets the current time and date from Workday
* `Get-WorkdayReport` - Returns the XML result from any Workday report, based on its URI
* `Invoke-WorkdayRequest` - Sends XML requests to Workday API, with proper authentication and receives XML response

## Sample Scripts

The module includes several sample scripts in the `source/samples` directory:

* `Sync_AD_to_Workday.ps1` - Synchronize Active Directory changes to Workday
* `Sync_Workday_to_AD.ps1` - Synchronize Workday data to Active Directory
* `Update_Email_By_WorkerID.ps1` - Update email addresses from a CSV file
* `Update-WorkdayWorkerPhotosSince.ps1` - Update worker photos since a specific date

## Documentation

For detailed help on any command, use:

```powershell
Get-Help <CommandName> -Full
```

For example:

```powershell
Get-Help Get-WorkdayWorker -Full
```

## Version

Current Version: **2.3.3**

See [CHANGELOG.md](CHANGELOG.md) for version history and release notes.

## Contributing

Contributions are welcome! Please feel free to submit issues, fork the repository, and create pull requests.

## Disclaimer

This module is provided as-is. Please use with caution and test thoroughly in your environment. This module could potentially cause unintended changes to your Workday data if used incorrectly. Always test in a development or sandbox environment first.

Any and all contributions are more than welcome and appreciated.

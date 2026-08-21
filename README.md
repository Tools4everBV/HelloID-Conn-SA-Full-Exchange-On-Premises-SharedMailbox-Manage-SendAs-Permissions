# HelloID-Conn-SA-Full-Exchange-On-Premises-SharedMailbox-Manage-SendAs-Permissions

| :warning: Important                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Best Practice:** Use **HelloID Products** for requesting and managing permissions (group memberships, mailbox access, application roles). Products provide governance, approval workflows, admin visibility, and full lifecycle management.<br>Use delegated forms for one-time operational actions (creating resources like shared mailboxes, password resets, attribute updates) only.<br><br>**[Read more: Products vs. Delegated Forms](https://docs.helloid.com/en/service-automation/products-vs--delegated-forms.html)** |

| :information_source: Information                                                                                                                                                                                                                                                                                                                                                          |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| This repository contains the connector and configuration code only. The implementer is responsible for acquiring the connection details such as username, password, certificate, etc. You might even need to sign a contract or agreement with the supplier before implementing this connector. Please contact the client's application manager to coordinate the connector requirements. |

## Description

_HelloID-Conn-SA-Full-Exchange-On-Premises-SharedMailbox-Manage-SendAs-Permissions_ is a template designed for use with HelloID Service Automation (SA) Delegated Forms. It can be imported into HelloID and customized according to your requirements.

This HelloID Service Automation Delegated Form provides Exchange On-Premises shared mailbox Send As permissions management functionality. The following workflow is supported:

1.  Search and select the shared mailbox to manage
2.  View current users with Send As permissions on the selected mailbox
3.  Select users to add or remove from Send As permissions
4.  Grant or revoke Send As permissions on the shared mailbox
5.  Confirm the changes and receive audit logging

## Getting started

### Requirements

- **Exchange On-Premises Environment**:<br>
  Access to an Exchange On-Premises server with Remote PowerShell enabled. The connector uses Exchange Management Shell commands via PowerShell remoting.
- **Service Account**:<br>
  A service account with sufficient permissions to manage mailbox permissions in Exchange. The account must have rights to use `Get-Mailbox`, `Add-ADPermission`, `Remove-ADPermission`, and `Get-ADPermission` cmdlets.
- **Network Connectivity**:<br>
  The HelloID agent or runner must have network access to the Exchange On-Premises server on the configured PowerShell endpoint (typically HTTP or HTTPS).

### Connection settings

The following user-defined variables are used by the connector.

| Setting               | Description                                                                                  | Mandatory |
| --------------------- | -------------------------------------------------------------------------------------------- | --------- |
| ExchangeConnectionUri | The URI to the Exchange PowerShell endpoint (e.g., http://exchangeserver/powershell)         | Yes       |
| ExchangeAdminUsername | The username of the service account with Exchange management permissions (e.g., domain\user) | Yes       |
| ExchangeAdminPassword | The password for the Exchange admin service account (stored as secret)                       | Yes       |

## Remarks

### Session Management and Cleanup

The connector uses PowerShell remoting sessions to connect to Exchange On-Premises. All datasources and tasks include `finally` blocks to ensure proper session cleanup, preventing session leaks and ensuring optimal performance. Sessions are automatically disconnected after each operation completes.

### Error Handling Pattern

All scripts implement a consistent error handling pattern using the `$actionMessage` variable. This provides detailed context about which operation failed, making troubleshooting easier. Error messages include line numbers and full exception details for comprehensive debugging.

### Optimized Command Imports

Instead of importing all Exchange cmdlets, the connector imports only the specific commands needed for each operation. This reduces memory usage and improves performance, especially important when running on HelloID agents with limited resources.

### Filter Capabilities

The shared mailbox search datasource supports wildcard filtering on multiple attributes (Name, SamAccountName, Alias, PrimarySmtpAddress). Use `*` as the search value to retrieve all shared mailboxes. This flexibility allows users to find mailboxes using various identifiers.

### Audit Logging

The task uses specific audit action types (`GrantMembership` and `RevokeMembership`) instead of generic actions, providing better visibility in HelloID audit logs and making it easier to track permission changes over time.

## Development resources

### Datasources

The following PowerShell datasources are used by this connector:

| Datasource Name                                            | Description                                                                                                                                                                 |
| ---------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Exchange-On-Premises-Get-Sharedmailbox-Wildcard-Name-Alias | Searches for shared mailboxes based on wildcard filter. Supports filtering by Name, SamAccountName, Alias, or PrimarySmtpAddress. Use `*` to retrieve all shared mailboxes. |
| Exchange-On-Premises-Get-Current-Send-As-Users             | Retrieves all users who currently have Send As permissions on the selected shared mailbox. Filters out system accounts (NT AUTHORITY).                                      |
| Exchange-On-Premises-Get-All-Users                         | Retrieves all user mailboxes that can be granted Send As permissions. Returns all UserMailbox recipients sorted by DisplayName.                                             |

### Tasks

| Task Name                                                        | Description                                                                                                                                                             |
| ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Exchange On-Premises - Sharedmailbox - Manage sendas permissions | Main task that grants or revokes Send As permissions on a shared mailbox. Processes both additions and removals in a single execution with comprehensive audit logging. |

### API endpoints

The following Exchange PowerShell cmdlets are used by the connector:

| Cmdlet              | Description                                                           |
| ------------------- | --------------------------------------------------------------------- |
| Get-Mailbox         | Retrieves mailbox information for shared mailboxes and user mailboxes |
| Add-ADPermission    | Grants Send As permissions to users on shared mailboxes               |
| Remove-ADPermission | Revokes Send As permissions from users on shared mailboxes            |
| Get-ADPermission    | Retrieves current permissions on a mailbox                            |
| Get-Recipient       | Retrieves recipient information for permission holders                |

### API documentation

- [Exchange PowerShell Documentation](https://learn.microsoft.com/en-us/powershell/exchange/)
- [Connect to Exchange Servers using Remote PowerShell](https://learn.microsoft.com/en-us/powershell/exchange/connect-to-exchange-servers-using-remote-powershell)
- [Get-Mailbox Cmdlet](https://learn.microsoft.com/en-us/powershell/module/exchange/get-mailbox)
- [Add-ADPermission Cmdlet](https://learn.microsoft.com/en-us/powershell/module/exchange/add-adpermission)
- [Remove-ADPermission Cmdlet](https://learn.microsoft.com/en-us/powershell/module/exchange/remove-adpermission)

## Getting help

> :bulb: **Tip:**  
> _For more information on Delegated Forms, please refer to our [documentation](https://docs.helloid.com/en/service-automation/delegated-forms.html) pages_.

## HelloID docs

The official HelloID documentation can be found at: https://docs.helloid.com/

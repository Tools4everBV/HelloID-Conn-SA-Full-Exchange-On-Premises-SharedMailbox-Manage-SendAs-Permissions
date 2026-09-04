# Changelog

All notable changes to this project will be documented in this file. The format is based on [Keep a Changelog](https://keepachangelog.com/), and this project adheres to [Semantic Versioning](https://semver.org/).

## [2.0.0] - 2026-08-21

### Changed

- Renamed all datasources with descriptive prefixed naming convention for better clarity
  - `Exchange-sharedmailbox-generate-table-wildcard-sendas` → `Exchange-On-Premises-Get-Sharedmailbox-Wildcard-Name-Alias`
  - `Exchange-sharedmailbox-generate-table-manage-permissions-sendas` → `Exchange-On-Premises-Get-Current-Send-As-Users`
  - `Exchange-user-generate-table-sharedmailbox-manage-memberships` → `Exchange-On-Premises-Get-All-Users`
- Renamed task from `Exchange on-premise - Manage send as permissions shared mailbox` to `Exchange On-Premises - Sharedmailbox - Manage sendas permissions`
- Refactored all scripts to use PowerShell splatting for better code readability and maintainability
- Improved error handling with consistent `$actionMessage` variable pattern across all scripts
- Enhanced Exchange connection handling with explicit `Authentication = 'Default'` parameter
- Updated session options with explicit values: `SkipCACheck`, `SkipCNCheck`, and `SkipRevocationCheck` all set to `$false`
- Optimized command imports to only import required Exchange cmdlets instead of all available commands
- Changed audit log action types from generic `UpdateResource` to specific `GrantMembership` and `RevokeMembership` for better tracking
- Improved filter capabilities in shared mailbox search to support multiple attributes (Name, SamAccountName, Alias, PrimarySmtpAddress)
- Changed terminology in task from "assign/remove" to "grant/revoke" for Send As permissions
- Updated category name from "Exchange On-Premise" to "Exchange On-Premises" (corrected spelling)
- Set `ExchangeAdminPassword` global variable as secret for improved security
- Converted GitHub callout syntax to table format for better compatibility with text editors

### Added

- Added `finally` blocks to all datasources and tasks for proper Exchange session cleanup
- Added comprehensive inline comments with Microsoft documentation references
- Added property selection in datasources to limit memory usage (`$propertiesToSelect`)
- Added support for wildcard `*` search to retrieve all shared mailboxes
- Added `$commands` array to specify exactly which Exchange cmdlets to import
- Added structured error messages with line numbers and full exception details
- Added connection and disconnection audit logging for better visibility

### Fixed

- Fixed potential session leak issues by ensuring Exchange sessions are always cleaned up in `finally` blocks
- Fixed inconsistent error logging by standardizing `$warningMessage` and `$auditMessage` variables

### Removed

- Removed unused `ADsharedMailboxSearchOU` global variable from all-in-one setup script

## [1.0.2] - 2022-08-24

### Added

- Added version number and updated code for SA-agent and auditlogging

## [1.0.1] - 2021-11-16

### Added

- Added version number and updated all-in-one script

## [1.0.0] - 2021-04-29

Initial release of HelloID-Conn-SA-Full-Exchange-On-Premises-SharedMailbox-Manage-SendAs-Permissions.

### Added

- Initial release for managing Send As permissions on Exchange On-Premises shared mailboxes
- PowerShell datasource to search shared mailboxes by alias or name
- PowerShell datasource to retrieve current users with Send As permissions
- PowerShell datasource to list available mail users
- Delegated form task to add or remove Send As permissions

### Changed

### Deprecated

### Removed

### Fixed

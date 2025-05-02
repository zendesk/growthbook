# Magic Thinking: admin.ts

# File Analysis: `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/controllers/admin.ts`

## 1. File Purpose

This file contains controller functions for admin-related operations in the GrowthBook application. It provides API endpoints for super administrators to manage organizations and users across the entire system. The controllers handle operations such as retrieving all organizations, updating organization details, enabling/disabling organizations, and managing user information. These functions are critical for platform administration and typically would only be accessible to users with superAdmin privileges.

## 2. Function Documentation

### `getOrganizations`
```typescript
/**
 * Retrieves all organizations in the system with pagination and search capabilities.
 * Only accessible to superAdmins.
 * 
 * @param req - AuthRequest object containing query parameters for pagination and search
 * @param req.query.page - Optional page number for pagination (defaults to 1 if not provided)
 * @param req.query.search - Optional search term to filter organizations
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} - JSON response with organizations, SSO connections, datasources and total count
 * @throws {403} - If the user is not a superAdmin
 * 
 * @calls
 * - /back-end/src/models/OrganizationModel.findAllOrganizations
 * - /back-end/src/models/SSOConnectionModel.getAllSSOConnections
 * - /back-end/src/models/DataSourceModel._dangerourslyGetAllDatasourcesByOrganizations
 */
```

### `putOrganization`
```typescript
/**
 * Updates an organization's details based on provided parameters.
 * Only accessible to superAdmins.
 * 
 * @param req - AuthRequest object containing organization update details
 * @param req.body.orgId - ID of the organization to update
 * @param req.body.name - New name for the organization
 * @param req.body.externalId - New external ID for the organization
 * @param req.body.licenseKey - New license key for the organization
 * @param req.body.ownerEmail - New owner email for the organization
 * @param req.body.verifiedDomain - New verified domain for the organization
 * @param req.body.autoApproveMembers - Whether to auto-approve new members
 * @param req.body.enterprise - Whether this is an enterprise organization
 * @param req.body.freeSeats - Number of free seats for the organization
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} - JSON response confirming success
 * @throws {403} - If the user is not a superAdmin
 * @throws {404} - If the organization is not found
 * 
 * @calls
 * - /back-end/src/services/organizations.getOrganizationById
 * - /back-end/src/models/OrganizationModel.updateOrganization
 * - /back-end/src/routers/organizations/organizations.controller.setLicenseKey
 * - /back-end/src/services/audit.auditDetailsUpdate
 */
```

### `disableOrganization`
```typescript
/**
 * Disables an organization by setting its 'disabled' field to true.
 * Only accessible to superAdmins.
 * 
 * @param req - AuthRequest object
 * @param req.body.orgId - ID of the organization to disable
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} - JSON response confirming success
 * @throws {403} - If the user is not a superAdmin
 * @throws {404} - If the organization is not found
 * 
 * @calls
 * - /back-end/src/services/organizations.getOrganizationById
 * - /back-end/src/models/OrganizationModel.updateOrganization
 * - /back-end/src/services/audit.auditDetailsUpdate
 */
```

### `enableOrganization`
```typescript
/**
 * Enables a previously disabled organization by setting its 'disabled' field to false.
 * Only accessible to superAdmins.
 * 
 * @param req - AuthRequest object
 * @param req.body.orgId - ID of the organization to enable
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} - JSON response confirming success
 * @throws {403} - If the user is not a superAdmin
 * @throws {404} - If the organization is not found
 * 
 * @calls
 * - /back-end/src/services/organizations.getOrganizationById
 * - /back-end/src/models/OrganizationModel.updateOrganization
 * - /back-end/src/services/audit.auditDetailsUpdate
 */
```

### `getMembers`
```typescript
/**
 * Retrieves all users/members in the system with pagination and search capabilities.
 * Only accessible to superAdmins.
 * Also provides organization information for each user.
 * 
 * @param req - AuthRequest object containing query parameters
 * @param req.query.page - Optional page number for pagination (defaults to 1)
 * @param req.query.search - Optional search term to filter members
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} - JSON response with members, total count, and organization info
 * @throws {403} - If the user is not a superAdmin
 * 
 * @calls
 * - /back-end/src/models/UserModel.getAllUsersFiltered
 * - /back-end/src/models/UserModel.getTotalNumUsers
 * - /back-end/src/models/OrganizationModel.findOrganizationsByMemberIds
 */
```

### `getOrganizationMembers`
```typescript
/**
 * Retrieves all members of a specific organization.
 * Only accessible to superAdmins.
 * 
 * @param req - AuthRequest object
 * @param req.params.orgId - ID of the organization to get members for
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} - JSON response with members of the organization
 * @throws {403} - If the user is not a superAdmin
 * @throws {404} - If the organization is not found
 * 
 * @calls
 * - /back-end/src/services/organizations.getOrganizationById
 * - /back-end/src/models/UserModel.getUsersByIds
 */
```

### `putMember`
```typescript
/**
 * Updates a user/member's details.
 * Only accessible to superAdmins.
 * 
 * @param req - AuthRequest object
 * @param req.body.userId - ID of the user to update
 * @param req.body.name - New name for the user
 * @param req.body.email - New email for the user
 * @param req.body.verified - New verification status for the user
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} - JSON response confirming success
 * @throws {403} - If the user is not a superAdmin
 * @throws {404} - If the member is not found
 * 
 * @calls
 * - /back-end/src/models/UserModel.getUserById
 * - /back-end/src/models/UserModel.updateUser
 * - /back-end/src/services/audit.auditDetailsUpdate
 */
```
# Magic Thinking: auth.ts

# File Analysis: `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/controllers/auth.ts`

## 1. File Purpose

This file contains controller functions that handle authentication-related operations in the GrowthBook application. It manages user authentication flows including login, registration, password management, SSO (Single Sign-On) integration, and session management. The controllers in this file serve as an interface between the authentication service layer and the API endpoints, handling various authentication scenarios such as local authentication, OAuth, and SSO.

## 2. Function Documentation

### `getHasOrganizations`
```typescript
/**
 * Checks if any organizations exist in the system.
 * Used to determine if this is a fresh installation or an existing one.
 * 
 * @param req - Express Request object
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} JSON response with a boolean indicating if organizations exist
 * 
 * @calls
 * - /back-end/src/models/OrganizationModel.hasOrganization
 */
```

### `postRefresh`
```typescript
/**
 * Attempts to refresh the user's authentication session.
 * First tries to use an existing idToken from cookies, then falls back to using a refresh token.
 * 
 * @param req - Express Request object
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} JSON response with a new idToken if successful
 * 
 * @calls
 * - Auth connection's refresh method
 * - Auth connection's getUnauthenticatedResponse method
 */
```

### `postOAuthCallback`
```typescript
/**
 * Processes OAuth callback after a user authenticates with an OAuth provider.
 * Sets authentication cookies upon successful authentication.
 * 
 * @param req - Express Request object
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} JSON response with status 200 on success or 400 on failure
 * @throws Error if authentication fails
 * 
 * @calls
 * - Auth connection's processCallback method
 */
```

### `sendLocalSuccessResponse`
```typescript
/**
 * Helper function to send a successful authentication response for local authentication.
 * Sets authentication cookies and returns a token to the client.
 * 
 * @param req - Express Request object
 * @param res - Express Response object
 * @param user - User object that was authenticated
 * @param projectId - Optional project ID to redirect to after authentication
 * 
 * @returns {Promise<void>} Sends JSON response directly
 * @throws Error if unable to create ID token
 * 
 * @calls
 * - Auth connection's processCallback method
 */
```

### `postLogout`
```typescript
/**
 * Handles user logout by clearing authentication cookies and performing any provider-specific logout.
 * 
 * @param req - Express Request object
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} JSON response with status 200 and optional redirect URI
 * 
 * @calls
 * - Auth connection's logout method
 * - /back-end/src/services/auth.deleteAuthCookies
 */
```

### `postLogin`
```typescript
/**
 * Handles local user login with email and password.
 * Verifies credentials and sets authentication tokens on success.
 * 
 * @param req - Express Request object with email and password in the body
 * @param req.body.email - User's email address
 * @param req.body.password - User's password
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} JSON response with authentication token on success or error message on failure
 * @throws Error for invalid email/password format
 * 
 * @calls
 * - /back-end/src/services/auth.validatePasswordFormat
 * - /back-end/src/models/UserModel.getUserByEmail
 * - /back-end/src/services/users.verifyPassword
 * - sendLocalSuccessResponse (local helper function)
 */
```

### `postRegister`
```typescript
/**
 * Handles user registration with email, name, and password.
 * Creates a new user account or logs in to an existing one if email already exists.
 * 
 * @param req - Express Request object with registration details in the body
 * @param req.body.email - User's email address
 * @param req.body.name - User's name
 * @param req.body.password - User's password
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} JSON response with authentication token on success or error message on failure
 * @throws Error for invalid arguments or password format
 * 
 * @calls
 * - /back-end/src/services/auth.validatePasswordFormat
 * - /back-end/src/models/UserModel.getUserByEmail
 * - /back-end/src/services/users.verifyPassword
 * - /back-end/src/models/UserModel.createUser
 * - sendLocalSuccessResponse (local helper function)
 */
```

### `postFirstTimeRegister`
```typescript
/**
 * Special registration endpoint for the first user in a new GrowthBook installation.
 * Creates user, organization, and initial project. Grants the user superAdmin privileges.
 * 
 * @param req - Express Request object with registration details in the body
 * @param req.body.email - Admin user's email address
 * @param req.body.name - Admin user's name
 * @param req.body.password - Admin user's password
 * @param req.body.companyname - Organization name
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} JSON response with authentication token and project ID on success
 * @throws Error if installation is not new or inputs are invalid
 * 
 * @calls
 * - /back-end/src/services/auth.isNewInstallation
 * - /back-end/src/services/auth.validatePasswordFormat
 * - /back-end/src/models/UserModel.getUserByEmail
 * - /back-end/src/models/UserModel.createUser
 * - /back-end/src/models/OrganizationModel.createOrganization
 * - /back-end/src/services/organizations.getContextForAgendaJobByOrgObject
 * - sendLocalSuccessResponse (local helper function)
 */
```

### `postForgotPassword`
```typescript
/**
 * Initiates password reset process by generating a reset token for the provided email.
 * 
 * @param req - Express Request object with email in the body
 * @param req.body.email - Email address for account to reset password
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} JSON response with status 200 on success
 * @throws Error if email is invalid
 * 
 * @calls
 * - /back-end/src/models/ForgotPasswordModel.createForgotPasswordToken
 */
```

### `getResetPassword`
```typescript
/**
 * Validates a password reset token and returns the associated email address.
 * 
 * @param req - Express Request object with token in the route params
 * @param req.params.token - Password reset token
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} JSON response with email on success
 * @throws Error if token is invalid or user not found
 * 
 * @calls
 * - /back-end/src/models/ForgotPasswordModel.getUserIdFromForgotPasswordToken
 * - /back-end/src/models/UserModel.getEmailFromUserId
 */
```

### `getSSOConnectionFromDomain`
```typescript
/**
 * Gets SSO connection details based on an email domain.
 * Sets an SSO connection ID cookie if found.
 * 
 * @param req - Express Request object with domain in the body
 * @param req.body.domain - Email domain to look up
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} JSON response with status 200 on success
 * @throws Error if no SSO connection found for the domain
 * 
 * @calls
 * - /back-end/src/models/SSOConnectionModel.getSSOConnectionByEmailDomain
 */
```

### `postResetPassword`
```typescript
/**
 * Completes the password reset process by updating the user's password.
 * Invalidates all existing sessions for the user.
 * 
 * @param req - Express Request object with token in route params and new password in body
 * @param req.params.token - Password reset token
 * @param req.body.password - New password
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} JSON response with email on success
 * @throws Error if token is invalid, password format is invalid, or user not found
 * 
 * @calls
 * - /back-end/src/models/ForgotPasswordModel.getUserIdFromForgotPasswordToken
 * - /back-end/src/models/UserModel.getEmailFromUserId
 * - /back-end/src/services/users.updatePassword
 * - /back-end/src/models/ForgotPasswordModel.deleteForgotPasswordToken
 * - /back-end/src/models/UserModel.resetMinTokenDate
 * - /back-end/src/models/AuthRefreshModel.deleteMany
 */
```

### `postChangePassword`
```typescript
/**
 * Changes password for an authenticated user.
 * Requires current password verification and invalidates all existing sessions except current one.
 * 
 * @param req - AuthRequest object with current and new passwords in body
 * @param req.body.currentPassword - User's current password
 * @param req.body.newPassword - User's desired new password
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} JSON response with new authentication token
 * @throws Error if current password is incorrect or user is invalid
 * 
 * @calls
 * - /back-end/src/services/organizations.getContextFromReq
 * - /back-end/src/models/UserModel.getUserById
 * - /back-end/src/services/users.verifyPassword
 * - /back-end/src/services/users.updatePassword
 * - /back-end/src/models/UserModel.resetMinTokenDate
 * - /back-end/src/models/AuthRefreshModel.deleteMany
 * - sendLocalSuccessResponse (local helper function)
 */
```
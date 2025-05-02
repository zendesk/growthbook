# Magic Thinking: vercel.ts

# File Purpose

This file serves as the controller for Vercel integration functionality in the GrowthBook backend. It handles HTTP requests related to connecting a GrowthBook organization with Vercel, managing authentication tokens, and synchronizing GrowthBook API keys as environment variables in Vercel projects.

# Function Documentation

## getHasToken
```typescript
async function getHasToken(req: AuthRequest, res: Response)
```
**Purpose**: Checks if the current organization has a Vercel authentication token configured.

**Parameters**:
- `req`: Authentication request object
- `res`: Express response object

**Returns**: JSON response with:
- `hasToken`: Boolean indicating whether the organization has a Vercel token connected

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`

## postToken
```typescript
async function postToken(req: AuthRequest<{ code: string; configurationId: string; teamId: string }>, res: Response)
```
**Purpose**: Exchanges an OAuth code for a Vercel access token and stores it in the organization's settings.

**Parameters**:
- `req`: Authentication request object containing:
  - `body.code`: OAuth authorization code from Vercel
  - `body.configurationId`: Vercel configuration ID
  - `body.teamId`: Vercel team ID
- `res`: Express response object

**Returns**: JSON response with status 200 if successful.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/OrganizationModel.ts`: `updateOrganization`
- `node-fetch`: Makes HTTP request to Vercel OAuth token endpoint
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/audit.ts`: `auditDetailsUpdate`

## postEnvVars
```typescript
async function postEnvVars(req: AuthRequest<{ gbVercelEnvMap: GbVercelEnvMap }>, res: Response)
```
**Purpose**: Creates GrowthBook API keys and sets them as environment variables in all GrowthBook-related Vercel projects.

**Parameters**:
- `req`: Authentication request object containing:
  - `body.gbVercelEnvMap`: Mapping of GrowthBook environments to Vercel environments
- `res`: Express response object

**Returns**: JSON response with status 200 if successful.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/vercel.ts`: 
  - `getGbRelatedVercelProjects`: Retrieves Vercel projects that are related to GrowthBook
  - `reduceGbVercelEnvMap`: Processes the environment mapping
  - `createOrgGbKeys`: Creates GrowthBook API keys
  - `postEnvVar`: Sets environment variables in Vercel projects

## getConfig
```typescript
async function getConfig(req: AuthRequest, res: Response)
```
**Purpose**: Retrieves the current configuration of GrowthBook API keys in Vercel projects for display in the UI.

**Parameters**:
- `req`: Authentication request object
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `apiKeyRowList`: Array of API key configurations showing how they are mapped in Vercel projects

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ApiKeyModel.ts`: `getAllApiKeysByOrganization`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/vercel.ts`:
  - `getGbRelatedVercelProjects`: Gets Vercel projects related to GrowthBook
  - `getEnvVars`: Retrieves environment variables from Vercel projects
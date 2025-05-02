# Magic Thinking: presentations.ts

# File Purpose

This file serves as the controller for the presentations functionality in the GrowthBook backend. It handles HTTP requests for creating, retrieving, updating, and deleting presentations, which are collections of experiment results organized into slides for sharing and presenting to stakeholders.

# Function Documentation

## getPresentations
```typescript
async function getPresentations(req: AuthRequest, res: Response)
```
**Purpose**: Retrieves all presentations for the current organization.

**Parameters**:
- `req`: Authentication request object
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `presentations`: Array of presentation objects for the organization

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/presentations.ts`: `getPresentationsByOrganization`

## getPresentation
```typescript
async function getPresentation(req: AuthRequest<null, { id: string }>, res: Response)
```
**Purpose**: Retrieves a specific presentation by ID along with its associated experiment data and snapshots.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the presentation to retrieve
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `presentation`: The presentation object
- `experiments`: Array of experiments with snapshots included in the presentation

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/presentations.ts`: `getPresentationById`, `getPresentationSnapshots`

## getPresentationPreview
```typescript
async function getPresentationPreview(req: AuthRequest, res: Response)
```
**Purpose**: Generates a preview of experiments that might be included in a presentation without creating an actual presentation.

**Parameters**:
- `req`: Authentication request object containing:
  - `query.expIds`: Comma-separated string of experiment IDs to include in the preview
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `experiments`: Array of experiments with snapshots for the preview

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/presentations.ts`: `getPresentationSnapshots`

## deletePresentation
```typescript
async function deletePresentation(req: AuthRequest<ExperimentInterface, { id: string }>, res: Response)
```
**Purpose**: Deletes a presentation by ID.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the presentation to delete
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `result`: Result of the deletion operation

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/presentations.ts`: `getPresentationById`, `deletePresentationById`

## postPresentation
```typescript
async function postPresentation(req: AuthRequest<Partial<PresentationInterface>>, res: Response)
```
**Purpose**: Creates a new presentation.

**Parameters**:
- `req`: Authentication request object containing:
  - `body`: Partial presentation object with presentation details
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `presentation`: The created presentation object

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/presentations.ts`: `createPresentation`

## updatePresentation
```typescript
async function updatePresentation(req: AuthRequest<PresentationInterface, { id: string }>, res: Response)
```
**Purpose**: Updates an existing presentation.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the presentation to update
  - `body`: Updated presentation data
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `presentation`: The updated presentation object

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/presentations.ts`: `getPresentationById`
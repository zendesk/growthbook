# Magic Thinking: ideas.ts

# File Purpose
This file serves as the controller for the ideas functionality in the GrowthBook backend. It handles HTTP requests for creating, reading, updating, deleting, and managing ideas, which are concepts for potential experiments or features that can be voted on by users.

# Function Documentation

## getIdeas
```typescript
async function getIdeas(req: AuthRequest<any, any, { project?: string }>, res: Response)
```
**Purpose**: Retrieves a list of ideas, optionally filtered by project.

**Parameters**:
- `req`: Authentication request object containing:
  - `query.project`: Optional string to filter ideas by project
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `ideas`: Array of idea objects

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/ideas.ts`: `getIdeasByOrganization`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`

## getEstimatedImpact
```typescript
async function getEstimatedImpact(req: AuthRequest<{ metric: string; segment?: string }>, res: Response)
```
**Purpose**: Calculates and retrieves estimated impact for a metric and optional segment.

**Parameters**:
- `req`: Authentication request object containing:
  - `body.metric`: Metric ID to analyze
  - `body.segment`: Optional segment to filter by
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `estimate`: Impact estimate data

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ImpactEstimateModel.ts`: `getImpactEstimate`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`

## postIdeas
```typescript
async function postIdeas(req: AuthRequest<Partial<IdeaInterface>>, res: Response)
```
**Purpose**: Creates a new idea.

**Parameters**:
- `req`: Authentication request object containing:
  - `body`: Partial idea object with idea details
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `idea`: The created idea object

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/ideas.ts`: `createIdea`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`

## getIdea
```typescript
async function getIdea(req: AuthRequest<Partial<IdeaInterface>, { id: string }>, res: Response)
```
**Purpose**: Retrieves a specific idea by ID, including related estimate and experiment data.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: Idea ID to retrieve
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `idea`: The idea object
- `estimate`: Associated impact estimate (if available)
- `experiment`: Associated experiment details (if available)

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/ideas.ts`: `getIdeaById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ImpactEstimateModel.ts`: `ImpactEstimateModel.findOne`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentByIdea`

## postIdea
```typescript
async function postIdea(req: AuthRequest<IdeaInterface, { id: string }>, res: Response)
```
**Purpose**: Updates an existing idea.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: Idea ID to update
  - `body`: Updated idea data
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `idea`: The updated idea object

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/ideas.ts`: `getIdeaById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/TagModel.ts`: `addTagsDiff`

## deleteIdea
```typescript
async function deleteIdea(req: AuthRequest<IdeaInterface, { id: string }>, res: Response)
```
**Purpose**: Deletes an idea by ID.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: Idea ID to delete
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `result`: Deletion result

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/ideas.ts`: `getIdeaById`, `deleteIdeaById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`

## postVote
```typescript
async function postVote(req: AuthRequest<Partial<Vote>, { id: string }>, res: Response)
```
**Purpose**: Records a vote (upvote or downvote) on an idea.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: Idea ID to vote on
  - `body`: Vote data including direction (`dir`)
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `idea`: The updated idea with the new vote

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/ideas.ts`: `getIdeaById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`

## getRecentIdeas
```typescript
async function getRecentIdeas(req: AuthRequest<unknown, { num: string }, { project?: string }>, res: Response)
```
**Purpose**: Retrieves a list of recent ideas, sorted by creation date, optionally filtered by project.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.num`: Maximum number of ideas to return
  - `query.project`: Optional project filter
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `ideas`: Array of recent idea objects

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/ideas.ts`: `getIdeasByQuery`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
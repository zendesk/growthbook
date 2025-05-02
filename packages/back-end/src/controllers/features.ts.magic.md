# Magic Thinking: features.ts

# File Purpose

This file serves as the controller for feature flags functionality in the GrowthBook backend. It handles HTTP requests for creating, managing, updating, evaluating, and querying feature flags, including their rules, environments, revisions, prerequisites, and usage analytics. It also provides public API endpoints for SDK clients to retrieve feature flag definitions.

# Function Documentation

## getPayloadParamsFromApiKey
```typescript
async function getPayloadParamsFromApiKey(key: string, req: Request): Promise<{organization: string; capabilities: SDKCapability[]; projects: string[]; environment: string; encrypted: boolean; encryptionKey?: string; includeVisualExperiments?: boolean; includeDraftExperiments?: boolean; includeExperimentNames?: boolean; includeRedirectExperiments?: boolean; includeRuleIds?: boolean; hashSecureAttributes?: boolean; remoteEvalEnabled?: boolean; savedGroupReferencesEnabled?: boolean;}>
```
**Purpose**: Authenticates and extracts configuration from an API key to determine what features and capabilities to include in SDK payloads.

**Parameters**:
- `key`: The API key string to validate
- `req`: Express request object potentially containing project filters

**Returns**: An object containing organization ID, SDK capabilities, project filters, environment, encryption settings, and feature flag inclusion settings.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/SdkConnectionModel.ts`: `findSDKConnectionByKey`, `markSDKConnectionUsed`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ApiKeyModel.ts`: `lookupOrganizationByApiKey`

## getFeaturesPublic
```typescript
async function getFeaturesPublic(req: Request, res: Response)
```
**Purpose**: Public API endpoint for SDK connections to retrieve feature flag definitions for a specific environment.

**Parameters**:
- `req`: Express request object containing API key parameter
- `res`: Express response object

**Returns**: JSON response with feature definitions, cache headers for CDN optimization.

**Calls**:
- `getPayloadParamsFromApiKey`: Local function to validate API key and get configuration
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextForAgendaJobByOrgId`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/features.ts`: `getFeatureDefinitions`

## getEvaluatedFeaturesPublic
```typescript
async function getEvaluatedFeaturesPublic(req: Request, res: Response)
```
**Purpose**: Public API endpoint for remote evaluation of features for a specific user context.

**Parameters**:
- `req`: Express request object containing API key parameter and user attributes in body
- `res`: Express response object

**Returns**: JSON response with evaluated feature values for the provided user attributes.

**Calls**:
- `getPayloadParamsFromApiKey`: Local function to validate API key and get configuration
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextForAgendaJobByOrgId`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/features.ts`: `getFeatureDefinitions`
- `@growthbook/proxy-eval`: `evaluateFeatures`

## postFeatures
```typescript
async function postFeatures(req: AuthRequest<Partial<FeatureInterface>>, res: Response<{ status: 200; feature: FeatureInterface }, EventUserForResponseLocals>)
```
**Purpose**: Creates a new feature flag.

**Parameters**:
- `req`: Authenticated request object containing:
  - `body`: Partial feature flag data including ID, environmentSettings, and other properties
- `res`: Express response object

**Returns**: JSON response with the created feature flag.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureModel.ts`: `createFeature`, `getFeature`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/features.ts`: `addIdsToRules`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/WatchModel.ts`: `upsertWatch`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/audit.ts`: `auditDetailsCreate`

## postFeatureRebase
```typescript
async function postFeatureRebase(req: AuthRequest<{ strategies: Record<string, MergeStrategy>; mergeResultSerialized: string }, { id: string; version: string }>, res: Response)
```
**Purpose**: Rebases a draft feature flag revision on top of the current live version, resolving conflicts.

**Parameters**:
- `req`: Authenticated request object containing:
  - `params.id`: Feature ID
  - `params.version`: Revision version
  - `body.strategies`: Strategy to use for resolving conflicts
  - `body.mergeResultSerialized`: Serialized result of the merge for verification
- `res`: Express response object

**Returns**: Status 200 on success.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureModel.ts`: `getFeature`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureRevisionModel.ts`: `getRevision`, `updateRevision`
- `shared/util`: `autoMerge`

## postFeatureRequestReview
```typescript
async function postFeatureRequestReview(req: AuthRequest<{ comment: string }, { id: string; version: string }>, res: Response)
```
**Purpose**: Requests a review for a draft feature revision.

**Parameters**:
- `req`: Authenticated request object containing:
  - `params.id`: Feature ID
  - `params.version`: Revision version
  - `body.comment`: Comment explaining the changes
- `res`: Express response object

**Returns**: Status 200 on success.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureModel.ts`: `getFeature`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureRevisionModel.ts`: `getRevision`, `markRevisionAsReviewRequested`

## postFeatureReviewOrComment
```typescript
async function postFeatureReviewOrComment(req: AuthRequest<{ comment: string; review?: ReviewSubmittedType }, { id: string; version: string }>, res: Response)
```
**Purpose**: Adds a review or comment to a feature revision in the review process.

**Parameters**:
- `req`: Authenticated request object containing:
  - `params.id`: Feature ID
  - `params.version`: Revision version
  - `body.comment`: Comment text
  - `body.review`: Type of review (Comment, Approve, Request Changes)
- `res`: Express response object

**Returns**: Status 200 on success.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureModel.ts`: `getFeature`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureRevisionModel.ts`: `getRevision`, `submitReviewAndComments`

## postFeaturePublish
```typescript
async function postFeaturePublish(req: AuthRequest<{ comment: string; mergeResultSerialized: string; adminOverride?: boolean; publishExperimentIds?: string[] }, { id: string; version: string }>, res: Response)
```
**Purpose**: Publishes a feature revision, making it live, optionally with linked experiments.

**Parameters**:
- `req`: Authenticated request object containing:
  - `params.id`: Feature ID
  - `params.version`: Revision version
  - `body.comment`: Publishing comment
  - `body.mergeResultSerialized`: Serialized merge result for verification
  - `body.adminOverride`: Optional flag to bypass approval requirements
  - `body.publishExperimentIds`: Optional IDs of experiments to publish along with the feature
- `res`: Express response object

**Returns**: Status 200 on success.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureModel.ts`: `getFeature`, `publishRevision`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureRevisionModel.ts`: `getRevision`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentsByIds`, `updateExperiment`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/experiments.ts`: `getChangesToStartExperiment`
- `shared/util`: `autoMerge`, `checkIfRevisionNeedsReview`, `getEnabledEnvironments`

## postFeatureRevert
```typescript
async function postFeatureRevert(req: AuthRequest<{ comment: string }, { id: string; version: string }>, res: Response<{ status: 200; version: number }, EventUserForResponseLocals>)
```
**Purpose**: Reverts a feature to a previous published revision.

**Parameters**:
- `req`: Authenticated request object containing:
  - `params.id`: Feature ID
  - `params.version`: Target revision version to revert to
  - `body.comment`: Comment explaining the reversion
- `res`: Express response object

**Returns**: JSON response with the version number that was reverted to.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureModel.ts`: `getFeature`, `applyRevisionChanges`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureRevisionModel.ts`: `getRevision`, `markRevisionAsPublished`
- `shared/util`: `isEqual`

## postFeatureFork
```typescript
async function postFeatureFork(req: AuthRequest<never, { id: string; version: string }>, res: Response<{ status: 200; version: number }, EventUserForResponseLocals>)
```
**Purpose**: Creates a new draft revision based on an existing revision (not necessarily the current one).

**Parameters**:
- `req`: Authenticated request object containing:
  - `params.id`: Feature ID
  - `params.version`: Version to fork from
- `res`: Express response object

**Returns**: JSON response with the new revision version number.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureModel.ts`: `getFeature`, `updateFeature`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureRevisionModel.ts`: `getRevision`, `createRevision`

## postFeatureDiscard
```typescript
async function postFeatureDiscard(req: AuthRequest<never, { id: string; version: string }>, res: Response<{ status: 200 }, EventUserForResponseLocals>)
```
**Purpose**: Discards a draft revision.

**Parameters**:
- `req`: Authenticated request object containing:
  - `params.id`: Feature ID
  - `params.version`: Version to discard
- `res`: Express response object

**Returns**: Status 200 on success.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureModel.ts`: `getFeature`, `updateFeature`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureRevisionModel.ts`: `getRevision`, `discardRevision`, `hasDraft`

## postFeatureRule
```typescript
async function postFeatureRule(req: AuthRequest<PostFeatureRuleBody, { id: string; version: string }>, res: Response<{ status: 200; version: number }, EventUserForResponseLocals>)
```
**Purpose**: Adds a new rule to a feature in a specific environment.

**Parameters**:
- `req`: Authenticated request object containing:
  - `params.id`: Feature ID
  - `params.version`: Revision version
  - `body.environment`: Environment ID
  - `body.rule`: Rule configuration
  - `body.safeRolloutFields`: Optional safe rollout settings
- `res`: Express response object

**Returns**: JSON response with the revision version number.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureModel.ts`: `getFeature`
- `getDraftRevision`: Local helper function
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureRevisionModel.ts`: `addFeatureRule`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `addLinkedFeatureToExperiment`

## postFeatureSync
```typescript
async function postFeatureSync(req: AuthRequest<Omit<FeatureInterface, "organization" | "version" | "dateCreated" | "dateUpdated">, { id: string }>, res: Response<{ status: 200; feature: FeatureInterface }, EventUserForResponseLocals>)
```
**Purpose**: Synchronizes a feature with provided data, creating or updating it as needed.

**Parameters**:
- `req`: Authenticated request object containing:
  - `params.id`: Feature ID
  - `body`: Feature data to sync
- `res`: Express response object

**Returns**: JSON response with the updated feature.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureModel.ts`: `getFeature`, `updateFeature`
- `postFeatures`: Local function if feature doesn't exist yet
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureRevisionModel.ts`: `createRevision`

## postFeatureExperimentRefRule
```typescript
async function postFeatureExperimentRefRule(req: AuthRequest<{ rule: ExperimentRefRule }, { id: string }>, res: Response<{ status: 200; version: number }, EventUserForResponseLocals>)
```
**Purpose**: Adds an experiment reference rule to a feature in all environments.

**Parameters**:
- `req`: Authenticated request object containing:
  - `params.id`: Feature ID
  - `body.rule`: Experiment reference rule
- `res`: Express response object

**Returns**: JSON response with the revision version number.

**Calls**:
- `/Users/rikish.kamboj
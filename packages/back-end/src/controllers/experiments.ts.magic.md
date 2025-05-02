# Magic Thinking: experiments.ts

# File Purpose
This file serves as the controller for the experiments-related functionality in the GrowthBook backend. It handles HTTP requests for creating, reading, updating, deleting, and managing various aspects of experiments including snapshots, variations, targeting, and analysis.

# Function Documentation

## getExperiments
```typescript
async function getExperiments(req: AuthRequest<unknown, unknown, {project?: string; includeArchived?: boolean; type?: ExperimentType}>, res: Response)
```
**Purpose**: Retrieves a list of experiments based on filter criteria.

**Parameters**:
- `req`: Authentication request object containing:
  - `query.project`: Optional string to filter experiments by project
  - `query.includeArchived`: Optional boolean to include archived experiments
  - `query.type`: Optional experiment type filter
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `experiments`: Array of experiment objects
- `hasArchived`: Boolean indicating if there are archived experiments

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getAllExperiments`, `hasArchivedExperiments`

## getExperimentsFrequencyMonth
```typescript
async function getExperimentsFrequencyMonth(req: AuthRequest<null, { num: string }, { project?: string }>, res: Response)
```
**Purpose**: Generates experiment frequency data grouped by month for various categorizations.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.num`: Number of months to include in the analysis
  - `query.project`: Optional project filter
- `res`: Express response object

**Returns**: JSON response with experiment frequency data categorized by:
- `all`: Month-by-month experiment counts
- `byStatus`: Counts broken down by experiment status
- `byProject`: Counts broken down by project
- `byResults`: Counts broken down by experiment results

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getAllExperiments`

## lookupExperimentByTrackingKey
```typescript
async function lookupExperimentByTrackingKey(req: AuthRequest<unknown, unknown, { trackingKey: string }>, res: ResponseWithStatusAndError<{ experimentId: string | null }>)
```
**Purpose**: Finds an experiment by its tracking key.

**Parameters**:
- `req`: Authentication request object containing:
  - `query.trackingKey`: The tracking key to search for
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `experimentId`: The ID of the found experiment or null if not found

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentByTrackingKey`

## getExperiment
```typescript
async function getExperiment(req: AuthRequest<null, { id: string }>, res: Response)
```
**Purpose**: Retrieves detailed information about a specific experiment.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: The ID of the experiment to retrieve
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `experiment`: The experiment object
- `visualChangesets`: Visual changesets associated with the experiment
- `urlRedirects`: URL redirects associated with the experiment
- `linkedFeatures`: Features linked to the experiment
- `envs`: Affected environments for the experiment
- `idea`: Associated idea object if available

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/VisualChangesetModel.ts`: `findVisualChangesetsByExperiment`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureModel.ts`: `getFeaturesByIds`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/IdeasModel.ts`: `IdeaModel.findOne`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/experiments.ts`: `getLinkedFeatureInfo`

## getExperimentPublic
```typescript
async function getExperimentPublic(req: AuthRequest<null, { uid: string }>, res: Response)
```
**Purpose**: Retrieves a publicly shared experiment by its UID.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.uid`: The UID of the experiment to retrieve
- `res`: Express response object

**Returns**: JSON response with the public experiment data, including the experiment object, latest snapshot, visual changesets, URL redirects, linked features, and SSR data for the report.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentByUid`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentSnapshotModel.ts`: `getLatestSnapshot`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/VisualChangesetModel.ts`: `findVisualChangesetsByExperiment`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/experiments.ts`: `getLinkedFeatureInfo`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/reports.ts`: `generateExperimentReportSSRData`

## _getSnapshot
```typescript
async function _getSnapshot({context, experiment, phase, dimension, withResults = true, type}: {context: ReqContext | ApiReqContext; experiment: string; phase?: string; dimension?: string; withResults?: boolean; type?: SnapshotType})
```
**Purpose**: Helper function to retrieve an experiment snapshot.

**Parameters**:
- `context`: Request context
- `experiment`: Experiment ID
- `phase`: Optional phase index string
- `dimension`: Optional dimension string
- `withResults`: Whether to include results (default: true)
- `type`: Optional snapshot type

**Returns**: The experiment snapshot object.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentSnapshotModel.ts`: `getLatestSnapshot`

## getSnapshotWithDimension
```typescript
async function getSnapshotWithDimension(req: AuthRequest<null, { id: string; phase: string; dimension: string }, { type?: SnapshotType }>, res: Response)
```
**Purpose**: Retrieves experiment snapshots with dimension information.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: Experiment ID
  - `params.phase`: Phase index string
  - `params.dimension`: Dimension string
  - `query.type`: Optional snapshot type
- `res`: Express response object

**Returns**: JSON response containing the requested snapshot, latest snapshot, and dimensionless snapshot.

**Calls**: The `_getSnapshot` helper function

## getSnapshot
```typescript
async function getSnapshot(req: AuthRequest<null, { id: string; phase: string }, { type?: SnapshotType }>, res: Response)
```
**Purpose**: Retrieves experiment snapshots for a specific phase.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: Experiment ID
  - `params.phase`: Phase index string
  - `query.type`: Optional snapshot type
- `res`: Express response object

**Returns**: JSON response containing the requested snapshot and latest snapshot.

**Calls**: The `_getSnapshot` helper function

## getSnapshotById
```typescript
async function getSnapshotById(req: AuthRequest<null, { id: string }>, res: Response)
```
**Purpose**: Retrieves an experiment snapshot by its ID.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: Snapshot ID
- `res`: Express response object

**Returns**: JSON response with the snapshot object.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentSnapshotModel.ts`: `findSnapshotById`

## postSnapshotNotebook
```typescript
async function postSnapshotNotebook(req: AuthRequest<null, { id: string }>, res: Response)
```
**Purpose**: Generates a notebook for an experiment snapshot.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: Snapshot ID
- `res`: Express response object

**Returns**: JSON response with the generated notebook.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/notebook.ts`: `generateExperimentNotebook`

## getSnapshots
```typescript
async function getSnapshots(req: AuthRequest<unknown, unknown, { experiments?: string }>, res: Response)
```
**Purpose**: Retrieves snapshots for multiple experiments.

**Parameters**:
- `req`: Authentication request object containing:
  - `query.experiments`: Comma-separated string of experiment IDs
- `res`: Express response object

**Returns**: JSON response with an array of snapshots.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentsByIds`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/experiments.ts`: `_getSnapshots`

## validateVariationIds
```typescript
function validateVariationIds(variations: Variation[])
```
**Purpose**: Validates variation IDs and keys in an experiment.

**Parameters**:
- `variations`: Array of variation objects

**Returns**: Nothing, but throws an error if there are duplicate variation keys.

## postExperiments
```typescript
async function postExperiments(req: AuthRequest<Partial<ExperimentInterfaceStringDates>, unknown, {allowDuplicateTrackingKey?: boolean; originalId?: string; autoRefreshResults?: boolean}>, res: Response<{status: 200; experiment: ExperimentInterface} | {status: 200; duplicateTrackingKey: boolean; existingId: string} | PrivateApiErrorResponse, EventUserForResponseLocals>)
```
**Purpose**: Creates a new experiment.

**Parameters**:
- `req`: Authentication request object containing:
  - `body`: Partial experiment data
  - `query.allowDuplicateTrackingKey`: Whether to allow duplicate tracking keys
  - `query.originalId`: ID of an experiment to copy from
  - `query.autoRefreshResults`: Whether to auto-refresh results
- `res`: Express response object

**Returns**: JSON response with the created experiment or duplicate tracking key information.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/DataSourceModel.ts`: `getDataSourceById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `createExperiment`, `getExperimentByTrackingKey`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/VisualChangesetModel.ts`: `findVisualChangesetsByExperiment`, `createVisualChangeset`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/MetricModel.ts`: `getMetricMap`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/WatchModel.ts`: `upsertWatch`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/experiments.ts`: `resetExperimentBanditSettings`

## postExperiment
```typescript
async function postExperiment(req: AuthRequest<ExperimentInterfaceStringDates & {currentPhase?: number; phaseStartDate?: string; phaseEndDate?: string; variationWeights?: number[];}, { id: string }>, res: Response<{status: number; experiment?: ExperimentInterface | null} | PrivateApiErrorResponse, EventUserForResponseLocals>)
```
**Purpose**: Updates an existing experiment.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: Experiment ID
  - `body`: Updated experiment data
- `res`: Express response object

**Returns**: JSON response with the updated experiment.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentById`, `updateExperiment`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/DataSourceModel.ts`: `getDataSourceById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/MetricModel.ts`: `getMetricMap`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FeatureModel.ts`: `getFeaturesByIds`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/TagModel.ts`: `addTagsDiff`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/VisualChangesetModel.ts`: `findVisualChangesetsByExperiment`, `syncVisualChangesWithVariations`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/WatchModel.ts`: `upsertWatch`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/experiments.ts`: `resetExperimentBanditSettings`, `determineNextBanditSchedule`

## postExperimentArchive
```typescript
async function postExperimentArchive(req: AuthRequest<null, { id: string }>, res: Response)
```
**Purpose**: Archives an experiment.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: Experiment ID
- `res`: Express response object

**Returns**: JSON response indicating success.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentById`, `updateExperiment`
- `/Users/rikish.k
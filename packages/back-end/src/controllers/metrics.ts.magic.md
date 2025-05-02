# Magic Thinking: metrics.ts

# File Purpose

This file serves as the controller for the metrics functionality in the GrowthBook backend. It handles HTTP requests related to creating, reading, updating, deleting, and analyzing metrics, as well as retrieving metric usage information and generating automated metrics.

# Function Documentation

## deleteMetric
```typescript
async function deleteMetric(req: AuthRequest<null, { id: string }>, res: Response<unknown, EventUserForResponseLocals>)
```
**Purpose**: Deletes a metric by ID after checking permissions.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the metric to delete
- `res`: Express response object

**Returns**: JSON response with status 200 if successful.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/MetricModel.ts`: `getMetricById`, `deleteMetricById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/audit.ts`: `auditDetailsDelete`

## getMetrics
```typescript
async function getMetrics(req: AuthRequest, res: Response)
```
**Purpose**: Retrieves all metrics for the current organization.

**Parameters**:
- `req`: Authentication request object
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `metrics`: Array of metric objects for the organization

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/MetricModel.ts`: `getMetricsByOrganization`

## getMetricUsage
```typescript
async function getMetricUsage(req: AuthRequest<null, { id: string }>, res: Response)
```
**Purpose**: Retrieves usage information about a metric, including where it's being used in ideas and experiments.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the metric to check usage for
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `ideas`: Array of ideas using the metric
- `experiments`: Array of experiments using the metric

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/MetricModel.ts`: `getMetricById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ImpactEstimateModel.ts`: `ImpactEstimateModel.find`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/ideas.ts`: `getIdeasByQuery`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentsByMetric`

## cancelLegacyMetricAnalysis
```typescript
async function cancelLegacyMetricAnalysis(req: AuthRequest<null, { id: string }>, res: Response)
```
**Purpose**: Cancels an ongoing legacy metric analysis job.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the metric to cancel analysis for
- `res`: Express response object

**Returns**: JSON response with status 200 if successful.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/MetricModel.ts`: `getMetricById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/datasource.ts`: `getIntegrationFromDatasourceId`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/queryRunners/LegacyMetricAnalysisQueryRunner.ts`: `LegacyMetricAnalysisQueryRunner`

## postLegacyMetricAnalysis
```typescript
async function postLegacyMetricAnalysis(req: AuthRequest<null, { id: string }>, res: Response)
```
**Purpose**: Initiates a legacy metric analysis job to refresh metric data.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the metric to analyze
- `res`: Express response object

**Returns**: JSON response with status 200 if successful.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/MetricModel.ts`: `getMetricById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/experiments.ts`: `refreshMetric`

## getMetric
```typescript
async function getMetric(req: AuthRequest<null, { id: string }>, res: Response)
```
**Purpose**: Retrieves a specific metric by ID.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the metric to retrieve
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `metric`: The requested metric object

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/MetricModel.ts`: `getMetricById`

## getMetricsFromTrackedEvents
```typescript
async function getMetricsFromTrackedEvents(req: AuthRequest<{ schema: string }, { datasourceId: string }>, res: Response)
```
**Purpose**: Retrieves potential metrics that can be auto-generated from tracked events in a data source.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.datasourceId`: ID of the data source
  - `body.schema`: Schema information for tracked events
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `trackedEvents`: Array of tracked events that can be converted to metrics

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/datasource.ts`: `getIntegrationFromDatasourceId`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/MetricModel.ts`: `getMetricsByDatasource`

## postAutoGeneratedMetrics
```typescript
async function postAutoGeneratedMetrics(req: AuthRequest<{datasourceId: string; projects?: string[]; metricsToCreate?: AutoMetricToCreate[];}>, res: Response)
```
**Purpose**: Creates auto-generated metrics from tracked events.

**Parameters**:
- `req`: Authentication request object containing:
  - `body.datasourceId`: ID of the data source
  - `body.projects`: Optional array of projects to associate with the metrics
  - `body.metricsToCreate`: Array of metrics to create automatically
- `res`: Express response object

**Returns**: JSON response with status 200 if successful.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/UserModel.ts`: `getUserById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/DataSourceModel.ts`: `getDataSourceById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/jobs/createAutoGeneratedMetrics.ts`: `queueCreateAutoGeneratedMetrics`

## postMetrics
```typescript
async function postMetrics(req: AuthRequest<Partial<MetricInterface>>, res: Response)
```
**Purpose**: Creates a new metric with the provided data.

**Parameters**:
- `req`: Authentication request object containing:
  - `body`: Partial metric object with metric details
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `metric`: The created metric object

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/DataSourceModel.ts`: `getDataSourceById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/experiments.ts`: `createMetric`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/audit.ts`: `auditDetailsCreate`

## putMetric
```typescript
async function putMetric(req: AuthRequest<Partial<MetricInterface>, { id: string }>, res: Response)
```
**Purpose**: Updates an existing metric with the provided data.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the metric to update
  - `body`: Partial metric object with updated fields
- `res`: Express response object

**Returns**: JSON response with status 200 if successful.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/MetricModel.ts`: `getMetricById`, `updateMetric`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/audit.ts`: `auditDetailsUpdate`

## getMetricExperimentResults
```typescript
async function getMetricExperimentResults(req: AuthRequest<unknown, { id: string }>, res: Response<{ status: 200; data: ExperimentWithSnapshot[] }>)
```
**Purpose**: Retrieves experiments using a specific metric, along with their snapshots.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the metric to get experiment results for
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `data`: Array of experiments with their snapshots that use the specified metric

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentsUsingMetric`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/experiments.ts`: `_getSnapshots`

## getMetricNorthstarData
```typescript
async function getMetricNorthstarData(req: AuthRequest<unknown, { id: string }>, res: Response<{status: 200; data: {experiments: ExperimentInterfaceStringDates[]; analysis: MetricAnalysisInterface | null; metric: MetricInterface | null;}}>)
```
**Purpose**: Retrieves northstar data for a metric, including experiments and analysis data.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the metric to get northstar data for
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `data`: Object containing experiments, analysis data, and metric information

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentsUsingMetric`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/MetricModel.ts`: `getMetricById`
- `shared/experiments`: `isFactMetricId`
- `shared/dates`: `daysBetween`
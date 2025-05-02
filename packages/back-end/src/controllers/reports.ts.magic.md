# Magic Thinking: reports.ts

# File Purpose

This file serves as the controller for the reports functionality in the GrowthBook backend. It handles HTTP requests related to creating, retrieving, updating, deleting, and analyzing experiment reports. Reports are ways to capture and share experiment analysis results, either as snapshots of existing experiments or as custom analysis configurations.

# Function Documentation

## postReportFromSnapshot
```typescript
async function postReportFromSnapshot(req: AuthRequest<ExperimentSnapshotReportArgs, { snapshot: string }>, res: Response)
```
**Purpose**: Creates a new report from an existing experiment snapshot.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.snapshot`: ID of the snapshot to create a report from
  - `body`: Report creation arguments including difference type and dimension
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `report`: The newly created report object

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentSnapshotModel.ts`: `findSnapshotById`, `createExperimentSnapshotModel`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ReportModel.ts`: `createReport`
- `shared/util`: `getSnapshotAnalysis`

## getReports
```typescript
async function getReports(req: AuthRequest<unknown, unknown, { project?: string }>, res: Response)
```
**Purpose**: Retrieves all reports for the current organization, optionally filtered by project.

**Parameters**:
- `req`: Authentication request object containing:
  - `query.project`: Optional project filter
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `reports`: Array of report objects
- `experiments`: Array of experiments referenced in the reports

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ReportModel.ts`: `getReportsByOrg`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentsByIds`

## getReportsOnExperiment
```typescript
async function getReportsOnExperiment(req: AuthRequest<unknown, { id: string }>, res: Response)
```
**Purpose**: Retrieves all reports related to a specific experiment.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the experiment to get reports for
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `reports`: Array of report objects for the experiment

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ReportModel.ts`: `getReportsByExperimentId`

## getReport
```typescript
async function getReport(req: AuthRequest<null, { id: string }>, res: Response)
```
**Purpose**: Retrieves a specific report by ID.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the report to retrieve
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `report`: The report object

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ReportModel.ts`: `getReportById`

## getReportPublic
```typescript
async function getReportPublic(req: Request<{ uid: string }>, res: Response)
```
**Purpose**: Retrieves a publicly shared report by its unique identifier.

**Parameters**:
- `req`: Request object containing:
  - `params.uid`: Public unique identifier of the report
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `report`: The report object
- `snapshot`: The associated experiment snapshot
- `experiment`: Basic metadata about the associated experiment
- `ssrData`: Server-side rendering data for the report

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ReportModel.ts`: `getReportByUid`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextForAgendaJobByOrgId`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentSnapshotModel.ts`: `findSnapshotById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/reports.ts`: `generateExperimentReportSSRData`

## deleteReport
```typescript
async function deleteReport(req: AuthRequest<null, { id: string }>, res: Response)
```
**Purpose**: Deletes a report by ID.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the report to delete
- `res`: Express response object

**Returns**: JSON response with status 200 if successful.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ReportModel.ts`: `getReportById`, `deleteReportById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentById`

## refreshReport
```typescript
async function refreshReport(req: AuthRequest<null, { id: string }, { force?: string }>, res: Response)
```
**Purpose**: Refreshes a report by rerunning its analysis with current data.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the report to refresh
  - `query.force`: Optional flag to force refresh instead of using cache
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `snapshot` or `updatedReport`: The refreshed report data

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ReportModel.ts`: `getReportById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/MetricModel.ts`: `getMetricMap`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FactTableModel.ts`: `getFactTableMap`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentSnapshotModel.ts`: `findSnapshotById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/reports.ts`: `createReportSnapshot`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/datasource.ts`: `getIntegrationFromDatasourceId`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/queryRunners/ExperimentReportQueryRunner.ts`: `ExperimentReportQueryRunner`

## putReport
```typescript
async function putReport(req: AuthRequest<Partial<ReportInterface>, { id: string }>, res: Response)
```
**Purpose**: Updates an existing report with new settings or metadata.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the report to update
  - `body`: Updated report data
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `updatedReport`: The updated report object

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ReportModel.ts`: `getReportById`, `updateReport`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentModel.ts`: `getExperimentById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/MetricModel.ts`: `getMetricMap`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/FactTableModel.ts`: `getFactTableMap`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/datasource.ts`: `getIntegrationFromDatasourceId`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/queryRunners/ExperimentReportQueryRunner.ts`: `ExperimentReportQueryRunner`
- `shared/dates`: `getValidDate`

## cancelReport
```typescript
async function cancelReport(req: AuthRequest<null, { id: string }>, res: Response)
```
**Purpose**: Cancels an ongoing report analysis job.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the report to cancel
- `res`: Express response object

**Returns**: JSON response with status 200 if successful.

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ReportModel.ts`: `getReportById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/ExperimentSnapshotModel.ts`: `findLatestRunningSnapshotByReportId`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/datasource.ts`: `getIntegrationFromDatasourceId`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/queryRunners/ExperimentResultsQueryRunner.ts`: `ExperimentResultsQueryRunner`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/queryRunners/ExperimentReportQueryRunner.ts`: `ExperimentReportQueryRunner`

## postNotebook
```typescript
async function postNotebook(req: AuthRequest<null, { id: string }>, res: Response)
```
**Purpose**: Generates a Jupyter notebook representation of a report for detailed analysis.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.id`: ID of the report to generate a notebook for
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `notebook`: The generated Jupyter notebook content

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/notebook.ts`: `generateReportNotebook`
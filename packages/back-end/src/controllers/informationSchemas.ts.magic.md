# Magic Thinking: informationSchemas.ts

# File Purpose

This file serves as the controller for the information schema functionality in the GrowthBook backend. It handles HTTP requests related to managing database schema information from connected data sources, allowing users to browse tables, columns, and their metadata.

# Function Documentation

## getInformationSchema
```typescript
async function getInformationSchema(req: AuthRequest<null, { datasourceId: string }>, res: Response)
```
**Purpose**: Retrieves the information schema for a specific data source.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.datasourceId`: ID of the data source to get schema information for
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `informationSchema`: The information schema object associated with the data source

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/DataSourceModel.ts`: `getDataSourceById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/InformationSchemaModel.ts`: `getInformationSchemaByDatasourceId`

## getTableData
```typescript
async function getTableData(req: AuthRequest<null, { datasourceId: string; tableId: string }>, res: Response)
```
**Purpose**: Retrieves metadata about a specific table in the information schema.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.datasourceId`: ID of the data source
  - `params.tableId`: ID of the table to retrieve metadata for
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `table`: The table metadata including columns and other information

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/InformationSchemaModel.ts`: `getInformationSchemaByDatasourceId`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/DataSourceModel.ts`: `getDataSourceById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/InformationSchemaTablesModel.ts`: `getInformationSchemaTableById`, `createInformationSchemaTable`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/informationSchema.ts`: `fetchTableData`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/jobs/updateStaleInformationSchemaTable.ts`: `queueUpdateStaleInformationSchemaTable`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/util/informationSchemas.ts`: `getPath`

## putTableData
```typescript
async function putTableData(req: AuthRequest<null, { datasourceId: string; tableId: string }>, res: Response)
```
**Purpose**: Requests an update for stale table metadata in the information schema.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.datasourceId`: ID of the data source
  - `params.tableId`: ID of the table to update
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `message`: Confirmation that the update job was scheduled

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/InformationSchemaTablesModel.ts`: `getInformationSchemaTableById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/jobs/updateStaleInformationSchemaTable.ts`: `queueUpdateStaleInformationSchemaTable`

## postInformationSchema
```typescript
async function postInformationSchema(req: AuthRequest<null, { datasourceId: string }>, res: Response)
```
**Purpose**: Creates a new information schema for a data source.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.datasourceId`: ID of the data source to create a schema for
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `message`: Confirmation that the creation job was scheduled

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/DataSourceModel.ts`: `getDataSourceById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/jobs/createInformationSchema.ts`: `queueCreateInformationSchema`

## putInformationSchema
```typescript
async function putInformationSchema(req: AuthRequest<{ informationSchemaId: string }, { datasourceId: string }>, res: Response)
```
**Purpose**: Updates an existing information schema for a data source.

**Parameters**:
- `req`: Authentication request object containing:
  - `params.datasourceId`: ID of the data source
  - `body.informationSchemaId`: ID of the information schema to update
- `res`: Express response object

**Returns**: JSON response with:
- `status`: HTTP status code (200)
- `message`: Confirmation that the update job was scheduled

**Calls**:
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/services/organizations.ts`: `getContextFromReq`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/models/DataSourceModel.ts`: `getDataSourceById`
- `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/jobs/updateInformationSchema.ts`: `queueUpdateInformationSchema`
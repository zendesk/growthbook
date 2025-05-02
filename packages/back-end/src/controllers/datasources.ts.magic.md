# Magic Thinking: datasources.ts

# File Analysis: datasources.ts

## 1. File Purpose
This file serves as a controller for managing data sources in the GrowthBook application. It provides API endpoints for creating, reading, updating, and deleting data sources, as well as related operations such as testing queries, managing dimension slices, and handling Google OAuth redirects. Data sources in GrowthBook are used to connect to various analytics platforms and databases that store experiment and user data.

## 2. Function Documentation

### `deleteDataSource`
**Purpose**: Deletes a data source from the system after performing various validation checks.
**Parameters**:
- `req`: AuthRequest<null, { id: string }> - The request object containing the ID of the data source to delete.
- `res`: Response - The response object to send back to the client.
**Return**: Promise<void> - Sends a 200 status response on success.
**Behavior**:
- Validates that the user has permission to delete the data source
- Checks that the data source is not the organization's default
- Ensures no metrics, segments, or dimensions are tied to the data source
- Deletes the data source and associated information schema data
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/models/DataSourceModel.getDataSourceById`
- `/src/models/DataSourceModel.deleteDatasource`
- `/src/models/MetricModel.getMetricsByDatasource`
- `/src/models/DimensionModel.findDimensionsByDataSource`
- `/src/models/InformationSchemaModel.deleteInformationSchemaById`
- `/src/models/InformationSchemaTablesByInformationSchemaId.deleteInformationSchemaTablesByInformationSchemaId`

### `getDataSources`
**Purpose**: Retrieves all data sources for the current organization.
**Parameters**:
- `req`: AuthRequest - The authenticated request object.
- `res`: Response - The response object.
**Return**: Promise<void> - Sends a 200 status response with data sources.
**Behavior**:
- Gets all data sources for the organization
- Maps each data source to include non-sensitive parameters
- Returns the list of data sources
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/models/DataSourceModel.getDataSourcesByOrganization`
- `/src/services/datasource.getSourceIntegrationObject`
- `/src/services/datasource.getNonSensitiveParams`

### `getDataSource`
**Purpose**: Retrieves a single data source by ID.
**Parameters**:
- `req`: AuthRequest<null, { id: string }> - The request containing the data source ID.
- `res`: Response<DataSourceInterfaceWithParams> - The response object.
**Return**: Promise<void> - Sends a 200 status response with the data source.
**Behavior**:
- Gets data source integration by ID
- Returns the data source with non-sensitive parameters
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/services/datasource.getIntegrationFromDatasourceId`

### `getDataSourceWithParams`
**Purpose**: Helper function to prepare a data source object for API responses.
**Parameters**:
- `integration`: SourceIntegrationInterface - The source integration object containing the data source.
**Return**: DataSourceInterfaceWithParams - The formatted data source with appropriate parameters.
**Behavior**:
- Extracts the data source from the integration
- Removes sensitive params and adds non-sensitive ones
- Includes any decryption errors
**Calls**:
- `/src/services/datasource.getNonSensitiveParams`

### `postDataSources`
**Purpose**: Creates a new data source.
**Parameters**:
- `req`: AuthRequest with body containing name, description, type, params, settings, and projects.
- `res`: Response - The response object.
**Return**: Promise<void> - Sends a 200 status response with the created data source ID and details or a 400 error.
**Behavior**:
- Validates user permissions for creating a data source
- Sets default event properties
- Creates the data source
- Returns the new data source with non-sensitive parameters
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/models/DataSourceModel.createDataSource`
- `/src/services/datasource.getSourceIntegrationObject`

### `postInbuiltDataSource`
**Purpose**: Creates a built-in ClickHouse data source (primarily for admin use).
**Parameters**:
- `req`: AuthRequest with optional configuration parameters and ID.
- `res`: Response - The response object.
**Return**: Promise<void> - Sends a 200 status response with the created data source or an error status.
**Behavior**:
- Verifies the user is a super admin
- Creates a ClickHouse user
- Sets up default settings for the data source
- Creates and returns the new data source
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/services/clickhouse.createClickhouseUser`
- `/src/models/DataSourceModel.createDataSource`

### `putDataSource`
**Purpose**: Updates an existing data source.
**Parameters**:
- `req`: AuthRequest with body containing updates and params ID.
- `res`: Response - The response object.
**Return**: Promise<void> - Sends a 200 status response with updated data source or an error status.
**Behavior**:
- Validates the user has permission to update the data source
- Handles special case for Google Analytics refresh tokens
- Tests connection if connection params changed
- Updates the data source
- Returns the updated data source with non-sensitive parameters
**Calls**:
- `/src/models/UserModel.getUserById`
- `/src/services/organizations.getContextFromReq`
- `/src/models/DataSourceModel.getDataSourceById`
- `/src/integrations/GoogleAnalytics.getOauth2Client`
- `/src/services/datasource.getSourceIntegrationObject`
- `/src/services/datasource.mergeParams`
- `/src/services/datasource.encryptParams`
- `/src/models/DataSourceModel.updateDataSource`
- `/src/jobs/createAutoGeneratedMetrics.queueCreateAutoGeneratedMetrics`

### `updateExposureQuery`
**Purpose**: Updates an exposure query within a data source.
**Parameters**:
- `req`: AuthRequest with updates and datasource/exposure query IDs.
- `res`: Response - The response object.
**Return**: Promise<void> - Sends a 200 status response or error.
**Behavior**:
- Validates user permissions
- Finds the specific exposure query in the data source
- Updates the query with the provided changes
- Updates the data source with the modified settings
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/models/DataSourceModel.getDataSourceById`
- `/src/models/DataSourceModel.updateDataSource`

### `postGoogleOauthRedirect`
**Purpose**: Generates a Google OAuth URL for authenticating with Google Analytics.
**Parameters**:
- `req`: AuthRequest<{ projects?: string[] }> - Request with optional projects list.
- `res`: Response - The response object.
**Return**: Promise<void> - Sends a 200 status response with the OAuth URL.
**Behavior**:
- Validates user permissions
- Gets an OAuth client
- Generates and returns an authentication URL
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/integrations/GoogleAnalytics.getOauth2Client`

### `getQueries`
**Purpose**: Retrieves queries by IDs.
**Parameters**:
- `req`: AuthRequest<null, { ids: string }> - The request with comma-separated query IDs.
- `res`: Response - The response object.
**Return**: Promise<void> - Sends a 200 status response with the queries.
**Behavior**:
- Gets organization context
- Fetches queries by IDs
- Returns the queries in the same order as requested
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/models/QueryModel.getQueriesByIds`

### `testLimitedQuery`
**Purpose**: Tests a query against a data source.
**Parameters**:
- `req`: AuthRequest with query string, datasource ID, and optional template variables.
- `res`: Response - The response object.
**Return**: Promise<void> - Sends a 200 status response with query results.
**Behavior**:
- Gets the data source
- Runs the test query against the data source
- Returns results, SQL, duration, and any errors
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/models/DataSourceModel.getDataSourceById`
- `/src/services/datasource.testQuery`

### `getDataSourceMetrics`
**Purpose**: Retrieves metrics associated with a data source.
**Parameters**:
- `req`: AuthRequest<null, { id: string }> - The request with data source ID.
- `res`: Response - The response object.
**Return**: Promise<void> - Sends a 200 status response with the metrics.
**Behavior**:
- Gets metrics for the specified data source
- Returns the metrics list
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/models/MetricModel.getMetricsByDatasource`

### `getDataSourceQueries`
**Purpose**: Retrieves queries associated with a data source.
**Parameters**:
- `req`: AuthRequest<null, { id: string }> - The request with data source ID.
- `res`: Response - The response object.
**Return**: Promise<void> - Sends a 200 status response with the queries.
**Behavior**:
- Validates permissions
- Gets queries for the specified data source
- Returns the queries list
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/models/DataSourceModel.getDataSourceById`
- `/src/models/QueryModel.getQueriesByDatasource`

### `getDimensionSlices`
**Purpose**: Retrieves dimension slices by ID.
**Parameters**:
- `req`: AuthRequest<null, { id: string }> - The request with dimension slice ID.
- `res`: Response - The response object.
**Return**: Promise<void> - Sends a 200 status response with dimension slices.
**Behavior**:
- Gets dimension slices for the specified ID
- Returns the dimension slices
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/models/DimensionSlicesModel.getDimensionSlicesById`

### `getLatestDimensionSlicesForDatasource`
**Purpose**: Retrieves the latest dimension slices for a data source and exposure query.
**Parameters**:
- `req`: AuthRequest with datasourceId and exposureQueryId.
- `res`: Response - The response object.
**Return**: Promise<void> - Sends a 200 status response with dimension slices.
**Behavior**:
- Gets the latest dimension slices for the specified data source and exposure query
- Returns the dimension slices
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/models/DimensionSlicesModel.getLatestDimensionSlices`

### `postDimensionSlices`
**Purpose**: Creates dimension slices for analysis.
**Parameters**:
- `req`: AuthRequest with dataSourceId, queryId, and lookbackDays.
- `res`: Response - The response object.
**Return**: Promise<void> - Sends a 200 status response with the created dimension slices.
**Behavior**:
- Gets data source integration
- Creates dimension slices model
- Runs dimension slices analysis
- Returns the resulting model
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/services/datasource.getIntegrationFromDatasourceId`
- `/src/models/DimensionSlicesModel.createDimensionSlices`
- `/src/queryRunners/DimensionSlicesQueryRunner`

### `cancelDimensionSlices`
**Purpose**: Cancels an in-progress dimension slices analysis.
**Parameters**:
- `req`: AuthRequest<null, { id: string }> - The request with dimension slice ID.
- `res`: Response - The response object.
**Return**: Promise<void> - Sends a 200 status response.
**Behavior**:
- Gets dimension slices by ID
- Gets data source integration
- Cancels the running queries
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/models/DimensionSlicesModel.getDimensionSlicesById`
- `/src/services/datasource.getIntegrationFromDatasourceId`
- `/src/queryRunners/DimensionSlicesQueryRunner`

### `fetchBigQueryDatasets`
**Purpose**: Retrieves available datasets from BigQuery.
**Parameters**:
- `req`: AuthRequest with projectId, client_email, and private_key.
- `res`: Response - The response object.
**Return**: Promise<void> - Sends a 200 status response with available datasets.
**Behavior**:
- Creates a BigQuery client with the provided credentials
- Lists available datasets
- Returns the dataset IDs
**Uses**:
- @google-cloud/bigquery library
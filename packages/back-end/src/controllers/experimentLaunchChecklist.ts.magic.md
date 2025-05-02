# Magic Thinking: experimentLaunchChecklist.ts

# File Analysis: experimentLaunchChecklist.ts

## 1. File Purpose

This file serves as a controller for managing experiment launch checklists in the GrowthBook application. Launch checklists are used to ensure experiments meet certain criteria before being launched, improving experiment quality and consistency. The file provides endpoints for creating, retrieving, and updating checklists at both organization and experiment levels. It also includes functionality for premium features related to customizing launch checklists.

## 2. Function Documentation

### `postExperimentLaunchChecklist`
**Purpose**: Creates a new experiment launch checklist for an organization or project.
**Parameters**:
- `req`: AuthRequest<{ tasks: ChecklistTask[]; projectId?: string }> - The request object containing the checklist tasks and optional project ID.
- `res`: Response - The Express response object to send back the result.
**Return**: Promise<void> - Sends a 200 status response with the created checklist on success, or 400 if a checklist already exists.
**Behavior**:
- Gets the authenticated user's context
- Verifies the user has permission to manage organization settings
- Checks if the organization has the premium "custom-launch-checklist" feature
- Verifies no existing checklist exists for the organization or project
- Creates a new checklist with the provided tasks
- Returns the created checklist or an error
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/enterprise.orgHasPremiumFeature`
- `/src/models/ExperimentLaunchChecklistModel.getExperimentLaunchChecklist`
- `/src/models/ExperimentLaunchChecklistModel.createExperimentLaunchChecklist`

### `getExperimentCheckListByOrg`
**Purpose**: Retrieves the experiment launch checklist for an organization.
**Parameters**:
- `req`: AuthRequest - The authenticated request object.
- `res`: Response - The Express response object.
**Return**: Promise<void> - Sends a 200 status response with the organization's checklist or an empty array if the feature is not available.
**Behavior**:
- Gets the organization context from the request
- Checks if the organization has the premium "custom-launch-checklist" feature
- If not, returns an empty checklist
- Retrieves and returns the organization's launch checklist
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/enterprise.orgHasPremiumFeature`
- `/src/models/ExperimentLaunchChecklistModel.getExperimentLaunchChecklist`

### `putExperimentLaunchChecklist`
**Purpose**: Updates an existing experiment launch checklist.
**Parameters**:
- `req`: AuthRequest<{ tasks: ChecklistTask[] }, { id: string }> - The request object containing updated tasks and the checklist ID to update.
- `res`: Response - The Express response object.
**Return**: Promise<void> - Sends a 200 status response on success, 404 if the checklist is not found.
**Behavior**:
- Gets the authenticated user's context
- Verifies the user has permission to manage organization settings
- Checks if the organization has the premium "custom-launch-checklist" feature
- Verifies the checklist exists
- Updates the checklist with the new tasks
- Returns success or error status
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/enterprise.orgHasPremiumFeature`
- `/src/models/ExperimentLaunchChecklistModel.getExperimentLaunchChecklistById`
- `/src/models/ExperimentLaunchChecklistModel.updateExperimentLaunchChecklist`

### `putManualLaunchChecklist`
**Purpose**: Updates the manual launch checklist status for a specific experiment.
**Parameters**:
- `req`: AuthRequest<{ checklist: { key: string; status: "complete" | "incomplete" }[] }, { id: string }> - The request object containing checklist items with their completion status and the experiment ID.
- `res`: Response - The Express response object.
**Return**: Promise<void> - Sends a 200 status response on success.
**Behavior**:
- Gets the authenticated user's context
- Retrieves the experiment by ID
- Verifies the user has permission to update the experiment
- Updates the experiment's manualLaunchChecklist field with the new checklist status
- Audits the checklist update action
- Returns success status
**Calls**:
- `/src/services/organizations.getContextFromReq`
- `/src/models/ExperimentModel.getExperimentById`
- `/src/models/ExperimentModel.updateExperiment`
- `/src/services/audit.auditDetailsUpdate`

The file also includes a TODO comment indicating that a `getExperimentCheckListByProject` method should be added in the future.

This controller manages two types of checklists:
1. Organization/project-level checklists that define the template of tasks experiments should complete (a premium feature)
2. Experiment-specific checklists that track completion status for individual experiments

The controller ensures that only users with appropriate permissions can create or modify checklists, and it enforces premium feature access for organization-level custom checklists.
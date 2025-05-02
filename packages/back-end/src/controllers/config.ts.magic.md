# Magic Thinking: config.ts

# File Analysis: `/Users/rikish.kamboj/Repos/zendesk-forks/growthbook/packages/back-end/src/controllers/config.ts`

## 1. File Purpose

This file contains controller functions for handling configuration-related endpoints in the GrowthBook application. It primarily focuses on providing experiment configuration data to client applications through API endpoints. The file enables the visual editor functionality by generating JavaScript code for client-side experiment implementation and provides experiment override configurations. These endpoints allow GrowthBook clients to fetch the necessary configuration data to properly implement A/B tests and feature flags on their websites or applications.

## 2. Function Documentation

### `canAutoAssignExperiment`
```typescript
/**
 * Determines if an experiment can be automatically assigned to users.
 * Experiments must have a target URL regex and at least one variation with DOM or CSS changes.
 * 
 * @param experiment - Experiment object to evaluate
 * @returns {boolean} - True if the experiment can be auto-assigned, false otherwise
 * 
 * @uses
 * - ExperimentInterface from back-end/types/experiment
 * - LegacyVariation from back-end/types/experiment
 */
```

### `getExperimentConfig`
```typescript
/**
 * API endpoint to retrieve experiment configuration overrides.
 * Validates the provided API key and returns experiment configuration data.
 * 
 * @param req - Express Request object with API key in route params
 * @param req.params.key - Publishable API key for organization authentication
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} - JSON response with experiment overrides and mappings or error message
 * @throws Will send a 400 error response if API key is invalid or secret, or if any error occurs
 * 
 * @calls
 * - /back-end/src/models/ApiKeyModel.lookupOrganizationByApiKey
 * - /back-end/src/services/organizations.getContextForAgendaJobByOrgId
 * - /back-end/src/services/organizations.getExperimentOverrides
 */
```

### `getExperimentsScript`
```typescript
/**
 * Generates and serves a JavaScript file for implementing visual experiments on client websites.
 * The script contains all running visual experiments for the organization associated with the API key.
 * 
 * @param req - Express Request object with API key in route params
 * @param req.params.key - Publishable API key for organization authentication
 * @param res - Express Response object
 * 
 * @returns {Promise<Response>} - JavaScript code response with content-type 'text/javascript'
 * @throws Will send JavaScript error console statements if API key is invalid or an error occurs
 * 
 * @calls
 * - /back-end/src/models/ApiKeyModel.lookupOrganizationByApiKey
 * - /back-end/src/services/organizations.getContextForAgendaJobByOrgId
 * - /back-end/src/models/ExperimentModel.getAllExperiments
 * 
 * @uses
 * - Reads base JavaScript template from ../templates/javascript.js
 * - APP_ORIGIN from back-end/src/util/secrets
 * - ExperimentInterface from back-end/types/experiment
 * - LegacyExperimentPhase from back-end/types/experiment
 * - LegacyVariation from back-end/types/experiment
 */
```

The implementation details of `getExperimentsScript` include:

1. It reads a base JavaScript template from the filesystem
2. It filters experiments to only include non-archived visual experiments
3. For each relevant experiment, it extracts:
   - Experiment key
   - Variation code (CSS and DOM mutations)
   - URL targeting rules
   - User group targeting
   - Traffic allocation (coverage and weights)
   - Experiment status (draft, running, stopped)
   - Winner information if applicable
4. It transforms this data into a compressed format to minimize script size
5. It injects the experiment configurations into the base script
6. It adds caching headers (max-age=600) to reduce server load

This script enables GrowthBook's visual editor functionality, allowing website owners to implement A/B tests without modifying their codebase directly.
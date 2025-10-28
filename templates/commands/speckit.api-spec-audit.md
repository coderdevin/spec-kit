---
description: Conduct a comprehensive quality audit of an API specification to ensure completeness, clarity, consistency, and adherence to RESTful best practices.
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Audit Philosophy: "Unit Tests for API Specifications"

**CORE PRINCIPLE**: This command audits the **API SPECIFICATION DOCUMENT** for quality, completeness, and clarity - NOT the implementation.

**NOT testing the API itself**:
- ❌ NOT "Verify the API returns 200 OK"
- ❌ NOT "Test authentication works correctly"
- ❌ NOT "Confirm error handling functions properly"
- ❌ NOT checking if the code matches the spec

**FOR API specification quality validation**:
- ✅ "Are all request parameters documented with types and constraints?"
- ✅ "Is 'high performance' quantified with specific latency/throughput metrics?"
- ✅ "Are error responses consistently structured across all endpoints?"
- ✅ "Are authentication requirements specified for all protected endpoints?"
- ✅ "Does the spec define what happens when required path variables are invalid?"

**Metaphor**: If your API spec is a contract written in structured English/JSON, this audit is the legal review ensuring the contract is complete, unambiguous, enforceable, and production-ready - NOT whether the parties are fulfilling the contract.

## Execution Steps

### 1. Identify API Specification Location

Parse `$ARGUMENTS` to determine which API spec to audit:

**Option A - Explicit path provided**:
- User provides full path: `.specify/features/user-management/specs/api/update-user-profile.md`
- Read the specified file directly

**Option B - Feature context with API name**:
- User mentions feature name + API name: "Audit the update user profile API in user-management feature"
- Look in: `.specify/features/[feature-name]/specs/api/[api-name].md`

**Option C - Scan and select**:
- User says "Audit the API spec" or "Audit API specs"
- Scan `.specify/features/*/specs/api/*.md` and `.specify/api-specs/*.md`
- If multiple found, list them and ask user to select
- If one found, proceed with audit

**Option D - No API spec found**:
- ERROR: "No API specification found. Please provide the path or create one using `/speckit.api-spec-generator` first."

### 2. Load API Specification

Read the complete API specification file and parse its structure:

**Expected sections** (based on api-spec-generator.md template):
- Basic Info (URL, Method, Auth, Permissions)
- Path Variables
- Request Parameters (Query, Headers, Body)
- Response (Success, Error responses)
- Business Workflow (Mermaid diagram)
- Error Codes
- Pseudo Code

**Extraction tasks**:
- Identify which sections are present vs. missing
- Note completeness of each section
- Flag placeholders or [NEEDS CLARIFICATION] markers
- Extract API metadata: endpoint URL, HTTP method, auth requirements

### 3. Generate Comprehensive API Specification Audit Checklist

Create a detailed checklist at the same location as the API spec with suffix `-audit.md`:
- If auditing `.specify/features/[feature]/specs/api/update-user.md`
- Create checklist: `.specify/features/[feature]/specs/api/update-user-audit.md`

**Checklist Structure**: Follow the API Spec Quality Dimensions below

### 4. Execute Multi-Dimensional Quality Audit

Analyze the API specification across all quality dimensions and populate the checklist:

## API Spec Quality Dimensions & Checklist Categories

### Category 1: Basic Information Completeness

**Purpose**: Verify core API metadata is fully specified

**Checklist Items** (CHK001-CHK015):

- [ ] CHK001 - Is the complete API endpoint URL specified with proper versioning? [Completeness, Basic Info]
- [ ] CHK002 - Is the HTTP method (GET/POST/PUT/PATCH/DELETE) explicitly stated? [Completeness, Basic Info]
- [ ] CHK003 - Are authentication requirements clearly specified (YES/NO)? [Completeness, Basic Info]
- [ ] CHK004 - Are required permissions documented with specific permission strings? [Completeness, Basic Info]
- [ ] CHK005 - Is there a clear, concise description of the API's purpose? [Completeness, Basic Info]
- [ ] CHK006 - Are path variables indicated with consistent bracket notation (e.g., `{userId}`)? [Consistency, Basic Info]
- [ ] CHK007 - Is the API version strategy clear (path versioning, header versioning, etc.)? [Clarity, Basic Info]
- [ ] CHK008 - Are permission requirements specific enough to implement authorization logic? [Clarity, Basic Info]
- [ ] CHK009 - Does the URL follow RESTful resource naming conventions (nouns, not verbs)? [Best Practice, Basic Info]
- [ ] CHK010 - Are collection resources properly pluralized (e.g., `/users` not `/user`)? [Best Practice, Basic Info]
- [ ] CHK011 - Is HTTP method semantically correct for the operation (POST for create, PATCH for partial update, etc.)? [Best Practice, Basic Info]
- [ ] CHK012 - Are nested resources properly represented in URL hierarchy? [Best Practice, Basic Info]
- [ ] CHK013 - Is the API description action-focused and implementation-agnostic? [Clarity, Basic Info]
- [ ] CHK014 - Are there any undefined or ambiguous terms in the basic info? [Ambiguity, Basic Info]
- [ ] CHK015 - Is the permission model consistent with organizational standards? [Consistency, Basic Info]

### Category 2: Path Variables Quality

**Purpose**: Ensure path variables are completely and clearly specified

**Checklist Items** (CHK016-CHK030):

- [ ] CHK016 - Are all path variables from the URL documented in the Path Variables table? [Completeness, Path Variables]
- [ ] CHK017 - Does each path variable have a specified data type? [Completeness, Path Variables]
- [ ] CHK018 - Is the "Required" status specified for each path variable? [Completeness, Path Variables]
- [ ] CHK019 - Does each path variable have a clear description of its purpose? [Completeness, Path Variables]
- [ ] CHK020 - Are constraints/validation rules specified for each path variable? [Completeness, Path Variables]
- [ ] CHK021 - Are data types specific enough (uuid vs. string, integer vs. long)? [Clarity, Path Variables]
- [ ] CHK022 - Are format constraints clearly defined (regex patterns, length limits, ranges)? [Clarity, Path Variables]
- [ ] CHK023 - Are enum values documented if path variable has limited allowed values? [Clarity, Path Variables]
- [ ] CHK024 - Are path variable naming conventions consistent (camelCase vs. snake_case vs. kebab-case)? [Consistency, Path Variables]
- [ ] CHK025 - Are path variable types consistent with their usage in request/response? [Consistency, Path Variables]
- [ ] CHK026 - Are there any path variables in the URL that aren't documented? [Gap, Path Variables]
- [ ] CHK027 - Are there documented path variables that don't appear in the URL? [Conflict, Path Variables]
- [ ] CHK028 - Are validation error scenarios defined for invalid path variable formats? [Coverage, Path Variables]
- [ ] CHK029 - Are path variables semantically meaningful (avoid generic `id` when more specific is possible)? [Best Practice, Path Variables]
- [ ] CHK030 - Are optional path variables properly indicated (or should they be query parameters instead)? [Best Practice, Path Variables]

### Category 3: Request Parameters Completeness

**Purpose**: Verify all request inputs are fully documented

**Checklist Items** (CHK031-CHK060):

**Query Parameters**:
- [ ] CHK031 - Are all query parameters documented with name, type, and description? [Completeness, Query Params]
- [ ] CHK032 - Is the "Required" status specified for each query parameter? [Completeness, Query Params]
- [ ] CHK033 - Are default values documented for optional query parameters? [Completeness, Query Params]
- [ ] CHK034 - Are constraints/validation rules specified (min/max, enum values, formats)? [Completeness, Query Params]
- [ ] CHK035 - Are query parameters used appropriately for filtering, sorting, pagination? [Best Practice, Query Params]
- [ ] CHK036 - Are pagination parameters standardized (limit/offset or page/size)? [Consistency, Query Params]
- [ ] CHK037 - Are filter parameter formats clearly defined? [Clarity, Query Params]
- [ ] CHK038 - Are sort parameter formats and allowed fields documented? [Clarity, Query Params]

**Headers**:
- [ ] CHK039 - Are all required headers documented? [Completeness, Headers]
- [ ] CHK040 - Is `Authorization` header format specified (Bearer, Basic, API Key)? [Completeness, Headers]
- [ ] CHK041 - Is `Content-Type` header specified for requests with body? [Completeness, Headers]
- [ ] CHK042 - Are custom headers documented with purpose and format? [Completeness, Headers]
- [ ] CHK043 - Are header naming conventions followed (standard casing)? [Consistency, Headers]
- [ ] CHK044 - Are example values provided for complex headers? [Clarity, Headers]

**Request Body**:
- [ ] CHK045 - Is request body structure provided for POST/PUT/PATCH methods? [Completeness, Request Body]
- [ ] CHK046 - Is the `Content-Type` specified (application/json, multipart/form-data, etc.)? [Completeness, Request Body]
- [ ] CHK047 - Are all fields documented in a structured table? [Completeness, Request Body]
- [ ] CHK048 - Is each field's data type explicitly specified? [Completeness, Request Body]
- [ ] CHK049 - Is the "Required" status specified for each field? [Completeness, Request Body]
- [ ] CHK050 - Are validation constraints documented (min/max length, regex, format)? [Completeness, Request Body]
- [ ] CHK051 - Are nested object structures clearly represented? [Clarity, Request Body]
- [ ] CHK052 - Are array fields documented with item types and constraints? [Clarity, Request Body]
- [ ] CHK053 - Are conditional requirements documented (e.g., "Required if X is true")? [Clarity, Request Body]
- [ ] CHK054 - Is the JSON structure example syntactically correct? [Quality, Request Body]
- [ ] CHK055 - Are field names consistent with response field names where applicable? [Consistency, Request Body]
- [ ] CHK056 - Do field naming conventions follow a consistent pattern? [Consistency, Request Body]
- [ ] CHK057 - Are mutually exclusive fields clearly documented? [Clarity, Request Body]
- [ ] CHK058 - Are enum/allowed values specified for fields with limited options? [Completeness, Request Body]
- [ ] CHK059 - Is the maximum request body size documented if there are limits? [Coverage, Request Body]
- [ ] CHK060 - Are complex validation rules clearly explained (cross-field validations)? [Clarity, Request Body]

### Category 4: Response Specification Quality

**Purpose**: Ensure response structures are completely and consistently defined

**Checklist Items** (CHK061-CHK100):

**Success Responses**:
- [ ] CHK061 - Is at least one success response documented (200/201/204)? [Completeness, Success Response]
- [ ] CHK062 - Is the appropriate HTTP status code used (200 vs. 201 vs. 204)? [Best Practice, Success Response]
- [ ] CHK063 - Is the response `Content-Type` specified? [Completeness, Success Response]
- [ ] CHK064 - Is the success response body structure provided? [Completeness, Success Response]
- [ ] CHK065 - Are all response fields documented in a structured table? [Completeness, Success Response]
- [ ] CHK066 - Is each response field's data type specified? [Completeness, Success Response]
- [ ] CHK067 - Does each response field have a clear description? [Completeness, Success Response]
- [ ] CHK068 - Are optional vs. always-present fields clearly indicated? [Clarity, Success Response]
- [ ] CHK069 - Are timestamp formats specified (ISO8601, Unix epoch, etc.)? [Clarity, Success Response]
- [ ] CHK070 - Are nested response structures clearly represented? [Clarity, Success Response]
- [ ] CHK071 - Are array responses documented with item structure? [Clarity, Success Response]
- [ ] CHK072 - Are null value semantics documented for nullable fields? [Clarity, Success Response]
- [ ] CHK073 - Is the response structure consistent with request body where applicable? [Consistency, Success Response]
- [ ] CHK074 - Is 204 No Content used appropriately (no response body)? [Best Practice, Success Response]
- [ ] CHK075 - Are pagination metadata fields included for collection responses? [Coverage, Success Response]
- [ ] CHK076 - Is response field naming consistent across different endpoints? [Consistency, Success Response]

**Error Responses**:
- [ ] CHK077 - Are error responses documented for all relevant HTTP status codes? [Completeness, Error Response]
- [ ] CHK078 - Is 400 Bad Request documented with validation error structure? [Completeness, Error Response]
- [ ] CHK079 - Is 401 Unauthorized documented for authenticated endpoints? [Completeness, Error Response]
- [ ] CHK080 - Is 403 Forbidden documented when permissions are required? [Completeness, Error Response]
- [ ] CHK081 - Is 404 Not Found documented for resource-specific endpoints? [Completeness, Error Response]
- [ ] CHK082 - Is 409 Conflict documented for endpoints with uniqueness constraints? [Coverage, Error Response]
- [ ] CHK083 - Is 422 Unprocessable Entity documented for business rule violations? [Coverage, Error Response]
- [ ] CHK084 - Is 429 Too Many Requests documented if rate limiting exists? [Coverage, Error Response]
- [ ] CHK085 - Is 500 Internal Server Error documented? [Completeness, Error Response]
- [ ] CHK086 - Is 503 Service Unavailable documented for external dependency failures? [Coverage, Error Response]
- [ ] CHK087 - Are error response structures consistent across all error types? [Consistency, Error Response]
- [ ] CHK088 - Do error responses include machine-readable error codes? [Best Practice, Error Response]
- [ ] CHK089 - Do error responses include human-readable error messages? [Best Practice, Error Response]
- [ ] CHK090 - Are validation error details included (field-level errors for 400)? [Clarity, Error Response]
- [ ] CHK091 - Are error response examples syntactically correct JSON? [Quality, Error Response]
- [ ] CHK092 - Are HTTP status codes used semantically correctly? [Best Practice, Error Response]
- [ ] CHK093 - Is the error response format consistent with organizational standards? [Consistency, Error Response]
- [ ] CHK094 - Are retry-able vs. non-retry-able errors distinguished? [Clarity, Error Response]
- [ ] CHK095 - Are error messages actionable (tell user how to fix)? [Quality, Error Response]
- [ ] CHK096 - Do error responses avoid exposing sensitive system information? [Security, Error Response]
- [ ] CHK097 - Are partial success scenarios addressed (for batch operations)? [Coverage, Error Response]
- [ ] CHK098 - Are timeout errors documented with appropriate status codes? [Coverage, Error Response]
- [ ] CHK099 - Are rate limit error responses documented with retry-after information? [Clarity, Error Response]
- [ ] CHK100 - Is there a catch-all 500 error for unexpected failures? [Coverage, Error Response]

### Category 5: Business Workflow Documentation

**Purpose**: Ensure business logic and process flow are clearly documented

**Checklist Items** (CHK101-CHK125):

- [ ] CHK101 - Is a high-level business workflow description provided? [Completeness, Business Workflow]
- [ ] CHK102 - Is a Mermaid.js workflow diagram included? [Completeness, Business Workflow]
- [ ] CHK103 - Does the diagram show the complete request-to-response flow? [Completeness, Business Workflow]
- [ ] CHK104 - Are validation steps clearly shown in the workflow? [Completeness, Business Workflow]
- [ ] CHK105 - Are authentication and authorization checks depicted? [Completeness, Business Workflow]
- [ ] CHK106 - Are business logic processing steps included? [Completeness, Business Workflow]
- [ ] CHK107 - Are error paths and decision points shown? [Coverage, Business Workflow]
- [ ] CHK108 - Are all possible workflow outcomes represented? [Coverage, Business Workflow]
- [ ] CHK109 - Is the Mermaid.js syntax valid and renderable? [Quality, Business Workflow]
- [ ] CHK110 - Are workflow steps described in accompanying text? [Clarity, Business Workflow]
- [ ] CHK111 - Are business rules explicitly listed and explained? [Completeness, Business Workflow]
- [ ] CHK112 - Is the order of operations (validation → auth → business logic) clear? [Clarity, Business Workflow]
- [ ] CHK113 - Are external system interactions documented in the workflow? [Coverage, Business Workflow]
- [ ] CHK114 - Are data transformations or mappings described? [Clarity, Business Workflow]
- [ ] CHK115 - Are side effects (notifications, events, logging) documented? [Coverage, Business Workflow]
- [ ] CHK116 - Are asynchronous operations clearly indicated? [Clarity, Business Workflow]
- [ ] CHK117 - Are transaction boundaries documented if applicable? [Coverage, Business Workflow]
- [ ] CHK118 - Are rollback/compensation logic requirements specified? [Coverage, Business Workflow]
- [ ] CHK119 - Are race condition or concurrency scenarios addressed? [Coverage, Business Workflow]
- [ ] CHK120 - Are state transitions documented for stateful resources? [Clarity, Business Workflow]
- [ ] CHK121 - Is the workflow description at the right level of abstraction (not too technical)? [Quality, Business Workflow]
- [ ] CHK122 - Do business rules align with functional requirements from feature spec? [Traceability, Business Workflow]
- [ ] CHK123 - Are conditional business logic branches clearly explained? [Clarity, Business Workflow]
- [ ] CHK124 - Are workflow steps numbered or labeled for easy reference? [Quality, Business Workflow]
- [ ] CHK125 - Is the critical path through the workflow obvious? [Clarity, Business Workflow]

### Category 6: Error Codes Catalog Quality

**Purpose**: Ensure error codes are comprehensive and well-documented

**Checklist Items** (CHK126-CHK150):

- [ ] CHK126 - Is an error codes table provided? [Completeness, Error Codes]
- [ ] CHK127 - Are all error codes referenced in error responses listed in the table? [Completeness, Error Codes]
- [ ] CHK128 - Does each error code have an associated HTTP status code? [Completeness, Error Codes]
- [ ] CHK129 - Is each error code's meaning/description provided? [Completeness, Error Codes]
- [ ] CHK130 - Is the cause of each error documented? [Completeness, Error Codes]
- [ ] CHK131 - Are resolution steps provided for each error? [Completeness, Error Codes]
- [ ] CHK132 - Are error code naming conventions consistent (UPPER_SNAKE_CASE)? [Consistency, Error Codes]
- [ ] CHK133 - Are error codes semantically meaningful and descriptive? [Clarity, Error Codes]
- [ ] CHK134 - Are error codes unique (no duplicates)? [Quality, Error Codes]
- [ ] CHK135 - Are HTTP status codes aligned with error code semantics? [Consistency, Error Codes]
- [ ] CHK136 - Are validation errors comprehensively covered? [Coverage, Error Codes]
- [ ] CHK137 - Are authentication/authorization errors covered? [Coverage, Error Codes]
- [ ] CHK138 - Are business rule violation errors covered? [Coverage, Error Codes]
- [ ] CHK139 - Are resource not found errors covered? [Coverage, Error Codes]
- [ ] CHK140 - Are conflict/duplicate resource errors covered? [Coverage, Error Codes]
- [ ] CHK141 - Are rate limiting errors covered? [Coverage, Error Codes]
- [ ] CHK142 - Are external dependency failure errors covered? [Coverage, Error Codes]
- [ ] CHK143 - Are timeout errors covered? [Coverage, Error Codes]
- [ ] CHK144 - Are system/internal errors covered? [Coverage, Error Codes]
- [ ] CHK145 - Are error codes grouped logically by category? [Quality, Error Codes]
- [ ] CHK146 - Are resolution steps actionable and clear? [Quality, Error Codes]
- [ ] CHK147 - Do error codes align with organizational error code standards? [Consistency, Error Codes]
- [ ] CHK148 - Are client errors (4xx) vs. server errors (5xx) properly categorized? [Best Practice, Error Codes]
- [ ] CHK149 - Are deprecated error codes marked as such? [Coverage, Error Codes]
- [ ] CHK150 - Is there an error code for "unexpected/unknown error"? [Coverage, Error Codes]

### Category 7: Pseudo Code Quality

**Purpose**: Ensure pseudo code provides clear implementation guidance

**Checklist Items** (CHK151-CHK175):

- [ ] CHK151 - Is pseudo code provided for the API implementation? [Completeness, Pseudo Code]
- [ ] CHK152 - Does pseudo code show the complete request-to-response flow? [Completeness, Pseudo Code]
- [ ] CHK153 - Are input extraction and validation steps included? [Completeness, Pseudo Code]
- [ ] CHK154 - Are authentication and authorization checks included? [Completeness, Pseudo Code]
- [ ] CHK155 - Is core business logic processing included? [Completeness, Pseudo Code]
- [ ] CHK156 - Are database/persistence operations included? [Completeness, Pseudo Code]
- [ ] CHK157 - Is error handling logic included? [Completeness, Pseudo Code]
- [ ] CHK158 - Are response construction steps included? [Completeness, Pseudo Code]
- [ ] CHK159 - Is the pseudo code language-agnostic? [Quality, Pseudo Code]
- [ ] CHK160 - Are variable names descriptive and meaningful? [Clarity, Pseudo Code]
- [ ] CHK161 - Is the code structure logical and easy to follow? [Clarity, Pseudo Code]
- [ ] CHK162 - Are comments used appropriately to explain logic? [Clarity, Pseudo Code]
- [ ] CHK163 - Are error cases handled with try-catch or equivalent? [Coverage, Pseudo Code]
- [ ] CHK164 - Are all error codes from the error catalog represented? [Consistency, Pseudo Code]
- [ ] CHK165 - Does pseudo code align with the business workflow? [Consistency, Pseudo Code]
- [ ] CHK166 - Are external dependencies/service calls clearly indicated? [Clarity, Pseudo Code]
- [ ] CHK167 - Are database queries/operations abstracted appropriately? [Quality, Pseudo Code]
- [ ] CHK168 - Is the validation logic comprehensive? [Coverage, Pseudo Code]
- [ ] CHK169 - Are business rules from the workflow section implemented? [Consistency, Pseudo Code]
- [ ] CHK170 - Is transaction management addressed if needed? [Coverage, Pseudo Code]
- [ ] CHK171 - Are logging statements included at appropriate points? [Coverage, Pseudo Code]
- [ ] CHK172 - Is the pseudo code at the right abstraction level (not too detailed, not too vague)? [Quality, Pseudo Code]
- [ ] CHK173 - Are edge cases handled in the pseudo code? [Coverage, Pseudo Code]
- [ ] CHK174 - Does pseudo code demonstrate proper resource cleanup? [Coverage, Pseudo Code]
- [ ] CHK175 - Is the function signature consistent with API specification? [Consistency, Pseudo Code]

### Category 8: RESTful Design Best Practices

**Purpose**: Ensure API follows REST architectural principles

**Checklist Items** (CHK176-CHK200):

- [ ] CHK176 - Does the URL use nouns (resources) rather than verbs (actions)? [Best Practice, REST]
- [ ] CHK177 - Are collection resources properly pluralized? [Best Practice, REST]
- [ ] CHK178 - Are resource hierarchies represented with proper URL nesting? [Best Practice, REST]
- [ ] CHK179 - Is the HTTP method semantically appropriate (GET for read, POST for create, etc.)? [Best Practice, REST]
- [ ] CHK180 - Are idempotent operations using idempotent methods (PUT, DELETE, GET)? [Best Practice, REST]
- [ ] CHK181 - Is PATCH used for partial updates vs. PUT for full replacement? [Best Practice, REST]
- [ ] CHK182 - Are URL nesting levels kept reasonable (typically ≤3 levels)? [Best Practice, REST]
- [ ] CHK183 - Are query parameters used for filtering, not resource identification? [Best Practice, REST]
- [ ] CHK184 - Is pagination implemented for collection endpoints? [Best Practice, REST]
- [ ] CHK185 - Are consistent pagination parameters used (limit/offset or page/size)? [Consistency, REST]
- [ ] CHK186 - Are sorting parameters documented for collection endpoints? [Coverage, REST]
- [ ] CHK187 - Are filtering capabilities documented for collection endpoints? [Coverage, REST]
- [ ] CHK188 - Is API versioning strategy consistent and clear? [Best Practice, REST]
- [ ] CHK189 - Are HTTP status codes used correctly and semantically? [Best Practice, REST]
- [ ] CHK190 - Is 201 Created used for successful POST requests? [Best Practice, REST]
- [ ] CHK191 - Does 201 response include Location header for created resource? [Best Practice, REST]
- [ ] CHK192 - Is 204 No Content used when no response body is needed? [Best Practice, REST]
- [ ] CHK193 - Are 4xx errors used for client errors vs. 5xx for server errors? [Best Practice, REST]
- [ ] CHK194 - Is HATEOAS considered or intentionally excluded? [Coverage, REST]
- [ ] CHK195 - Are resource URLs consistent and predictable? [Consistency, REST]
- [ ] CHK196 - Is Content-Type negotiation supported/documented? [Coverage, REST]
- [ ] CHK197 - Are Accept headers considered for content negotiation? [Coverage, REST]
- [ ] CHK198 - Is caching strategy documented (Cache-Control, ETag)? [Coverage, REST]
- [ ] CHK199 - Are conditional requests supported (If-None-Match, If-Modified-Since)? [Coverage, REST]
- [ ] CHK200 - Is the API stateless (no server-side session state)? [Best Practice, REST]

### Category 9: Security & Authentication Requirements

**Purpose**: Ensure security concerns are properly documented

**Checklist Items** (CHK201-CHK225):

- [ ] CHK201 - Is authentication requirement clearly specified? [Completeness, Security]
- [ ] CHK202 - Is the authentication mechanism documented (OAuth2, JWT, API Key, etc.)? [Completeness, Security]
- [ ] CHK203 - Are authorization/permission requirements specified? [Completeness, Security]
- [ ] CHK204 - Is the permission model clearly defined? [Clarity, Security]
- [ ] CHK205 - Are authentication errors (401) properly documented? [Coverage, Security]
- [ ] CHK206 - Are authorization errors (403) properly documented? [Coverage, Security]
- [ ] CHK207 - Are sensitive data fields identified? [Coverage, Security]
- [ ] CHK208 - Is data sanitization/validation addressed for user inputs? [Coverage, Security]
- [ ] CHK209 - Are injection attack vectors considered in validation rules? [Coverage, Security]
- [ ] CHK210 - Is HTTPS/TLS enforcement documented? [Best Practice, Security]
- [ ] CHK211 - Are rate limiting requirements specified? [Coverage, Security]
- [ ] CHK212 - Are rate limit thresholds quantified? [Clarity, Security]
- [ ] CHK213 - Is rate limit exceeded error (429) documented? [Coverage, Security]
- [ ] CHK214 - Are CORS requirements documented for browser-based clients? [Coverage, Security]
- [ ] CHK215 - Is input validation comprehensive (length, format, type, range)? [Coverage, Security]
- [ ] CHK216 - Are file upload size limits specified if applicable? [Coverage, Security]
- [ ] CHK217 - Are allowed file types/MIME types specified for uploads? [Coverage, Security]
- [ ] CHK218 - Is sensitive data properly masked in error messages? [Security, Security]
- [ ] CHK219 - Are security headers documented (CSP, X-Frame-Options, etc.)? [Coverage, Security]
- [ ] CHK220 - Is audit logging requirement mentioned for sensitive operations? [Coverage, Security]
- [ ] CHK221 - Are data retention/deletion policies referenced? [Coverage, Security]
- [ ] CHK222 - Is PII (personally identifiable information) handling addressed? [Coverage, Security]
- [ ] CHK223 - Are compliance requirements mentioned (GDPR, HIPAA, etc.)? [Coverage, Security]
- [ ] CHK224 - Is token expiration/refresh strategy documented? [Coverage, Security]
- [ ] CHK225 - Are password/secret handling requirements specified if applicable? [Coverage, Security]

### Category 10: Performance & Scalability Considerations

**Purpose**: Ensure performance expectations are documented

**Checklist Items** (CHK226-CHK245):

- [ ] CHK226 - Are performance requirements/SLAs specified? [Completeness, Performance]
- [ ] CHK227 - Are latency targets quantified (e.g., p95 < 200ms)? [Clarity, Performance]
- [ ] CHK228 - Are throughput requirements specified (requests per second)? [Clarity, Performance]
- [ ] CHK229 - Are timeout values documented? [Completeness, Performance]
- [ ] CHK230 - Are payload size limits specified? [Coverage, Performance]
- [ ] CHK231 - Are pagination limits documented for collection endpoints? [Coverage, Performance]
- [ ] CHK232 - Is caching strategy documented? [Coverage, Performance]
- [ ] CHK233 - Are cache headers specified (Cache-Control, ETag, Expires)? [Coverage, Performance]
- [ ] CHK234 - Are expensive operations identified? [Coverage, Performance]
- [ ] CHK235 - Is asynchronous processing considered for long-running operations? [Coverage, Performance]
- [ ] CHK236 - Are bulk operation endpoints considered vs. individual operations? [Coverage, Performance]
- [ ] CHK237 - Is response compression documented (gzip, brotli)? [Coverage, Performance]
- [ ] CHK238 - Are database query optimization considerations mentioned? [Coverage, Performance]
- [ ] CHK239 - Is connection pooling or resource management addressed? [Coverage, Performance]
- [ ] CHK240 - Are concurrent request limits specified? [Coverage, Performance]
- [ ] CHK241 - Is graceful degradation strategy documented? [Coverage, Performance]
- [ ] CHK242 - Are slow query scenarios handled (timeouts, partial results)? [Coverage, Performance]
- [ ] CHK243 - Is request/response size optimization considered? [Coverage, Performance]
- [ ] CHK244 - Are performance monitoring/metrics requirements specified? [Coverage, Performance]
- [ ] CHK245 - Is load testing criteria mentioned? [Coverage, Performance]

### Category 11: Data Validation & Type Safety

**Purpose**: Ensure data validation is comprehensive and unambiguous

**Checklist Items** (CHK246-CHK270):

- [ ] CHK246 - Are data types specified for all request parameters? [Completeness, Validation]
- [ ] CHK247 - Are data types specified for all response fields? [Completeness, Validation]
- [ ] CHK248 - Are string length limits (min/max) specified? [Coverage, Validation]
- [ ] CHK249 - Are numeric ranges (min/max) specified? [Coverage, Validation]
- [ ] CHK250 - Are date/time formats explicitly specified (ISO8601, etc.)? [Clarity, Validation]
- [ ] CHK251 - Are enum values explicitly listed for constrained fields? [Clarity, Validation]
- [ ] CHK252 - Are regex patterns provided for format validation? [Clarity, Validation]
- [ ] CHK253 - Are email format validations specified? [Coverage, Validation]
- [ ] CHK254 - Are URL format validations specified? [Coverage, Validation]
- [ ] CHK255 - Are UUID format requirements specified? [Clarity, Validation]
- [ ] CHK256 - Are array length limits (min/max items) specified? [Coverage, Validation]
- [ ] CHK257 - Are nested object depth limits specified if applicable? [Coverage, Validation]
- [ ] CHK258 - Are null vs. empty string semantics clarified? [Clarity, Validation]
- [ ] CHK259 - Are validation error messages specified in error responses? [Completeness, Validation]
- [ ] CHK260 - Are field-level validation errors included in 400 responses? [Coverage, Validation]
- [ ] CHK261 - Are cross-field validation rules documented? [Coverage, Validation]
- [ ] CHK262 - Are conditional validation rules documented? [Coverage, Validation]
- [ ] CHK263 - Are type coercion rules documented (string "true" → boolean)? [Clarity, Validation]
- [ ] CHK264 - Are timezone handling rules specified for timestamps? [Clarity, Validation]
- [ ] CHK265 - Are precision requirements specified for decimal/float fields? [Clarity, Validation]
- [ ] CHK266 - Are currency format requirements specified? [Clarity, Validation]
- [ ] CHK267 - Are phone number format requirements specified? [Clarity, Validation]
- [ ] CHK268 - Are case sensitivity rules documented (usernames, codes, etc.)? [Clarity, Validation]
- [ ] CHK269 - Are whitespace handling rules documented (trim, preserve)? [Clarity, Validation]
- [ ] CHK270 - Are character encoding requirements specified (UTF-8, etc.)? [Completeness, Validation]

### Category 12: Documentation Quality & Usability

**Purpose**: Ensure the API spec is clear, complete, and usable by all stakeholders

**Checklist Items** (CHK271-CHK295):

- [ ] CHK271 - Is the API spec document well-structured and easy to navigate? [Quality, Documentation]
- [ ] CHK272 - Are all required sections present? [Completeness, Documentation]
- [ ] CHK273 - Are there any placeholder values that need to be filled? [Completeness, Documentation]
- [ ] CHK274 - Are there any [NEEDS CLARIFICATION] markers remaining? [Completeness, Documentation]
- [ ] CHK275 - Is technical jargon explained or avoided? [Clarity, Documentation]
- [ ] CHK276 - Are JSON examples syntactically correct? [Quality, Documentation]
- [ ] CHK277 - Are JSON examples realistic and helpful? [Quality, Documentation]
- [ ] CHK278 - Are table formats consistent and properly formatted? [Quality, Documentation]
- [ ] CHK279 - Are code blocks properly formatted with language tags? [Quality, Documentation]
- [ ] CHK280 - Is the Mermaid diagram syntactically valid? [Quality, Documentation]
- [ ] CHK281 - Are there any broken internal references? [Quality, Documentation]
- [ ] CHK282 - Are assumptions clearly documented? [Completeness, Documentation]
- [ ] CHK283 - Are external dependencies documented? [Completeness, Documentation]
- [ ] CHK284 - Are related APIs cross-referenced? [Coverage, Documentation]
- [ ] CHK285 - Is versioning/changelog information present? [Coverage, Documentation]
- [ ] CHK286 - Can a frontend developer implement API calls from this spec? [Usability, Documentation]
- [ ] CHK287 - Can a backend developer implement the endpoint from this spec? [Usability, Documentation]
- [ ] CHK288 - Can a QA engineer write test cases from this spec? [Usability, Documentation]
- [ ] CHK289 - Can technical writers create API docs from this spec? [Usability, Documentation]
- [ ] CHK290 - Are examples provided for complex request/response scenarios? [Quality, Documentation]
- [ ] CHK291 - Is the spec free of ambiguous or vague language? [Clarity, Documentation]
- [ ] CHK292 - Are acronyms defined on first use? [Clarity, Documentation]
- [ ] CHK293 - Is the specification consistent with organizational API standards? [Consistency, Documentation]
- [ ] CHK294 - Are all tables properly formatted with aligned columns? [Quality, Documentation]
- [ ] CHK295 - Is the overall specification ready for implementation? [Quality, Documentation]

### Category 13: Edge Cases & Exception Scenarios

**Purpose**: Ensure edge cases and exceptional scenarios are covered

**Checklist Items** (CHK296-CHK320):

- [ ] CHK296 - Are empty/zero-state scenarios addressed (empty lists, no data)? [Coverage, Edge Cases]
- [ ] CHK297 - Are boundary value scenarios addressed (min/max limits)? [Coverage, Edge Cases]
- [ ] CHK298 - Are large dataset handling scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK299 - Are null/missing value scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK300 - Are duplicate request scenarios addressed (idempotency)? [Coverage, Edge Cases]
- [ ] CHK301 - Are concurrent modification scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK302 - Are partial failure scenarios addressed (for batch operations)? [Coverage, Edge Cases]
- [ ] CHK303 - Are cascading deletion scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK304 - Are orphaned resource scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK305 - Are circular reference scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK306 - Are resource exhaustion scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK307 - Are timeout scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK308 - Are network failure scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK309 - Are external service failure scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK310 - Are data inconsistency scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK311 - Are malformed request scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK312 - Are extremely long string input scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK313 - Are special character/unicode handling scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK314 - Are timezone edge cases addressed (DST, UTC boundaries)? [Coverage, Edge Cases]
- [ ] CHK315 - Are leap year/date edge cases addressed if applicable? [Coverage, Edge Cases]
- [ ] CHK316 - Are backwards compatibility scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK317 - Are deprecated field handling scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK318 - Are migration/transition scenarios addressed? [Coverage, Edge Cases]
- [ ] CHK319 - Are rollback scenarios addressed for data-modifying operations? [Coverage, Edge Cases]
- [ ] CHK320 - Are race condition scenarios addressed? [Coverage, Edge Cases]

### Category 14: Integration & Dependencies

**Purpose**: Ensure external integrations and dependencies are documented

**Checklist Items** (CHK321-CHK340):

- [ ] CHK321 - Are external service dependencies documented? [Completeness, Integration]
- [ ] CHK322 - Are external API calls documented in the workflow? [Coverage, Integration]
- [ ] CHK323 - Are external dependency failure scenarios addressed? [Coverage, Integration]
- [ ] CHK324 - Are external dependency timeout scenarios addressed? [Coverage, Integration]
- [ ] CHK325 - Are circuit breaker patterns mentioned for external calls? [Coverage, Integration]
- [ ] CHK326 - Are retry strategies documented for external calls? [Coverage, Integration]
- [ ] CHK327 - Are fallback behaviors documented for external failures? [Coverage, Integration]
- [ ] CHK328 - Are database dependencies clearly identified? [Completeness, Integration]
- [ ] CHK329 - Are cache dependencies documented? [Coverage, Integration]
- [ ] CHK330 - Are message queue dependencies documented? [Coverage, Integration]
- [ ] CHK331 - Are event publishing requirements documented? [Coverage, Integration]
- [ ] CHK332 - Are webhook/callback requirements documented? [Coverage, Integration]
- [ ] CHK333 - Are third-party service SLAs considered? [Coverage, Integration]
- [ ] CHK334 - Are data synchronization requirements documented? [Coverage, Integration]
- [ ] CHK335 - Are cross-service transaction requirements addressed? [Coverage, Integration]
- [ ] CHK336 - Are API gateway/proxy requirements documented? [Coverage, Integration]
- [ ] CHK337 - Are service mesh requirements documented if applicable? [Coverage, Integration]
- [ ] CHK338 - Are integration points with other internal APIs documented? [Coverage, Integration]
- [ ] CHK339 - Are data transformation requirements between services documented? [Coverage, Integration]
- [ ] CHK340 - Are API versioning compatibility requirements with dependencies specified? [Coverage, Integration]

### Category 15: Testing & Quality Assurance Readiness

**Purpose**: Ensure the spec enables comprehensive testing

**Checklist Items** (CHK341-CHK360):

- [ ] CHK341 - Can unit test scenarios be derived from the spec? [Usability, Testing]
- [ ] CHK342 - Can integration test scenarios be derived from the spec? [Usability, Testing]
- [ ] CHK343 - Can contract test scenarios be derived from the spec? [Usability, Testing]
- [ ] CHK344 - Are positive test scenarios clearly defined? [Coverage, Testing]
- [ ] CHK345 - Are negative test scenarios clearly defined? [Coverage, Testing]
- [ ] CHK346 - Are boundary test scenarios clearly defined? [Coverage, Testing]
- [ ] CHK347 - Are error scenario test cases clearly defined? [Coverage, Testing]
- [ ] CHK348 - Are security test scenarios identifiable? [Coverage, Testing]
- [ ] CHK349 - Are performance test criteria specified? [Coverage, Testing]
- [ ] CHK350 - Are load test requirements specified? [Coverage, Testing]
- [ ] CHK351 - Are test data requirements identifiable from the spec? [Usability, Testing]
- [ ] CHK352 - Are mock/stub requirements for dependencies clear? [Usability, Testing]
- [ ] CHK353 - Are acceptance criteria measurable and testable? [Quality, Testing]
- [ ] CHK354 - Are success criteria clearly defined? [Completeness, Testing]
- [ ] CHK355 - Are failure criteria clearly defined? [Completeness, Testing]
- [ ] CHK356 - Can the spec be used to generate API documentation? [Usability, Testing]
- [ ] CHK357 - Can the spec be converted to OpenAPI/Swagger format? [Usability, Testing]
- [ ] CHK358 - Are monitoring/observability requirements testable? [Coverage, Testing]
- [ ] CHK359 - Are logging requirements testable? [Coverage, Testing]
- [ ] CHK360 - Is the spec detailed enough to enable test automation? [Usability, Testing]

## Checklist File Structure

Generate the audit checklist file with this structure:

# API Specification Audit: [API Name]

**API Endpoint**: [URL]
**HTTP Method**: [Method]
**Audit Date**: [Date]
**Auditor**: AI Agent via /speckit.api-spec-audit
**Specification File**: [Path to API spec file]

---

## Audit Summary

**Total Quality Checks**: 360
**Passed**: [Count] ✅
**Failed**: [Count] ❌
**Warnings**: [Count] ⚠️
**Not Applicable**: [Count] ⊘

**Overall Quality Score**: [Passed / Total * 100]%

**Readiness Assessment**: 
- [ ] ✅ Ready for Implementation
- [ ] ⚠️ Minor Issues - Can proceed with caution
- [ ] ❌ Major Issues - Requires revision before implementation

---

## Critical Issues [Priority: HIGH]

[List any critical issues that must be addressed before implementation]

1. [Critical issue 1]
2. [Critical issue 2]

## Important Warnings [Priority: MEDIUM]

[List important issues that should be addressed]

1. [Warning 1]
2. [Warning 2]

## Suggestions [Priority: LOW]

[List optional improvements]

1. [Suggestion 1]
2. [Suggestion 2]

---

## Detailed Audit Checklist

### Category 1: Basic Information Completeness

[Include all CHK001-CHK015 items with status]

- [✅/❌/⚠️/⊘] CHK001 - Is the complete API endpoint URL specified with proper versioning? [Completeness, Basic Info]
  - **Status**: [Pass/Fail/Warning/N/A]
  - **Evidence**: [Quote from spec or note why it failed]
  - **Issue**: [If failed, describe the specific problem]
  - **Recommendation**: [If failed, suggest how to fix]

[Continue for all categories and items...]

### Category 2: Path Variables Quality
### Category 3: Request Parameters Completeness
### Category 4: Response Specification Quality
### Category 5: Business Workflow Documentation
### Category 6: Error Codes Catalog Quality
### Category 7: Pseudo Code Quality
### Category 8: RESTful Design Best Practices
### Category 9: Security & Authentication Requirements
### Category 10: Performance & Scalability Considerations
### Category 11: Data Validation & Type Safety
### Category 12: Documentation Quality & Usability
### Category 13: Edge Cases & Exception Scenarios
### Category 14: Integration & Dependencies
### Category 15: Testing & Quality Assurance Readiness

---

## Implementation Readiness Report

### Frontend Development Readiness
- Can frontend developers implement API calls from this spec? [Yes/No/Partial]
- Issues: [List any issues]

### Backend Development Readiness
- Can backend developers implement endpoints from this spec? [Yes/No/Partial]
- Issues: [List any issues]

### QA/Testing Readiness
- Can QA engineers write comprehensive test cases from this spec? [Yes/No/Partial]
- Issues: [List any issues]

### Documentation Readiness
- Can technical writers create user-facing API docs from this spec? [Yes/No/Partial]
- Issues: [List any issues]

---

## Recommendations & Next Steps

### Must Fix (Before Implementation)
1. [High priority fix 1]
2. [High priority fix 2]

### Should Fix (Before Release)
1. [Medium priority fix 1]
2. [Medium priority fix 2]

### Consider (Future Improvement)
1. [Low priority improvement 1]
2. [Low priority improvement 2]

---

## Audit Metadata

**Checklist Version**: 1.0
**Audit Methodology**: Spec Kit API Spec Audit Framework
**Total Audit Items**: 360
**Audit Completion**: [Date/Time]

### 5. Perform Automated Quality Checks

Beyond the checklist, perform automated validation:

**Syntax Checks**:
- Validate JSON examples are syntactically correct
- Validate Mermaid diagram syntax
- Check markdown table formatting

**Completeness Checks**:
- Count sections present vs. expected
- Flag any [NEEDS CLARIFICATION] markers
- Identify placeholder values (e.g., `[TODO]`, `TBD`)

**Consistency Checks**:
- Cross-reference error codes in responses with error catalog
- Verify path variables in URL match documented variables
- Check field names consistency between request/response
- Verify HTTP status codes in workflow match error responses

**Best Practice Checks**:
- Validate RESTful URL patterns
- Check HTTP method appropriateness
- Verify HTTP status code usage

### 6. Generate Quality Score

Calculate overall quality metrics:

```
Total Checks: 360
Passed: [count]
Failed: [count]
Warnings: [count]
Not Applicable: [count]

Quality Score = (Passed / (Total - Not Applicable)) * 100%

Readiness Levels:
- 95-100%: ✅ Ready for Implementation
- 80-94%: ⚠️ Minor Issues - Can proceed with caution
- Below 80%: ❌ Major Issues - Requires revision
```

### 7. Prioritize Issues

Categorize all failures by priority:

**HIGH (Must Fix)**:
- Missing critical sections (Basic Info, Response, Error Codes)
- Security vulnerabilities (no auth specified, sensitive data exposure)
- Ambiguous/conflicting requirements
- Invalid syntax (broken JSON, Mermaid errors)

**MEDIUM (Should Fix)**:
- Incomplete documentation (missing edge cases, validation rules)
- Inconsistencies (naming conventions, error formats)
- Missing best practices (pagination, rate limiting)

**LOW (Consider)**:
- Optional improvements (additional examples, enhanced clarity)
- Style/formatting issues
- Documentation quality enhancements

### 8. Report Completion

Output a comprehensive audit summary to the user:

## API Specification Audit Complete

**API**: [Name]
**Specification**: [File path]
**Audit Checklist**: [Audit file path]

### Quality Metrics
- **Overall Score**: X%
- **Total Checks**: 360
- **Passed**: X ✅
- **Failed**: X ❌
- **Warnings**: X ⚠️

### Readiness Status
[✅ Ready / ⚠️ Minor Issues / ❌ Major Issues]

### Critical Issues (Must Fix)
[Count] issues found:
1. [Issue summary]
2. [Issue summary]

### Important Warnings (Should Fix)
[Count] warnings found:
1. [Warning summary]
2. [Warning summary]

### Suggestions (Optional)
[Count] suggestions:
1. [Suggestion summary]

### Next Actions
1. Review the detailed checklist at: [path]
2. Address all critical issues
3. Consider fixing important warnings
4. Re-run `/speckit.api-spec-audit` after fixes to verify improvements

**Detailed Audit**: See `[audit-checklist-path]` for complete analysis.

---

## Quality Assurance Guidelines

### What This Audit Checks

✅ **Specification Quality** (We DO audit):
- Completeness of API documentation
- Clarity and specificity of requirements
- Consistency across sections
- RESTful design adherence
- Security considerations documented
- Error handling comprehensiveness
- Testability and implementation readiness

❌ **Implementation Quality** (We DON'T audit):
- Whether the actual API code works correctly
- Whether the implementation matches the spec
- Performance of the running system
- Actual security vulnerabilities in code
- Database query optimization in implementation

### Audit Principles

1. **Comprehensive**: Cover all aspects of API specification quality
2. **Objective**: Use measurable, verifiable criteria
3. **Actionable**: Provide specific, fixable recommendations
4. **Prioritized**: Clearly indicate what MUST vs. SHOULD vs. COULD be fixed
5. **Consistent**: Apply the same standards across all API specs
6. **Pragmatic**: Balance completeness with practical implementation needs

---

## Usage Examples

### Example 1: Audit specific API spec
```
/speckit.api-spec-audit .specify/features/user-management/specs/api/update-user-profile.md
```

### Example 2: Audit in feature context
```
/speckit.api-spec-audit Audit the update user profile API
```

### Example 3: Audit all APIs
```
/speckit.api-spec-audit Audit all API specifications
```

---

## Notes

- This command is designed for **API specification quality audit**
- Audits the SPECIFICATION document, not the implementation
- Generates a comprehensive 360-item checklist
- Provides quality scoring and readiness assessment
- Prioritizes issues by severity (High/Medium/Low)
- Can be run multiple times to verify improvements
- Audit checklist is saved alongside the API spec for reference
- Use this command before beginning API implementation to ensure spec quality
- Re-run after addressing issues to verify all items are resolved


# MYTH-2.0: The Smith of Dev - Design Document

## Overview

MYTH-2.0 is a comprehensive AI-orchestrated development platform built on Next.js 15 with TypeScript 5.9. The platform provides six specialized modules (Agent AI, MERN Orchestrator, Application AI, Data Insight Dashboard, Prompt AI, and URL AI) that work together to enable rapid application development through AI assistance.

The architecture follows a modular design where each module operates independently but shares common infrastructure for authentication, database access, sandboxing, and AI model integration. The platform uses E2B Code Interpreter SDK for secure code execution, MongoDB Atlas for persistent storage, and supports multiple AI models for flexibility and resilience.

Key design principles:
- **Modularity**: Each module is independently deployable and testable
- **Security**: All code execution happens in isolated sandboxes with resource limits
- **Scalability**: Multi-sandbox provisioning supports concurrent user operations
- **Resilience**: Automatic fallback between AI models and graceful error handling
- **Real-time**: Live preview and hot-reload capabilities for rapid iteration

## Architecture

### High-Level System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     Next.js 15 Frontend                          │
│  (TypeScript 5.9, Tailwind CSS 4, Framer Motion, Radix UI)      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    API Layer (Next.js Routes)                    │
│  ┌──────────────┬──────────────┬──────────────┬──────────────┐  │
│  │ Agent Routes │ MERN Routes  │ Mobile Routes│ Data Routes  │  │
│  └──────────────┴──────────────┴──────────────┴──────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│  E2B Sandboxes   │  │  AI Model APIs   │  │  Data Layer      │
│  (Code Exec)     │  │  (GPT-4o, etc)   │  │  (MongoDB/Turso) │
└──────────────────┘  └──────────────────┘  └──────────────────┘
```

### Module Architecture

Each module follows a consistent pattern:

```
Module (e.g., Agent AI)
├── Frontend Component (React)
├── API Routes (Next.js)
├── Service Layer (Business Logic)
├── Sandbox Manager (E2B Integration)
├── Database Models (MongoDB)
└── AI Integration (Model Selection & Routing)
```

## Components and Interfaces

### 1. Agent AI Module

**Purpose**: Enable visual node-based agent building using React Flow

**Key Components**:
- `NodeCanvas`: React Flow canvas component for drag-and-drop node creation
- `NodeFactory`: Creates typed nodes with input/output ports
- `AgentExecutor`: Orchestrates node execution and data flow
- `AgentPersistence`: Saves/loads agent configurations to MongoDB

**Interfaces**:

```typescript
interface Node {
  id: string;
  type: 'input' | 'output' | 'processor' | 'ai_model' | 'conditional';
  position: { x: number; y: number };
  data: {
    label: string;
    config: Record<string, unknown>;
    modelSelection?: 'gpt-4o' | 'claude-3.5-sonnet' | 'gemini-2.5-flash' | 'llama-3.3';
  };
}

interface Edge {
  id: string;
  source: string;
  target: string;
  data?: {
    label?: string;
    dataType?: string;
  };
}

interface AgentGraph {
  id: string;
  userId: string;
  nodes: Node[];
  edges: Edge[];
  createdAt: Date;
  updatedAt: Date;
}

interface ExecutionContext {
  agentId: string;
  nodeId: string;
  input: Record<string, unknown>;
  output?: Record<string, unknown>;
  status: 'pending' | 'running' | 'completed' | 'failed';
  error?: string;
}
```

**API Endpoints**:
- `POST /api/agents` - Create new agent
- `GET /api/agents/:id` - Retrieve agent configuration
- `PUT /api/agents/:id` - Update agent configuration
- `DELETE /api/agents/:id` - Delete agent
- `POST /api/agents/:id/execute` - Execute agent
- `GET /api/agents/:id/execution/:executionId` - Get execution status

### 2. MERN Orchestrator Module

**Purpose**: Generate and manage full-stack MERN applications with multi-sandbox provisioning

**Key Components**:
- `MERNGenerator`: Generates Express.js backend and React frontend code
- `SandboxProvisioner`: Creates and manages isolated sandbox environments
- `DatabaseConnector`: Manages MongoDB Atlas connections
- `HotReloadManager`: Implements hot-reload for development

**Interfaces**:

```typescript
interface MERNApplication {
  id: string;
  userId: string;
  name: string;
  description: string;
  sandboxId: string;
  mongodbUri: string;
  endpoints: {
    api: string;
    frontend: string;
  };
  status: 'provisioning' | 'running' | 'stopped' | 'error';
  createdAt: Date;
  updatedAt: Date;
}

interface SandboxInstance {
  id: string;
  applicationId: string;
  status: 'active' | 'inactive' | 'error';
  resourceUsage: {
    cpu: number;
    memory: number;
    disk: number;
  };
  limits: {
    cpuLimit: number;
    memoryLimit: number;
    diskLimit: number;
  };
}

interface APIEndpoint {
  method: 'GET' | 'POST' | 'PUT' | 'DELETE' | 'PATCH';
  path: string;
  handler: string;
  middleware?: string[];
  validation?: Record<string, unknown>;
}
```

**API Endpoints**:
- `POST /api/mern/applications` - Create new MERN application
- `GET /api/mern/applications/:id` - Get application details
- `PUT /api/mern/applications/:id` - Update application
- `DELETE /api/mern/applications/:id` - Delete application
- `POST /api/mern/applications/:id/deploy` - Deploy application
- `GET /api/mern/applications/:id/logs` - Get sandbox logs
- `POST /api/mern/applications/:id/code` - Update application code

### 3. Application AI Module (Mobile)

**Purpose**: Generate React Native Expo applications with live preview

**Key Components**:
- `ExpoGenerator`: Generates React Native Expo-compatible code
- `LivePreviewRenderer`: Renders app in sandbox with hot-reload
- `ComponentLibrary`: Provides Radix UI and Tailwind CSS components
- `ExportManager`: Generates production-ready project structure

**Interfaces**:

```typescript
interface MobileApplication {
  id: string;
  userId: string;
  name: string;
  description: string;
  components: ComponentDefinition[];
  screens: ScreenDefinition[];
  previewUrl?: string;
  status: 'generating' | 'ready' | 'previewing' | 'error';
  createdAt: Date;
  updatedAt: Date;
}

interface ComponentDefinition {
  id: string;
  name: string;
  type: 'button' | 'input' | 'card' | 'list' | 'custom';
  props: Record<string, unknown>;
  children?: ComponentDefinition[];
}

interface ScreenDefinition {
  id: string;
  name: string;
  route: string;
  components: ComponentDefinition[];
  navigation?: {
    type: 'stack' | 'tab' | 'drawer';
    transitions?: string[];
  };
}
```

**API Endpoints**:
- `POST /api/mobile/applications` - Create new mobile app
- `GET /api/mobile/applications/:id` - Get app details
- `PUT /api/mobile/applications/:id` - Update app
- `POST /api/mobile/applications/:id/preview` - Start live preview
- `GET /api/mobile/applications/:id/preview` - Get preview URL
- `POST /api/mobile/applications/:id/export` - Export project

### 4. Data Insight Dashboard Module

**Purpose**: Provide Python sandbox for data visualization with Streamlit

**Key Components**:
- `StreamlitSandbox`: Manages Streamlit runtime in E2B sandbox
- `DataUploadManager`: Handles dataset uploads and storage
- `CodeExecutor`: Executes Python code with error capture
- `DashboardRenderer`: Renders Streamlit output

**Interfaces**:

```typescript
interface Dashboard {
  id: string;
  userId: string;
  name: string;
  pythonCode: string;
  datasets: DatasetReference[];
  sandboxId: string;
  status: 'idle' | 'executing' | 'ready' | 'error';
  lastError?: string;
  createdAt: Date;
  updatedAt: Date;
}

interface DatasetReference {
  id: string;
  name: string;
  fileUrl: string;
  uploadedAt: Date;
  size: number;
}

interface ExecutionResult {
  dashboardId: string;
  output: string;
  executionTime: number;
  status: 'success' | 'error';
  error?: {
    message: string;
    traceback: string;
  };
}
```

**API Endpoints**:
- `POST /api/dashboards` - Create new dashboard
- `GET /api/dashboards/:id` - Get dashboard
- `PUT /api/dashboards/:id` - Update dashboard code
- `POST /api/dashboards/:id/execute` - Execute Python code
- `POST /api/dashboards/:id/datasets` - Upload dataset
- `GET /api/dashboards/:id/output` - Get rendered output
- `POST /api/dashboards/:id/export` - Export dashboard

### 5. Prompt AI Module

**Purpose**: Generate complete React applications from natural language prompts

**Key Components**:
- `PromptParser`: Parses natural language requirements
- `CodeGenerator`: Generates React/TypeScript code using AI
- `CodeValidator`: Validates generated code for syntax and type safety
- `PreviewManager`: Manages live preview with hot-reload

**Interfaces**:

```typescript
interface GeneratedApplication {
  id: string;
  userId: string;
  prompt: string;
  generatedCode: {
    components: Record<string, string>;
    pages: Record<string, string>;
    styles: Record<string, string>;
    config: Record<string, unknown>;
  };
  previewUrl?: string;
  status: 'generating' | 'ready' | 'error';
  generationTime: number;
  createdAt: Date;
  updatedAt: Date;
}

interface GenerationRequest {
  prompt: string;
  modelSelection?: 'gpt-4o' | 'claude-3.5-sonnet' | 'gemini-2.5-flash' | 'llama-3.3';
  preferences?: {
    styling?: 'minimal' | 'modern' | 'corporate';
    framework?: 'react' | 'next.js';
    includeTests?: boolean;
  };
}

interface CodeValidationResult {
  valid: boolean;
  errors: Array<{
    file: string;
    line: number;
    message: string;
    severity: 'error' | 'warning';
  }>;
}
```

**API Endpoints**:
- `POST /api/prompt-ai/generate` - Generate app from prompt
- `GET /api/prompt-ai/applications/:id` - Get generated app
- `PUT /api/prompt-ai/applications/:id` - Update generated app
- `POST /api/prompt-ai/applications/:id/preview` - Start preview
- `GET /api/prompt-ai/applications/:id/preview` - Get preview URL
- `POST /api/prompt-ai/applications/:id/export` - Export project
- `GET /api/prompt-ai/applications/:id/generation-status` - Get generation status

### 6. URL AI Module

**Purpose**: Clone and transform existing websites

**Key Components**:
- `WebsiteFetcher`: Fetches and parses website HTML/CSS/JS
- `WebsiteCloner`: Creates local copy of website
- `TransformationEngine`: Applies modifications to cloned website
- `DependencyBundler`: Bundles external dependencies

**Interfaces**:

```typescript
interface ClonedWebsite {
  id: string;
  userId: string;
  sourceUrl: string;
  clonedAt: Date;
  files: FileEntry[];
  dependencies: DependencyEntry[];
  status: 'cloning' | 'ready' | 'error';
  lastError?: string;
  previewUrl?: string;
  createdAt: Date;
  updatedAt: Date;
}

interface FileEntry {
  path: string;
  type: 'html' | 'css' | 'js' | 'image' | 'other';
  content: string;
  size: number;
}

interface DependencyEntry {
  name: string;
  version: string;
  type: 'npm' | 'cdn' | 'local';
  url?: string;
}

interface TransformationRequest {
  websiteId: string;
  modifications: Array<{
    type: 'replace' | 'remove' | 'add' | 'modify';
    selector?: string;
    content?: string;
    attributes?: Record<string, string>;
  }>;
}
```

**API Endpoints**:
- `POST /api/url-ai/clone` - Clone website from URL
- `GET /api/url-ai/clones/:id` - Get cloned website
- `POST /api/url-ai/clones/:id/transform` - Apply transformations
- `GET /api/url-ai/clones/:id/preview` - Get preview
- `POST /api/url-ai/clones/:id/export` - Export project
- `GET /api/url-ai/clones/:id/status` - Get cloning status

### 7. Shared Infrastructure

**AI Model Router**:
```typescript
interface AIModelConfig {
  provider: 'openai' | 'anthropic' | 'google' | 'meta';
  model: 'gpt-4o' | 'claude-3.5-sonnet' | 'gemini-2.5-flash' | 'llama-3.3';
  apiKey: string;
  baseUrl?: string;
}

interface ModelRequest {
  prompt: string;
  model: AIModelConfig;
  temperature?: number;
  maxTokens?: number;
  systemPrompt?: string;
}

interface ModelResponse {
  content: string;
  model: string;
  tokensUsed: number;
  executionTime: number;
}
```

**Sandbox Manager**:
```typescript
interface SandboxConfig {
  runtime: 'node' | 'python' | 'browser';
  cpuLimit: number;
  memoryLimit: number;
  diskLimit: number;
  timeoutSeconds: number;
}

interface SandboxExecution {
  sandboxId: string;
  code: string;
  language: string;
  status: 'pending' | 'running' | 'completed' | 'failed' | 'timeout';
  output?: string;
  error?: string;
  executionTime: number;
}
```

**Authentication & Authorization**:
```typescript
interface User {
  id: string;
  email: string;
  role: 'admin' | 'developer' | 'viewer';
  permissions: string[];
  createdAt: Date;
  updatedAt: Date;
}

interface Session {
  userId: string;
  token: string;
  expiresAt: Date;
  createdAt: Date;
}
```

## Data Models

### MongoDB Collections

**agents**:
```typescript
{
  _id: ObjectId,
  userId: string,
  name: string,
  description: string,
  nodes: Node[],
  edges: Edge[],
  createdAt: Date,
  updatedAt: Date,
  isPublic: boolean,
  tags: string[]
}
```

**mern_applications**:
```typescript
{
  _id: ObjectId,
  userId: string,
  name: string,
  description: string,
  sandboxId: string,
  mongodbUri: string,
  apiEndpoints: APIEndpoint[],
  status: string,
  createdAt: Date,
  updatedAt: Date
}
```

**mobile_applications**:
```typescript
{
  _id: ObjectId,
  userId: string,
  name: string,
  description: string,
  components: ComponentDefinition[],
  screens: ScreenDefinition[],
  status: string,
  createdAt: Date,
  updatedAt: Date
}
```

**dashboards**:
```typescript
{
  _id: ObjectId,
  userId: string,
  name: string,
  pythonCode: string,
  datasets: DatasetReference[],
  sandboxId: string,
  status: string,
  createdAt: Date,
  updatedAt: Date
}
```

**generated_applications**:
```typescript
{
  _id: ObjectId,
  userId: string,
  prompt: string,
  generatedCode: object,
  status: string,
  generationTime: number,
  createdAt: Date,
  updatedAt: Date
}
```

**cloned_websites**:
```typescript
{
  _id: ObjectId,
  userId: string,
  sourceUrl: string,
  files: FileEntry[],
  dependencies: DependencyEntry[],
  status: string,
  createdAt: Date,
  updatedAt: Date
}
```

**users**:
```typescript
{
  _id: ObjectId,
  email: string,
  passwordHash: string,
  role: string,
  permissions: string[],
  createdAt: Date,
  updatedAt: Date
}
```

## Error Handling

### Error Categories

1. **Validation Errors** (400):
   - Invalid input parameters
   - Missing required fields
   - Type mismatches

2. **Authentication Errors** (401):
   - Invalid credentials
   - Expired tokens
   - Missing authentication

3. **Authorization Errors** (403):
   - Insufficient permissions
   - Resource access denied

4. **Not Found Errors** (404):
   - Resource doesn't exist
   - Invalid resource ID

5. **Sandbox Errors** (500):
   - Code execution failures
   - Resource limit exceeded
   - Timeout exceeded

6. **AI Model Errors** (503):
   - Model API unavailable
   - Rate limit exceeded
   - Invalid model response

### Error Response Format

```typescript
interface ErrorResponse {
  error: {
    code: string;
    message: string;
    details?: Record<string, unknown>;
    timestamp: Date;
    requestId: string;
  };
}
```

### Fallback Strategies

- **AI Model Fallback**: If primary model fails, automatically retry with secondary model
- **Sandbox Fallback**: If sandbox creation fails, retry with exponential backoff
- **Database Fallback**: If MongoDB unavailable, queue operations for retry

## Testing Strategy

### Unit Testing Approach

Unit tests focus on specific examples, edge cases, and error conditions:

- **Component Tests**: Test React components in isolation with mock data
- **Service Tests**: Test business logic with various input scenarios
- **Validation Tests**: Test input validation and error handling
- **Integration Tests**: Test component interactions and data flow

### Property-Based Testing Approach

Property-based tests verify universal properties across all inputs:

- **Code Generation Properties**: Verify generated code is syntactically valid
- **Data Persistence Properties**: Verify data round-trips correctly
- **Sandbox Isolation Properties**: Verify sandbox operations don't affect other instances
- **AI Model Properties**: Verify model responses are consistent and valid

### Test Configuration

- Minimum 100 iterations per property test
- Each property test tagged with feature name and property number
- Unit tests for specific examples and edge cases
- Property tests for universal correctness properties

## Correctness Properties

A property is a characteristic or behavior that should hold true across all valid executions of a system—essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.

### Property-Based Testing Overview

Property-based testing (PBT) validates software correctness by testing universal properties across many generated inputs. Each property is a formal specification that should hold for all valid inputs.

### Core Principles

1. **Universal Quantification**: Every property must contain an explicit "for all" statement
2. **Requirements Traceability**: Each property must reference the requirements it validates
3. **Executable Specifications**: Properties must be implementable as automated tests
4. **Comprehensive Coverage**: Properties should cover all testable acceptance criteria

### Correctness Properties

#### Agent AI Module Properties

Property 1: Node Rendering with Ports
*For any* node type added to the canvas, the rendered node SHALL have properly configured input and output ports matching the node type specification.
**Validates: Requirements 1.2**

Property 2: Connection Validation
*For any* pair of nodes with compatible port types, connecting them SHALL establish a valid data flow path that can be traversed during execution.
**Validates: Requirements 1.3**

Property 3: Model Selection Support
*For any* AI model selection (GPT-4o, Claude 3.5 Sonnet, Gemini 2.5 Flash, Llama 3.3), the node configuration SHALL accept and store the selection for later execution.
**Validates: Requirements 1.4**

Property 4: Agent Configuration Round Trip
*For any* valid agent graph, saving it to MongoDB and retrieving it SHALL produce an equivalent graph structure with all nodes, edges, and configurations preserved.
**Validates: Requirements 1.5**

Property 5: Data Flow Through Nodes
*For any* connected agent graph with valid input data, executing the agent SHALL pass data through connected nodes in topological order, with each node receiving output from its predecessors.
**Validates: Requirements 1.6**

Property 6: Error Propagation and Halting
*For any* agent execution where a node fails, the System SHALL log the error, prevent execution of dependent nodes, and return a descriptive error message containing the failed node ID and error details.
**Validates: Requirements 1.7**

#### MERN Orchestrator Properties

Property 7: Sandbox Provisioning
*For any* MERN application creation request, the System SHALL provision a dedicated sandbox with Node.js runtime and return a unique sandbox ID.
**Validates: Requirements 2.1**

Property 8: Database Connection Validation
*For any* MongoDB Atlas credentials provided, the System SHALL validate the connection by executing a test query and return success or failure with specific error details.
**Validates: Requirements 2.2**

Property 9: API Endpoint Generation
*For any* API endpoint specification, the System SHALL generate valid Express.js route code with proper request/response handling and middleware integration.
**Validates: Requirements 2.3**

Property 10: Unique Deployment Endpoints
*For any* MERN application deployment, the System SHALL provision it in an isolated sandbox and return a unique endpoint URL that is not shared with other applications.
**Validates: Requirements 2.4**

Property 11: Sandbox Isolation
*For any* two simultaneously running MERN applications, operations in one sandbox SHALL not affect the state, files, or execution of the other sandbox.
**Validates: Requirements 2.5**

Property 12: Hot Reload Performance
*For any* code modification in a running MERN application, the System SHALL detect the change, reload the sandbox, and reflect the changes within 2 seconds.
**Validates: Requirements 2.6**

Property 13: Resource Limit Enforcement
*For any* sandbox that exceeds its configured CPU, memory, or disk limits, the System SHALL gracefully terminate the application and provide resource usage details to the user.
**Validates: Requirements 2.7**

#### Application AI (Mobile) Properties

Property 14: React Native Code Generation
*For any* mobile app specification, the System SHALL generate syntactically valid React Native Expo-compatible code that can be executed in an Expo environment.
**Validates: Requirements 3.1**

Property 15: Live Preview Rendering
*For any* generated mobile application, requesting a live preview SHALL render the application in a sandbox and make it accessible via a preview URL.
**Validates: Requirements 3.2**

Property 16: Preview Update Performance
*For any* component modification in a mobile app, the System SHALL update the live preview within 1 second of the modification.
**Validates: Requirements 3.3**

Property 17: Export Project Structure
*For any* mobile application export, the System SHALL generate a valid Expo project structure with all necessary configuration files and dependencies.
**Validates: Requirements 3.4**

Property 18: Component Availability
*For any* UI component selection request, the System SHALL provide Radix UI and Tailwind CSS styled components that are properly integrated and functional.
**Validates: Requirements 3.5**

Property 19: Error Highlighting and Suggestions
*For any* generated code containing syntax errors, the System SHALL identify the errors, highlight them in the preview, and provide correction suggestions.
**Validates: Requirements 3.6**

#### Data Insight Dashboard Properties

Property 20: Streamlit Sandbox Setup
*For any* dashboard creation request, the System SHALL provision a Python sandbox with Streamlit runtime and make it ready for code execution.
**Validates: Requirements 4.1**

Property 21: Python Code Execution
*For any* valid Python code for data visualization, the System SHALL execute it in the sandbox and render the Streamlit interface with the expected output.
**Validates: Requirements 4.2**

Property 22: Dataset Accessibility
*For any* dataset uploaded to a dashboard, the System SHALL store it in the sandbox and make it accessible to Python scripts via standard file paths.
**Validates: Requirements 4.3**

Property 23: Dashboard Re-execution Performance
*For any* modification to visualization code, the System SHALL re-execute the code and update the dashboard within 3 seconds.
**Validates: Requirements 4.4**

Property 24: Dashboard Export
*For any* dashboard export request, the System SHALL generate a standalone Streamlit application with all code and dependencies included.
**Validates: Requirements 4.5**

Property 25: Python Error Capture
*For any* Python code execution that fails, the System SHALL capture the complete error traceback and display it to the user for debugging.
**Validates: Requirements 4.6**

#### Prompt AI Properties

Property 26: React App Generation from Prompt
*For any* natural language prompt describing an application, the System SHALL generate a complete React application structure with components, pages, and configuration.
**Validates: Requirements 5.1**

Property 27: TypeScript and Tailwind Usage
*For any* generated React application, the code SHALL use TypeScript for type safety and Tailwind CSS for styling throughout all components.
**Validates: Requirements 5.2**

Property 28: Generation Performance
*For any* standard app generation request, the System SHALL complete generation within 30 seconds.
**Validates: Requirements 5.3**

Property 29: Generated App Preview
*For any* generated application, the System SHALL render it in a sandbox with live hot-reload capability and make it accessible via preview URL.
**Validates: Requirements 5.4**

Property 30: Modification Preservation
*For any* user modification to generated code, the System SHALL preserve the modifications and allow iterative refinement without losing changes.
**Validates: Requirements 5.5**

Property 31: Production-Ready Export
*For any* generated application export, the System SHALL provide a complete, production-ready React project structure with all necessary configuration and dependencies.
**Validates: Requirements 5.6**

Property 32: Model Fallback on Generation Failure
*For any* code generation request where the primary AI model fails, the System SHALL automatically retry with an alternative model and notify the user of the fallback.
**Validates: Requirements 5.7**

#### URL AI Properties

Property 33: Website Fetching and Parsing
*For any* valid website URL, the System SHALL fetch and parse the HTML, CSS, and JavaScript content successfully.
**Validates: Requirements 6.1**

Property 34: Layout and Styling Preservation
*For any* cloned website, the visual layout and styling SHALL be preserved in the cloned version, maintaining visual fidelity to the original.
**Validates: Requirements 6.2**

Property 35: Transformation with Structural Integrity
*For any* website transformation request, the System SHALL apply modifications while maintaining the structural integrity of the HTML document.
**Validates: Requirements 6.3**

Property 36: Real-Time Preview Updates
*For any* modification to cloned website code, the System SHALL update the live preview in real-time, reflecting changes immediately.
**Validates: Requirements 6.4**

Property 37: Standalone Website Export
*For any* cloned website export, the System SHALL generate a standalone HTML/CSS/JavaScript project that can be deployed independently.
**Validates: Requirements 6.5**

Property 38: External Dependency Handling
*For any* website using external dependencies, the System SHALL either bundle them or provide fallback implementations to ensure functionality.
**Validates: Requirements 6.6**

Property 39: Website Fetch Error Handling
*For any* website fetch failure, the System SHALL return a descriptive error message indicating the specific reason for failure (network error, invalid URL, timeout, etc.).
**Validates: Requirements 6.7**

#### Multi-Model AI Support Properties

Property 40: Model Selection Support
*For any* AI agent or generation task configuration, the System SHALL allow selection from all four supported models (GPT-4o, Claude 3.5 Sonnet, Gemini 2.5 Flash, Llama 3.3).
**Validates: Requirements 7.1**

Property 41: Model Request Routing
*For any* request with a specified AI model, the System SHALL route the request to the appropriate model API endpoint.
**Validates: Requirements 7.2**

Property 42: Automatic Model Fallback
*For any* AI model API that becomes unavailable, the System SHALL automatically fallback to an alternative model and continue processing.
**Validates: Requirements 7.3**

Property 43: Model Parameter Application
*For any* specified model parameters (temperature, max_tokens, etc.), the System SHALL apply them to the model request and respect the configured values.
**Validates: Requirements 7.4**

Property 44: Context Preservation During Model Switch
*For any* mid-workflow model switch, the System SHALL maintain the execution context and continue processing without losing state or progress.
**Validates: Requirements 7.5**

#### Sandbox Isolation and Security Properties

Property 45: Sandbox Code Execution
*For any* code execution request, the System SHALL run the code in an E2B Code Interpreter sandbox with enforced resource limits.
**Validates: Requirements 8.1**

Property 46: Resource Quota Enforcement
*For any* sandbox creation, the System SHALL enforce configured CPU, memory, and disk quotas and prevent exceeding these limits.
**Validates: Requirements 8.2**

Property 47: Unauthorized Resource Access Blocking
*For any* attempt by user code to access unauthorized system resources, the System SHALL block the access and log the attempt with details.
**Validates: Requirements 8.3**

Property 48: Sandbox Cleanup
*For any* sandbox session that ends, the System SHALL clean up all resources and remove temporary files, leaving no residual state.
**Validates: Requirements 8.4**

Property 49: Multi-User Sandbox Isolation
*For any* two users running code simultaneously, the System SHALL maintain complete isolation between their sandbox instances with no cross-contamination.
**Validates: Requirements 8.5**

Property 50: Resource Limit Termination
*For any* sandbox that exceeds its resource limits, the System SHALL terminate the process and prevent resource exhaustion of the host system.
**Validates: Requirements 8.6**

#### Data Persistence Properties

Property 51: MongoDB Credential Validation
*For any* MongoDB Atlas credentials provided, the System SHALL validate the connection and store credentials securely for later use.
**Validates: Requirements 9.1**

Property 52: Data Persistence Round Trip
*For any* data written by an application, the System SHALL persist it to the configured database (MongoDB Atlas or Turso) and retrieve it identically on query.
**Validates: Requirements 9.2**

Property 53: Query Performance
*For any* database query, the System SHALL retrieve results from the configured database and return them within 500ms.
**Validates: Requirements 9.3**

Property 54: Database Connection Retry Logic
*For any* database connection failure, the System SHALL retry with exponential backoff and notify the user after 3 failed attempts.
**Validates: Requirements 9.4**

Property 55: Database Configuration Export
*For any* application export, the System SHALL include database connection configuration in the exported project for seamless deployment.
**Validates: Requirements 9.5**

Property 56: Credential Rotation Without Data Loss
*For any* database credential rotation, the System SHALL support updating credentials without losing any persisted data.
**Validates: Requirements 9.6**

#### Real-Time Collaboration Properties

Property 57: Real-Time State Updates
*For any* configuration modification, the System SHALL update the state in real-time across all connected clients within 500ms.
**Validates: Requirements 10.1**

Property 58: Conflict Resolution
*For any* concurrent edits to the same resource by multiple users, the System SHALL implement conflict resolution and notify users of conflicts.
**Validates: Requirements 10.2**

Property 59: State Preservation During Navigation
*For any* navigation between modules, the System SHALL preserve the application state and allow seamless transitions without losing context.
**Validates: Requirements 10.3**

Property 60: State Restoration After Refresh
*For any* browser refresh, the System SHALL restore the previous state from persistent storage, allowing users to continue where they left off.
**Validates: Requirements 10.4**

Property 61: Session Expiration Handling
*For any* expired user session, the System SHALL gracefully disconnect and prompt re-authentication without data loss.
**Validates: Requirements 10.5**

#### Error Handling Properties

Property 62: User-Friendly Error Messages
*For any* operation failure, the System SHALL display a user-friendly error message with actionable suggestions for resolution.
**Validates: Requirements 11.1**

Property 63: Code Generation Error Details
*For any* code generation failure, the System SHALL provide the specific line or component that caused the failure for debugging.
**Validates: Requirements 11.2**

Property 64: Sandbox Timeout Notifications
*For any* sandbox operation timeout, the System SHALL notify the user and suggest increasing timeout or simplifying the operation.
**Validates: Requirements 11.3**

Property 65: Invalid Action Prevention
*For any* invalid user action, the System SHALL prevent the action and explain why it's invalid with specific guidance.
**Validates: Requirements 11.4**

Property 66: Unexpected Error Logging
*For any* unexpected error, the System SHALL log the error with full context and provide a support ticket reference to the user.
**Validates: Requirements 11.5**

#### Performance Properties

Property 67: Initial Load Performance
*For any* platform load request, the System SHALL render the initial interface within 2 seconds.
**Validates: Requirements 12.1**

Property 68: Code Generation Performance
*For any* standard code generation request, the System SHALL complete generation within 30 seconds.
**Validates: Requirements 12.2**

Property 69: Preview Render Performance
*For any* application preview request, the System SHALL render the preview within 1 second.
**Validates: Requirements 12.3**

Property 70: Search Performance
*For any* search or filter operation, the System SHALL return results within 500ms.
**Validates: Requirements 12.4**

Property 71: Large Dataset Pagination
*For any* large dataset processing, the System SHALL implement pagination and lazy loading to handle data efficiently.
**Validates: Requirements 12.5**

Property 72: Graceful Degradation
*For any* slow network connection, the System SHALL gracefully degrade functionality and provide offline capabilities where possible.
**Validates: Requirements 12.6**

#### Authentication and Authorization Properties

Property 73: Login Authentication
*For any* login attempt with valid credentials, the System SHALL authenticate the user and issue a secure session token.
**Validates: Requirements 13.1**

Property 74: Resource Authorization
*For any* resource access request, the System SHALL verify authorization and enforce access control based on user permissions.
**Validates: Requirements 13.2**

Property 75: Permission Updates
*For any* user role change, the System SHALL update permissions immediately and reflect changes in subsequent access control decisions.
**Validates: Requirements 13.3**

Property 76: Logout Session Invalidation
*For any* user logout, the System SHALL invalidate the session token and clear sensitive data from memory and storage.
**Validates: Requirements 13.4**

Property 77: Unauthorized Access Logging
*For any* unauthorized access attempt, the System SHALL log the attempt with user ID, resource, and timestamp for security auditing.
**Validates: Requirements 13.5**

#### API Endpoint Properties

Property 78: API Request Validation
*For any* API endpoint call, the System SHALL validate the request format and return a properly formatted response or validation error.
**Validates: Requirements 14.1**

Property 79: API Authentication and Rate Limiting
*For any* API request with authentication, the System SHALL verify the token and enforce rate limiting to prevent abuse.
**Validates: Requirements 14.2**

Property 80: API Input Validation
*For any* API endpoint processing data, the System SHALL validate input and return validation errors with specific field information.
**Validates: Requirements 14.3**

Property 81: HTTP Status Code Correctness
*For any* API operation completion, the System SHALL return appropriate HTTP status codes (200, 201, 400, 401, 404, 500) based on the operation result.
**Validates: Requirements 14.4**

Property 82: 404 Error Handling
*For any* API request for a non-existent resource, the System SHALL return a 404 error with a descriptive message.
**Validates: Requirements 14.5**


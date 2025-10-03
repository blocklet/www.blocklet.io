# Blocklet Platform Architecture

## Overview

The Blocklet Platform is a comprehensive ecosystem for developing, deploying, and managing modern web applications. It provides a standardized runtime environment (Blocklet Server), development tools, and application framework with built-in Decentralized Identity (DID) support.

**Platform Analogy**:

- **Blocklet** = Applications (like apps from App Store)
- **Blocklet Server** = Runtime Environment (like iOS for devices)
- **Blocklet Store** = Application Marketplace (like App Store for distribution)

## Core Components

### 1. Blocklet Server (@core/webapp)

**Purpose**: Main dashboard and control center for managing blocklets and server configuration.

**Core Features**:

- **Web Dashboard**: React-based management interface with Material-UI components
- **GraphQL API Server**: Centralized API using Express.js and GraphQL for data operations
- **Authentication & Authorization**: DID-based authentication with JWT tokens and role-based access control
- **Blocklet Lifecycle Management**: Install, start, stop, update, and remove blocklets
- **User Management**: Multi-user support with team collaboration features
- **Settings & Configuration**: Server configuration, SSL certificates, domain management
- **Real-time Communication**: WebSocket support for live updates and notifications
- **Audit Logging**: Comprehensive logging and monitoring of server activities

**Technology Stack**:

- Frontend: React 19, Material-UI 7, Vite, TypeScript
- Backend: Node.js, Express, GraphQL, Sequelize ORM
- Database: SQLite (with PostgreSQL migration support)
- Real-time: WebSocket, GraphQL subscriptions
- Testing: Jest, Cypress E2E

**Key Files**:

- `api/index.js` - Main Express server with GraphQL endpoints
- `src/App.jsx` - React application entry point
- `api/gql/` - GraphQL schema and resolvers

### 2. Blocklet Services (@core/blocklet-services)

**Purpose**: Provides unified, standardized services that every blocklet can utilize without reimplementation.

**Core Features**:

- **Multi-Channel Authentication Services**: DID Connect, Passkey, email, OAuth (Google, Apple, GitHub, Twitter), passport management, session handling
- **Form Services**: Form builder and collector for dynamic form creation
- **Multi-Channel Communication Services**: Email services with React Email templates, Slack integration, webhook notifications
- **Theme Management**: Dynamic theming and branding capabilities
- **File Services**: Avatar management, file uploads, and storage
- **API Documentation**: OpenAPI/Swagger documentation generation
- **Studio Tools**: Preferences management, localization support
- **Enhanced Security Services**: XSS protection, CORS handling, rate limiting, request blocking, mod_security-based WAF

**Service Endpoints**:

- Public: `/well-known/service/health`, `/well-known/service/login`
- Internal: `/well-known/service/api/gql`, `/well-known/service/api/blocklet/*`
- DID Connect: `/api/connect/relay/*`, `/api/did/*`
- WebSocket: `/api/connect/relay/websocket`, `/websocket`

**Technology Stack**:

- Backend: Node.js, Express, TypeScript
- Frontend: React 19, Material-UI, Vite
- Email: React Email, Nodemailer
- Authentication: DID Connect, JWT, Auth0 integration
- Database: Sequelize ORM support

### 3. Blocklet Framework (@blocklet/meta)

**Purpose**: Core library for parsing, validating, and managing blocklet metadata and specifications.

**Core Features**:

- **Metadata Parsing**: Parse and validate `blocklet.yml` configurations
- **Schema Validation**: Comprehensive validation using Joi schemas with semver support
- **Blocklet Specification**: Define interfaces, services, and deployment requirements
- **DID Management**: Generate and manage blocklet DIDs and wallets
- **Engine Detection**: Detect and validate blocklet runtime engines
- **Security**: Multi-signature verification and response signing
- **File Operations**: List, select, update, and read blocklet files
- **Type Safety**: Full TypeScript support with generated types

**Key Capabilities**:

```typescript
// Example usage
import { parse, validate, constants } from '@blocklet/meta';
const meta = parse('/path/to/blocklet');
const isValid = validate(meta);
```

**Technology Stack**:

- Language: TypeScript with complete type definitions
- Validation: Joi with semver extension, AJV
- Crypto: DID utilities, wallet management
- Utils: fs-extra, lodash, js-yaml

### 4. Blocklet CLI (@core/cli)

**Purpose**: Command-line interface for managing both blocklets and Blocklet Server instances.

**Core Features**:

#### Blocklet Management Commands:

- `blocklet init` - Create new blocklet projects from templates
- `blocklet dev` - Development server with hot reloading
- `blocklet bundle` - Bundle blocklets for deployment
- `blocklet deploy` - Deploy to Blocklet Server instances
- `blocklet upload` - Publish to Blocklet Store
- `blocklet version` - Version management with semantic versioning

#### Server Management Commands:

- `blocklet server init` - Initialize new Blocklet Server instances
- `blocklet server start/stop` - Server lifecycle management
- `blocklet server status` - Monitor server and blocklet status
- `blocklet server logs` - Access server and blocklet logs
- `blocklet server upgrade` - Self-upgrade capabilities

#### Advanced Features:

- **Interactive Prompts**: User-friendly CLI with autocomplete and fuzzy search
- **Configuration Management**: YAML-based configuration with environment support
- **Template System**: Multiple starter templates for different blocklet types
- **Development Tools**: Live reload, debugging support, bundling optimization
- **Blocklet Store Integration**: Connect to and manage multiple Blocklet Stores

**Technology Stack**:

- Core: Node.js, Commander.js for CLI framework
- UI: Inquirer.js, chalk for colored output, ora for spinners
- Build: Webpack with NCC compiler, archiver for bundling
- Utils: fs-extra, glob, semver, git integration

### 5. Blocklet SDK Node.js (@blocklet/node-sdk)

**Purpose**: Server-side SDK providing backend services and integrations for blocklet development.

**Core Features**:

#### Authentication & Security:

- **Multi-Channel Authentication**: DID wallet, Passkey, email, OAuth (Google, Apple, GitHub, Twitter, etc.)
- **Wallet Integration**: DID wallet management and cryptographic operations
- **DID Connect**: Server-side DID Connect authentication flows
- **JWT Management**: Token generation, validation, and refresh mechanisms
- **Permission System**: Role-based access control and authorization

#### Services:

- **Blocklet Service**: Core blocklet operations and lifecycle management
- **Multi-Channel Notification Service**: Browser notifications, native push, email, Slack, webhooks
- **Database Integration**: NeDB embedded database with migration support
- **Component System**: Reusable server-side components

#### Middleware & Utils:

- **Express Middleware**: Ready-to-use middleware for common operations
- **Error Handling**: Structured error handling with Blocklet Error types
- **Configuration**: Environment-based configuration management
- **Logging**: Structured logging with debug support

**Key APIs**:

```typescript
import { BlockletService, Notification, Database, getWallet } from '@blocklet/sdk';

// Authentication
const authenticator = new BlockletAuthenticator();

// Database operations
const db = new Database('my-collection');

// Notifications
const notification = new Notification();
```

**Technology Stack**:

- Runtime: Node.js with TypeScript support
- Database: NeDB (embedded NoSQL)
- Authentication: DID Connect, JWT
- HTTP: Axios for external requests
- Utils: Lodash, semver, LRU cache

### 6. Blocklet SDK Browser (@blocklet/js-sdk)

**Purpose**: Client-side SDK for frontend blocklet development with browser-specific functionality.

**Core Features**:

#### Client Services:

- **Authentication Service**: Frontend authentication flows and session management
- **User Session Service**: Client-side session state management
- **Token Service**: Automatic token refresh and storage management
- **Blocklet Service**: Client-side blocklet information and operations
- **Federated Service**: Cross-blocklet communication and federation

#### HTTP & Communication:

- **Axios Integration**: Pre-configured HTTP client with interceptors
- **Fetch API Support**: Modern fetch-based request handling
- **CSRF Protection**: Built-in CSRF token management
- **Automatic Retries**: Intelligent retry mechanisms for failed requests

#### State Management:

- **Token Management**: Secure token storage using cookies
- **Cache Layer**: LRU cache for optimized performance
- **Session Persistence**: Persistent session state across browser sessions

**Key APIs**:

```typescript
import { BlockletSDK } from '@blocklet/js-sdk';

const sdk = new BlockletSDK();

// User authentication
await sdk.user.login(credentials);

// Blocklet operations
const blockletInfo = await sdk.blocklet.getInfo();

// Federated operations
await sdk.federated.communicate(targetBlocklet, data);
```

**Technology Stack**:

- Build: Unbuild for modern bundling
- HTTP: Axios with interceptors
- Storage: js-cookie for secure cookie management
- Cache: quick-lru for performance optimization
- Utils: Lodash, UF

## Architecture Patterns

### Clean Architecture

The platform follows clean architecture principles with clear separation of concerns:

- **Entities**: Core business logic in `@blocklet/meta` and `@blocklet/constant`
- **Use Cases**: Service layers in SDKs and core services
- **Interfaces**: HTTP APIs, GraphQL schemas, CLI commands
- **Frameworks**: Express.js, React, database adapters

### Microservices Design

- **Blocklet Server**: Main orchestration service
- **Blocklet Services**: Shared service layer
- **Individual Blocklets**: Independent microservices
- **Gateway Routing**: Nginx-based routing with Node.js fallback

### Event-Driven Architecture

- **WebSocket Communication**: Real-time updates and notifications
- **Event Hub**: Centralized event management
- **Pub/Sub Patterns**: Decoupled component communication
- **Audit Logging**: Event sourcing for compliance

## Development Workflow

### 1. Blocklet Development Lifecycle

```bash
# Initialize new blocklet
blocklet init my-app --template react

# Development with hot reload
blocklet dev

# Bundle for deployment
blocklet bundle

# Deploy to server
blocklet deploy .blocklet/bundle --endpoint http://server.example.com
```

### 2. Server Management Lifecycle

```bash
# Initialize server
blocklet server init

# Start server
blocklet server start

# Monitor status
blocklet server status

# View logs
blocklet server logs
```

### 3. Blocklet Store Integration

```bash
# Connect to Blocklet Store
blocklet connect https://store.blocklet.io

# Upload blocklet
blocklet upload .blocklet/bundle
```

## Security Architecture

### DID-Based Authentication

- **Decentralized Identity**: Self-sovereign identity management
- **Cryptographic Verification**: Ed25519 signature verification
- **Multi-Factor Authentication**: DID + traditional auth methods
- **Zero-Knowledge Proofs**: Privacy-preserving authentication

### API Security

- **JWT Tokens**: Stateless authentication with refresh mechanisms
- **CORS Protection**: Configurable cross-origin resource sharing
- **Advanced Rate Limiting**: Built-in rate limiting and DDoS protection
- **Request Blocking**: Intelligent request filtering and blocking
- **Web Application Firewall**: mod_security-based WAF for advanced threat protection
- **Input Validation**: Comprehensive input sanitization and validation

### Blocklet Isolation

- **Process Isolation**: Each blocklet runs in isolated processes
- **Network Isolation**: Controlled inter-blocklet communication
- **File System Isolation**: Restricted file system access
- **Resource Limits**: CPU and memory constraints

## Data Flow Architecture

### Request Processing Flow

1. **Gateway Router** (Nginx/Node.js) receives requests
2. **Authentication Middleware** validates tokens/sessions
3. **Blocklet Services** provide shared functionality
4. **Target Blocklet** processes business logic
5. **Response** flows back through the same path

### Inter-Blocklet Communication

1. **Service Discovery**: Automatic blocklet discovery via registry
2. **API Gateway**: Centralized routing and load balancing
3. **Authentication Proxy**: Transparent authentication forwarding
4. **Event Messaging**: Asynchronous event-based communication

## Deployment Architecture

### Single Server Deployment

- **All-in-One**: Complete platform on single server
- **SQLite Database**: Embedded database for simplicity
- **Local Storage**: File-based storage for assets
- **Process Management**: PM2 for process orchestration

### Clustered Deployment

- **Load Balancing**: Nginx-based load balancing
- **Database Clustering**: PostgreSQL with replication
- **Shared Storage**: Network file systems for assets
- **Auto-Scaling**: Horizontal scaling based on load

### Cloud-Native Deployment

- **Container Support**: Docker-based deployment
- **Kubernetes Integration**: K8s manifests and operators
- **Cloud Storage**: S3-compatible object storage
- **Monitoring**: Integrated monitoring and alerting

## Storage Architecture

### Extensible Storage Engine

The Blocklet Platform supports multiple database backends through an extensible storage engine architecture, allowing you to choose the most appropriate database for your deployment scale and requirements.

#### Supported Storage Engines

**1. SQLite (Default)**

- **Use Case**: Development, testing, and small to medium deployments
- **Advantages**:
  - Zero configuration required
  - Embedded database (no separate server needed)
  - Single-file storage for easy backup and migration
  - Excellent performance for read-heavy workloads
  - Perfect for single-server deployments
- **Limitations**:
  - Limited concurrent write performance
  - Not suitable for clustered deployments
  - Maximum database size limitations

**2. PostgreSQL**

- **Use Case**: Production deployments, clustered environments, and large-scale applications
- **Advantages**:
  - Superior concurrent access and write performance
  - Advanced features (JSON support, full-text search, extensions)
  - Robust replication and high availability
  - Horizontal scaling with read replicas
  - ACID compliance for critical data
- **Configuration**: Requires connection credentials and server setup

## Graceful Reload System

The Blocklet Platform implements a sophisticated graceful reload system that enables zero-downtime updates for both the Blocklet Server and individual blocklets, ensuring continuous service availability during updates and configuration changes.

### Core Concepts

**Graceful Reload**: The process of updating server or blocklet code/configuration without dropping active connections or interrupting ongoing requests.
**Zero-Downtime Updates**: Ability to deploy new versions while maintaining service availability for end users.

## Performance Optimizations

### Frontend Optimizations

- **Code Splitting**: Dynamic imports for reduced bundle size
- **Tree Shaking**: Eliminate unused code
- **Service Workers**: Offline capability and caching
- **Virtual Scrolling**: Handle large data sets efficiently

### Backend Optimizations

- **Connection Pooling**: Database connection management
- **Caching Layers**: Redis integration for session storage
- **Compression**: Gzip compression for responses
- **Database Indexing**: Optimized database queries
- **Multi-Engine Support**: Choose optimal database for workload

### Infrastructure Optimizations

- **CDN Integration**: Static asset delivery
- **HTTP/2 Support**: Modern protocol support
- **SSL Termination**: Efficient SSL handling
- **Health Checks**: Automated health monitoring
- **Zero-Downtime Updates**: Graceful reload for continuous availability

## Monitoring & Observability

### Logging

- **Structured Logging**: JSON-based log format
- **Log Aggregation**: Centralized log collection
- **Log Rotation**: Automatic log management
- **Debug Modes**: Configurable debug levels

### Metrics

- **Performance Metrics**: Response times, throughput
- **Resource Metrics**: CPU, memory, disk usage
- **Business Metrics**: User activity, blocklet usage
- **Custom Metrics**: Application-specific metrics

### Alerting

- **Threshold Alerts**: Configurable alert thresholds
- **Health Checks**: Automated health monitoring
- **Incident Response**: Integration with incident management
- **Notification Channels**: Multiple alert delivery methods

## Conclusion

The Blocklet Platform represents a comprehensive, modern approach to decentralized application development and deployment. By providing standardized tools, services, and runtime environments, it enables developers to focus on business logic while the platform handles infrastructure concerns.

The modular architecture ensures scalability, maintainability, and extensibility, while the DID-based security model provides robust authentication and authorization mechanisms. The platform's emphasis on developer experience, from CLI tools to comprehensive SDKs, makes it accessible to developers of all skill levels.

This architecture supports both simple single-server deployments for development and testing, as well as complex multi-server production deployments with full clustering and cloud-native capabilities.

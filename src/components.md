# ArcBlock Platform: A Deep Dive into the Core Components

Following the high-level architectural overview, this report provides a granular analysis of the seven core components that form the backbone of the ArcBlock platform. Each component is engineered to fulfill a specific role, yet they are deeply interconnected, creating a cohesive and powerful end-to-end environment for the entire lifecycle of decentralized application development, deployment, and management. This analysis draws upon the platform's multi-layered architecture, which positions Identity as its foundation, followed by Blockchain and Storage, with the Compute layer running on top, and the Components and Applications layers serving the developer and end-user.

1. Blocklet Server
The Blocklet Server is the foundational runtime environment of the ArcBlock platform, functioning as a standardized container service designed specifically for executing Blocklets. As the core of the   

Compute layer, it is the essential infrastructure that powers every application and service within the ecosystem. Engineered for maximum deployment flexibility, the Blocklet Server can operate across a diverse range of environments, from major cloud providers and dedicated on-premise hardware to local development machines and even low-cost devices like the Raspberry Pi.  

Architectural Breakdown:

The server's architecture is modular, comprising three primary, deeply integrated systems that work in concert to provide a secure, scalable, and manageable environment for dApps.  

Blocklet Gateway: This component acts as an intelligent, automated service gateway and reverse proxy for all hosted Blocklets. It is responsible for managing all inbound network traffic, abstracting away complex DevOps tasks from the developer. Its key functions include service discovery, load balancing, automatic SSL/TLS certificate management for secure HTTPS connections, and customizable URL mapping and redirects. This allows developers to deploy a Blocklet and have it become securely accessible on the web almost instantly.  

Blocklet Runtime: This is the sandboxed execution environment where the Blocklet's application code runs. The runtime manages the complete lifecycle of every Blocklet, including its installation, initiation, termination, and upgrades. It ensures that each Blocklet operates in an isolated process, preventing interference with other applications or the core server system, which is critical for security and stability in a multi-tenant environment.  

Blocklet Security: Security is a core, built-in pillar of the Blocklet Server. This component provides a suite of protective features, including the management of access keys for programmatic requests, a robust authentication and authorization layer, integrated logging for monitoring and auditing, and tools for team collaboration with role-based access controls.  

2. Blocklet Service
A Blocklet Service is a standardized, out-of-the-box set of functionalities that a Blocklet can expose to other applications or end-users. These services are managed by the Blocklet Server and are accessible through predictable URL endpoints, typically prefixed with   

/.well-known/service.  

This architectural pattern is fundamental to the composability of the ArcBlock ecosystem. It allows developers to build Blocklets that offer reusable capabilities—such as data conversion, image optimization, or event handling—without requiring other developers to understand their internal implementation. Other Blocklets can then programmatically discover and call these services through a well-defined interface. The developer documentation references several types of services, including an Event Bus Service and an OpenAPI Service, indicating a robust framework for inter-blocklet communication and discoverability.  

3. Blocklet Store
The Blocklet Store is the central, decentralized marketplace for the ArcBlock ecosystem, functioning as the "App Store for all Blocklets". As a key part of the   

Components layer, it is the primary hub where developers publish their applications and components, and where users and other developers can discover, acquire, and deploy them with a single click.  

Key Features and Functionality:

Federated Architecture: A defining feature of the Blocklet Store is that it is, itself, a Blocklet. This allows any individual, enterprise, or community to deploy their own private or curated instance of the store, preventing vendor lock-in and fostering a truly decentralized software distribution model.  

Advanced Discovery: The store incorporates powerful search and filtering tools, including an intelligent search engine that uses advanced algorithms to deliver more accurate and relevant results, making it easy to find specific applications or components.  

Structured Publishing Workflow: The store provides a formal review and publication process. Developers can upload their Blocklets, which then enter a defined lifecycle (e.g., DRAFT, PENDING_REVIEW, APPROVED, PUBLISHED), ensuring quality control and transparency.  

Flexible Permission Models: To control who can publish, store administrators can choose between an exclusive, invitation-based model or a more open, staking-based model where developers stake ABT tokens to gain publishing rights.

Customization and User Experience: The store supports theming for visual customization, a compact list display for better usability, and real-time status updates to keep developers informed.  

4. Blocklet Launcher
The Blocklet Launcher is a hosted service designed to be the primary on-ramp for non-technical users into the ArcBlock ecosystem. Positioned in the   

Components layer, it provides a streamlined, no-code, "app store"-like experience that abstracts away the complexities of server setup and application deployment, making decentralized technology accessible to a broader audience.  

Key Features and Functionality:

Simplified Deployment: Users can browse, select, and launch dApps with just a few clicks from an intuitive dashboard, without needing any command-line or coding knowledge.  

Digital Space Management: The Launcher provides a central interface for users to purchase, activate, pause, and manage their "space," which is effectively a managed Blocklet Server instance hosted by ArcBlock.  

Seamless App Recovery: It is deeply integrated with DID Spaces, enabling users to effortlessly restore their applications and data from personal backups, ensuring data sovereignty and resilience.  

User-Centric Design: The entire experience is built around user convenience, featuring direct application launching, real-time progress tracking, flexible payment options (fiat and crypto), and robust security anchored by Decentralized Identity (DID) technology.  

Federation: Like the Blocklet Store, the Launcher is also a Blocklet, meaning anyone can host their own dedicated launcher service.  

5. Blocklet SDK
The Blocklet Software Development Kit (SDK) is the essential library for developers building on the ArcBlock platform. Packaged as @blocklet/sdk for Node.js environments, it provides a comprehensive set of APIs that enable a Blocklet to securely and efficiently communicate with its host Blocklet Server and other platform services.  

Core Modules and Functionalities:

Authentication Service (AuthService): A powerful module for managing user identity, including retrieving user profiles, creating and managing roles and permissions, and issuing Verifiable Credentials (in the form of "passports") to users.  

DID Connect: Facilitates secure, passwordless user login by integrating with the user's external DID Wallet.  

Wallet Management (getWallet): Provides programmatic access to the Blocklet's own internal DID wallet, allowing it to perform cryptographic operations such as signing data.  

Environment & Configuration: Offers simple APIs to access system environment variables and the Blocklet's specific configuration files, allowing the application to adapt to its runtime environment.  

Notifications: Enables the Blocklet to send rich, interactive notifications directly to a user's DID Wallet.  

Middleware: Includes middleware for popular web frameworks like Express.js, which automatically injects runtime information into the front-end (window.blocklet) and handles request verification.  

6. Blocklet CLI
The Blocklet Command Line Interface (CLI), packaged as @blocklet/cli, is the primary tool for developers and administrators who prefer to work from the command line. It provides comprehensive control over the entire development and server management lifecycle.  

Dual Command Structure:

The CLI is logically organized into two distinct top-level command sets, which mirrors a typical DevOps workflow :  

blocklet server: This command set is dedicated to administering the Blocklet Server instance itself. It covers infrastructure-level tasks such as initializing a new server (init), controlling the server daemon (start, stop, status), viewing logs (logs), and performing maintenance (upgrade).  

blocklet: This command set focuses on the application development lifecycle. Developers use these commands to scaffold new projects (create), run a local development server with hot-reloading (dev), package the application for distribution (bundle), manage versions (version), and deploy the final package to a server (deploy) or publish it to a Blocklet Store (upload).

This clear separation provides developers and power users with full, scriptable control over their Blocklet environment, enabling automation and integration with CI/CD pipelines.

7. Blocklet Framework (Specification)
The Blocklet Framework is the conceptual and technical blueprint that defines what a Blocklet is and how it functions within the ArcBlock ecosystem. It is not a single piece of software but rather the collection of standards, protocols, and tools that enable the platform's composable, microservice-based architecture. The Blocklet Specification, centered around the   

blocklet.yml file, serves as the foundation for all Blocklet development.  

The blocklet.yml Manifest:

At the core of the Blocklet Framework is the blocklet.yml file, a central manifest that must be included in every Blocklet project. This YAML file declares all essential metadata and configuration, serving as the single source of truth for the Blocklet Server. Its key sections include:  

Core Metadata: Defines the Blocklet's fundamental identity, including its unique name, cryptographically derived Decentralized Identifier (did), and version.

Execution & Environment: Specifies how the Blocklet runs (e.g., the main entry point script) and its system requirements, such as the minimum required server version.  

Interfaces & Services: Declares the web interfaces and services the Blocklet exposes, including their URL path, port, and protocol. This is how a Blocklet makes itself accessible to users and other Blocklets.  

Composition (Components): Lists other Blocklets that it depends on, enabling the creation of complex applications by composing smaller, reusable components.  

UI & Theming: Defines navigation menu items and theme settings that the Blocklet can contribute to a unified user interface, allowing for seamless UI composition across multiple components.  

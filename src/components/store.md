# Blocklet Store

## Overview

Blocklet Store is a production-ready decentralized marketplace that enables organizations to discover, purchase, and distribute applications with enterprise-grade security and scalability. Built on ArcBlock's decentralized identity (DID) infrastructure, it provides a complete ecosystem for developers to monetize their applications and for businesses to access verified blockchain solutions.

## Key Features & Benefits

### 🏪 **Enterprise Marketplace**

- **Decentralized App Discovery**: Search and browse blockchain applications with advanced filtering, powered by Meilisearch for lightning-fast results
- **Verified Publisher Network**: DID-based developer verification ensures authenticity and builds trust in the ecosystem
- **Multi-Version Management**: Access complete version histories with rollback capabilities for stable deployments
- **Category Organization**: Streamlined browsing with admin-controlled categorization for better user experience

### 💰 **Advanced Monetization**

- **Blockchain-Native Payments**: NFT-based purchasing system with cryptographic proof of ownership and tamper-proof transactions
- **Flexible Pricing Models**: Support for free, one-time purchase, and subscription-based applications
- **Automated Revenue Sharing**: Built-in payment distribution system for developers, partners, and platform operators
- **Download Verification**: Cryptographic tokens prevent unauthorized access and ensure legitimate usage

### 👨‍💻 **Developer-First Experience**

- **Seamless CLI Integration**: Publish and update applications directly from development workflow using @blocklet/cli
- **API Token Management**: Secure authentication system for automated deployments and CI/CD integration
- **Web-Based Publishing**: Direct upload interface for quick deployments without command-line tools
- **Analytics Dashboard**: Track downloads, revenue, user feedback, and performance metrics
- **Component Library**: Reusable UI components for consistent user experiences across applications

### 🔒 **Enterprise Security**

- **Decentralized Identity**: DID-based authentication eliminates single points of failure and enhances privacy
- **Role-Based Permissions**: Granular access control for developers, administrators, and end users
- **Anti-Fraud Protection**: Multi-layer verification system prevents unauthorized distribution and piracy
- **Complete Audit Trail**: Immutable transaction logs for compliance and security monitoring

### 🚀 **Flexible Deployment**

- **Self-Hosted Control**: Deploy on your own infrastructure with full data sovereignty using Blocklet Platform
- **Global CDN Integration**: AWS CloudFront Lambda distribution for worldwide performance and availability
- **Scalable Architecture**: Multi-instance support with component isolation for enterprise workloads
- **AI-Ready Integration**: Built-in AI search enhancement and MCP tooling for AI agents

### 📊 **Business Intelligence**

- **Real-Time Analytics**: Monitor marketplace performance, user engagement, and revenue metrics
- **Community Features**: User ratings, reviews, and feedback systems powered by DID Comments
- **Market Insights**: Track trending applications, developer success metrics, and user behavior patterns
- **Custom Branding**: White-label capabilities for organizations wanting their own branded marketplace

## Quick Start

### Try the Live Service

Access **[store.blocklet.dev](https://store.blocklet.dev)** to explore the marketplace immediately:

- Browse and download free applications
- Purchase premium applications with crypto payments
- Publish your own blocklets to the marketplace

## Deployment

### Self-Hosted

Deploy your own instance using [Blocklet Platform](https://www.blocklet.io):

1. Install Blocklet Server on your infrastructure
2. Deploy using: `blocklets/blocklet-store/blocklet.yml`
3. Configure environment:
   ```bash
   CHAIN_HOST=https://beta.abtnetwork.io/api/
   COMPONENT_STORE_URL=https://store.blocklet.dev
   ```

**Production Requirements:** PostgreSQL/SQLite, Meilisearch, SSL certificate, payment integration (for paid apps)

### Cloud Options

- **Blocklet Cloud**: Managed hosting with auto-scaling
- **AWS Lambda**: CloudFront CDN integration for global distribution
- **Docker**: Containerized deployment for any cloud provider

## API Reference

Public REST endpoints:

- `GET /api/v2/blocklets.json` - Search marketplace with pagination
- `GET /api/blocklets/{did}/blocklet.json` - Get application details
- `GET /api/nft/display` - Purchase verification interface
- `GET /api/payment/download-token` - Generate secure download access

## AI & MCP Support

### AI-Powered Search (optional)

- Enhances search relevance by expanding user queries into high-signal keywords via an external AI provider.
- Integrates with AIGNE Hub to run models behind your own gateway; no keys stored in Blocklet Store.
- Tunable prompts and caching via the admin UI under the Preferences → AI tab.

### Model Context Protocol (MCP)

- Exposes a JSON-RPC 2.0 MCP server so AI agents can discover and call marketplace tools.
- Endpoint: `POST /mcp`
- Recommended headers: `x-blocklet-server-version`, `x-blocklet-store-version` (used for compatibility filtering).

Supported tools:

- `search_blocklets`: Search marketplace content by keyword, category, price, owner DID, etc., with pagination and sorting.

## Documentation & Support

- **[ArcBlock Documentation](https://www.arcblock.io)** - Platform overview and concepts
- **[Developer Portal](https://developer.blocklet.io)** - Publishing guides and API references
- **[Community Forum](https://community.arcblock.io)** - Ask questions and share knowledge
- **[GitHub Issues](https://github.com/blocklet/blocklet-store/issues)** - Report bugs and request features

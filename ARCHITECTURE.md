# Architecture Overview

This document provides a high-level overview of the documentation site architecture.

## System Architecture

```mermaid
graph TB
    subgraph "Content Layer"
        MDX["MDX Files<br/>(Pages & Components)"]
        YAML["YAML Frontmatter<br/>(Metadata)"]
    end
    
    subgraph "Configuration Layer"
        CONFIG["docs.json<br/>(Site Config)"]
    end
    
    subgraph "Build & Deployment"
        MINT["Mintlify CLI<br/>(mint dev, mint build)"]
        DEV["Local Preview<br/>(mint dev)"]
        PROD["Production Build"]
    end
    
    subgraph "Output"
        SITE["Static Documentation Site<br/>(HTML/CSS/JS)"]
    end
    
    subgraph "Validation"
        LINKS["Link Checker<br/>(mint broken-links)"]
    end
    
    MDX --> MINT
    YAML --> MINT
    CONFIG --> MINT
    MINT --> DEV
    MINT --> PROD
    PROD --> SITE
    SITE --> LINKS
    LINKS -.->|Reports| MINT
    
    style MDX fill:#e1f5ff
    style YAML fill:#e1f5ff
    style CONFIG fill:#fff3e0
    style MINT fill:#f3e5f5
    style SITE fill:#e8f5e9
    style LINKS fill:#fce4ec
```

## Development Workflow

```mermaid
graph LR
    A["Edit MDX<br/>Files"] -->|Save| B["mint dev<br/>Watches Changes"]
    B -->|Processes| C["Renders<br/>Site"]
    C -->|Serves at| D["localhost:3000"]
    D -->|View in| E["Browser"]
    E -->|Test Links| F["mint broken-links"]
    F -->|Reports| A
    
    style A fill:#e3f2fd
    style B fill:#f3e5f5
    style C fill:#e8f5e9
    style D fill:#fff3e0
    style E fill:#fce4ec
    style F fill:#ffebee
```

## Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Content** | MDX | Markdown with JSX for dynamic content |
| **Metadata** | YAML | Frontmatter for page configuration |
| **Build Tool** | Mintlify CLI | Builds and serves documentation |
| **Language** | JavaScript/TypeScript | Development and scripting |
| **Deployment** | Static Hosting | CDN-friendly HTML output |

## File Structure

```
.
├── docs.json              # Site configuration
├── AGENTS.md              # Project instructions (this file)
├── ARCHITECTURE.md        # Architecture overview (this file)
├── pages/                 # Documentation pages (MDX)
│   ├── index.mdx
│   ├── getting-started.mdx
│   └── ...
└── public/                # Static assets
    ├── images/
    └── ...
```

## Key Development Commands

- **`mint dev`** — Start local preview server with hot reload
- **`mint build`** — Build production-ready static site
- **`mint broken-links`** — Validate all internal and external links

## Next Steps

1. Customize `docs.json` for your project settings
2. Update `AGENTS.md` with your project-specific guidelines
3. Create documentation pages in the `pages/` directory
4. Run `mint dev` to preview changes locally
5. Run `mint broken-links` before deploying to catch dead links

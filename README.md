# n8n Self-Hosting & Workflows

<div align="center">

**Production-ready Docker deployment setups and community workflows for n8n**

[![n8n](https://img.shields.io/badge/n8n-Self--Hosting-orange.svg)](https://n8n.io/)
[![Docker](https://img.shields.io/badge/Docker-Compose-blue.svg)](https://www.docker.com/)
[![Status](https://img.shields.io/badge/Status-Production--Ready-green.svg)]()

*Helping the n8n community with robust deployment infrastructure and powerful automation workflows*

</div>

---

## 📋 What's Inside

This repository provides **two essential resources** for the n8n community:

### 🐳 [Docker Self-Hosting Setup](n8n-selfhosting/docker/)
Production-ready Docker Compose configuration for self-hosting n8n with:
- **PostgreSQL 16** for data persistence
- **Redis 7** for queue management (Bull/BullMQ)
- **Scalable workers** (horizontal scaling with replicas)
- **Health checks** on all services
- **Security best practices** documented
- **Comprehensive documentation** with architecture diagrams

👉 **[Get Started with Docker Setup →](n8n-selfhosting/docker/README.md)**

### 🤖 [Community Workflows](n8n-workflows/)
Ready-to-use n8n workflows for common automation scenarios:

#### [LinkedIn Post Content Generator](n8n-workflows/linkedin-post-content-generator/)
Automated AI-powered LinkedIn content creation pipeline featuring:
- **AI Research** with Firecrawl API
- **Dual Content Variations** with LLM (GPT/Claude)
- **AI Image Generation** with DALL-E/Stable Diffusion
- **Email Review System** with webhook approvals
- **Google Drive Integration** for content storage
- **12-Stage Pipeline** with automatic retries

👉 **[Explore LinkedIn Workflow →](n8n-workflows/linkedin-post-content-generator/README.md)**

---

## 🚀 Quick Start

### For Self-Hosting n8n

```bash
# Clone this repository
git clone https://github.com/YOUR_USERNAME/n8n-selfhosting-workflows.git
cd n8n-selfhosting-workflows/n8n-selfhosting/docker

# Copy environment template
cp .env-example .env

# Edit .env with your configuration
nano .env

# Launch n8n stack
docker compose up -d

# Access n8n at http://localhost:5678
```

📖 **[Full Docker Setup Guide →](n8n-selfhosting/docker/README.md)**

---

### For Using Workflows

```bash
# Navigate to the workflow you want
cd n8n-workflows/linkedin-post-content-generator

# Read the comprehensive setup guide
cat README.md

# Import the workflow JSON into your n8n instance
# Follow the README for complete configuration steps
```

📖 **[Workflow Documentation →](n8n-workflows/linkedin-post-content-generator/README.md)**

---

## 📂 Repository Structure

```
n8n-selfhosting-workflows/
│
├── 📁 n8n-selfhosting/          # Docker deployment infrastructure
│   └── docker/
│       ├── docker-compose.yml   # Production Docker Compose config
│       ├── .env-example         # Environment variables template
│       ├── .gitignore           # Git exclusions
│       └── README.md            # Complete setup documentation (794 lines)
│
├── 📁 n8n-workflows/            # Community automation workflows
│   └── linkedin-post-content-generator/
│       ├── linkedin_post_content_generator.json   # n8n workflow
│       ├── linkedin_post_pipeline.xlsx            # Sample Google Sheet template
│       └── README.md            # Workflow documentation (850+ lines)
│
├── 📁 info/                     # Project documentation
│   ├── IMPLEMENTATION_PLAN.md   # Development roadmap
│   └── TODO.md                  # Task tracking
│
├── LICENSE
└── README.md                    # This file
```

---

## 🎯 Who Is This For?

### DevOps Engineers & System Administrators
- ✅ Production-ready Docker Compose setup
- ✅ Scalable architecture with worker replicas
- ✅ Database persistence (PostgreSQL)
- ✅ Queue management (Redis)
- ✅ Health monitoring and troubleshooting guides

### Content Creators & Marketers
- ✅ Automated LinkedIn content generation
- ✅ AI-powered research and writing
- ✅ Image generation with AI
- ✅ Review and approval workflows
- ✅ Content organization in Google Drive

### n8n Community Members
- ✅ Learn from production-tested configurations
- ✅ Reusable workflow templates
- ✅ Best practices for self-hosting
- ✅ Security-first approach
- ✅ Comprehensive documentation

### Developers & Automation Enthusiasts
- ✅ Complex workflow examples (12-stage pipeline)
- ✅ API integrations (Google, Firecrawl, LLMs)
- ✅ Webhook implementations
- ✅ Error handling patterns
- ✅ State management examples

---

## ✨ Key Features

### Docker Setup
- 🔐 **Security First**: No hardcoded credentials, .gitignore configured
- 📊 **Scalable**: Horizontal worker scaling with single command
- 🏗️ **Production-Ready**: Health checks, restart policies, resource limits
- 📖 **Well-Documented**: 794 lines of setup and troubleshooting guides
- 🎨 **Architecture Diagrams**: Visual representations of system flow

### Workflows
- 🤖 **AI-Powered**: Integrates with GPT, Claude, DALL-E, Stable Diffusion
- 🔄 **Robust Pipeline**: 12 stages with automatic error handling
- 📧 **Review System**: Email notifications with webhook approvals
- 💾 **Organized Storage**: Google Drive folder structure per post
- 🎯 **Production-Tested**: Used in real content creation workflows

---

## 📚 Documentation Quality

Both components feature **comprehensive, production-grade documentation**:

| Component | Lines | Words | Diagrams | Guides | Troubleshooting |
|-----------|-------|-------|----------|--------|-----------------|
| **Docker Setup** | 794 | ~8,000 | 3 | Setup, Config, Security | 8 scenarios |
| **LinkedIn Workflow** | 850+ | ~12,000 | 1 | 5 API setups, Config | 10 scenarios |
| **Total** | **1,600+** | **~20,000** | **4** | **Multiple** | **18 scenarios** |

---

## 🛠️ Technologies Used

### Infrastructure
- **Docker Compose** - Container orchestration
- **PostgreSQL 16** - Primary database
- **Redis 7** - Job queue (Bull/BullMQ)
- **n8n 1.123.28+** - Workflow automation platform

### Workflow Integrations
- **Google Drive API** - File storage
- **Google Sheets API** - Pipeline state management
- **Gmail API** - Email notifications
- **Firecrawl API** - Web research
- **Azure OpenAI / OpenAI / Claude** - LLM content generation
- **DALL-E / Stable Diffusion** - AI image generation

---

## 🔒 Security & Privacy

✅ **All personal information removed**
- No hardcoded credentials or API keys
- Generic placeholders for all IDs
- Environment-based configuration
- .gitignore protects sensitive files

✅ **Best practices documented**
- Strong password generation
- Basic authentication setup
- Environment isolation
- Secure credential storage in n8n

✅ **Safe for public sharing**
- Community-reviewed configurations
- No exposed personal data
- Security checklists included

---

## 🤝 Contributing

Contributions are welcome! Whether you want to:
- 🐛 Report a bug or issue
- 💡 Suggest a feature or improvement
- 📖 Improve documentation
- 🔧 Submit a workflow or enhancement
- ⭐ Share your use case

Feel free to:
1. Open an issue for discussion
2. Fork the repository
3. Create a pull request
4. Share your feedback

---

## 📝 License

This repository is provided **as-is for community use**. Feel free to:
- ✅ Use in personal or commercial projects
- ✅ Modify and customize for your needs
- ✅ Share with the n8n community
- ✅ Learn and build upon these examples

---

## 🌟 Support the Project

If this repository helps you:
- ⭐ **Star the repository** to show support
- 🔄 **Share** with others in the n8n community
- 💬 **Provide feedback** on what works or needs improvement
- 🤝 **Contribute** your own workflows or enhancements

---

## 📞 Resources & Links

### Official n8n Resources
- 🌐 [n8n.io](https://n8n.io/) - Official website
- 📖 [n8n Documentation](https://docs.n8n.io/) - Official docs
- 💬 [n8n Community Forum](https://community.n8n.io/) - Community support
- 🐙 [n8n GitHub](https://github.com/n8n-io/n8n) - Source code

### This Repository
- 🐳 [Docker Setup Guide](n8n-selfhosting/docker/README.md) - Self-hosting documentation
- 🤖 [LinkedIn Workflow](n8n-workflows/linkedin-post-content-generator/README.md) - Workflow documentation
- 📋 [Implementation Plan](info/IMPLEMENTATION_PLAN.md) - Project roadmap
- ✅ [TODO List](info/TODO.md) - Task tracking and status

---

## 💡 Use Cases

### With Docker Setup
- Self-host n8n for your organization
- Deploy n8n with horizontal scaling
- Run n8n in production environments
- Set up development/staging environments
- Learn Docker Compose best practices

### With LinkedIn Workflow
- Automate LinkedIn content creation
- Generate AI-powered social media posts
- Manage content review and approval
- Organize content assets in Drive
- Schedule post creation pipelines

---

## ⚙️ System Requirements

### For Docker Setup
- **OS**: Linux, macOS, Windows (with WSL2)
- **Docker**: 20.10+
- **Docker Compose**: v2.0+
- **RAM**: 4GB minimum, 8GB recommended
- **Storage**: 10GB minimum for volumes

### For Workflows
- **n8n Instance**: Self-hosted or cloud
- **API Keys**: Based on workflow requirements
- **Google Account**: For Drive/Sheets/Gmail integrations
- **LLM API Access**: OpenAI, Azure OpenAI, or Claude
- **Image API**: DALL-E or Stable Diffusion

---

## 🎉 Getting Started

Choose your path:

### 🐳 **Want to Self-Host n8n?**
Start here → **[Docker Setup Guide](n8n-selfhosting/docker/README.md)**

### 🤖 **Want Ready-Made Workflows?**
Start here → **[Workflows Directory](n8n-workflows/)**

### 📖 **Want to Learn More?**
Browse the **[Documentation](info/)** folder

---

<div align="center">

**Made with ❤️ for the n8n community**

*Help others automate their workflows!*

If this helps you, consider ⭐ **starring the repository**

---

**Questions?** Open an issue | **Ideas?** Submit a PR | **Success story?** Share it!

</div>

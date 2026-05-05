# n8n Workflows Collection

<div align="center">

**Ready-to-use automation workflows for n8n**

[![n8n](https://img.shields.io/badge/n8n-Workflows-orange.svg)](https://n8n.io/)

*Professional automation templates for content creation and image generation*

</div>

---

## 📋 Available Workflows

### 🎨 Image Generation with Gemini (3 Workflows)

Generate professional infographics and images using Google Gemini's AI:

#### 1. [Basic Text-to-Image](image-generation/basic-text-to-image/)
**Simplest workflow** - Text prompt → Image

- ⚡ 4 nodes, easiest setup
- 📝 Direct prompt editing
- 🎯 Perfect for testing and learning
- ⏱️ ~10 seconds per image

👉 **[Start Here →](image-generation/basic-text-to-image/README.md)**

---

#### 2. [Web Search + Image Generation](image-generation/web-search-image-generation/)
**Research-powered** - Web search → Enriched prompt → Image

- 🔍 Firecrawl web research
- 📊 Context-aware generation
- 🎯 Best for current topics and accurate data
- ⏱️ ~15-25 seconds per image

👉 **[Explore →](image-generation/web-search-image-generation/README.md)**

---

#### 3. [Enhanced Prompt Engineering + Image Generation](image-generation/enhanced-prompt-image-generation/)
**Most advanced** - AI analyzes content → Auto-selects style → Professional infographic

- 🧠 AI prompt optimization with LangChain
- 🎨 6 professional visual styles (auto-selected)
- 📊 Content structure analysis
- 🏆 Production-quality output
- ⏱️ ~20-40 seconds per image

👉 **[View Advanced →](image-generation/enhanced-prompt-image-generation/README.md)**

---

### 📝 Content Generation

#### [LinkedIn Post Content Generator](linkedin-post-content-generator/)
**Full pipeline** - Research → Content → Images → Review → Approval

- 🤖 AI-powered research and writing
- 📧 Email review system
- 🖼️ Dual content & image variations
- ☁️ Google Drive integration
- 🔄 12-stage automated pipeline

👉 **[Complete Documentation →](linkedin-post-content-generator/README.md)**

---

## 🎯 Which Workflow Should I Use?

### For Image Generation:

| Use Case | Recommended Workflow | Why? |
|----------|---------------------|------|
| **Testing/Learning** | [Basic](image-generation/basic-text-to-image/) | Simple setup, immediate results |
| **Current Events** | [Web Search](image-generation/web-search-image-generation/) | Accurate, up-to-date information |
| **Professional Posts** | [Enhanced](image-generation/enhanced-prompt-image-generation/) | Publication-quality infographics |
| **Quick Mockups** | [Basic](image-generation/basic-text-to-image/) | Fast iterations |
| **Tech Diagrams** | [Enhanced](image-generation/enhanced-prompt-image-generation/) | Smart style selection |
| **Data Visualization** | [Web Search](image-generation/web-search-image-generation/) | Real statistics |

### Workflow Comparison Table:

| Feature | Basic | Web Search | Enhanced |
|---------|-------|------------|----------|
| **Setup Time** | 5 min | 10 min | 15 min |
| **API Keys Needed** | 1 (Gemini) | 2 (Gemini + Firecrawl) | 2 (Gemini + Firecrawl) |
| **Web Research** | ❌ | ✅ | ✅ |
| **AI Optimization** | ❌ | ❌ | ✅ |
| **Prompt Length** | 50-200 words | 100-300 words | 200-500 words |
| **Style Variety** | Manual | Manual | Automatic (6 styles) |
| **Execution Time** | ~10s | ~20s | ~30s |
| **Best For** | Learning | Research topics | Production |
| **Complexity** | ⭐ | ⭐⭐ | ⭐⭐⭐ |
| **Output Quality** | Good | Very Good | Excellent |

---

## 🚀 Getting Started

### Prerequisites

All image workflows require:
- ✅ n8n instance (self-hosted or cloud)
- ✅ Google Gemini API key ([Get free key](https://ai.google.dev/))

Additional for Web Search & Enhanced:
- ✅ Firecrawl API key ([Get free key](https://firecrawl.dev/))

### Quick Start Path

1. **Start Simple**: Try [Basic Text-to-Image](image-generation/basic-text-to-image/) first
   - Import workflow
   - Add Gemini API key
   - Generate your first image

2. **Add Research**: Upgrade to [Web Search](image-generation/web-search-image-generation/)
   - Get Firecrawl API key
   - Import workflow
   - Generate research-based images

3. **Go Professional**: Move to [Enhanced](image-generation/enhanced-prompt-image-generation/)
   - Same APIs as Web Search
   - Import advanced workflow
   - Let AI optimize your prompts

---

## 📦 Installation

### Method 1: Individual Workflows

1. Navigate to the workflow folder you want
2. Download the `.json` file
3. In n8n, click **Import from File**
4. Select the JSON file
5. Configure credentials
6. Execute!

### Method 2: Bulk Import

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/n8n-selfhosting-workflows.git

# Navigate to workflows folder
cd n8n-selfhosting-workflows/n8n-workflows

# Each subfolder contains a workflow JSON
# Import them one by one in n8n
```

---

## 🔧 Common Setup Steps

### 1. Google Gemini API Key

All image workflows need this:

1. Visit [Google AI Studio](https://ai.google.dev/)
2. Sign in → **Get API Key**
3. Copy the key
4. In n8n: **Credentials** → **Add Credential** → **Google PaLM API**
5. Paste key → Save

### 2. Firecrawl API Key

For Web Search and Enhanced workflows:

1. Visit [Firecrawl](https://firecrawl.dev/)
2. Sign up (free tier available)
3. Dashboard → Copy API key
4. In n8n: **Credentials** → **Add Credential** → **Firecrawl API**
5. Paste key → Save

### 3. Update Workflow Credentials

After importing a workflow:

1. Click on nodes with credential icons (🔑)
2. Select your configured credential
3. Save the node
4. Repeat for all credential nodes

---

## 💡 Example Use Cases

### Basic Workflow
- Quick social media images
- Blog post headers
- Presentation slides
- Testing different prompts
- Learning AI image generation

### Web Search Workflow
- Technology trend infographics
- Industry statistics visualizations
- Product comparison charts
- Current event illustrations
- Market analysis graphics

### Enhanced Workflow
- Professional LinkedIn posts
- Technical architecture diagrams
- Editorial-quality infographics
- Complex process flows
- Publication-ready visuals

---

## 📊 Performance & Costs

### Execution Times

- **Basic**: ~10 seconds
- **Web Search**: ~20 seconds
- **Enhanced**: ~30 seconds
- **LinkedIn Pipeline**: ~5-15 minutes (full pipeline)

### API Costs (Approximate)

#### Google Gemini
- Free tier: Limited requests per day
- Paid: ~$0.01-0.05 per image
- Check current rates: [ai.google.dev/pricing](https://ai.google.dev/pricing)

#### Firecrawl
- Free tier: 500 searches/month
- Paid: Volume-based pricing
- Check: [firecrawl.dev/pricing](https://firecrawl.dev/pricing)

**💡 Tip**: Start with free tiers to test, upgrade as needed

---

## 🎓 Learning Path

### Beginner
1. Read [Basic workflow README](image-generation/basic-text-to-image/README.md)
2. Import and run Basic workflow
3. Experiment with different prompts
4. Learn prompt engineering basics

### Intermediate
1. Try [Web Search workflow](image-generation/web-search-image-generation/README.md)
2. Understand web research integration
3. See how context improves images
4. Customize prompt templates

### Advanced
1. Study [Enhanced workflow](image-generation/enhanced-prompt-image-generation/README.md)
2. Analyze AI prompt engineer system message
3. Understand style selection logic
4. Modify for your specific use cases

---

## 🤝 Contributing

### Share Your Workflows

Created a useful variation? Share it!

1. Fork the repository
2. Add your workflow folder
3. Include README documentation
4. Submit a pull request

### Report Issues

Found a bug or have a suggestion?
- Open an issue on GitHub
- Include workflow name and error details
- Suggest improvements

---

## 📚 Documentation

Each workflow folder contains:
- **README.md**: Complete setup and usage guide
- **{workflow-name}.json**: n8n workflow file
- **Examples**: Sample prompts and outputs (where applicable)

---

## 🔗 Resources

### n8n Resources
- [n8n Documentation](https://docs.n8n.io/)
- [n8n Community Forum](https://community.n8n.io/)
- [n8n Workflow Gallery](https://n8n.io/workflows/)

### API Documentation
- [Google Gemini Docs](https://ai.google.dev/docs)
- [Firecrawl API Docs](https://docs.firecrawl.dev/)
- [LangChain Docs](https://docs.langchain.com/)

### Related Repos
- [n8n Docker Setup](../n8n-selfhosting/docker/) - Self-host n8n with Docker
- [Main Repository](../) - Full project documentation

---

## 🎉 What's Next?

### Planned Additions
- Image style transfer workflows
- Video generation workflows
- Multi-variation batch generation
- Integration with design tools

### Stay Updated
⭐ **Star the repository** to get notified of new workflows

---

## 📄 License

These workflows are provided as-is for community use. Modify and adapt as needed for your projects.

---

<div align="center">

**Part of the n8n-selfhosting-workflows collection**

[🐳 Docker Setup](../n8n-selfhosting/docker/) | [📚 Main README](../)

---

**Made with ❤️ for the n8n community**

*Automate your visual content creation* 🎨

</div>

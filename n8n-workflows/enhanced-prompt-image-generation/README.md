# Enhanced Prompt Engineering + Image Generation

<div align="center">

**AI-powered prompt optimization → Web research → Professional infographic generation**

[![n8n](https://img.shields.io/badge/n8n-workflow-orange.svg)](https://n8n.io/)
[![Firecrawl](https://img.shields.io/badge/Firecrawl-Web%20Search-green.svg)](https://firecrawl.dev/)
[![Gemini](https://img.shields.io/badge/Gemini-Image%20Gen-blue.svg)](https://ai.google.dev/)
[![LangChain](https://img.shields.io/badge/LangChain-Agent-purple.svg)](https://langchain.com/)

</div>

---

## 📋 Overview

This is the **most advanced** workflow in the collection. It uses AI to analyze your content, automatically choose the best visual style, and write a detailed image generation prompt optimized for professional LinkedIn-quality infographics.

**Perfect for**: Creating publication-ready infographics, complex technical diagrams, professional social media content, and editorial-quality visualizations.

---

## ✨ Features

- 🧠 **AI Prompt Engineer** - LangChain agent analyzes content and writes optimized prompts
- 🎨 **6 Professional Styles** - Isometric, comparison, hub-spoke, timeline, editorial, stat poster
- 🔍 **Web Research Integration** - Firecrawl searches for visual accuracy
- 📊 **Content Structure Analysis** - Detects steps, comparisons, statistics, ecosystems
- 🎯 **Style Auto-Selection** - Chooses best visual format based on content
- 📝 **200+ Word Prompts** - Hyper-detailed scene descriptions
- 🏷️ **Smart Naming** - Identifies tools/companies and labels them accurately
- ⚡ **Error Resilient** - Handles search failures gracefully

---

## 🏗️ Workflow Structure

```
Manual Trigger
     ↓
Set Topic + Description (Code Node)
     ↓
Firecrawl Web Search (8 results)
     ↓
Prepare Request (Code Node)
   • Analyze content structure
   • Detect named tools
   • Build research context
     ↓
AI Prompt Engineer (LangChain Agent)
   • Choose visual style [A-F]
   • Write 200+ word prompt
   • Specify every element
     ↓
Extract Prompt (Code Node)
     ↓
Generate Image (Gemini)
     ↓
Decode Image (Code Node)
     ↓
Output: Professional Infographic
```

---

## 🎨 Visual Styles

The AI automatically selects from 6 professional styles based on your content:

### [A] Isometric Scene
**Best for**: 2-5 step sequential processes or pipelines

**Example topics**: 
- "Data pipeline: Ingest → Process → Store → Analyze"
- "CI/CD workflow with GitHub Actions"

**Visual**: 3D platforms with physical objects representing each tool, neon data flows, dark background

---

### [B] Comparison Illustration
**Best for**: "X vs Y", "before/after", "old vs new" content

**Example topics**:
- "Monolithic vs Microservices architecture"
- "Traditional ML vs Generative AI"

**Visual**: Split-screen design with contrasting sides, visual metaphors

---

### [C] Hub-and-Spoke / Ecosystem
**Best for**: "X at the center" or multi-tool ecosystems

**Example topics**:
- "Kubernetes ecosystem tools"
- "Modern JavaScript framework landscape"

**Visual**: Central concept with orbiting elements, radial connections

---

### [D] Timeline / Evolution
**Best for**: Historical progression, era comparisons

**Example topics**:
- "Evolution of web development 2000-2026"
- "Cloud computing generations"

**Visual**: Horizontal timeline with era segments, gradual visual evolution

---

### [E] Editorial Scene
**Best for**: Strategy, leadership, complex systems, single big idea

**Example topics**:
- "The modern DevOps engineer role"
- "Building a data-driven organization"

**Visual**: Cohesive narrative scene like a magazine cover, environmental storytelling

---

### [F] Bold Stat Poster
**Best for**: Dramatic statistics, benchmark results, surprising numbers

**Example topics**:
- "87% of developers now use AI coding assistants"
- "Cloud spending reached $500B in 2026"

**Visual**: Hero number in massive typography, atmospheric background

---

## 📦 Prerequisites

### Required APIs & Credentials

- ✅ **n8n instance** (self-hosted or cloud)
- ✅ **Google Gemini API key** ([Get here](https://ai.google.dev/))
  - For both LLM (prompt engineering) and image generation
- ✅ **Firecrawl API key** ([Get here](https://firecrawl.dev/))

### n8n Requirements

- n8n version 1.0+ with LangChain nodes
- LangChain package: `@n8n/n8n-nodes-langchain`
- Firecrawl integration: `@mendable/n8n-nodes-firecrawl`

---

## 🚀 Setup Instructions

### Step 1: Get API Keys

#### Firecrawl API
1. Visit [firecrawl.dev](https://firecrawl.dev/)
2. Sign up for free account
3. Copy API key from dashboard

#### Google Gemini API
1. Go to [Google AI Studio](https://ai.google.dev/)
2. Create API key
3. Note: You'll use this for **both** the LLM and image generation

### Step 2: Configure n8n Credentials

#### Create Firecrawl Credential
1. n8n → **Credentials** → **Add Credential**
2. Search **Firecrawl API**
3. Enter API key
4. Test and save

#### Create Google Gemini Credential
1. n8n → **Credentials** → **Add Credential**
2. Search **Google PaLM API** or **Google Gemini**
3. Enter API key
4. Test and save

### Step 3: Import Workflow

1. Download `Image_generation_gemini.json`
2. In n8n, click **Import from File**
3. Select the JSON file
4. Click **Import**

### Step 4: Update Credentials in Workflow

#### Update 3 Nodes:

**1. firecrawl_search node**
- Click node → Select Firecrawl credential → Save

**2. gemini_text_lm node**
- Click node → Select Google Gemini credential → Save

**3. generate_image node**
- Click node → Select Google Gemini credential → Save

### Step 5: Customize Your Topic

1. Click **set_inputs** node
2. Edit `TOPIC` and `DESCRIPTION`:

```javascript
const TOPIC = 'Your topic here';
const DESCRIPTION = `Detailed description of what you want to visualize`;
```

3. Save and execute

---

## 🎯 How to Use

### Basic Usage

1. Open workflow in n8n
2. Edit **set_inputs** node with your topic
3. Click **Execute Workflow**
4. Wait 15-30 seconds
5. Download image from **decode_image** node

### Writing Effective Topics

The AI prompt engineer performs best when you provide:

#### ✅ Clear Structure
```javascript
const TOPIC = 'The modern data stack for startups';
const DESCRIPTION = `Show how startups use Fivetran for data ingestion, 
Snowflake for warehousing, dbt for transformation, and Metabase for visualization. 
This is a 4-layer pipeline architecture.`;
```

#### ✅ Named Tools/Companies
```javascript
const TOPIC = 'AI agent frameworks comparison';
const DESCRIPTION = `Compare LangChain, AutoGPT, and CrewAI. 
Show key features, use cases, and when to choose each one. 
This is a comparison of 3 distinct tools.`;
```

#### ✅ Specific Concepts
```javascript
const TOPIC = 'Zero-trust security model';
const DESCRIPTION = `Explain the 5 pillars: identity verification, device security, 
application protection, data encryption, and network segmentation. 
Use a hub-and-spoke visual with security at the center.`;
```

---

## 🔧 How It Works

### Phase 1: Web Research (2-5 seconds)

**firecrawl_search** node:
- Searches for: `{topic} {description} visual architecture illustration 2025 2026`
- Returns 8 web results with titles and descriptions
- Provides current information about tools, logos, trends

### Phase 2: Content Analysis (< 1 second)

**prepare_request** node:
- Counts steps/stages in description
- Detects comparisons ("vs", "versus", "before/after")
- Finds statistics (percentages, numbers)
- Extracts named tools/companies (capitalized words)
- Provides style hints to the AI agent

### Phase 3: AI Prompt Engineering (10-20 seconds)

**prompt_engineer** LangChain agent:
1. **Analyzes** content structure (process? comparison? ecosystem?)
2. **Selects** optimal visual style [A-F]
3. **Writes** detailed 200+ word prompt including:
   - Exact dimensions and format
   - Background colors with hex codes
   - Position of every element
   - Every text label (exact wording)
   - Data flows and connections
   - Lighting and style keywords

### Phase 4: Image Generation (5-15 seconds)

**generate_image** node:
- Sends AI-optimized prompt to Gemini
- Gemini creates professional infographic
- Returns image data

### Phase 5: Decoding

**decode_image** node:
- Converts to downloadable file
- Names file: `{topic-slug}_{timestamp}.jpg`

---

## 💡 Example Use Cases

### Example 1: Tech Stack Architecture

**Input**:
```javascript
const TOPIC = 'Modern JAMstack architecture';
const DESCRIPTION = `Show the 3-layer stack: 
Frontend (Next.js + React), 
API layer (Vercel serverless functions), 
Backend services (Supabase database + Stripe payments). 
This is a 3-stage sequential pipeline.`;
```

**AI Analysis**: Detects 3 sequential steps → Chooses Isometric style [A]

**Output**: 3D isometric scene with 3 platforms, each showing its tool with labels, connected by neon data flows

---

### Example 2: Feature Comparison

**Input**:
```javascript
const TOPIC = 'GitHub Copilot vs Claude Code Assistant';
const DESCRIPTION = `Compare GitHub Copilot (IDE-native, trained on public code) 
versus Claude (context-aware, explanatory, security-focused). 
Show strengths and tradeoffs of each.`;
```

**AI Analysis**: Detects "vs" comparison → Chooses Comparison style [B]

**Output**: Split-screen with two zones, visual icons for each feature, contrasting colors

---

### Example 3: Ecosystem Map

**Input**:
```javascript
const TOPIC = 'The n8n automation ecosystem';
const DESCRIPTION = `n8n at the center connecting to: 
Google Workspace, Slack, GitHub, databases (PostgreSQL, MongoDB), 
AI services (OpenAI, Claude), and webhooks. 
Show how n8n acts as the integration hub.`;
```

**AI Analysis**: Detects hub-and-spoke pattern → Chooses Hub-Spoke style [C]

**Output**: Central n8n logo with 6-8 orbiting tool icons, radial connections, labels

---

## 🎨 Customization

### Modify AI System Prompt

The **prompt_engineer** node has an extensive system message defining:
- Style selection criteria
- Prompt quality requirements
- Visual specifications

To customize, click **prompt_engineer** → **Options** → **System Message**

### Adjust Web Search

In **firecrawl_search** node:
```json
"limit": 8  // Change to 4 (faster) or 12 (more context)
```

### Change Image Model

In **generate_image** node:
```json
"modelId": "models/gemini-3-pro-image-preview"
// Update when new Gemini models are available
```

---

## 📊 Output Quality

### What You Get

- **Professional-grade infographics** suitable for:
  - LinkedIn posts
  - Blog headers
  - Presentation slides
  - Documentation
  - Technical articles

### Dimensions

- Default: 1440x756px (landscape) or 1080x1080px (square)
- Optimized for social media sharing
- High resolution for clarity

### Style Consistency

- AI ensures cohesive color palettes
- Professional typography
- Clear visual hierarchy
- Industry-standard design patterns

---

## 🐛 Troubleshooting

### Issue: "No image in response"

**Cause**: AI wrote a text-heavy prompt without image instructions

**Solution**:
- Add "infographic illustration" to DESCRIPTION
- Be more specific about visual elements
- Mention "NOT a text document" in description

### Issue: Text in image is garbled

**Cause**: Gemini struggles with complex text rendering

**Solution**:
- Limit text labels to 5 words max
- Request "bold, large text only"
- Avoid paragraphs in image

### Issue: AI chose wrong style

**Cause**: Content structure ambiguous

**Solution**:
- Be explicit: "This is a 3-step process" or "This is a comparison"
- Count steps explicitly: "Show 4 stages"
- Mention desired style: "hub-and-spoke diagram"

### Issue: Missing tool names/logos

**Cause**: Tools not detected in analysis

**Solution**:
- Capitalize tool names: "GitHub", "PostgreSQL", "React"
- List tools explicitly: "Show these tools: X, Y, Z"
- Add to description: "Label each tool by name"

---

## ⚡ Performance & Costs

### Execution Time

- **Total**: 20-40 seconds per image
  - Web search: 3-5s
  - AI prompt engineering: 10-20s
  - Image generation: 5-15s

### API Costs

- **Firecrawl**: Free tier → 500 searches/month, then paid
- **Google Gemini LLM**: ~$0.0001-0.0005 per request (prompt engineering)
- **Google Gemini Image**: ~$0.01-0.05 per image (check current rates)

**Tip**: Test with a few runs first to gauge costs for your volume

---

## 🔄 Workflow Comparison

| Feature | Basic | Web Search | **Enhanced** |
|---------|-------|------------|--------------|
| Prompt Input | Manual | Manual + Search | AI-Optimized |
| Web Research | ❌ | ✅ | ✅ (8 results) |
| Style Selection | Manual | Manual | 🤖 **Automatic** |
| Content Analysis | ❌ | ❌ | ✅ **Smart** |
| Prompt Quality | Basic | Good | **Professional** |
| Setup Complexity | Simple | Moderate | **Advanced** |
| Best For | Testing | Current topics | **Production** |

---

## 🎓 Learning Resources

### Understanding the AI Prompt Engineer

The workflow's system message is a **masterclass in prompt engineering**. Read it to learn:
- How to structure visual specifications
- Style selection criteria
- Quality checklist principles

### Further Reading

- [LangChain Agents](https://docs.langchain.com/docs/components/agents/)
- [n8n LangChain Integration](https://docs.n8n.io/integrations/langchain/)
- [Prompt Engineering for Images](https://ai.google.dev/docs/image_generation)

---

## 🚀 Next Steps

### Batch Generation

Modify to process multiple topics:
1. Add Google Sheets input
2. Loop through rows
3. Generate images for each topic
4. Save to Drive

### A/B Testing

Generate multiple variations:
1. Duplicate the prompt engineering step
2. Use different style preferences
3. Compare outputs
4. Choose best result

### Integration Ideas

- **Slack bot**: Request images via Slack commands
- **Scheduled reports**: Auto-generate weekly trend infographics
- **Content calendar**: Batch-create images for upcoming posts
- **API endpoint**: Expose as webhook for external tools

---

## 🤝 Contributing

This workflow represents hundreds of hours of prompt engineering research. Improvements welcome!

**Share your results**: Post successful images and topics to inspire others.

---

## 📄 License

This workflow is provided as-is for community use. Modify as needed for your use case.

---

<div align="center">

**The most advanced workflow in the n8n-selfhosting-workflows collection**

[← Simpler Version](../web-search-image-generation/) | [Back to Workflows](../)

---

**Made for the n8n community** 🚀

*Professional infographics made simple*

</div>

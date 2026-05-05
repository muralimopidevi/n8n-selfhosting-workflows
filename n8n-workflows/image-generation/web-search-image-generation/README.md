# Web Search + Image Generation

<div align="center">

**Research-powered image generation: Search the web → Generate contextual images with Google Gemini**

[![n8n](https://img.shields.io/badge/n8n-workflow-orange.svg)](https://n8n.io/)
[![Firecrawl](https://img.shields.io/badge/Firecrawl-Web%20Search-green.svg)](https://firecrawl.dev/)
[![Gemini](https://img.shields.io/badge/Gemini-Image%20Gen-blue.svg)](https://ai.google.dev/)

</div>

---

## 📋 Overview

This workflow combines web research with AI image generation. It searches the web for current information about your topic, then uses that research to generate a contextually accurate and up-to-date infographic.

**Perfect for**: Creating images about current events, trending technologies, or topics requiring accurate real-world information.

---

## ✨ Features

- 🔍 **Web research integration** - Uses Firecrawl to search for current information
- 🎨 **Context-aware generation** - Images reflect real-world data
- 📊 **Automatic prompt building** - Combines topic + research into detailed prompts
- ⚡ **Smart error handling** - Continues even if search fails
- 🏷️ **Topic-based filenames** - Images named after your search topic

---

## 🏗️ Workflow Structure

```
Manual Trigger
     ↓
Set Search Topic (Code Node)
     ↓
Firecrawl Web Search
     ↓
Build Prompt (Code Node)
     ↓
Generate Image (Gemini)
     ↓
Decode Image (Code Node)
     ↓
Output: Image Binary
```

### Nodes

1. **manual_trigger**: Start the workflow manually
2. **set_search_topic**: Define topic and description for research
3. **firecrawl_search**: Search web for relevant content
4. **build_prompt**: Combine topic + research into image prompt
5. **generate_image**: Generate image using Gemini
6. **decode_image**: Convert output to usable image file

---

## 📦 Prerequisites

### Required APIs & Credentials

- ✅ **n8n instance** (self-hosted or cloud)
- ✅ **Google Gemini API key** ([Get here](https://ai.google.dev/))
- ✅ **Firecrawl API key** ([Get here](https://firecrawl.dev/))

### n8n Requirements

- n8n version 1.0+ with LangChain nodes
- Firecrawl integration: `@mendable/n8n-nodes-firecrawl`
- Google Gemini node: `@n8n/n8n-nodes-langchain`

---

## 🚀 Setup Instructions

### Step 1: Get Firecrawl API Key

1. Go to [firecrawl.dev](https://firecrawl.dev/)
2. Sign up for a free account
3. Navigate to **API Keys** in your dashboard
4. Copy your API key

**Free Tier**: Firecrawl offers a free tier with limited searches per month

### Step 2: Get Google Gemini API Key

1. Go to [Google AI Studio](https://ai.google.dev/)
2. Sign in with Google account
3. Navigate to **API Keys**
4. Create an API key
5. Copy the key

### Step 3: Configure n8n Credentials

#### Firecrawl Credential

1. In n8n, go to **Credentials** → **Add Credential**
2. Search for **Firecrawl API**
3. Enter your Firecrawl API key
4. Test and save

#### Google Gemini Credential

1. In n8n, go to **Credentials** → **Add Credential**
2. Search for **Google PaLM API** or **Google Gemini**
3. Enter your Google API key
4. Test and save

### Step 4: Import Workflow

1. Download or copy `web_search_image_generation.json`
2. In n8n, click **Import from File**
3. Select the JSON file
4. Click **Import**

### Step 5: Update Credential References

#### Update Firecrawl Node

1. Click on **firecrawl_search** node
2. Select your Firecrawl credential
3. Save

#### Update Gemini Node

1. Click on **generate_image** node
2. Select your Google Gemini credential
3. Save

---

## 🎯 How to Use

### Basic Usage

1. Open the workflow in n8n
2. Click on the **set_search_topic** node
3. Edit `TOPIC` and `DESCRIPTION` variables:

```javascript
const TOPIC = 'Your search topic here';
const DESCRIPTION = `Detailed description of what you want to research and visualize`;
```

4. Click **Execute Workflow**
5. View generated image in final node output

---

## 📝 Example Use Cases

### Example 1: Technology Trends

```javascript
const TOPIC = 'The rise of AI agents in 2026';
const DESCRIPTION = `Research how autonomous AI agents are being used in 
software development, customer service, and business automation. Focus on 
real tools and platforms that launched in 2025-2026.`;
```

**Result**: Image showing current AI agent platforms with accurate logos and names from web research.

### Example 2: Market Analysis

```javascript
const TOPIC = 'Remote work statistics 2026';
const DESCRIPTION = `Find latest data on remote work adoption rates, 
productivity metrics, and popular tools. Show percentage breakdowns and 
growth trends from 2020 to 2026.`;
```

**Result**: Infographic with actual statistics from recent studies and reports.

### Example 3: Product Comparison

```javascript
const TOPIC = 'Top project management tools comparison';
const DESCRIPTION = `Compare features, pricing, and user bases of leading 
project management platforms: Asana, Monday.com, ClickUp, Notion. 
Show current market positions and key differentiators.`;
```

**Result**: Comparison chart with accurate current information about each platform.

---

## 🔧 How It Works

### 1. Web Search Phase

The **firecrawl_search** node:
- Combines your topic + description into a search query
- Adds keywords like "visual infographic 2026" for relevant results
- Returns up to 6 web pages with titles and descriptions

### 2. Prompt Building Phase

The **build_prompt** node:
- Extracts titles and snippets from search results
- Combines topic, description, and research context
- Creates a detailed image generation prompt including:
  - Visual requirements (size, style, colors)
  - Research context for accuracy
  - Design specifications

### 3. Image Generation Phase

The **generate_image** node:
- Sends the enriched prompt to Google Gemini
- Receives image data (base64 or binary)
- Handles different response formats automatically

### 4. Decoding Phase

The **decode_image** node:
- Converts Gemini output to downloadable image file
- Creates filename from topic + timestamp
- Adds metadata (size, mime type, generation time)

---

## 🎨 Customization

### Adjust Search Results Count

In **firecrawl_search** node, change the `limit` parameter:

```json
"limit": 6  // Default (balance speed vs context)
"limit": 3  // Faster, less context
"limit": 10 // Slower, more context
```

### Modify Prompt Template

In **build_prompt** node, customize the image prompt template:

```javascript
const imagePrompt = `Create a professional, editorial-quality infographic...
// Add your custom instructions here
// Example: Emphasize specific colors, layouts, or styles
`;
```

### Search Query Optimization

In **firecrawl_search** node, modify the query construction:

```javascript
"query": "={{ $json.topic + ' ' + $json.description + ' visual infographic 2026' }}"
// Change to:
"query": "={{ $json.topic + ' statistics data visualization' }}"
// or
"query": "={{ $json.topic + ' architecture diagram blueprint' }}"
```

---

## 📊 Output

### Image Files

- **Format**: JPEG or PNG (automatic detection)
- **Filename**: `{topic-slug}_{timestamp}.jpg`
  - Example: `remote_work_statistics_2026_2026-05-05-143022.jpg`
- **Dimensions**: As specified in prompt (default 1440x756px)

### Metadata

```json
{
  "filename": "ai_trends_2026_2026-05-05-143000.jpg",
  "mimeType": "image/jpeg",
  "size_bytes": 245890,
  "generated_at": "2026-05-05T14:30:00.000Z"
}
```

---

## 🐛 Troubleshooting

### Issue: Firecrawl search fails

**Symptoms**: "No web research available" in logs

**Solutions**:
- Check Firecrawl API key is valid
- Verify you haven't exceeded API quota
- Try a more specific search topic
- Check Firecrawl service status

**Note**: Workflow continues even if search fails, using only topic/description

### Issue: Generated image doesn't match research

**Cause**: Prompt building might not emphasize research context enough

**Solutions**:
- Edit **build_prompt** node to give more weight to search results
- Increase search result limit from 6 to 10
- Make topic and description more specific

### Issue: Image includes outdated information

**Cause**: Gemini's training data vs web search results

**Solutions**:
- Add "2026" or "latest" to your topic
- Include "current data" in description
- Modify prompt to emphasize "use research context for accuracy"

### Issue: Search returns irrelevant results

**Cause**: Topic too broad or vague

**Solutions**:
- Make topic more specific
- Add clarifying keywords to description
- Adjust the search query in **firecrawl_search** node

---

## 💡 Tips for Better Results

### Search Topic Tips

✅ **Good topics**:
- "State of AI infrastructure in 2026"
- "Docker vs Kubernetes adoption rates 2025-2026"
- "Top 5 CRM platforms market share 2026"

❌ **Avoid vague topics**:
- "Technology stuff"
- "Business things"
- "Some data"

### Description Tips

1. **Be specific**: Mention exact aspects you want researched
2. **Include timeframes**: "2025-2026", "latest", "current"
3. **Name entities**: "OpenAI GPT-4", "Microsoft Azure", "GitHub Copilot"
4. **Request data types**: "statistics", "comparisons", "architecture", "workflow"

### Image Style Preferences

Add style instructions to your description:
- "Use modern isometric 3D style"
- "Create a professional comparison chart"
- "Design a timeline visualization"
- "Show as a hub-and-spoke diagram"

---

## 📈 Performance

### Execution Time

- **Web search**: 2-5 seconds
- **Prompt building**: < 1 second
- **Image generation**: 5-15 seconds
- **Total**: ~10-25 seconds per execution

### API Costs

- **Firecrawl**: Free tier available, paid plans for high volume
- **Google Gemini**: Usage-based pricing, check current rates at [ai.google.dev/pricing](https://ai.google.dev/pricing)

---

## 🔄 Next Steps

### Upgrade to Enhanced Version

Try the **[Enhanced Prompt Engineering workflow](../enhanced-prompt-image-generation/)** which adds:
- AI-powered prompt optimization
- Style selection based on content structure
- Multi-variation generation
- Advanced visual analysis

### Batch Processing

Modify the workflow to:
1. Read topics from a CSV or Google Sheets
2. Loop through each topic
3. Generate multiple images automatically

### Integration Ideas

- **Webhook trigger**: Generate images from external requests
- **Schedule trigger**: Daily trend visualization
- **Slack integration**: Post generated images to channels
- **Google Drive**: Auto-save images to Drive folders

---

## 🎓 Learning Resources

### Firecrawl Documentation
- [Firecrawl API Docs](https://docs.firecrawl.dev/)
- [Search API Reference](https://docs.firecrawl.dev/api/search)

### Google Gemini
- [Gemini API Documentation](https://ai.google.dev/docs)
- [Image Generation Guide](https://ai.google.dev/docs/image_generation)

### n8n Resources
- [n8n Workflow Examples](https://n8n.io/workflows/)
- [Working with Binary Data](https://docs.n8n.io/data/binary-data/)
- [Error Handling](https://docs.n8n.io/workflows/error-handling/)

---

## 🤝 Contributing

Improvements and variations welcome! Share your customizations with the community.

---

## 📄 License

This workflow is provided as-is for community use. Modify as needed for your use case.

---

<div align="center">

**Part of the n8n-selfhosting-workflows collection**

[← Basic Version](../basic-text-to-image/) | [View Enhanced Version →](../enhanced-prompt-image-generation/)

[Back to Workflows](../)

</div>

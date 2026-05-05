# Basic Text-to-Image with Gemini

<div align="center">

**Simple text prompt to image generation using Google Gemini's image generation capabilities**

[![n8n](https://img.shields.io/badge/n8n-workflow-orange.svg)](https://n8n.io/)
[![Gemini](https://img.shields.io/badge/Gemini-Image%20Gen-blue.svg)](https://ai.google.dev/)

</div>

---

## 📋 Overview

This is the simplest workflow for generating images with Google Gemini. You provide a text prompt, and the workflow generates a professional-quality image.

**Perfect for**: Quick image generation, testing prompts, learning the basics of AI image generation in n8n.

---

## ✨ Features

- ✅ **Simple 4-node workflow** - Easy to understand and modify
- ✅ **Direct prompt input** - Edit the prompt directly in the workflow
- ✅ **Automatic image decoding** - Handles Gemini's output format
- ✅ **Timestamped filenames** - Organized output with date-time stamps
- ✅ **Error handling** - Clear error messages if generation fails

---

## 🏗️ Workflow Structure

```
Manual Trigger
     ↓
Set Prompt (Code Node)
     ↓
Generate Image (Gemini)
     ↓
Decode Image (Code Node)
     ↓
Output: Image Binary
```

### Nodes

1. **manual_trigger**: Start the workflow manually
2. **set_prompt**: Define your image generation prompt
3. **generate_image**: Calls Google Gemini image generation API
4. **decode_image**: Converts Gemini output to usable image binary

---

## 📦 Prerequisites

### Required

- ✅ **n8n instance** (self-hosted or cloud)
- ✅ **Google Gemini API key** (get from [Google AI Studio](https://ai.google.dev/))
- ✅ **n8n Google Gemini credential** configured

### n8n Version

- Requires n8n version 1.0+ with LangChain nodes support
- Google Gemini node: `@n8n/n8n-nodes-langchain`

---

## 🚀 Setup Instructions

### Step 1: Get Google Gemini API Key

1. Go to [Google AI Studio](https://ai.google.dev/)
2. Sign in with your Google account
3. Navigate to **API Keys** section
4. Click **Get API Key** or **Create API Key**
5. Copy the API key

### Step 2: Configure n8n Credential

1. In n8n, go to **Credentials** menu
2. Click **Add Credential**
3. Search for **Google PaLM API** or **Google Gemini**
4. Enter your API key
5. Test the connection
6. Save the credential

### Step 3: Import Workflow

1. Copy the `basic_text_to_image.json` file
2. In n8n, click **Import from File** or **Import from URL**
3. Paste the workflow JSON
4. Click **Import**

### Step 4: Update Credential Reference

1. Click on the **generate_image** node
2. In the **Credentials** section, select your Google Gemini credential
3. Save the node

### Step 5: Customize the Prompt

1. Click on the **set_prompt** node
2. Edit the `PROMPT` variable with your desired image description
3. Save the node

---

## 🎯 How to Use

### Running the Workflow

1. Open the workflow in n8n
2. Click the **Execute Workflow** button
3. The workflow will run automatically
4. View the generated image in the final node output

### Customizing Your Prompt

Edit the `PROMPT` variable in the **set_prompt** node:

```javascript
const PROMPT = `Your detailed image description here.

Include:
- Subject matter
- Style (professional, artistic, technical)
- Colors and mood
- Image dimensions (e.g., 1440x756px)
- Any specific elements to include`;
```

### Prompt Writing Tips

✅ **Good prompt example**:
```
Create a professional LinkedIn-style infographic showing cloud computing architecture.
Use a modern blue and white color scheme with clean lines.
Show three layers: Frontend (React), Backend (Node.js), Database (PostgreSQL).
Include server icons and connecting arrows.
1440x756px landscape format, suitable for business social media.
```

❌ **Avoid vague prompts**:
```
Make a nice image about technology
```

### Best Practices

1. **Be specific**: Describe exactly what you want
2. **Mention dimensions**: Gemini works best with specified sizes
3. **Describe style**: Professional, editorial, minimalist, etc.
4. **Include colors**: Suggest a color palette
5. **Purpose context**: LinkedIn post, blog header, presentation slide
6. **Text elements**: If you want text in the image, specify exact wording (max 5 words per label)

---

## 📤 Output

### Binary Image Data

The workflow outputs:
- **Binary attachment**: Ready-to-download image file
- **Filename**: `gemini_image_YYYY-MM-DD-HHMMSS.jpg` or `.png`
- **Metadata**: File size, mime type, generation timestamp

### Downloading Images

1. After execution, click on the **decode_image** node
2. View the output in the node panel
3. Click the **Download** icon on the binary data
4. Image will be saved to your downloads folder

---

## 🔧 Configuration

### Model Selection

The workflow uses `models/gemini-3-pro-image-preview` by default. You can change this in the **generate_image** node:

```json
"modelId": {
  "value": "models/gemini-3-pro-image-preview"
}
```

### Image Format

Gemini typically outputs JPEG or PNG. The workflow automatically detects and handles both formats.

---

## 🐛 Troubleshooting

### Issue: "No image in response"

**Cause**: Gemini returned text instead of an image

**Solutions**:
- Check that you're using an image generation model
- Verify your prompt is requesting an image
- Ensure your API key has image generation permissions

### Issue: "Credential not found"

**Cause**: Google Gemini credential not configured

**Solutions**:
- Create a Google PaLM/Gemini credential in n8n
- Update the credential reference in the **generate_image** node
- Verify the API key is valid

### Issue: "API quota exceeded"

**Cause**: Too many requests or quota limits reached

**Solutions**:
- Check your Google AI Studio quota
- Wait a few minutes before trying again
- Consider upgrading your API plan

### Issue: "Image quality is poor"

**Cause**: Prompt lacks detail or specificity

**Solutions**:
- Add more descriptive details to your prompt
- Specify style, colors, composition
- Include dimensions (e.g., 1440x756px)
- Mention "high quality", "professional", "detailed"

---

## 💡 Example Prompts

### Example 1: Tech Infographic
```javascript
const PROMPT = `Create a professional infographic showing the AI development stack.
Modern isometric 3D style with a dark navy background (#0A1628).
Three platforms arranged horizontally:
1. LEFT: Data layer - database cylinder with glowing data streams
2. CENTER: Model layer - neural network sphere with pulsing nodes
3. RIGHT: API layer - cloud server with connection lines
Connect platforms with cyan neon data flows.
Label each platform in bold white text.
1440x756px landscape, high quality, suitable for LinkedIn.`;
```

### Example 2: Comparison Illustration
```javascript
const PROMPT = `Create a professional comparison illustration: Traditional AI vs Modern AI.
Split the image vertically down the middle.
LEFT side: Grey tones, locked padlock icon, single server, label "Traditional AI"
RIGHT side: Vibrant blue and green, open network diagram, cloud icons, label "Modern AI"
Add 3 bullet points under each side.
Clean, modern design, 1080x1080px square format.`;
```

### Example 3: Process Flow
```javascript
const PROMPT = `Create a professional workflow diagram showing content creation process.
Five circular stages arranged in a horizontal flow:
1. Research (magnifying glass icon)
2. Draft (document icon)
3. Edit (pencil icon)
4. Review (checkmark icon)
5. Publish (rocket icon)
Connect stages with right-pointing arrows.
Use blue gradient background, white icons, bold labels below each stage.
1440x756px landscape, editorial quality.`;
```

---

## 🎓 Learning Resources

### Prompt Engineering for Images
- [Google AI Prompting Guide](https://ai.google.dev/docs/prompting_intro)
- [Image Prompt Best Practices](https://ai.google.dev/docs/image_generation)

### n8n Documentation
- [n8n Workflow Basics](https://docs.n8n.io/workflows/)
- [Working with Binary Data](https://docs.n8n.io/data/binary-data/)
- [Code Node Guide](https://docs.n8n.io/code-examples/)

---

## 🔄 Next Steps

Once you're comfortable with basic image generation, try:

1. **[Web Search + Image Generation](../web-search-image-generation/)** - Add research before generation
2. **[Enhanced Prompt Engineering](../enhanced-prompt-image-generation/)** - Use AI to optimize your prompts
3. **Batch Processing** - Generate multiple images from a list of prompts
4. **Automated Workflows** - Trigger image generation from webhooks or schedules

---

## 📝 Notes

- **API Costs**: Google Gemini has usage quotas and potential costs for high volume
- **Rate Limits**: Be mindful of API rate limits when running multiple executions
- **Generation Time**: Image generation typically takes 5-15 seconds
- **Quality**: Results depend heavily on prompt quality and specificity

---

## 🤝 Contributing

Have improvements or variations of this workflow? Share them!

---

## 📄 License

This workflow is provided as-is for community use. Modify as needed for your use case.

---

<div align="center">

**Part of the n8n-selfhosting-workflows collection**

[← Back to Workflows](../) | [View Enhanced Version →](../enhanced-prompt-image-generation/)

</div>

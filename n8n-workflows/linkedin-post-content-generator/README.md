# LinkedIn Post Content Generator - n8n Workflow

<div align="center">

**Automated LinkedIn Content Creation Pipeline with AI-Powered Research, Generation, and Review**

[![n8n](https://img.shields.io/badge/n8n-workflow-orange.svg)](https://n8n.io/)
[![Status](https://img.shields.io/badge/Status-Production--Ready-green.svg)]()

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Workflow Stages](#-workflow-stages)
- [Prerequisites](#-prerequisites)
- [Google Drive Setup](#-google-drive-setup)
- [Google Sheets Setup](#-google-sheets-setup)
- [API & Credentials Setup](#-api--credentials-setup)
- [n8n Workflow Import](#-n8n-workflow-import)
- [Configuration](#-configuration)
- [How to Use](#-how-to-use)
- [Workflow Stages Explained](#-workflow-stages-explained)
- [Review & Approval Process](#-review--approval-process)
- [Troubleshooting](#-troubleshooting)

---

## 🌟 Overview

This n8n workflow automates the complete LinkedIn post creation process, from research to final approval. It combines:

- **AI-powered research** using Firecrawl for topic exploration
- **Multi-version content generation** using LLMs (Azure OpenAI, etc.)
- **AI image generation** with multiple style variations
- **Email-based review workflow** with approval/rethink/stop options
- **Automatic retry logic** with version control
- **Google Drive integration** for organized file storage

The workflow runs automatically (every 3 minutes by default), picks up new topics from a Google Sheet, researches them, generates content variations, creates images, and sends you an email for approval—all hands-free until the review stage.

---

## ✨ Features

### 🤖 Automated Pipeline
- **Scheduled execution** - Checks for new topics every 3 minutes
- **Multi-stage processing** - Handles research, content gen, imaging, and review
- **Resume capability** - Can resume from any stage if interrupted
- **Error handling** - Comprehensive error tracking and logging

### 📝 Content Generation
- **Dual-version content** - Generates 2 different content variations
- **AI research integration** - Uses Firecrawl to gather topic research
- **Smart prompting** - Engineer-optimized prompts for each angle
- **Character limits** - Validates content length (500-1000 chars target)

### 🎨 Image Generation
- **Dual-image creation** - 2 AI-generated images per post
- **Multiple models** - Supports various image generation APIs
- **Base64 handling** - Automatic decode and storage
- **Organized storage** - Images saved with content in Google Drive

### ✅ Review Workflow
- **Email notifications** - HTML email with all content/image variations
- **4 approval combinations** - Choose your preferred content + image pairing
- **Rethink option** - Regenerate content/images from scratch
- **Image-only regeneration** - Keep content, regenerate images
- **Stop option** - Cancel a post at any stage

### 📊 Version Control
- **Rewrite tracking** - Counts regeneration attempts
- **Max rewrites limit** - Prevents infinite loops (default: 10)
- **Stage tracking** - Every stage logged in Google Sheet
- **File versioning** - Content versions saved as V1/V2

---

## 🏗️ Workflow Stages

```
1. PENDING          →  Waiting to be picked up
2. RESEARCHING      →  Firecrawl gathering data
3. RESEARCHED       →  Research saved, ready for content
4. CONTENT_GEN      →  LLM generating content variations
5. DRAFTED          →  Content saved, ready for images
6. IMAGING          →  AI generating images
7. IMAGED           →  Images saved, ready for review
8. REVIEW_SENT      →  Email sent, waiting for decision
9. RETHINKING       →  User requested regeneration
10. REVIEW_PENDING  →  Prior review waiting, new topics deprioritized
11. APPROVED        →  Final approval given
12. REVIEW_STOPPED  →  User cancelled the post
```

---

## 📦 Prerequisites

### Required Services

1. **n8n instance** (self-hosted or cloud)
2. **Google Account** with:
   - Google Drive API enabled
   - Google Sheets API enabled
   - Gmail API enabled (for sending review emails)
3. **Firecrawl API** account and API key
4. **LLM API** - One of:
   - Azure OpenAI
   - OpenAI
   - Anthropic Claude
   - Any other compatible LLM
5. **Image Generation API** - One of:
   - DALL-E (OpenAI)
   - Stable Diffusion
   - Midjourney API
   - Replicate
   - Any compatible image API

### System Requirements
- n8n version 1.0+ (for latest node versions)
- Sufficient execution time limits (some stages can take 2-5 minutes)
- Webhook support (for review decision handling)

---

## 🗂️ Google Drive Setup

### Step 1: Create Root Folder Structure

In your Google Drive, create this exact structure:

```
📁 N8N (or any name you prefer)
└── 📁 linkedin_archive
```

### Step 2: Get Folder ID

1. Open the `linkedin_archive` folder in Google Drive
2. Look at the URL: `https://drive.google.com/drive/folders/YOUR_FOLDER_ID_HERE`
3. Copy the folder ID (everything after `/folders/`): `YOUR_FOLDER_ID_HERE`
4. Save this ID - you'll need it for workflow configuration

### Step 3: Folder Permissions

Ensure your Google Drive credentials have **write access** to this folder.

### What Gets Created

For each post, the workflow creates:

```
📁 linkedin_archive
└── 📁 topic-{post_id}
    ├── 📄 research.json           (AI research data)
    ├── 📄 content_v1.txt          (First content variation)
    ├── 📄 content_v2.txt          (Second content variation)
    ├── 🖼️ image_v1.png            (First image)
    └── 🖼️ image_v2.png            (Second image)
```

---

## 📊 Google Sheets Setup

### Step 1: Create a New Google Sheet

1. Go to [Google Sheets](https://sheets.google.com)
2. Create a new spreadsheet
3. Name it: `linkedin_post_pipeline` (or any name)

### Step 2: Set Up Columns

Create a sheet named `linkedin_posts` with these **exact** column headers (Row 1):

| A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| post_id | topic | description | user_emails | stage | created_at | research_drive_url | post_folder_url | content_v1_drive_url | content_v2_drive_url | image_v1_drive_url | image_v2_drive_url | review_decision | rewrite_count | error_log | locked |

### Step 3: Column Descriptions

| Column | Description | You Fill | Auto-Filled |
|--------|-------------|----------|-------------|
| `post_id` | Unique ID | | ✅ |
| `topic` | Post topic | ✅ | |
| `description` | Additional context | ✅ | |
| `user_emails` | Your email(s) | ✅ | |
| `stage` | Current stage | ✅ (set to `pending`) | ✅ (updated automatically) |
| `created_at` | Timestamp | | ✅ |
| `research_drive_url` | Research file link | | ✅ |
| `post_folder_url` | Post folder link | | ✅ |
| `content_v1_drive_url` | Content V1 link | | ✅ |
| `content_v2_drive_url` | Content V2 link | | ✅ |
| `image_v1_drive_url` | Image V1 link | | ✅ |
| `image_v2_drive_url` | Image V2 link | | ✅ |
| `review_decision` | Your approval choice | | ✅ (via webhook) |
| `rewrite_count` | Regeneration count | | ✅ |
| `error_log` | Error messages | | ✅ |
| `locked` | Processing lock | | ✅ |

### Step 4: Add Your First Topic

Add a row with your data:

| post_id | topic | description | user_emails | stage | (other columns empty) |
|---------|-------|-------------|-------------|-------|---------------------|
| *(leave empty)* | AI in Healthcare | Focus on recent breakthroughs | your.email@example.com | pending | |

### Step 5: Get Sheet ID

1. Look at your sheet URL: `https://docs.google.com/spreadsheets/d/YOUR_GOOGLE_SHEET_ID_HERE/edit`
2. Copy the sheet ID (between `/d/` and `/edit`): `YOUR_GOOGLE_SHEET_ID_HERE`
3. Save this ID - you'll need it for workflow configuration

---

## 🔑 API & Credentials Setup

### 1. Google APIs Setup

#### Enable APIs
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project (or use existing)
3. Enable these APIs:
   - Google Drive API
   - Google Sheets API
   - Gmail API

#### Create OAuth2 Credentials
1. Go to **APIs & Services** > **Credentials**
2. Click **Create Credentials** > **OAuth client ID**
3. Application type: **Web application**
4. Add authorized redirect URIs:
   - `https://your-n8n-instance.com/rest/oauth2-credential/callback`
   - `http://localhost:5678/rest/oauth2-credential/callback` (for local dev)
5. Save **Client ID** and **Client Secret**

### 2. Firecrawl API Setup

1. Sign up at [Firecrawl](https://www.firecrawl.dev/)
2. Get your API key from the dashboard
3. Save it for n8n credentials

### 3. LLM API Setup

Choose one or more:

#### Azure OpenAI
1. Create Azure OpenAI resource
2. Deploy models (e.g., gpt-4, gpt-3.5-turbo)
3. Get:
   - API endpoint
   - API key
   - Deployment names

#### OpenAI
1. Go to [OpenAI Platform](https://platform.openai.com/)
2. Create API key
3. Save for n8n

#### Other LLMs
- Follow provider documentation
- Ensure API compatibility with n8n HTTP Request nodes

### 4. Image Generation API Setup

Choose one:

#### DALL-E (OpenAI)
- Use same OpenAI API key
- Model: `dall-e-3` or `dall-e-2`

#### Stable Diffusion / Other
- Get API endpoint and key
- Configure in workflow

---

## 📥 n8n Workflow Import

### Step 1: Import JSON

1. Open your n8n instance
2. Click **+** (Add Workflow)
3. Click **Import from File**
4. Select `linkedin_post_content_generator.json`
5. Click **Save**

### Step 2: Configure Credentials

The workflow uses these credentials (create each in n8n):

#### Google Sheets OAuth2
- Name: `google_sheets_linkedin`
- Type: Google Sheets OAuth2 API
- Use OAuth2 credentials from Google Cloud Console

#### Google Drive OAuth2
- Name: `google_drive_linkedin`
- Type: Google Drive OAuth2 API
- Use same OAuth2 credentials

#### Gmail OAuth2
- Name: `google_gmail_oauth2_api`
- Type: Gmail OAuth2 API
- Use same OAuth2 credentials

#### Firecrawl API
- Name: `firecrawl_api`
- Type: Header Auth (or HTTP Request)
- Header: `Authorization`
- Value: `Bearer YOUR_FIRECRAWL_API_KEY`

#### LLM API (Azure OpenAI example)
- Name: `azure_openai_api`
- Type: HTTP Header Auth
- Configure endpoint, key, deployment

#### Image Generation API
- Configure based on your chosen service

---

## ⚙️ Configuration

### Step 1: Update Google Sheet ID

Find all nodes with Google Sheets operations and update:

**Current (placeholder):**
```json
"documentId": {
  "value": "YOUR_GOOGLE_SHEET_ID_HERE"
}
```

**Replace with your ID:**
```json
"documentId": {
  "value": "YOUR_GOOGLE_SHEET_ID_HERE"
}
```

**Nodes to update:**
- `read_all_rows`
- `update_stage_researching`
- `update_sheet_research_url`
- `update_sheet_drafted`
- `update_sheet_imaged`
- `update_sheet_review_sent`
- `update_sheet_approved`
- `update_sheet_stopped`
- (and others with Google Sheets operations)

### Step 2: Update Parent Folder ID

Find the `create_post_folder` node and update:

**Current (placeholder):**
```json
"folderId": {
  "value": "YOUR_LINKEDIN_ARCHIVE_FOLDER_ID_HERE"
}
```

**Replace with your folder ID:**
```json
"folderId": {
  "value": "YOUR_LINKEDIN_ARCHIVE_FOLDER_ID_HERE"
}
```

### Step 3: Configure LLM Nodes

Update these nodes with your LLM settings:

- `prompt_engineer` - Generates content prompts
- `agent_v1` - Generates content variation 1
- `agent_v2` - Generates content variation 2
- `image_prompt_engineer` - Generates image prompts

**Example for Azure OpenAI:**
- Endpoint: `https://your-resource.openai.azure.com`
- API Version: `2024-02-15-preview`
- Deployment: `gpt-4` (or your deployment name)

### Step 4: Configure Image Generation Nodes

Update these nodes:

- `generate_image_v1`
- `generate_image_v2`

Configure based on your image API (DALL-E, Stable Diffusion, etc.)

### Step 5: Configure Schedule Trigger

The `trigger_main` node runs every 3 minutes by default.

**To change frequency:**
1. Open `trigger_main` node
2. Adjust interval (minutes, hours, or cron)
3. Example: Every 5 minutes, every hour, etc.

### Step 6: Set Webhook URL (for review responses)

The `webhook_review` node receives approval responses.

1. Open the node
2. Copy the webhook URL (e.g., `https://your-n8n.com/webhook/linkedin-review`)
3. This URL is automatically included in review emails

---

## 🚀 How to Use

### Initial Setup (One Time)

1. ✅ Complete all prerequisites and setup steps above
2. ✅ Import workflow and configure credentials
3. ✅ Update Google Sheet ID and folder IDs
4. ✅ Test credentials in n8n
5. ✅ Create Google Drive folder structure
6. ✅ Set up Google Sheet with correct columns

### Creating a Post

#### Step 1: Add Topic to Sheet

Open your Google Sheet and add a new row:

| post_id | topic | description | user_emails | stage |
|---------|-------|-------------|-------------|-------|
| | "Future of Remote Work" | "Focus on productivity tools and work-life balance trends" | your.email@example.com | pending |

**Tips:**
- Leave `post_id` empty (auto-generated)
- `topic` - Clear, concise topic (50-100 chars)
- `description` - Additional context, angle, focus areas (optional but recommended)
- `user_emails` - Your email for review (can be comma-separated for multiple)
- `stage` - **MUST be `pending`** (lowercase)

#### Step 2: Activate Workflow

1. In n8n, open the workflow
2. Click **Active** toggle (top-right)
3. Workflow will check every 3 minutes

#### Step 3: Monitor Progress

Watch the `stage` column update automatically:

```
pending → researching → researched → content_generating → 
drafted → imaging → imaged → review_sent
```

**Each stage takes:**
- Research: 1-3 minutes
- Content generation: 2-4 minutes  
- Image generation: 1-2 minutes
- **Total: ~5-10 minutes** from pending to review_sent

#### Step 4: Check Your Email

You'll receive an email titled:
**"LinkedIn Post Review: [Your Topic]"**

The email contains:
- 📝 **Content Version 1** with link
- 📝 **Content Version 2** with link
- 🖼️ **Image Version 1** with preview
- 🖼️ **Image Version 2** with preview

#### Step 5: Review & Approve

Click one of the buttons in the email:

**Approve a Combination:**
- **Content V1 + Image V1** ✅
- **Content V1 + Image V2** ✅
- **Content V2 + Image V1** ✅
- **Content V2 + Image V2** ✅

**Other Actions:**
- **Rethink** - Regenerate content + images from scratch
- **New Images** - Keep content, regenerate images only
- **Stop Post** - Cancel this post entirely

#### Step 6: Final Result

After approval:
- `stage` updates to `approved`
- All files remain in Google Drive folder
- You can copy content and images to LinkedIn

### Adding Multiple Posts

Simply add multiple rows with `stage = pending`. The workflow processes one at a time (oldest first).

---

## 🔄 Workflow Stages Explained

### Stage 1: PENDING
- **Trigger**: New row with `stage = pending`
- **Action**: Workflow picks up, generates unique `post_id`, updates `created_at`
- **Next**: `researching`

### Stage 2: RESEARCHING
- **Action**: Firecrawl API searches the web for topic
- **Output**: Research JSON saved to Google Drive
- **Sheet updated**: `research_drive_url`, `post_folder_url`
- **Next**: `researched`

### Stage 3: RESEARCHED
- **Action**: Research complete, ready for content generation
- **Next**: `content_generating`

### Stage 4: CONTENT_GENERATING
- **Action**:
  1. Downloads research from Drive
  2. LLM generates two prompts (different angles)
  3. LLM generates two content variations (V1, V2)
  4. Saves content as text files to Drive
- **Sheet updated**: `content_v1_drive_url`, `content_v2_drive_url`
- **Next**: `drafted`

### Stage 5: DRAFTED
- **Action**: Content ready for image generation
- **Next**: `imaging`

### Stage 6: IMAGING
- **Action**:
  1. Downloads content variations
  2. LLM generates image prompts (2 variations)
  3. Image API generates images (V1, V2)
  4. Decodes base64, saves to Drive
- **Sheet updated**: `image_v1_drive_url`, `image_v2_drive_url`
- **Next**: `imaged`

### Stage 7: IMAGED
- **Action**: All assets ready for review
- **Next**: `review_sent`

### Stage 8: REVIEW_SENT
- **Action**: Email sent to `user_emails` with all content/images
- **Waiting**: User decision via webhook
- **Next**: `approved`, `rethinking`, `drafted`, or `review_stopped`

### Stage 9: RETHINKING
- **Action**: User requested regeneration
- **Workflow**:
  1. Deletes old content/images
  2. Reuses research
  3. Regenerates content + images
- **Next**: Back to `drafted` → `imaging` → `review_sent`

### Stage 10: REVIEW_PENDING
- **Purpose**: Deprioritize rows with review already sent
- **Note**: New pending topics get priority over review_pending

### Stage 11: APPROVED
- **Action**: User approved, post complete
- **Final stage**: No further automation

### Stage 12: REVIEW_STOPPED
- **Action**: User cancelled post
- **Final stage**: Post abandoned

---

## 📧 Review & Approval Process

### Email Structure

```
Subject: LinkedIn Post Review: [Your Topic]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📝 CONTENT VARIATIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Content V1
Version 1
[View on Drive →]

[Content preview or full text]

─────────────────────────────────────────────

Content V2
Version 2
[View on Drive →]

[Content preview or full text]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🖼️ IMAGE VARIATIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Image V1
[View on Drive →]
[Image preview]

─────────────────────────────────────────────

Image V2
[View on Drive →]
[Image preview]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ APPROVE a Combination
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Click to approve your preferred content + image pairing:

[Content V1 + Image V1]   [Content V1 + Image V2]

[Content V2 + Image V1]   [Content V2 + Image V2]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔄 Other Actions
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[New Images]  [Rethink]  [Stop Post]

New Images - Regenerates both images. You will receive a new review email.
Rethink - Rewrites content + images from scratch. New email will arrive.
Stop Post - Cancels this post. Stage set to review_stopped.
```

### Decision Options

| Button | Action | What Happens |
|--------|--------|--------------|
| **Content V1 + Image V1** | Approve combination | `stage` → `approved`, `review_decision` saved |
| **Content V1 + Image V2** | Approve combination | `stage` → `approved`, `review_decision` saved |
| **Content V2 + Image V1** | Approve combination | `stage` → `approved`, `review_decision` saved |
| **Content V2 + Image V2** | Approve combination | `stage` → `approved`, `review_decision` saved |
| **New Images** | Regenerate images only | `stage` → `imaging`, content unchanged, new email sent |
| **Rethink** | Regenerate everything | `stage` → `rethinking`, deletes content/images, new email sent |
| **Stop Post** | Cancel post | `stage` → `review_stopped`, no further action |

### Rewrite Limits

- Default max rewrites: **10**
- After 10 rethinks, workflow stops automatically
- Counter resets when post is approved or stopped
- Check `rewrite_count` column in Google Sheet

---

## 🔧 Troubleshooting

### Common Issues

#### 1. Workflow Not Triggering

**Problem**: New `pending` rows not picked up

**Solutions:**
- Check workflow is **Active** (toggle in top-right)
- Verify schedule trigger is enabled
- Check `stage` is exactly `pending` (lowercase, no spaces)
- Ensure no `locked` value in locked column
- Check n8n execution logs for errors

#### 2. Google Sheets Connection Error

**Problem**: "Cannot read from sheet" or "Authentication failed"

**Solutions:**
- Re-authenticate Google Sheets credential in n8n
- Verify sheet name is exactly `linkedin_posts`
- Check column headers match exactly (case-sensitive)
- Ensure Google Sheets API is enabled in Google Cloud Console
- Verify OAuth2 credentials have correct redirect URI

#### 3. Google Drive Permission Denied

**Problem**: "Cannot create folder" or "Permission denied"

**Solutions:**
- Re-authenticate Google Drive credential
- Verify `linkedin_archive` folder exists
- Check folder ID is correct (from Drive URL)
- Ensure Google Drive API is enabled
- Grant drive.file scope to OAuth2 credentials

#### 4. Firecrawl API Errors

**Problem**: "Research failed" or "API key invalid"

**Solutions:**
- Verify Firecrawl API key is correct
- Check API quota/usage limits
- Test Firecrawl API directly outside n8n
- Review error in `error_log` column

#### 5. LLM API Errors

**Problem**: Content generation fails

**Solutions:**
- Check LLM API key/credentials
- Verify endpoint URL (Azure OpenAI: ensure correct resource name)
- Check API version (Azure: try `2024-02-15-preview`)
- Verify deployment name (Azure: must match actual deployment)
- Check rate limits and quotas
- Review prompt length (token limits)

#### 6. Image Generation Fails

**Problem**: Images not created

**Solutions:**
- Check image API credentials
- Verify API supports requested size/format
- Check API rate limits
- Review base64 decode errors in logs
- Ensure image node configuration matches API docs

#### 7. Email Not Sent

**Problem**: Review email not received

**Solutions:**
- Check Gmail API credential
- Verify email in `user_emails` column is correct
- Check spam/junk folder
- Review n8n execution logs for email send errors
- Ensure Gmail API is enabled
- Grant gmail.send scope to OAuth2 credentials

#### 8. Webhook Not Working

**Problem**: Button clicks in email don't update sheet

**Solutions:**
- Check webhook URL is accessible (not blocked by firewall)
- Verify webhook node is active
- Test webhook URL directly with a POST request
- Check execution logs for webhook errors
- Ensure n8n is accessible from internet (if email client on different network)

#### 9. Stage Stuck

**Problem**: Row stuck in `researching`, `content_generating`, etc.

**Solutions:**
- Check `error_log` column for error details
- Manually reset `stage` to `pending` to retry
- Clear `locked` column value
- Check corresponding node logs in n8n
- Verify all API credentials are valid

#### 10. Multiple Emails Received

**Problem**: Getting duplicate review emails

**Solutions:**
- Check workflow isn't running multiple times
- Verify only ONE active execution per schedule
- Review `deprioritize_review_pending` node logic
- Check `stage` isn't being manually changed during execution

### Debug Mode

To enable detailed logging:

1. Open any Code node
2. Add: `console.log('Debug: ', JSON.stringify($input.all(), null, 2));`
3. Run workflow
4. Check n8n execution logs for output

### Manual Recovery

If a post is stuck:

1. Check `error_log` column for error message
2. Identify failed stage
3. Fix underlying issue (API key, permissions, etc.)
4. Reset `stage` to appropriate value:
   - `pending` - Start from beginning
   - `researched` - Skip research, generate content
   - `drafted` - Skip content, generate images
   - `imaged` - Skip images, send review
5. Clear `locked` column
6. Wait for next schedule trigger

---

## 📚 Additional Resources

### Related Documentation
- [n8n Documentation](https://docs.n8n.io/)
- [Firecrawl API Docs](https://docs.firecrawl.dev/)
- [Google Sheets API](https://developers.google.com/sheets/api)
- [Google Drive API](https://developers.google.com/drive/api)
- [Azure OpenAI Service](https://learn.microsoft.com/en-us/azure/cognitive-services/openai/)

### Workflow Customization

**Common Customizations:**
- Adjust schedule frequency (trigger node)
- Change content length limits (Code nodes with character checks)
- Modify prompt templates (LLM nodes)
- Add additional content variations (duplicate agent nodes)
- Change image sizes/styles (image generation nodes)
- Customize email template (build_review_email node)
- Add Slack/Discord notifications (add messaging nodes)

---

## � Future Updates

### Planned: LinkedIn Direct Posting

In a future update, this workflow will include **LinkedIn API nodes** to automatically post approved content directly to LinkedIn.

**Current Limitation**: Personal LinkedIn profiles don't have API access for automated posting - only LinkedIn Company Pages support the LinkedIn node in n8n.

**Workaround for Now**: This workflow generates and saves the content and images to Google Drive, making it easy for you to manually copy-paste to LinkedIn.

**When LinkedIn Posting is Added**:
- Approved posts will automatically publish to your LinkedIn Company Page
- Personal profile users will still need to post manually
- The workflow will mark posts as "published" in the Google Sheet

Stay tuned for this enhancement! ⭐

---

## �📄 License

This workflow template is provided as-is for community use. Modify as needed for your use case.

---

## 🤝 Contributing

Issues and suggestions welcome! If you improve this workflow, consider sharing your enhancements.

---

## ⚠️ Important Notes

1. **API Costs**: This workflow makes multiple API calls. Monitor your usage and costs.
2. **Execution Time**: Full pipeline can take 5-15 minutes per post.
3. **Rate Limits**: Respect API rate limits. Adjust schedule if needed.
4. **Error Handling**: The workflow includes comprehensive error handling, but monitor `error_log` column.
5. **Privacy**: All content stored in your Google Drive. Ensure proper access controls.
6. **Webhook Security**: Consider adding authentication to webhook endpoint for production use.

---

<div align="center">

**Made for the n8n community** 🚀

If this workflow helps you, consider ⭐ starring the repository!

</div>

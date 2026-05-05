# n8n Self-Hosting & Workflows - AI Assistant Instructions

> **Repository Context**: This is a community resource for n8n self-hosting and workflow automation. All code and documentation must be production-ready, security-audited, and beginner-friendly.

## Repository Overview

**Purpose**: Provide production-ready Docker setup and reusable n8n workflows  
**Audience**: DevOps engineers, content creators, n8n community members  
**Status**: v1.0.0 released, actively maintained  
**Security**: Zero personal information policy (security-first)

## Core Principles

### 1. Security First
- **NEVER** commit personal information (credentials, API keys, domains, IDs)
- Always use placeholders: `YOUR_X_HERE`, `CHANGE_ME`, `your-domain.com`
- Verify all JSON files have generic credential IDs before committing
- Check documentation for leaked personal references
- Review .gitignore coverage

### 2. Documentation Excellence
- Every workflow needs comprehensive README (400+ lines minimum)
- Include: Overview, Prerequisites, Setup, Configuration, Troubleshooting, Examples
- Add troubleshooting sections with common errors
- Provide learning context (why/when to use)
- Use comparison tables when multiple options exist

### 3. Production Quality
- Test all workflows before documenting
- Include error handling in workflows
- Document performance characteristics (execution time, costs)
- Provide real-world use cases
- Add retry logic and failure handling

### 4. Community Friendly
- Write for beginners, not experts
- Explain technical concepts clearly
- Provide step-by-step instructions
- Include visual aids (diagrams, code blocks)
- Add decision-making guides

## Project Structure

```
n8n-selfhosting-workflows/
├── n8n-selfhosting/docker/     # Docker infrastructure
│   ├── docker-compose.yml
│   ├── .env-example
│   └── README.md               # 794 lines
├── n8n-workflows/              # Workflow library
│   ├── image-generation/
│   ├── linkedin-post-content-generator/
│   └── README.md
├── info/                       # Private docs (gitignored)
├── .github/                    # CI/CD & agent configs
└── README.md                   # Main landing page
```

## File Management Rules

### Files to ALWAYS Exclude from Git
- `.env` files (any environment files with actual values)
- `info/` folder (internal planning docs)
- `.vscode/` (editor-specific settings)
- `.idea/` (IDE configs)
- Any files with personal information

### Files That MUST Be Public
- `docker-compose.yml` (with env var references only)
- `.env-example` (with placeholder values)
- All `README.md` files
- Workflow JSON files (with credential IDs replaced)
- `LICENSE`

### Before Every Commit - Security Checklist
1. [ ] Search for personal Google Sheet IDs
2. [ ] Search for personal Google Drive folder IDs
3. [ ] Search for personal domains (e.g., `flow.valopt.info`)
4. [ ] Search for specific credential IDs (replace with `YOUR_X_CREDENTIAL_ID`)
5. [ ] Check for hardcoded API keys
6. [ ] Verify .env files are gitignored
7. [ ] Review email addresses
8. [ ] Check for personal file paths

## Workflow Development Guidelines

### Creating New Workflows

1. **Planning Phase**
   - Define clear use case and target audience
   - Identify required APIs and credentials
   - Sketch workflow architecture (nodes + connections)
   - Estimate execution time and costs

2. **Development Phase**
   - Build workflow in n8n
   - Test with real data
   - Add error handling nodes
   - Implement retry logic
   - Verify all credentials are parameterized

3. **Documentation Phase**
   - Create comprehensive README (use existing as template)
   - Document every node's purpose
   - Add setup instructions for each API
   - Create troubleshooting section
   - Include example inputs/outputs
   - Add performance metrics

4. **Security Phase**
   - Export JSON and remove personal credential IDs
   - Replace specific values with placeholders
   - Test import with generic credentials
   - Verify no personal information leaked

5. **Organization Phase**
   - Place in appropriate folder (or create new category)
   - Update parent README with workflow comparison
   - Add to main README
   - Create/update decision guides

### Workflow README Template Structure

```markdown
# [Workflow Name]

[2-3 sentence overview]

## Use Cases
- [Specific scenario 1]
- [Specific scenario 2]

## What This Workflow Does
[Detailed explanation]

## Prerequisites
[APIs, credentials, services needed]

## Architecture
[Node-by-node breakdown]

## Setup Instructions
### Step 1: [API Setup]
### Step 2: [Credential Configuration]
### Step 3: [Import Workflow]
### Step 4: [Configure Nodes]

## Configuration Reference
[All configurable parameters]

## Usage
[How to run and what to expect]

## Troubleshooting
### Issue 1: [Common error]
**Cause**: ...
**Solution**: ...

## Performance & Costs
[Execution time, API costs, rate limits]

## Examples
[Real-world usage examples]

## Advanced Customization
[Optional enhancements]
```

## Documentation Standards

### Markdown Formatting
- Use `#` for main title (one per file)
- Use `##` for major sections
- Use `###` for subsections
- Use code blocks with language tags (```json, ```bash, etc.)
- Use tables for comparisons
- Use bullet points for lists
- Use numbered lists for sequential steps

### Code Examples
- Always specify language in code blocks
- Include comments explaining complex logic
- Show complete examples (not snippets)
- Provide both input and expected output

### Links
- Use relative paths for internal links: `[Docker Setup](n8n-selfhosting/docker/README.md)`
- Use absolute URLs for external links
- Always verify links work before committing
- Update links when moving files

## Git Workflow

### Commit Message Convention
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types**:
- `feat`: New feature (workflow, major documentation)
- `docs`: Documentation updates
- `refactor`: Code/structure reorganization
- `chore`: Maintenance, cleanup
- `fix`: Bug fixes
- `security`: Security improvements

**Examples**:
```
feat(workflows): Add email automation workflow

Add new workflow for automated email processing with Gmail API.
Includes 500-line documentation and troubleshooting guide.

Closes #12

---

docs: Update main README with workflow comparison table

Simplify repository structure section for better scalability.

---

security: Remove personal Google Sheet IDs from LinkedIn workflow

Replaced 18 occurrences of personal IDs with generic placeholders.
```

### Branch Strategy
- `main`: Production-ready code only
- Feature branches: `feat/workflow-name`
- Documentation: `docs/update-readme`
- Security fixes: `security/remove-personal-data`

### Release Strategy
- Use semantic versioning: `v1.0.0`, `v1.1.0`, `v2.0.0`
- Create annotated Git tags: `git tag -a v1.1.0 -m "Release notes"`
- Publish GitHub releases with detailed notes
- Include changelog in release body

## AI Agent Capabilities

### Available Tools
When assisting with this repository, you have access to:

**File Operations**:
- Read, write, edit files
- Search codebase (semantic, grep, file search)
- List directories

**Git Operations**:
- View status, diffs, logs
- Create commits (but always confirm with user first)
- View branches and tags

**Web Tools** (via MCP):
- Firecrawl: Web scraping and research
- Tavily: Web search
- GitHub: Issues, PRs, releases
- Playwright: Browser automation for testing

**Analysis**:
- Code review
- Security scanning
- Link validation
- Pattern matching

### Best Practices for AI Assistance

1. **Always Read Context First**
   - Read `info/ThisProject-Info.md` for project context
   - Check `info/TODO.md` for current tasks
   - Review existing similar files for patterns

2. **Security Scanning Process**
   ```bash
   # Search for potential leaks
   grep -r "gmail.com" n8n-workflows/
   grep -r "drive.google.com" n8n-workflows/
   grep -r "@" n8n-workflows/*.json
   grep -r "http://" n8n-workflows/*.json
   ```

3. **Documentation Review Checklist**
   - [ ] Title and overview present
   - [ ] Prerequisites documented
   - [ ] Setup instructions clear and complete
   - [ ] All APIs/credentials explained
   - [ ] Troubleshooting section exists
   - [ ] Examples provided
   - [ ] Links verified
   - [ ] No personal information

4. **File Organization**
   - Group related workflows in folders
   - Create folder README for navigation
   - Update parent README when adding workflows
   - Keep main README high-level

## Common Tasks

### Adding a New Workflow

```bash
# 1. Create folder structure
mkdir -p n8n-workflows/new-workflow-name

# 2. Add workflow JSON (with personal info removed)
# Place: n8n-workflows/new-workflow-name/workflow.json

# 3. Create comprehensive README
# Place: n8n-workflows/new-workflow-name/README.md

# 4. Update workflows overview
# Edit: n8n-workflows/README.md

# 5. Update main README
# Edit: README.md (add to workflow list)

# 6. Security check
grep -r "YOUR_PERSONAL_DOMAIN" .
grep -r "your@email.com" .

# 7. Commit with proper message
git add n8n-workflows/new-workflow-name/
git commit -m "feat(workflows): Add [workflow name]

[Description of what it does]
[Key features]
[Documentation stats]"
```

### Updating Documentation After Structure Change

```bash
# 1. Find all links to the changed path
grep -r "old-path" .

# 2. Update all references
# Use multi-file replace or sed

# 3. Verify no broken links
# Test all relative path links

# 4. Commit
git commit -m "docs: Update paths after restructure"
```

### Security Audit Workflow

```bash
# 1. Search for common personal info patterns
grep -ri "\.apps\.googleusercontent\.com" n8n-workflows/
grep -ri "spreadsheets/d/" n8n-workflows/
grep -ri "drive\.google\.com/drive/folders/" n8n-workflows/
grep -ri "https://flow\." n8n-workflows/
grep -ri "your-actual-domain" n8n-workflows/

# 2. Check credential IDs in JSON
grep -i '"id":.*"name":.*"api"' n8n-workflows/**/*.json

# 3. Verify .gitignore coverage
git status --ignored

# 4. Document findings and fix
# Create security commit if needed
```

## Questions to Ask Before Proceeding

When working on this repository, ask:

1. **For new workflows**:
   - What is the target audience (beginner/intermediate/advanced)?
   - What APIs/services are required?
   - Have all credentials been replaced with placeholders?
   - Is documentation comprehensive (400+ lines)?
   - Does it fit existing folder structure or need new category?

2. **For documentation updates**:
   - Are all links still valid after changes?
   - Is the change reflected in all necessary places (main README, folder README)?
   - Does it maintain consistency with existing docs?

3. **For security changes**:
   - Have all occurrences been found (not just some)?
   - Are placeholders descriptive enough?
   - Is .gitignore properly configured?

4. **For structure changes**:
   - Will this break existing links?
   - Does it improve or complicate navigation?
   - Is it future-proof (scalable)?

## Emergency Procedures

### If Personal Information Was Committed

1. **DO NOT** just delete the file - it stays in Git history
2. Remove from all commits using `git filter-branch` or BFG Repo-Cleaner
3. Force push to remote (if already pushed)
4. Rotate any exposed credentials immediately
5. Document the incident in `info/` folder

### If Documentation Becomes Inconsistent

1. Create checklist of all files that need updating
2. Update systematically (don't skip any)
3. Use search to verify all references updated
4. Test all links
5. Commit with clear message explaining scope

## Support Resources

- **n8n Docs**: https://docs.n8n.io/
- **Docker Docs**: https://docs.docker.com/
- **This Project Context**: `info/ThisProject-Info.md`
- **Current Tasks**: `info/TODO.md`
- **Implementation Plan**: `info/IMPLEMENTATION_PLAN.md`

---

**Last Updated**: May 5, 2026  
**Version**: 1.0.0  
**For**: AI assistants working with this repository

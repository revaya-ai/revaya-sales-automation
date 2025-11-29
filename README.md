# Revaya Sales Automation Workflow

Automated AI-powered lead research and proposal generation system for Revaya. This n8n workflow handles everything from initial contact form submission to final proposal delivery.

## Overview

This workflow automates your entire sales process in two stages:

### Stage 1: Pre-Call Research (Lead Intake)
When someone fills out your contact form:
- 6 AI agents research the lead (company, contact, website, competition)
- Creates branded Google Doc with research summary
- Generates discovery questions and objection handling strategies
- Emails you complete research package
- Saves everything to Google Sheets

### Stage 2: Post-Call Proposal (After Discovery)
After your discovery call, you input call notes and the workflow:
- Designs custom solution based on their needs
- Calculates pricing with value justification
- Builds project timeline with milestones
- Writes personalized proposal in your voice
- Saves proposal to Google Drive
- Emails you with proposal link

## Quick Start

### 1. Prerequisites
- n8n instance (self-hosted or cloud)
- Google Workspace account
- Google Gemini API key

### 2. Import Workflow
1. Download `Revaya_Proposal_Generation_Workflow.json`
2. In n8n: Workflows → Import from File
3. Select the downloaded JSON file

### 3. Configure Credentials
Connect these services in n8n:
- Google Drive OAuth2
- Google Sheets OAuth2
- Gmail OAuth2
- Google Gemini API (key already in workflow)

### 4. Update Settings
- **Google Drive Folder ID**: Update to your research folder
- **Google Sheets Document ID**: Update to your leads spreadsheet
- **Email Address**: Verify it's set to `hello@company.com`

### 5. Set Up Your Form
Configure your contact form to POST to:
```
https://[your-n8n-domain]/webhook/company-lead-intake
```

With these fields:
```json
{
  "first_name": "Sarah",
  "last_name": "Johnson",
  "email": "sarah@company.com",
  "phone": "555-123-4567",
  "company_name": "Acme Corp",
  "interested_in": "Website Design",
  "website": "https://acmecorp.com",
  "pain_points": "Our website is slow and outdated",
  "referred_by": "John Smith"
}
```

## Documentation

📁 **[Full Setup Guide](docs/Setup_Guide.md)** - Complete step-by-step setup instructions

📋 **[Form Field Mapping](docs/Your_Form_Field_Mapping.md)** - Exact field requirements for your forms

🔧 **[Webhook Guide](docs/Webhook_Form_Fields_Guide.md)** - Technical webhook documentation

📊 **[Update Summary](docs/Workflow_Update_Summary.md)** - Complete changelog from Winnicki Digital to Revaya

🎨 **[Research Doc Preview](docs/Research_Doc_Format_Preview.md)** - Visual guide to branded research docs

## Features

### AI-Powered Research
- **Company Intelligence**: Business model, market position, tech stack analysis
- **Contact Research**: Role inference, decision authority, communication style
- **Website Analysis**: UX audit, technical assessment, SEO evaluation
- **Competitive Context**: Market positioning, competitor analysis, opportunities
- **Discovery Questions**: Tailored questions based on research
- **Objection Handling**: Anticipated objections with strategic responses

### Branded Output
- Research documents use Revaya brand colors (#3d3d99, #61bdb4)
- Montserrat font, professional formatting
- Partnership-focused messaging throughout
- Founder-led voice ("I" not "we")

### Partnership Positioning
Every agent prompt includes Revaya's core differentiator:
> "We stay. We grow with you."

Emphasizes:
- Long-term partnership vs one-off projects
- Direct access to expert (no agency layers)
- Continuous optimization and evolution
- One relationship that grows with the client

## Workflow Structure

```
Lead Intake Form
    ↓
[6 AI Research Agents Run in Parallel]
    ↓
Format Research as Branded HTML
    ↓
Convert to Google Doc
    ↓
Save to Google Drive + Sheets
    ↓
Email Research Summary
    ↓
[Discovery Call Happens]
    ↓
Post-Call Form Submitted
    ↓
[4 AI Agents Generate Proposal]
    ↓
Save Proposal to Google Drive
    ↓
Email Proposal Link
```

## Required Google Sheets Columns

Your leads spreadsheet needs these columns:
- `first_name`, `last_name`, `email`, `phone`
- `company_name`, `website`, `interested_in`
- `pain_points`, `referred_by`
- `lead_id`, `research_date`
- `company_intelligence`, `contact_research`
- `website_analysis`, `competitive_context`
- `discovery_questions`, `objections`
- `research_doc_url`, `proposal_url`

## Testing

### Test Lead Intake
```bash
curl -X POST https://YOUR-N8N-URL/webhook/revaya-lead-intake \
  -H "Content-Type: application/json" \
  -d '{
    "first_name": "Test",
    "last_name": "User",
    "email": "test@test.com",
    "phone": "555-0000",
    "company_name": "Test Company",
    "website": "https://test.com",
    "interested_in": "Website Design",
    "pain_points": "Testing the workflow",
    "referred_by": "GitHub"
  }'
```

### Test Post-Call Proposal
```bash
curl -X POST https://YOUR-N8N-URL/webhook/revaya-post-call \
  -H "Content-Type: application/json" \
  -d '{
    "services_needed": "Website redesign",
    "pain_points": "Slow website",
    "business_goals": "Increase conversions",
    "budget_discussed": "Yes",
    "budget_amount": "$5,000",
    "timeline_preference": "2 months",
    "priority_level": "High",
    "call_notes": "Good fit for partnership"
  }'
```

## Brand Guidelines

The workflow follows Revaya's brand standards:

**Colors:**
- Primary: `#3d3d99` (Revaya Purple)
- Secondary: `#61bdb4` (Revaya Teal)
- Supporting: `#9a9ef5`, `#ff6b7a`, `#d4ff00`

**Typography:**
- Font: Montserrat
- Size: 12pt body, 18pt H2, 24pt H1

**Tone:**
- Warm but direct
- Confident but humble
- Clear and actionable
- Authentic and real

## Services Covered

The workflow is configured for Revaya's three service pillars:

1. **AI Solutions** - Automation, voice agents, AI integrations
2. **AI Consulting** - Discovery, strategy, roadmapping
3. **Website Design and Development** - Design, updates, optimization

**Platforms:** Wix, Shopify, Webflow, Squarespace, WordPress

## Support

**Issues?** Open an issue in this repository

**Questions?** Check the [docs](docs/) folder for detailed guides

**Updates?** Watch this repo for improvements and new features

## License

Proprietary - Revaya Internal Use Only

## Version

**Current Version:** 2.0 (Revaya Rebrand)
- Updated from Winnicki Digital to Revaya
- Added research document generation
- Enhanced partnership positioning
- Updated service offerings

**Last Updated:** November 29, 2025

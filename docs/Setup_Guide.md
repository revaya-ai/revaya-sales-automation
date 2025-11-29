# Revaya Workflow Setup Guide

## Quick Start Checklist

Before importing the workflow, ensure you have:

- [ ] n8n instance running
- [ ] Google Drive connected to n8n
- [ ] Google Docs connected to n8n  
- [ ] Google Sheets connected to n8n
- [ ] Gmail connected to n8n
- [ ] Google Gemini API key (already in workflow)

---

## Step 1: Import Workflow

1. Open n8n
2. Click "Add workflow" > "Import from File"
3. Upload `Revaya_Proposal_Generation_Workflow.json`
4. Click "Import"

---

## Step 2: Configure Credentials

The workflow uses these credentials that you'll need to connect:

### Google Drive OAuth2
**Used by nodes:**
- Save Research Doc to Drive
- Save Proposal to Google Drive

**Setup:**
1. Click on "Save Research Doc to Drive" node
2. Click the credential dropdown
3. Select your Google Drive OAuth2 credential (or create new)
4. Test the connection

### Google Sheets OAuth2
**Used by nodes:**
- Save to Google Sheets
- Update Sheets with Call Data
- Update Sheets with Proposal URL
- Lookup Lead in Sheets

**Setup:**
1. Click on any Google Sheets node
2. Select your Google Sheets OAuth2 credential
3. Ensure it has access to your leads spreadsheet

### Gmail OAuth2
**Used by nodes:**
- Send Email to Shannon
- Email Shannon

**Setup:**
1. Click on "Send Email to Shannon" node
2. Select your Gmail OAuth2 credential
3. Verify the email address matches: `hello@revaya.com`

---

## Step 3: Update Google Sheets

Your leads spreadsheet needs these columns:

### Required Columns:
- `first_name`
- `last_name`
- `email` (used as unique identifier)
- `phone`
- `company` (or `company_name`)
- `website`
- `interested_in`
- `additional_info`
- `lead_id`
- `research_date`
- `company_intelligence`
- `contact_research`
- `website_analysis`
- `competitive_context`
- `discovery_questions`
- `objections`
- **`research_doc_url`** ← NEW COLUMN
- `proposal_url`

**Sheet ID in workflow:** `1tq4zDWDhcQy_rQyv9mnxKv5K0Ugy9uwcEVTElNkjoQA`

If your Sheet ID is different:
1. Open each Google Sheets node
2. Click "Document ID"
3. Select your spreadsheet
4. Save

---

## Step 4: Verify Folder IDs

### Research Documents Folder
**Current Folder ID:** `1iyhBxETUbze9hoj6YUFdrf8JuXtq2Spf`
**URL:** https://drive.google.com/drive/folders/1iyhBxETUbze9hoj6YUFdrf8JuXtq2Spf

**To verify/change:**
1. Open "Save Research Doc to Drive" node
2. Check "Folder ID" field
3. If different, update to your folder ID

### Proposals Folder
**To set a specific folder for proposals:**
1. Open "Save Proposal to Google Drive" node
2. Update "Folder ID" from `root` to your proposals folder ID
3. Save

---

## Step 5: Test the Workflow

### Test Pre-Call Research Path

**Sample webhook payload:**
```json
{
  "first_name": "John",
  "last_name": "Smith",
  "email": "john@testcompany.com",
  "phone": "555-123-4567",
  "company_name": "Test Company Inc",
  "website": "https://testcompany.com",
  "interested_in": "Website redesign and AI automation",
  "additional_info": "Current website is outdated, needs modern design and automation for customer inquiries"
}
```

**Webhook URL:**
`https://[your-n8n-domain]/webhook/revaya-lead-intake`

**Expected Results:**
1. ✓ 6 AI agents run (Company Intel, Contact, Website, Competitive, Questions, Objections)
2. ✓ Research summary HTML is formatted
3. ✓ HTML converted to binary
4. ✓ **Google Doc created** in folder: 1iyhBxETUbze9hoj6YUFdrf8JuXtq2Spf
5. ✓ Google Sheets updated with all research + doc URL
6. ✓ Email sent to hello@revaya.com with research summary + link to doc
7. ✓ Webhook responds with success

**Check:**
- [ ] Google Doc was created with proper formatting
- [ ] Document uses Revaya brand colors (#3d3d99, #61bdb4)
- [ ] Document has Montserrat font
- [ ] Email includes button to view research doc
- [ ] Google Sheets has `research_doc_url` populated

### Test Post-Call Proposal Path

**Sample webhook payload:**
```json
{
  "lead_id": "LEAD-20241129123456-ABC123",
  "services_needed": "Website redesign, AI chatbot",
  "pain_points": "Website is slow, no automation for customer inquiries",
  "business_goals": "Increase online conversions by 30%, reduce customer service load",
  "budget_discussed": "Yes",
  "budget_amount": "$5,000-$8,000",
  "timeline_preference": "2-3 months",
  "priority_level": "High",
  "call_notes": "Very motivated to start soon. Decision maker on the call."
}
```

**Webhook URL:**
`https://[your-n8n-domain]/webhook/revaya-post-call`

**Expected Results:**
1. ✓ Lead looked up in Google Sheets
2. ✓ Solution designed
3. ✓ Pricing calculated
4. ✓ Timeline built
5. ✓ Proposal written
6. ✓ Google Sheets updated with call data
7. ✓ Proposal saved to Google Drive
8. ✓ Sheets updated with proposal URL
9. ✓ Email sent with proposal link
10. ✓ Webhook responds with success

---

## Step 6: Update Webhook Integrations

If you have forms or systems that feed into the old webhooks, update them:

**Old webhooks:**
- `winnicki-lead-intake` → `revaya-lead-intake`
- `winnicki-post-call` → `revaya-post-call`

**Update in:**
- [ ] Website contact forms
- [ ] CRM integrations
- [ ] Zapier/Make.com automations
- [ ] API integrations
- [ ] Any other webhook consumers

---

## Step 7: Monitor First Few Runs

For the first 3-5 leads, manually verify:

1. **Research Doc Quality**
   - Formatting looks professional
   - Brand colors display correctly
   - All sections populated
   - No formatting errors

2. **Email Delivery**
   - Arrives at hello@revaya.com
   - Research doc link works
   - Proposal link works
   - No broken links

3. **Google Sheets**
   - All columns populated
   - URLs are clickable
   - Data is accurate

---

## Troubleshooting

### Research Doc Not Created

**Problem:** Node "Save Research Doc to Drive" fails

**Solutions:**
1. Check Google Drive credentials are connected
2. Verify folder ID: `1iyhBxETUbze9hoj6YUFdrf8JuXtq2Spf`
3. Ensure folder exists and credential has write access
4. Check for errors in "Convert HTML to Binary" node

### Research Doc Has No Formatting

**Problem:** Doc is plain text, no colors or formatting

**Solutions:**
1. Verify "Convert HTML to Binary" node has correct MIME type: `text/html`
2. Check "Save Research Doc to Drive" has Google File Conversion enabled
3. Ensure conversion MIME type is: `application/vnd.google-apps.document`

### Email Doesn't Include Research Doc Link

**Problem:** Email arrives but no button for research doc

**Solutions:**
1. Check "Format Email" node has the updated HTML with button
2. Verify button code includes: `$('Save Research Doc to Drive').item.json.webViewLink`
3. Ensure "Save Research Doc to Drive" node completed successfully

### Google Sheets Not Updating with Doc URL

**Problem:** `research_doc_url` column is empty

**Solutions:**
1. Add `research_doc_url` column to your spreadsheet
2. Check "Save to Google Sheets" node column mapping
3. Verify it references: `$('Save Research Doc to Drive').item.json.webViewLink`

---

## Optional Enhancements

### Set Specific Proposal Folder

Instead of saving to root, create a "Proposals" folder:

1. Create folder in Google Drive
2. Get folder ID from URL
3. Update "Save Proposal to Google Drive" node
4. Change folder ID from `root` to your folder ID

### Update Pricing in Agent 8

The pricing structure is still project-based. Update to reflect your retainer model:

1. Open "Agent 8: Pricing Calculator" node
2. Update the pricing structure in the prompt
3. Add retainer/partnership pricing tiers
4. Save and test

### Add Notification Slack Channel

Instead of just email, send to Slack:

1. Add Slack node after "Save Research Doc to Drive"
2. Configure to send to your team channel
3. Include research doc link in Slack message

---

## Support

If you encounter issues:

1. Check n8n execution logs for errors
2. Verify all credentials are properly connected
3. Test each node individually using "Test step"
4. Ensure all folder IDs and Sheet IDs are correct

---

## What's Different from Winnicki Digital Version

### Branding Updates
- All "Winnicki Digital" → "Revaya"
- Email: hello@revaya.com
- Website: revaya.com
- Webhook paths: revaya-*

### Service Updates
- Updated to 3 pillars: AI Solutions, AI Consulting, Website Design & Development
- Removed HighLevel
- Added Squarespace, WordPress

### New Features
- **Research Summary Google Doc** (3 new nodes)
- Branded document formatting
- Research doc link in email
- Research doc URL in Google Sheets

### Partnership Positioning
- All agents updated with partnership language
- "We stay. We grow with you." messaging
- Discovery-first approach
- Founder-led voice ("I" not "we")

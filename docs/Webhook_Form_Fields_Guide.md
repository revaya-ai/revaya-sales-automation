# Revaya Webhook Form Fields Guide

## Overview

The workflow has **2 webhooks** that expect different data:

1. **revaya-lead-intake** - Triggered when a new lead fills out your contact form
2. **revaya-post-call** - Triggered manually after you complete a discovery call

---

## Webhook 1: Lead Intake Form (Pre-Call Research)

**Webhook URL:** `https://[your-n8n-domain]/webhook/revaya-lead-intake`

**Trigger:** When someone fills out your website contact form or lead capture form

### Required Fields

| Field Name | Type | Description | Example |
|------------|------|-------------|---------|
| `first_name` | Text | Lead's first name | "Sarah" |
| `last_name` | Text | Lead's last name | "Johnson" |
| `email` | Email | Lead's email (used as unique ID) | "sarah@acmecorp.com" |
| `phone` | Text | Lead's phone number | "555-123-4567" |
| `company_name` | Text | Lead's company name | "Acme Corporation" |
| `website` | URL | Lead's company website | "https://acmecorp.com" |
| `interested_in` | Text | What they're interested in | "Website redesign and AI automation" |
| `additional_info` | Text (long) | Any additional context | "Our current website is 5 years old and we get a lot of customer service calls that could be automated" |

### Sample JSON Payload

```json
{
  "first_name": "Sarah",
  "last_name": "Johnson",
  "email": "sarah@acmecorp.com",
  "phone": "555-123-4567",
  "company_name": "Acme Corporation",
  "website": "https://acmecorp.com",
  "interested_in": "Website redesign and AI automation",
  "additional_info": "Our current website is 5 years old and we get a lot of customer service calls that could be automated. Looking to modernize and streamline operations."
}
```

### What Happens Next

1. ✓ 6 AI agents research the lead:
   - Company intelligence
   - Contact research
   - Website analysis
   - Competitive context
   - Discovery questions
   - Objection handling

2. ✓ **Branded Google Doc created** with research summary

3. ✓ All research saved to Google Sheets

4. ✓ Email sent to `hello@revaya.com` with:
   - Complete research summary
   - Link to Google Doc
   - Discovery questions for your call
   - Anticipated objections

---

## Webhook 2: Post-Call Proposal (After Discovery Call)

**Webhook URL:** `https://[your-n8n-domain]/webhook/revaya-post-call`

**Trigger:** Manually after you complete a discovery call with the lead

### Required Fields

| Field Name | Type | Description | Example |
|------------|------|-------------|---------|
| `lead_id` | Text | Lead ID from intake (optional but recommended) | "LEAD-20241129123456-ABC123" |
| `services_needed` | Text | Services discussed on call | "Website redesign, AI chatbot, automation" |
| `pain_points` | Text (long) | Pain points discussed | "Website is slow, no mobile optimization, spending 10 hours/week on manual customer inquiries" |
| `business_goals` | Text (long) | Their business goals | "Increase online conversions by 30%, reduce customer service load by 50%, modernize brand image" |
| `budget_discussed` | Text | Was budget discussed? | "Yes" or "No" |
| `budget_amount` | Text | Budget range if discussed | "$5,000 - $8,000" or "Not specified" |
| `timeline_preference` | Text | Their preferred timeline | "2-3 months" or "ASAP" |
| `priority_level` | Text | How urgent is this? | "High", "Medium", or "Low" |
| `call_notes` | Text (long) | Your notes from the call | "Very motivated to start soon. Sarah is the decision maker. Mentioned competitor is charging $15k. Wants to see proposal by Friday." |
| `call_date` | Date (optional) | When the call happened | "2024-11-29" |

### Sample JSON Payload

```json
{
  "lead_id": "LEAD-20241129123456-ABC123",
  "services_needed": "Website redesign, AI chatbot for customer inquiries, email automation",
  "pain_points": "Current website is slow (5+ sec load time), not mobile optimized, spending 10+ hours per week manually responding to the same customer questions, losing leads because of delayed responses",
  "business_goals": "Increase online conversions by 30%, reduce customer service workload by 50%, modernize brand to compete with larger competitors, improve customer response time to under 1 hour",
  "budget_discussed": "Yes",
  "budget_amount": "$5,000 - $8,000 for initial build, $500-1000/month for ongoing",
  "timeline_preference": "Would like to launch new site in 2-3 months, can start AI chatbot immediately",
  "priority_level": "High",
  "call_notes": "Sarah is the sole decision maker - no approval needed from others. Very motivated to start ASAP. Currently paying $200/month to competitor for basic hosting. Mentioned competitor quoted $15k for similar work. Wants to see proposal by Friday. Follow up scheduled for Monday. Strong fit for partnership model - she understands ongoing optimization value.",
  "call_date": "2024-11-29"
}
```

### What Happens Next

1. ✓ Looks up lead's pre-call research in Google Sheets

2. ✓ 4 AI agents create proposal:
   - Solution design
   - Pricing breakdown
   - Timeline/milestones
   - Complete proposal document

3. ✓ **Proposal saved to Google Drive**

4. ✓ Google Sheets updated with call data and proposal URL

5. ✓ Email sent to `hello@revaya.com` with:
   - Link to proposal in Google Drive
   - Call summary
   - Quick stats (budget, timeline, priority)

---

## How to Set Up Your Forms

### Option 1: Website Contact Form (for Lead Intake)

**Popular form builders that integrate with n8n:**

- **Typeform** - Use n8n Typeform trigger
- **Google Forms** - Use n8n Google Forms trigger  
- **Webflow Forms** - Use webhook submission
- **Wix Forms** - Configure webhook in form settings
- **Custom HTML Form** - POST directly to webhook URL

**Example Webflow/Wix/Custom Form Setup:**

```html
<form action="https://[your-n8n-domain]/webhook/revaya-lead-intake" method="POST">
  <input type="text" name="first_name" placeholder="First Name" required>
  <input type="text" name="last_name" placeholder="Last Name" required>
  <input type="email" name="email" placeholder="Email" required>
  <input type="tel" name="phone" placeholder="Phone" required>
  <input type="text" name="company_name" placeholder="Company Name" required>
  <input type="url" name="website" placeholder="Website URL" required>
  <input type="text" name="interested_in" placeholder="What are you interested in?" required>
  <textarea name="additional_info" placeholder="Tell us more about your needs"></textarea>
  <button type="submit">Submit</button>
</form>
```

---

### Option 2: Manual Post-Call Form (Internal Use)

**Recommendation:** Create a simple internal form (Google Form, Airtable, or custom) that YOU fill out after each discovery call.

**Google Form Setup:**

1. Create Google Form with the 9 fields listed above
2. In Google Forms settings, go to "Responses"
3. Click the three dots → "Select response destination"
4. Choose Google Sheets
5. In Google Sheets, use Apps Script or n8n Google Sheets trigger to watch for new rows
6. Send data to `revaya-post-call` webhook

**Or use Zapier/Make.com:**

1. Trigger: New Google Form Response
2. Action: POST to webhook `revaya-post-call` with form data

**Or use n8n Form directly:**

1. Create an n8n form node before the webhook
2. Configure fields
3. Form submits to the workflow directly

---

## Field Validation Tips

### Lead Intake Form

**Must validate:**
- Email format (user@domain.com)
- Website format (https://domain.com)
- Phone can be flexible format

**Recommended:**
- Mark all fields as required
- Set `interested_in` as dropdown or checkboxes:
  - Website Design
  - AI Automation
  - AI Consulting
  - Voice Agents
  - Other

### Post-Call Form

**Must validate:**
- Budget discussed: dropdown ("Yes", "No", "Partially")
- Priority level: dropdown ("High", "Medium", "Low")
- Timeline preference: text or dropdown

**Recommended:**
- Pre-fill `lead_id` from your Google Sheet if possible
- Use text areas for long fields: pain_points, business_goals, call_notes
- Make `budget_amount` optional (only required if budget_discussed = "Yes")

---

## Testing Your Forms

### Test Lead Intake:

**Use this curl command:**

```bash
curl -X POST https://[your-n8n-domain]/webhook/revaya-lead-intake \
  -H "Content-Type: application/json" \
  -d '{
    "first_name": "Test",
    "last_name": "Lead",
    "email": "test@example.com",
    "phone": "555-000-0000",
    "company_name": "Test Company",
    "website": "https://example.com",
    "interested_in": "Testing the workflow",
    "additional_info": "This is a test submission"
  }'
```

**Expected result:**
- Google Doc created in your research folder
- Email sent to hello@revaya.com
- Google Sheets updated
- Success response returned

---

### Test Post-Call:

```bash
curl -X POST https://[your-n8n-domain]/webhook/revaya-post-call \
  -H "Content-Type: application/json" \
  -d '{
    "lead_id": "LEAD-TEST-123",
    "services_needed": "Website and AI chatbot",
    "pain_points": "Slow website, manual customer service",
    "business_goals": "Increase conversions, reduce workload",
    "budget_discussed": "Yes",
    "budget_amount": "$5,000",
    "timeline_preference": "2 months",
    "priority_level": "High",
    "call_notes": "Good fit for partnership model",
    "call_date": "2024-11-29"
  }'
```

**Expected result:**
- Proposal generated
- Proposal saved to Google Drive
- Email sent with proposal link
- Success response returned

---

## Common Issues

### "Field not found" error

**Problem:** Form field names don't match webhook expectations

**Solution:** Field names must be EXACT:
- ✓ `first_name` 
- ✗ `firstName` or `First_Name`

### Empty values in Google Doc

**Problem:** Some fields showing as blank

**Solution:** 
- Check that form is sending data in JSON format
- Verify field names match exactly
- Test with curl command first

### Email not arriving

**Problem:** No email notification

**Solution:**
- Check Gmail credentials in n8n
- Verify email address is `hello@revaya.com`
- Check spam folder
- Look at n8n execution logs

---

## Quick Reference

### Lead Intake (8 fields)
```
first_name, last_name, email, phone, company_name, website, interested_in, additional_info
```

### Post-Call (10 fields)
```
lead_id, services_needed, pain_points, business_goals, budget_discussed, budget_amount, timeline_preference, priority_level, call_notes, call_date
```

---

## Next Steps

1. Set up your website contact form with the 8 lead intake fields
2. Configure form to POST to: `https://[your-n8n]/webhook/revaya-lead-intake`
3. Test with sample data
4. Create internal post-call form with the 10 proposal fields
5. Configure to POST to: `https://[your-n8n]/webhook/revaya-post-call`
6. Test the full workflow end-to-end

Need help setting up a specific form platform? Let me know which one you're using.

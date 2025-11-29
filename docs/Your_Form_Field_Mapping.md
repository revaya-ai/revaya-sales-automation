# Your Form → Webhook Field Mapping

## ✅ Perfect! Your form matches the workflow

Here's the exact mapping from your form to the webhook field names:

| Your Form Field | Webhook Field Name | Status |
|-----------------|-------------------|--------|
| First name * | `first_name` | ✓ Ready |
| Last name * | `last_name` | ✓ Ready |
| Email * | `email` | ✓ Ready |
| Phone * | `phone` | ✓ Ready |
| Company name * | `company_name` | ✓ Ready |
| I'm interested in * | `interested_in` | ✓ Ready |
| Website (if applicable) | `website` | ✓ Ready |
| What's Your Top Pain Point(s) * | `pain_points` | ✓ Ready |
| Referred by | `referred_by` | ✓ Ready |

---

## What I Updated in the Workflow

### 1. Added `pain_points` Field Support
- Your form uses "What's Your Top Pain Point(s)" 
- Workflow now accepts this as `pain_points`
- Also works with `additional_info` as fallback
- Shows in research doc and email

### 2. Added `referred_by` Field Support
- New field captured from your form
- Stored in Google Sheets
- Displayed in research doc and email
- Optional (won't break if empty)

### 3. Made `website` Truly Optional
- Your form says "(if applicable)"
- Workflow handles missing website gracefully
- Won't crash if someone doesn't have a website

---

## Updated Google Sheets Columns

Add these two new columns to your spreadsheet:

- `pain_points` - Top pain points from the lead
- `referred_by` - Who referred this lead

---

## Form Configuration

### Field Names Must Be Exactly:

```javascript
{
  "first_name": "Sarah",
  "last_name": "Johnson", 
  "email": "sarah@company.com",
  "phone": "555-123-4567",
  "company_name": "Acme Corp",
  "interested_in": "Website Design",  // From dropdown
  "website": "https://acmecorp.com",  // Can be empty
  "pain_points": "Our website is slow and outdated. We're losing customers.",
  "referred_by": "John Smith"  // Can be empty
}
```

### In Your Form Builder

Make sure your form submits these exact field names with underscores (snake_case).

**Example for common platforms:**

**Webflow:**
1. Click on each form field
2. Set "Name" attribute to match exactly:
   - First name field → Name: `first_name`
   - Last name field → Name: `last_name`
   - Email field → Name: `email`
   - Phone field → Name: `phone`
   - Company name field → Name: `company_name`
   - Interested in dropdown → Name: `interested_in`
   - Website field → Name: `website`
   - Pain points textarea → Name: `pain_points`
   - Referred by field → Name: `referred_by`

**Wix:**
1. Select form
2. Click "Settings" → "Form Fields"
3. For each field, set "Field Key":
   - First Name → Field Key: `first_name`
   - Last Name → Field Key: `last_name`
   - Email → Field Key: `email`
   - (etc.)

---

## Webhook Setup

**Your form should POST to:**
```
https://[your-n8n-domain]/webhook/revaya-lead-intake
```

**Method:** POST
**Content-Type:** application/json

---

## Test Your Form

### Sample Test Payload

```bash
curl -X POST https://YOUR-N8N-URL/webhook/revaya-lead-intake \
  -H "Content-Type: application/json" \
  -d '{
    "first_name": "Sarah",
    "last_name": "Johnson",
    "email": "sarah@testcompany.com",
    "phone": "555-123-4567",
    "company_name": "Test Company Inc",
    "interested_in": "Website Design",
    "website": "https://testcompany.com",
    "pain_points": "Our website is slow and outdated. We are losing potential customers because of poor mobile experience.",
    "referred_by": "John Smith"
  }'
```

### Expected Results

1. ✓ Research summary Google Doc created
2. ✓ Doc includes "Top Pain Points" section
3. ✓ Doc includes "Referred By" field
4. ✓ Google Sheets updated with pain_points and referred_by columns
5. ✓ Email sent to hello@revaya.com with complete research
6. ✓ All 6 AI agents run successfully

---

## What Shows in the Research Doc

```
Research Summary
────────────────────────────

Lead Information
───────────────

Name: Sarah Johnson
Company: Test Company Inc
Email: sarah@testcompany.com
Phone: 555-123-4567
Website: https://testcompany.com
Interested In: Website Design

Top Pain Points: 
Our website is slow and outdated. We are losing 
potential customers because of poor mobile experience.

Referred By: John Smith

────────────────────────────
[Rest of research sections...]
```

---

## Quick Pre-Flight Checklist

Before going live:

- [ ] Update Google Sheets to include `pain_points` column
- [ ] Update Google Sheets to include `referred_by` column
- [ ] Configure form field names to match exactly (snake_case with underscores)
- [ ] Set form to POST to `/webhook/revaya-lead-intake`
- [ ] Test with sample data using curl command above
- [ ] Verify Google Doc is created in correct folder
- [ ] Verify email arrives at hello@revaya.com
- [ ] Check that pain_points and referred_by appear in doc and email
- [ ] Test with empty website field to ensure it doesn't break
- [ ] Test with empty referred_by field to ensure it doesn't break

---

## Next Steps

1. Add the two new columns to your Google Sheet:
   - `pain_points`
   - `referred_by`

2. Configure your form field names to match the mapping table above

3. Test with a sample submission

4. Go live!

Need help configuring a specific form platform? Let me know which one you're using.

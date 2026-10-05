# Social Links Click Tracker - n8n Setup Guide

## Webhook URL
```
https://minjarul.dpdns.org/webhook/social-clicks
```

---

## Step 1: Create Data Table

In n8n, create a data table named `social_clicks` with these columns:

| Column Name | Type | Description |
|-------------|------|-------------|
| visitor_id | text | Unique visitor ID |
| event | text | click or page_view |
| platform | text | facebook or whatsapp |
| timestamp | text | ISO 8601 datetime |
| url | text | Page URL clicked from |
| referrer | text | Source page |
| ip_address | text | User IP address |
| country | text | Country name |
| city | text | City name |
| device | text | Mobile or Desktop |
| browser | text | Chrome, Firefox, etc. |
| os | text | Android, iOS, Windows |
| screen_resolution | text | e.g., 390x844 |

---

## Step 2: Import Workflow

1. Download `n8n-workflow.json` from GitHub repo
2. In n8n: **Workflows** → **Import from File**
3. Select the JSON file
4. Save and activate the workflow

---

## Step 3: Test the Webhook

```bash
curl -X POST https://minjarul.dpdns.org/webhook/social-clicks \
  -H "Content-Type: application/json" \
  -d '{
    "visitor_id": "test_user_123",
    "event": "click",
    "platform": "facebook",
    "timestamp": "2026-10-05T14:00:00.000Z",
    "url": "https://minjarul1.github.io/social-links-site/",
    "referrer": "direct",
    "ip_address": "103.123.45.67",
    "screen_resolution": "390x844",
    "language": "en-BD",
    "device": "Mobile",
    "browser": "Chrome",
    "os": "Android"
  }'
```

**Expected Response:**
```json
{
  "success": true,
  "message": "Click tracked",
  "data": {
    "visitor_id": "test_user_123",
    "platform": "facebook",
    "country": "Bangladesh",
    "city": "Dhaka"
  }
}
```

---

## Step 4: View Collected Data

1. Open n8n workflow editor
2. Click on **Webhook** node
3. Enable **Test** mode
4. Run workflow manually or wait for real clicks
5. Check **Store in Data Table** output for results

Or query the data table directly in n8n's Data Store tab.

---

## What Data You'll See

### Per Click Record:
- **Visitor ID**: Unique identifier (persists across sessions)
- **Platform**: facebook or whatsapp
- **Device**: Mobile or Desktop
- **Browser**: Chrome, Safari, Firefox, etc.
- **OS**: Android, iOS, Windows, macOS
- **Location**: Country and City (from IP)
- **Timestamp**: Exact time of click
- **Screen Resolution**: User's display size

### Aggregated Stats:
- Total clicks per platform
- Unique visitors count
- Device breakdown (Mobile vs Desktop)
- Browser distribution
- Geographic distribution

---

## Dashboard Queries (Optional)

### Total Clicks Today:
```sql
SELECT COUNT(*) FROM social_clicks 
WHERE timestamp >= datetime('now', '-1 day')
```

### Clicks by Platform:
```sql
SELECT platform, COUNT(*) as count 
FROM social_clicks 
GROUP BY platform
```

### Unique Visitors:
```sql
SELECT COUNT(DISTINCT visitor_id) FROM social_clicks
```

### Mobile vs Desktop:
```sql
SELECT device, COUNT(*) as count 
FROM social_clicks 
GROUP BY device
```

---

## Files in This Repo

| File | Purpose |
|------|---------|
| `index.html` | Tracking website (URL stays same) |
| `n8n-workflow.json` | n8n workflow definition |
| `SETUP.md` | This guide |

---

## Next Steps

1. ✅ Website is live at https://minjarul1.github.io/social-links-site/
2. 📋 Import the n8n workflow
3. 🧪 Test with a click
4. 📊 View collected data in n8n

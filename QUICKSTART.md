# Quick Start Guide - AI Daily Message Blueprint

## 5-Minute Setup

### 1. Create Input Text Helpers (2 minutes)

Go to: **Settings → Devices & Services → Helpers**

Click **"+ Create Helper"** → **Text** → Create these 7:

| Name | Entity ID | Max Length |
|------|-----------|------------|
| Message History 1 | input_text.message_history_1 | 255 |
| Message History 2 | input_text.message_history_2 | 255 |
| Message History 3 | input_text.message_history_3 | 255 |
| Message History 4 | input_text.message_history_4 | 255 |
| Message History 5 | input_text.message_history_5 | 255 |
| Message History 6 | input_text.message_history_6 | 255 |
| Message History 7 | input_text.message_history_7 | 255 |

### 2. Import Blueprint (30 seconds)

Click this button:
[![Import Blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://github.com/oneofthegeeks/HomeAssistant/blob/master/blueprints/automation/ai_daily_message_automation.yaml)

Or manually copy `ai_daily_message_automation.yaml` to `/config/blueprints/automation/`

### 3. Create Automation (2 minutes)

1. **Settings → Automations → Create → Use Blueprint**
2. Select **"AI-Powered Daily Message Automation"**
3. Fill in:
   - **Send Time**: 09:00:00
   - **AI Agent**: conversation.google_ai_conversation (or your AI)
   - **AI Prompt**: (see examples below)
   - **Target Phone**: Your recipient's number
   - **Sender ID**: Your SMS number
   - **SMS Service**: goto_sms.send_sms (or your service)
   - **History Slots 1-7**: Select your 7 helpers
   - **Max Length**: 80
4. Click **Save**

### 4. Add Statistics (Optional - 2 minutes)

Want to track how many messages you've sent?

1. **Settings → Devices & Services → Helpers → Create Helper → Counter**
2. Name: `Message Counter` (entity: `input_text.message_counter`)
3. In your automation settings, select this counter for "Message Counter (Optional)"

See [REPORTING.md](REPORTING.md) for advanced statistics and analytics.

### 5. Add to Dashboard (1 minute)

Edit dashboard → Add Card → Entities → Add:
- input_text.message_history_1
- input_text.message_history_2
- input_text.message_history_3
- input_text.message_history_4
- input_text.message_history_5
- input_text.message_history_6
- input_text.message_history_7

**Want better dashboards?** See [DASHBOARD.md](DASHBOARD.md) for:
- Statistics displays
- Charts and graphs
- Custom layouts
- Multiple automation views

### 6. Test It!

Manually trigger the automation from:
**Settings → Automations → [Your Automation] → Run**

Check your dashboard to see the message history!

Or create a test script from [example_scripts.yaml](example_scripts.yaml)

## Example AI Prompts

### Trash Talk
```
Generate a single funny golf trash talk message about swinging too fast. 
Under 80 characters. Start with "Morning/Hey/Good morning [Name]". 
Include 1-2 emojis. Return ONLY the message.
```

### Motivation
```
Generate an inspiring morning motivational quote.
Under 80 characters. Be uplifting. Include 1 emoji.
Return ONLY the message.
```

### Reminder
```
Generate a funny workout reminder message.
Under 80 characters. Be encouraging and humorous.
Include gym emoji. Return ONLY the message.
```

## Done!

Your automation will now:
- ✅ Send AI messages daily at your chosen time
- ✅ Track last 7 messages on dashboard
- ✅ Generate unique content every day

## What's Next?

Now that you're set up, explore these guides:

📊 **[DASHBOARD.md](DASHBOARD.md)** - Create beautiful dashboards
- Multiple layout styles
- Statistics and charts
- History visualization
- Custom controls

📈 **[REPORTING.md](REPORTING.md)** - Track your messaging
- Statistics tracking
- Success rates
- Time-based reports
- Export data

📝 **[EXAMPLES.md](EXAMPLES.md)** - More configurations
- Different use cases
- AI prompt examples
- Dashboard layouts

## Need Help?

Check the full [README.md](README.md) for:
- Troubleshooting
- Advanced configuration
- Multiple recipient setup
- Custom SMS services

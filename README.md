# AI-Powered Daily Message Automation Blueprint

Send personalized, AI-generated messages daily via SMS with automatic message history tracking.

## 📚 Documentation

- **[QUICKSTART.md](QUICKSTART.md)** - Fast setup guide (5 minutes)
- **[DASHBOARD.md](DASHBOARD.md)** - 📊 Dashboard layouts, visualization, and display options
- **[REPORTING.md](REPORTING.md)** - 📈 Statistics tracking, analytics, and reporting
- **[EXAMPLES.md](EXAMPLES.md)** - Configuration examples for different use cases
- **[HELPER_SETUP_GUIDE.md](HELPER_SETUP_GUIDE.md)** - ⚠️ Essential guide for creating input text helpers (especially for multiple automations)

## Features

- ✅ **AI-Generated Content** - Fresh, unique messages every day using Google AI, OpenAI, Ollama, or any Home Assistant conversation agent
- ✅ **Customizable Prompts** - Full control over message style, tone, and content
- ✅ **Message History** - Tracks last 7 messages with timestamps
- ✅ **SMS Integration** - Works with goto_sms, Twilio, or any SMS service
- ✅ **Dashboard Display** - View message history on your dashboard (see [DASHBOARD.md](DASHBOARD.md))
- ✅ **Statistics & Analytics** - Track total messages sent, success rates, and trends (see [REPORTING.md](REPORTING.md))
- ✅ **Length Control** - Automatic message truncation for SMS limits
- ✅ **Easy Setup** - Blueprint-based configuration via UI

## Use Cases

- 🏌️ Daily trash talk to friends about their golf swing
- 💪 Morning motivational messages
- 📝 Daily reminders with personality
- 🎉 Birthday/anniversary countdown messages
- 🏋️ Workout encouragement messages
- 📚 Study/learning reminders
- 🌱 Habit-building affirmations

## Prerequisites

> ⚠️ **CRITICAL**: You MUST create 7 input_text helpers for EACH automation BEFORE configuring the blueprint!
> 
> **For multiple automations**, use descriptive names to keep them organized:
> - Golf automation: `golf_message_1` through `golf_message_7`
> - Workout automation: `workout_message_1` through `workout_message_7`
> - Motivation automation: `motivation_message_1` through `motivation_message_7`

### Required

1. **AI Conversation Agent** - One of:
   - Google Generative AI (Gemini) - Recommended
   - OpenAI Conversation
   - Ollama (local AI)
   - Any other Home Assistant conversation agent

2. **SMS Service** - One of:
   - goto_sms integration
   - Twilio integration
   - Any Home Assistant SMS notification service

3. **Input Text Helpers** - Create 7 helpers (Settings → Devices & Services → Helpers):
   - `input_text.message_history_1` (Latest Message)
   - `input_text.message_history_2`
   - `input_text.message_history_3`
   - `input_text.message_history_4`
   - `input_text.message_history_5`
   - `input_text.message_history_6`
   - `input_text.message_history_7` (Oldest Message)
   
   **Settings for each:**
   - Mode: Text (not password)
   - Max Length: 255

### Optional

- **Test Script** - For manual triggering (see below)

## Installation

### Step 1: Install the Blueprint

#### Option A: Import via URL
[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://github.com/oneofthegeeks/HomeAssistant/blob/master/blueprints/automation/ai_daily_message_automation.yaml)

#### Option B: Manual Installation
1. Copy `ai_daily_message_automation.yaml` to `/config/blueprints/automation/`
2. Restart Home Assistant

### Step 2: Create Input Text Helpers ⚠️ REQUIRED!

Go to **Settings → Devices & Services → Helpers → Create Helper → Text**

**Create 7 text helpers** for this automation. Set **Max Length to 255** for each.

#### Naming Convention (IMPORTANT for Multiple Automations)

If you're creating **only one automation**, simple names work:
- `Message History 1` through `Message History 7`

**If creating multiple automations**, use descriptive prefixes to keep them organized:

**Golf Trash Talk Automation:**
- `Golf Message 1` (entity: `input_text.golf_message_1`)
- `Golf Message 2` (entity: `input_text.golf_message_2`)
- ... through `Golf Message 7`

**Workout Motivation Automation:**
- `Workout Message 1` (entity: `input_text.workout_message_1`)
- `Workout Message 2` (entity: `input_text.workout_message_2`)
- ... through `Workout Message 7`

**Daily Motivation Automation:**
- `Motivation Message 1` (entity: `input_text.motivation_message_1`)
- ... through `Motivation Message 7`

> 💡 **Tip**: The friendly name becomes the entity_id (spaces become underscores, lowercase).
> Choose names that clearly identify which automation they belong to!

### Step 3: Create Automation from Blueprint

1. Go to **Settings → Automations & Scenes**
2. Click **Create Automation → Use Blueprint**
3. Select **AI-Powered Daily Message Automation**
4. Configure parameters:

   - **Send Time**: When to send daily (e.g., 09:00:00)
   - **AI Agent**: Select your conversation agent
   - **AI Prompt**: Customize your message generation prompt
   - **Phone Numbers**: Target and sender numbers
   - **SMS Service**: Your SMS service call (e.g., `goto_sms.send_sms`)
   - **History Slots**: Select the 7 input_text helpers you created in Step 2 (matching this automation's purpose)
   - **Max Length**: Message character limit (80 for short, 160 for standard SMS)
   - **Message Counter** (Optional): Select a counter helper to track total messages sent (see [REPORTING.md](REPORTING.md))

5. Click **Save** and give your automation a descriptive name (e.g., "Daily Golf Trash Talk" or "Morning Workout Motivation")

## Configuration Examples

### Golf Trash Talk Example
```yaml
AI Prompt: |
  Generate a single funny golf trash talk text message for [YOUR_NAME] about his fast swing. 
  Must be under 80 characters. 
  Start with Morning/Hey/Good morning [YOUR_NAME]. 
  Include 1-2 emojis. 
  Return ONLY the message text.

Max Length: 80
Send Time: 09:00:00
```

### Daily Motivation Example
```yaml
AI Prompt: |
  Generate a single inspiring motivational message for starting the day.
  Must be under 80 characters.
  Be uplifting and energetic.
  Include 1-2 emojis.
  Return ONLY the message text.

Max Length: 80
Send Time: 06:00:00
```

### Workout Reminder Example
```yaml
AI Prompt: |
  Generate a single funny, encouraging workout reminder message.
  Must be under 80 characters.
  Be motivating but humorous.
  Include gym/fitness emojis.
  Return ONLY the message text.

Max Length: 80
Send Time: 17:00:00
```

## Dashboard Display

### Basic Dashboard Card

Add this to your dashboard to see message history:

```yaml
type: entities
title: Message History (Last 7)
entities:
  - entity: input_text.message_history_1
    name: Latest
  - input_text.message_history_2
  - input_text.message_history_3
  - input_text.message_history_4
  - input_text.message_history_5
  - input_text.message_history_6
  - entity: input_text.message_history_7
    name: Oldest
show_header_toggle: false
```

### Want More?

For advanced dashboard layouts with statistics, charts, and better visualization, see **[DASHBOARD.md](DASHBOARD.md)**

**Dashboard Options:**
- Multiple layout styles (grid, vertical, markdown)
- Statistics displays (total sent, success rates)
- History graphs and timelines
- Custom buttons and controls
- Multiple automation management

## Testing

### Quick Test
Simply trigger your automation manually:
1. Go to **Settings → Automations & Scenes**
2. Find your automation (e.g., "Daily Golf Trash Talk")
3. Click the **Run** button (▶️)
4. Check your dashboard to verify the message appears

### Create a Test Script (Optional)

For easy testing from your dashboard, create a script:

Go to **Settings → Automations & Scenes → Scripts → Add Script**

Or add this to `scripts.yaml`:

```yaml
test_golf_message:
  alias: Test Golf Message
  description: Manually trigger golf message automation
  mode: single
  sequence:
    - action: automation.trigger
      target:
        entity_id: automation.daily_golf_trash_talk
      data:
        skip_condition: true
```

Then add a button to your dashboard:

```yaml
type: button
name: Send Test Message
icon: mdi:send
tap_action:
  action: call-service
  service: script.test_golf_message
```

## Troubleshooting

### Message Not Sending
1. Check SMS service configuration
2. Verify phone number format
3. Check Home Assistant logs for errors

### AI Not Generating Messages
1. Verify AI conversation agent is configured
2. Test agent in Developer Tools → Assist
3. Simplify AI prompt if getting errors
4. Check API keys/credentials

### Message History Not Updating
1. Verify all 7 input_text helpers exist
2. Check helper entity IDs match blueprint config
3. Ensure Max Length is 255 for each helper

### Message Too Long Error
- Reduce "Max Message Length" in blueprint config
- Shorten AI prompt requirements
- Add explicit length requirement in prompt

## Advanced Customization

### Multiple Recipients

Create multiple automations from the same blueprint with different:
- Phone numbers
- AI prompts
- Send times

### Custom SMS Services

If using a different SMS service, update the `sms_service` parameter to match your service call format. You may need to adjust the data fields in the blueprint.

### Different AI Models

The blueprint works with any Home Assistant conversation agent:
- Google Gemini (cloud)
- OpenAI GPT (cloud)
- Ollama (local)
- Custom conversation agents

Select your preferred agent in the blueprint configuration.

## Example Automation

Here's what the configured automation does:

1. **Trigger**: Fires at specified time daily
2. **AI Generation**: Calls conversation agent with your prompt
3. **Message Processing**: Truncates to max length if needed
4. **SMS Send**: Sends message via configured SMS service
5. **History Update**: Shifts messages through 7 history slots
6. **Dashboard**: Automatically displays on dashboard

## Credits

Created by [@oneofthegeeks](https://github.com/oneofthegeeks)

## License

MIT License - Feel free to use, modify, and share!

## Support

For issues or questions:
- Open an issue on GitHub
- Check Home Assistant Community forums
- Review Home Assistant logs for errors

## Version History

- **v1.0.0** (2025-11-24): Initial release
  - AI-powered message generation
  - SMS integration
  - 7-message history tracking
  - Blueprint configuration

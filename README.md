# AI-Powered Daily Message Automation Blueprint

Send personalized, AI-generated messages daily via SMS with automatic message history tracking.

## Features

- ✅ **AI-Generated Content** - Fresh, unique messages every day using Google AI, OpenAI, Ollama, or any Home Assistant conversation agent
- ✅ **Customizable Prompts** - Full control over message style, tone, and content
- ✅ **Message History** - Tracks last 7 messages with timestamps
- ✅ **SMS Integration** - Works with goto_sms, Twilio, or any SMS service
- ✅ **Dashboard Display** - View message history on your dashboard
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

### Step 2: Create Input Text Helpers

Go to **Settings → Devices & Services → Helpers → Create Helper → Text**

Create 7 text helpers with these names (or customize):
- `Message History 1`
- `Message History 2`
- `Message History 3`
- `Message History 4`
- `Message History 5`
- `Message History 6`
- `Message History 7`

**Important:** Set Max Length to 255 for each.

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
   - **History Slots**: Select your 7 input_text helpers
   - **Max Length**: Message character limit (80 for short, 160 for standard SMS)

5. Click **Save**

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

Add this to your dashboard to see message history:

```yaml
type: entities
title: Message History (Last 7)
entities:
  - input_text.message_history_1
  - input_text.message_history_2
  - input_text.message_history_3
  - input_text.message_history_4
  - input_text.message_history_5
  - input_text.message_history_6
  - input_text.message_history_7
show_header_toggle: false
```

## Testing

### Create a Test Script

Add this to `scripts.yaml`:

```yaml
test_ai_message:
  alias: Test AI Message
  description: Manually trigger AI message generation and send
  mode: single
  sequence:
    - service: automation.trigger
      data:
        entity_id: automation.YOUR_AUTOMATION_NAME_HERE
        skip_condition: true
```

Then add a button to your dashboard:

```yaml
type: button
name: Send Test Message
icon: mdi:send
tap_action:
  action: call-service
  service: script.test_ai_message
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

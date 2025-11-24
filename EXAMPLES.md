# Example Configurations for AI Daily Message Blueprint

> **⚠️ IMPORTANT**: Before using any configuration, you must create 7 input_text helpers for EACH automation!
> 
> Notice how each example below uses uniquely named helpers (e.g., `golf_message_1-7`, `motivation_1-7`, `workout_1-7`).
> This allows you to run multiple automations simultaneously without conflicts.
> 
> **To create helpers**: Settings → Devices & Services → Helpers → Create Helper → Text (Max Length: 255)

## Configuration 1: Golf Trash Talk

**Required Helpers**: Create these 7 text helpers first:
- `Golf Message 1` through `Golf Message 7` (entities: `input_text.golf_message_1` through `input_text.golf_message_7`)

```yaml
name: Daily Golf Trash Talk
description: Sends daily golf swing trash talk at 9 AM
use_blueprint:
  path: ai_daily_message_automation.yaml
  input:
    time_trigger: "09:00:00"
    ai_agent: conversation.google_ai_conversation
    ai_prompt: |
      Generate a single funny golf trash talk text message for [YOUR_NAME] about his fast swing. 
      Must be under 80 characters. 
      Start with Morning/Hey/Good morning [YOUR_NAME]. 
      Include 1-2 emojis. 
      Return ONLY the message text.
    phone_number: "+1234567890"
    sender_id: "+1987654321"
    sms_service: "goto_sms.send_sms"
    history_slot_1: input_text.golf_message_1
    history_slot_2: input_text.golf_message_2
    history_slot_3: input_text.golf_message_3
    history_slot_4: input_text.golf_message_4
    history_slot_5: input_text.golf_message_5
    history_slot_6: input_text.golf_message_6
    history_slot_7: input_text.golf_message_7
    max_message_length: 80
```

## Configuration 2: Morning Motivation

**Required Helpers**: Create these 7 text helpers first:
- `Motivation 1` through `Motivation 7` (entities: `input_text.motivation_1` through `input_text.motivation_7`)

```yaml
name: Daily Morning Motivation
description: Sends inspiring message every morning at 6 AM
use_blueprint:
  path: ai_daily_message_automation.yaml
  input:
    time_trigger: "06:00:00"
    ai_agent: conversation.google_ai_conversation
    ai_prompt: |
      Generate an inspiring motivational message to start the day.
      Must be under 80 characters.
      Be uplifting and energetic.
      Include 1-2 motivational emojis.
      Return ONLY the message text.
    phone_number: "+1234567890"
    sender_id: "+1987654321"
    sms_service: "goto_sms.send_sms"
    history_slot_1: input_text.motivation_1
    history_slot_2: input_text.motivation_2
    history_slot_3: input_text.motivation_3
    history_slot_4: input_text.motivation_4
    history_slot_5: input_text.motivation_5
    history_slot_6: input_text.motivation_6
    history_slot_7: input_text.motivation_7
    max_message_length: 80
```

## Configuration 3: Evening Workout Reminder

**Required Helpers**: Create these 7 text helpers first:
- `Workout 1` through `Workout 7` (entities: `input_text.workout_1` through `input_text.workout_7`)

```yaml
name: Evening Workout Reminder
description: Funny workout reminder at 5 PM
use_blueprint:
  path: ai_daily_message_automation.yaml
  input:
    time_trigger: "17:00:00"
    ai_agent: conversation.google_ai_conversation
    ai_prompt: |
      Generate a funny, encouraging workout reminder message.
      Must be under 80 characters.
      Be motivating but add humor.
      Include gym/fitness emojis.
      Start with "Hey" or "Time to".
      Return ONLY the message text.
    phone_number: "+1234567890"
    sender_id: "+1987654321"
    sms_service: "goto_sms.send_sms"
    history_slot_1: input_text.workout_1
    history_slot_2: input_text.workout_2
    history_slot_3: input_text.workout_3
    history_slot_4: input_text.workout_4
    history_slot_5: input_text.workout_5
    history_slot_6: input_text.workout_6
    history_slot_7: input_text.workout_7
    max_message_length: 80
```

## Configuration 4: Study Reminder (College Student)

```yaml
name: Evening Study Reminder
description: Encouraging study message at 7 PM
use_blueprint:
  path: ai_daily_message_automation.yaml
  input:
    time_trigger: "19:00:00"
    ai_agent: conversation.google_ai_conversation
    ai_prompt: |
      Generate an encouraging study reminder message for a college student.
      Must be under 80 characters.
      Be supportive and motivating.
      Include study/book emojis.
      Mention grades or success.
      Return ONLY the message text.
    phone_number: "15551234567"
    sender_id: "15559876543"
    sms_service: "goto_sms.send_sms"
    history_slot_1: input_text.study_1
    history_slot_2: input_text.study_2
    history_slot_3: input_text.study_3
    history_slot_4: input_text.study_4
    history_slot_5: input_text.study_5
    history_slot_6: input_text.study_6
    history_slot_7: input_text.study_7
    max_message_length: 80
```

## Configuration 5: Water Reminder

```yaml
name: Hydration Reminder
description: Funny reminder to drink water at noon
use_blueprint:
  path: ai_daily_message_automation.yaml
  input:
    time_trigger: "12:00:00"
    ai_agent: conversation.google_ai_conversation
    ai_prompt: |
      Generate a funny reminder to drink water and stay hydrated.
      Must be under 80 characters.
      Be humorous but health-focused.
      Include water emoji.
      Make it fun and memorable.
      Return ONLY the message text.
    phone_number: "15551234567"
    sender_id: "15559876543"
    sms_service: "goto_sms.send_sms"
    history_slot_1: input_text.water_1
    history_slot_2: input_text.water_2
    history_slot_3: input_text.water_3
    history_slot_4: input_text.water_4
    history_slot_5: input_text.water_5
    history_slot_6: input_text.water_6
    history_slot_7: input_text.water_7
    max_message_length: 80
```

## Tips for Writing AI Prompts

### Be Specific
- State exact character limits
- Specify greeting style
- Define tone (funny, serious, motivational)
- Request emoji usage explicitly

### Keep It Simple
- One message per prompt
- Clear formatting requirements
- "Return ONLY the message text" prevents extra AI commentary

### Test and Iterate
- Run test messages to see AI output
- Adjust prompt based on results
- Fine-tune character limit if needed

### Character Limits
- **80 chars**: Short and punchy (Twitter-style)
- **120 chars**: Standard text message
- **160 chars**: Maximum SMS length

## Dashboard Examples

### Simple List View
```yaml
type: entities
title: Message History
entities:
  - input_text.golf_message_1
  - input_text.golf_message_2
  - input_text.golf_message_3
  - input_text.golf_message_4
  - input_text.golf_message_5
  - input_text.golf_message_6
  - input_text.golf_message_7
```

### With Test Button
```yaml
type: vertical-stack
cards:
  - type: button
    name: Send Test Message
    icon: mdi:send
    tap_action:
      action: call-service
      service: script.test_golf_message
  - type: entities
    title: Last 7 Messages
    entities:
      - input_text.golf_message_1
      - input_text.golf_message_2
      - input_text.golf_message_3
      - input_text.golf_message_4
      - input_text.golf_message_5
      - input_text.golf_message_6
      - input_text.golf_message_7
```

### Markdown Display
```yaml
type: markdown
title: Message History
content: |
  **Latest:** {{ states('input_text.golf_message_1') }}
  
  {{ states('input_text.golf_message_2') }}
  {{ states('input_text.golf_message_3') }}
  {{ states('input_text.golf_message_4') }}
  {{ states('input_text.golf_message_5') }}
  {{ states('input_text.golf_message_6') }}
  {{ states('input_text.golf_message_7') }}
```

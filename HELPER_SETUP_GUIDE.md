# Input Text Helper Setup Guide

## Quick Reference for Multiple Automations

### Why Name Helpers Descriptively?

If you run **multiple automations** (Golf + Workout + Motivation, etc.), each needs its own set of 7 input_text helpers. Descriptive names prevent confusion!

### Naming Pattern

**Format**: `[Purpose] Message [1-7]`

Examples:
- Golf automation → `Golf Message 1` through `Golf Message 7`
- Workout automation → `Workout Message 1` through `Workout Message 7`
- Motivation automation → `Motivation Message 1` through `Motivation Message 7`

### How to Create (Step-by-Step)

1. Go to **Settings → Devices & Services → Helpers**
2. Click **+ Create Helper**
3. Select **Text**
4. Configure:
   - **Name**: `Golf Message 1` (or your chosen prefix)
   - **Icon**: 📝 (optional)
   - **Max Length**: **255** (REQUIRED!)
5. Click **Create**
6. Repeat for Message 2 through Message 7

### Entity IDs

Home Assistant converts names to entity IDs automatically:
- `Golf Message 1` → `input_text.golf_message_1`
- `Workout Message 1` → `input_text.workout_message_1`
- `Motivation Message 1` → `input_text.motivation_message_1`

### Automation Mapping Table

| Automation Type | Helper Prefix | Entity ID Pattern |
|----------------|---------------|-------------------|
| Golf Trash Talk | `Golf Message` | `input_text.golf_message_1-7` |
| Workout Reminder | `Workout Message` | `input_text.workout_message_1-7` |
| Morning Motivation | `Motivation Message` | `input_text.motivation_message_1-7` |
| Study Reminder | `Study Message` | `input_text.study_message_1-7` |
| Custom | `[Your Name] Message` | `input_text.[your_name]_message_1-7` |

### Dashboard Organization

When displaying multiple automations on your dashboard:

```yaml
type: vertical-stack
cards:
  - type: entities
    title: Golf Trash Talk History
    entities:
      - input_text.golf_message_1
      - input_text.golf_message_2
      - input_text.golf_message_3
      - input_text.golf_message_4
      - input_text.golf_message_5
      - input_text.golf_message_6
      - input_text.golf_message_7
  
  - type: entities
    title: Workout Motivation History
    entities:
      - input_text.workout_message_1
      - input_text.workout_message_2
      - input_text.workout_message_3
      - input_text.workout_message_4
      - input_text.workout_message_5
      - input_text.workout_message_6
      - input_text.workout_message_7
```

### Troubleshooting

**Error: "Entity not found"**
- Make sure you created all 7 helpers
- Check entity IDs match exactly (Settings → Devices & Services → Helpers)
- Verify Max Length is set to 255

**Messages not appearing in dashboard**
- Refresh your dashboard (Ctrl+F5 / Cmd+Shift+R)
- Check helper entity IDs are correct in your automation configuration
- Verify the automation has run at least once

### Bulk Creation Script (Advanced)

For Home Assistant users comfortable with YAML, add to `configuration.yaml`:

```yaml
input_text:
  # Golf Messages
  golf_message_1:
    name: Golf Message 1
    max: 255
  golf_message_2:
    name: Golf Message 2
    max: 255
  golf_message_3:
    name: Golf Message 3
    max: 255
  golf_message_4:
    name: Golf Message 4
    max: 255
  golf_message_5:
    name: Golf Message 5
    max: 255
  golf_message_6:
    name: Golf Message 6
    max: 255
  golf_message_7:
    name: Golf Message 7
    max: 255
  
  # Workout Messages
  workout_message_1:
    name: Workout Message 1
    max: 255
  # ... repeat for workout_message_2 through workout_message_7
```

Then restart Home Assistant to create all helpers at once.

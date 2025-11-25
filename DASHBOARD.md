# Dashboard Guide for AI Message Automation

This guide shows you how to create professional, informative dashboards to monitor your AI message automations with statistics, charts, and better visualization.

## Table of Contents

- [Quick Dashboard Setup](#quick-dashboard-setup)
- [Dashboard Layouts](#dashboard-layouts)
- [Statistics & Counters](#statistics--counters)
- [History Visualization](#history-visualization)
- [Multiple Automation Dashboards](#multiple-automation-dashboards)
- [Advanced Features](#advanced-features)

---

## Quick Dashboard Setup

### Basic Message History Card

The simplest way to view your message history:

```yaml
type: entities
title: Recent Messages
entities:
  - entity: input_text.message_history_1
    name: Latest
  - entity: input_text.message_history_2
    name: Yesterday
  - entity: input_text.message_history_3
  - entity: input_text.message_history_4
  - entity: input_text.message_history_5
  - entity: input_text.message_history_6
  - entity: input_text.message_history_7
    name: Oldest
show_header_toggle: false
```

### Enhanced Card with Icons

Add visual appeal with custom icons and names:

```yaml
type: entities
title: 📱 Golf Message History
entities:
  - entity: input_text.golf_message_1
    icon: mdi:message-star
    name: Most Recent
  - entity: input_text.golf_message_2
    icon: mdi:message-text
  - entity: input_text.golf_message_3
    icon: mdi:message-text
  - entity: input_text.golf_message_4
    icon: mdi:message-text-outline
  - entity: input_text.golf_message_5
    icon: mdi:message-text-outline
  - entity: input_text.golf_message_6
    icon: mdi:message-text-outline
  - entity: input_text.golf_message_7
    icon: mdi:message-text-clock
    name: Oldest
state_color: true
```

---

## Dashboard Layouts

### Layout 1: Simple & Clean

Perfect for a single automation:

```yaml
type: vertical-stack
cards:
  - type: markdown
    title: 📨 Daily Message System
    content: |
      **Status:** Active
      **Next Send:** {{ state_attr('automation.daily_golf_trash_talk', 'last_triggered') }}

  - type: button
    name: Send Test Message Now
    icon: mdi:send
    tap_action:
      action: call-service
      service: script.test_golf_message
    hold_action:
      action: none

  - type: entities
    title: Message History (Last 7 Days)
    entities:
      - input_text.golf_message_1
      - input_text.golf_message_2
      - input_text.golf_message_3
      - input_text.golf_message_4
      - input_text.golf_message_5
      - input_text.golf_message_6
      - input_text.golf_message_7
```

### Layout 2: Statistics Dashboard

Track your messaging activity (requires counter setup - see [REPORTING.md](REPORTING.md)):

```yaml
type: vertical-stack
cards:
  - type: horizontal-stack
    cards:
      - type: statistic
        entity: counter.golf_messages_sent
        name: Total Sent
        icon: mdi:send
      - type: statistic
        entity: sensor.golf_messages_this_week
        name: This Week
        icon: mdi:calendar-week
      - type: statistic
        entity: sensor.golf_messages_this_month
        name: This Month
        icon: mdi:calendar-month

  - type: entities
    title: Recent Messages
    entities:
      - input_text.golf_message_1
      - input_text.golf_message_2
      - input_text.golf_message_3
      - input_text.golf_message_4
      - input_text.golf_message_5
```

### Layout 3: Markdown Display (Beautiful & Compact)

Use markdown for a cleaner, more readable display:

```yaml
type: markdown
title: 📱 Message History
content: |
  ### Latest Messages

  🟢 **Latest:**
  {{ states('input_text.golf_message_1') }}

  ---

  📅 **Previous Messages:**

  • {{ states('input_text.golf_message_2') }}

  • {{ states('input_text.golf_message_3') }}

  • {{ states('input_text.golf_message_4') }}

  • {{ states('input_text.golf_message_5') }}

  • {{ states('input_text.golf_message_6') }}

  🕐 **Oldest:**
  {{ states('input_text.golf_message_7') }}

  ---

  📊 **Total Sent:** {{ states('counter.golf_messages_sent') }}
```

### Layout 4: Grid Layout (Modern Look)

```yaml
type: grid
columns: 2
square: false
cards:
  - type: button
    name: Send Test
    icon: mdi:send
    tap_action:
      action: call-service
      service: script.test_golf_message

  - type: button
    name: View Stats
    icon: mdi:chart-line
    tap_action:
      action: navigate
      navigation_path: /lovelace/messaging-stats

  - type: entities
    title: Recent (1-4)
    entities:
      - input_text.golf_message_1
      - input_text.golf_message_2
      - input_text.golf_message_3
      - input_text.golf_message_4

  - type: entities
    title: Older (5-7)
    entities:
      - input_text.golf_message_5
      - input_text.golf_message_6
      - input_text.golf_message_7
```

---

## Statistics & Counters

To track how many messages you've sent, create a counter helper and template sensors.

### Step 1: Create Counter Helper

Go to **Settings → Devices & Services → Helpers → Create Helper → Counter**

- **Name:** Golf Messages Sent
- **Entity ID:** `counter.golf_messages_sent`
- **Icon:** mdi:send
- **Step:** 1
- **Minimum:** 0
- **Maximum:** Leave blank

### Step 2: Update Blueprint to Increment Counter

Add this action to your automation (see [REPORTING.md](REPORTING.md) for detailed instructions):

```yaml
- action: counter.increment
  target:
    entity_id: counter.golf_messages_sent
```

### Step 3: Display Statistics

```yaml
type: entities
title: 📊 Messaging Statistics
entities:
  - entity: counter.golf_messages_sent
    name: Total Messages Sent
    icon: mdi:email-arrow-right
  - entity: sensor.golf_last_sent
    name: Last Sent
    icon: mdi:clock-outline
  - entity: automation.daily_golf_trash_talk
    name: Automation Status
state_color: true
```

---

## History Visualization

### Option 1: History Graph

View message activity over time:

```yaml
type: history-graph
title: Message Activity
hours_to_show: 168
entities:
  - entity: input_text.golf_message_1
    name: Latest Message
  - entity: counter.golf_messages_sent
    name: Total Sent
```

### Option 2: Logbook Card

See a timeline of message events:

```yaml
type: logbook
title: Message Timeline
hours_to_show: 168
entities:
  - input_text.golf_message_1
  - input_text.golf_message_2
  - automation.daily_golf_trash_talk
```

### Option 3: Custom Message Timeline

Create a markdown card that shows timestamps:

```yaml
type: markdown
title: 📅 Message Timeline
content: |
  {% set messages = [
    states('input_text.golf_message_1'),
    states('input_text.golf_message_2'),
    states('input_text.golf_message_3'),
    states('input_text.golf_message_4'),
    states('input_text.golf_message_5'),
    states('input_text.golf_message_6'),
    states('input_text.golf_message_7')
  ] %}

  {% for msg in messages if msg %}
  • {{ msg }}
  {% endfor %}
```

---

## Multiple Automation Dashboards

When running multiple automations (Golf, Workout, Motivation, etc.), organize them clearly:

### Tabbed View

```yaml
type: vertical-stack
cards:
  - type: markdown
    content: |
      # 📱 My Message Automations

      Manage all your AI-powered message automations in one place.

  - type: horizontal-stack
    cards:
      - type: button
        name: Golf
        icon: mdi:golf
        tap_action:
          action: navigate
          navigation_path: /lovelace/golf-messages
      - type: button
        name: Workout
        icon: mdi:dumbbell
        tap_action:
          action: navigate
          navigation_path: /lovelace/workout-messages
      - type: button
        name: Motivation
        icon: mdi:heart
        tap_action:
          action: navigate
          navigation_path: /lovelace/motivation-messages
```

### Combined Statistics View

```yaml
type: grid
columns: 3
square: false
cards:
  - type: statistic
    entity: counter.golf_messages_sent
    name: Golf Messages
    icon: mdi:golf

  - type: statistic
    entity: counter.workout_messages_sent
    name: Workout Messages
    icon: mdi:dumbbell

  - type: statistic
    entity: counter.motivation_messages_sent
    name: Motivation Messages
    icon: mdi:heart
```

### Side-by-Side Comparison

```yaml
type: horizontal-stack
cards:
  - type: entities
    title: 🏌️ Golf Messages
    entities:
      - input_text.golf_message_1
      - input_text.golf_message_2
      - input_text.golf_message_3

  - type: entities
    title: 💪 Workout Messages
    entities:
      - input_text.workout_message_1
      - input_text.workout_message_2
      - input_text.workout_message_3

  - type: entities
    title: ❤️ Motivation Messages
    entities:
      - input_text.motivation_message_1
      - input_text.motivation_message_2
      - input_text.motivation_message_3
```

---

## Advanced Features

### Conditional Cards (Show Only When Active)

Only show the dashboard when messages exist:

```yaml
type: conditional
conditions:
  - entity: input_text.golf_message_1
    state_not: unavailable
card:
  type: entities
  title: Message History
  entities:
    - input_text.golf_message_1
    - input_text.golf_message_2
```

### Message Search (Custom Button Card)

If you have [custom button card](https://github.com/custom-cards/button-card) installed:

```yaml
type: custom:button-card
name: Search Messages
icon: mdi:magnify
tap_action:
  action: call-service
  service: browser_mod.popup
  service_data:
    title: Search Messages
    content:
      type: entities
      entities:
        - input_text.golf_message_1
        - input_text.golf_message_2
        - input_text.golf_message_3
        - input_text.golf_message_4
        - input_text.golf_message_5
        - input_text.golf_message_6
        - input_text.golf_message_7
```

### Auto-Refresh Card

Add a refresh button to update the view:

```yaml
type: vertical-stack
cards:
  - type: button
    name: Refresh
    icon: mdi:refresh
    tap_action:
      action: call-service
      service: homeassistant.update_entity
      service_data:
        entity_id:
          - input_text.golf_message_1

  - type: entities
    title: Message History
    entities:
      - input_text.golf_message_1
      - input_text.golf_message_2
```

### Export Button

Create a button to export message history (requires custom script):

```yaml
type: button
name: Export Message History
icon: mdi:download
tap_action:
  action: call-service
  service: script.export_message_history
```

### Last Sent Timestamp Display

Show when the last message was sent:

```yaml
type: entities
title: Message Status
entities:
  - entity: sensor.golf_last_message_time
    name: Last Sent
    icon: mdi:clock
  - entity: sensor.golf_next_message_time
    name: Next Send
    icon: mdi:clock-outline
```

---

## Tips for Better Dashboards

### 1. Use Descriptive Names
Instead of "Message History 1", use "Latest Message" or "Today's Message"

### 2. Add Visual Hierarchy
Use different card types and layouts to create visual interest

### 3. Color Code by Category
Use custom icons and colors to distinguish between different message types

### 4. Keep It Simple
Don't overwhelm users - show the most important 3-4 messages prominently

### 5. Add Context
Use markdown cards to add explanations and instructions

### 6. Test on Mobile
Ensure your dashboard looks good on phones and tablets

### 7. Use Notifications
Set up notifications when messages fail to send (see [REPORTING.md](REPORTING.md))

---

## Troubleshooting Dashboard Issues

### Messages Not Showing
- Verify helpers are created with correct entity IDs
- Check that automation is running and populating helpers
- Refresh the dashboard (F5 or pull-down on mobile)

### Statistics Not Updating
- Ensure counter helper is created
- Verify counter.increment action is in automation
- Check automation logs for errors

### Cards Not Rendering
- Validate YAML syntax (use a YAML validator)
- Check for typos in entity IDs
- Ensure all referenced entities exist

### Slow Dashboard
- Reduce number of history-graph hours
- Limit number of entities displayed
- Use conditional cards to show/hide content

---

## Next Steps

- **[REPORTING.md](REPORTING.md)** - Set up statistics tracking and analytics
- **[EXAMPLES.md](EXAMPLES.md)** - See complete configuration examples
- **[README.md](README.md)** - Main documentation

---

For more help, visit the [GitHub repository](https://github.com/oneofthegeeks/ai-sms-automation-blueprint) or open an issue.

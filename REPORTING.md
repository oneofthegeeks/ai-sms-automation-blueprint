# Reporting & Analytics Guide

This guide shows you how to track statistics, create reports, and get insights from your AI message automation. Track total messages sent, success rates, delivery times, and more.

## Table of Contents

- [Quick Setup: Message Counter](#quick-setup-message-counter)
- [Statistics Sensors](#statistics-sensors)
- [Success/Failure Tracking](#successfailure-tracking)
- [Time-Based Reports](#time-based-reports)
- [Export & Data Analysis](#export--data-analysis)
- [Notifications & Alerts](#notifications--alerts)
- [Advanced Analytics](#advanced-analytics)

---

## Quick Setup: Message Counter

The simplest way to track how many messages you've sent.

### Step 1: Create Counter Helper

Go to **Settings → Devices & Services → Helpers → Create Helper → Counter**

**Configuration:**
- **Name:** Golf Messages Sent (or customize for your automation)
- **Entity ID:** `counter.golf_messages_sent`
- **Icon:** mdi:send
- **Step:** 1
- **Minimum:** 0
- **Maximum:** Leave blank (unlimited)

**For Multiple Automations:**
Create separate counters:
- `counter.golf_messages_sent`
- `counter.workout_messages_sent`
- `counter.motivation_messages_sent`

### Step 2: Modify Your Automation

Edit your automation YAML or use the automation editor to add a counter increment action.

#### Via Automation Editor
1. Open your automation
2. Go to "Actions" section
3. Click "Add Action"
4. Search for "Counter: Increment"
5. Select your counter entity
6. Save

#### Via YAML (Add to actions section)

```yaml
# Add this AFTER the message is successfully sent
- action: counter.increment
  target:
    entity_id: counter.golf_messages_sent
```

**Full example placement:**

```yaml
action:
  # ... AI generation code ...
  # ... SMS send code ...
  # ... History update code ...

  # Add at the end - increment counter after successful send
  - action: counter.increment
    target:
      entity_id: counter.golf_messages_sent
```

### Step 3: Display Counter on Dashboard

```yaml
type: statistic
entity: counter.golf_messages_sent
name: Total Messages Sent
icon: mdi:send
```

Or as an entity:

```yaml
type: entities
entities:
  - entity: counter.golf_messages_sent
    name: Total Sent
    icon: mdi:email-send
```

---

## Statistics Sensors

Create template sensors to track various statistics.

### Daily Message Count

Go to **Settings → Devices & Services → Helpers → Create Helper → Template**

Or add to `configuration.yaml`:

```yaml
template:
  - sensor:
      - name: "Golf Messages Today"
        unique_id: golf_messages_today
        state: >
          {% set today = now().date() %}
          {% set history = [
            states('input_text.golf_message_1'),
            states('input_text.golf_message_2'),
            states('input_text.golf_message_3'),
            states('input_text.golf_message_4'),
            states('input_text.golf_message_5'),
            states('input_text.golf_message_6'),
            states('input_text.golf_message_7')
          ] %}
          {{ history | select('match', today.strftime('%m/%d')) | list | length }}
        icon: mdi:calendar-today
```

### Weekly Message Count

```yaml
template:
  - sensor:
      - name: "Golf Messages This Week"
        unique_id: golf_messages_this_week
        state: >
          {% set start_of_week = (now() - timedelta(days=now().weekday())).date() %}
          {% set history = [
            states('input_text.golf_message_1'),
            states('input_text.golf_message_2'),
            states('input_text.golf_message_3'),
            states('input_text.golf_message_4'),
            states('input_text.golf_message_5'),
            states('input_text.golf_message_6'),
            states('input_text.golf_message_7')
          ] %}
          {% set count = namespace(value=0) %}
          {% for msg in history %}
            {% if msg %}
              {% set msg_date = strptime(msg.split(':')[0].strip(), '%m/%d %H:%M').date() %}
              {% if msg_date >= start_of_week %}
                {% set count.value = count.value + 1 %}
              {% endif %}
            {% endif %}
          {% endfor %}
          {{ count.value }}
        icon: mdi:calendar-week
```

### Last Message Sent Time

```yaml
template:
  - sensor:
      - name: "Golf Last Message Time"
        unique_id: golf_last_message_time
        state: >
          {{ states('input_text.golf_message_1').split(':')[0].strip() if states('input_text.golf_message_1') else 'Never' }}
        icon: mdi:clock-outline
```

### Next Message Time (Countdown)

```yaml
template:
  - sensor:
      - name: "Golf Next Message"
        unique_id: golf_next_message
        state: >
          {% set trigger_time = state_attr('automation.daily_golf_trash_talk', 'last_triggered') %}
          {% if trigger_time %}
            {% set next_time = (trigger_time + timedelta(days=1)) %}
            {{ next_time.strftime('%H:%M') }}
          {% else %}
            Not scheduled
          {% endif %}
        icon: mdi:clock-alert
```

### Average Message Length

```yaml
template:
  - sensor:
      - name: "Golf Average Message Length"
        unique_id: golf_avg_message_length
        state: >
          {% set history = [
            states('input_text.golf_message_1'),
            states('input_text.golf_message_2'),
            states('input_text.golf_message_3'),
            states('input_text.golf_message_4'),
            states('input_text.golf_message_5'),
            states('input_text.golf_message_6'),
            states('input_text.golf_message_7')
          ] %}
          {% set valid = history | select() | list %}
          {% if valid | length > 0 %}
            {% set total = valid | map('length') | sum %}
            {{ (total / (valid | length)) | round(1) }}
          {% else %}
            0
          {% endif %}
        unit_of_measurement: "chars"
        icon: mdi:ruler
```

---

## Success/Failure Tracking

Track when messages fail to send and get notified.

### Create Success Counter

```yaml
# In Helpers, create two counters:
# 1. counter.golf_messages_success
# 2. counter.golf_messages_failed
```

### Enhanced Automation with Error Handling

Modify your automation to track success/failure:

```yaml
action:
  # ... AI generation ...
  # ... Variable setup ...

  # Try to send SMS with error handling
  - action: !input sms_service
    data:
      message: '{{ new_message }}'
      target: !input phone_number
      sender_id: !input sender_id
    continue_on_error: true

  # Check if send was successful
  - choose:
      - conditions:
          - condition: template
            value_template: "{{ not states.persistent_notification.items() | selectattr('attributes.message', 'search', 'failed') | list }}"
        sequence:
          # Success - increment success counter
          - action: counter.increment
            target:
              entity_id: counter.golf_messages_success

          # Update history on success only
          - action: input_text.set_value
            target:
              entity_id: '{{ slot_1 }}'
            data:
              value: '{{ new_entry }}'
    default:
      # Failed - increment failure counter
      - action: counter.increment
        target:
          entity_id: counter.golf_messages_failed

      # Send notification about failure
      - action: notify.mobile_app
        data:
          title: "Message Send Failed"
          message: "Golf message failed to send at {{ now().strftime('%H:%M') }}"
```

### Success Rate Sensor

```yaml
template:
  - sensor:
      - name: "Golf Message Success Rate"
        unique_id: golf_message_success_rate
        state: >
          {% set success = states('counter.golf_messages_success') | int %}
          {% set failed = states('counter.golf_messages_failed') | int %}
          {% set total = success + failed %}
          {% if total > 0 %}
            {{ ((success / total) * 100) | round(1) }}
          {% else %}
            100
          {% endif %}
        unit_of_measurement: "%"
        icon: mdi:check-circle
```

---

## Time-Based Reports

### Messages Sent This Month

```yaml
template:
  - sensor:
      - name: "Golf Messages This Month"
        unique_id: golf_messages_this_month
        state: >
          {% set this_month = now().strftime('%m') %}
          {% set history = [
            states('input_text.golf_message_1'),
            states('input_text.golf_message_2'),
            states('input_text.golf_message_3'),
            states('input_text.golf_message_4'),
            states('input_text.golf_message_5'),
            states('input_text.golf_message_6'),
            states('input_text.golf_message_7')
          ] %}
          {{ history | select('match', this_month + '/') | list | length }}
        icon: mdi:calendar-month
```

### Peak Sending Time Sensor

```yaml
template:
  - sensor:
      - name: "Golf Peak Send Time"
        unique_id: golf_peak_send_time
        state: >
          {{ state_attr('automation.daily_golf_trash_talk', 'last_triggered').strftime('%H:%M') if state_attr('automation.daily_golf_trash_talk', 'last_triggered') else 'Not set' }}
        icon: mdi:clock-star
```

### Days Since Last Message

```yaml
template:
  - sensor:
      - name: "Golf Days Since Last Message"
        unique_id: golf_days_since_last
        state: >
          {% set last_trigger = state_attr('automation.daily_golf_trash_talk', 'last_triggered') %}
          {% if last_trigger %}
            {{ (now() - last_trigger).days }}
          {% else %}
            Never sent
          {% endif %}
        unit_of_measurement: "days"
        icon: mdi:calendar-clock
```

---

## Export & Data Analysis

### Method 1: Manual CSV Export via Template

Create a script to export message history:

**Add to `scripts.yaml`:**

```yaml
export_golf_messages:
  alias: Export Golf Message History
  description: Export message history to persistent notification
  sequence:
    - action: persistent_notification.create
      data:
        title: "Golf Message Export"
        notification_id: golf_export
        message: |
          Date,Time,Message
          {{ states('input_text.golf_message_1') | replace(':', ',', 1) }}
          {{ states('input_text.golf_message_2') | replace(':', ',', 1) }}
          {{ states('input_text.golf_message_3') | replace(':', ',', 1) }}
          {{ states('input_text.golf_message_4') | replace(':', ',', 1) }}
          {{ states('input_text.golf_message_5') | replace(':', ',', 1) }}
          {{ states('input_text.golf_message_6') | replace(':', ',', 1) }}
          {{ states('input_text.golf_message_7') | replace(':', ',', 1) }}
```

**Dashboard button:**

```yaml
type: button
name: Export Messages
icon: mdi:download
tap_action:
  action: call-service
  service: script.export_golf_messages
```

### Method 2: Recorder Database Query

Messages are automatically stored in Home Assistant's database. Query them using:

**SQL Query (via SQLite):**

```sql
SELECT state, last_changed
FROM states
WHERE entity_id = 'input_text.golf_message_1'
ORDER BY last_changed DESC
LIMIT 100;
```

### Method 3: Long-term Statistics

Enable long-term statistics for your counters in `configuration.yaml`:

```yaml
recorder:
  include:
    entities:
      - counter.golf_messages_sent
      - counter.golf_messages_success
      - counter.golf_messages_failed
  commit_interval: 30
  purge_keep_days: 365
```

This allows you to view historical data in Home Assistant's history and statistics panels for up to a year.

### Method 4: External Logging

Send message data to external services:

**To Google Sheets (requires AppDaemon or Node-RED):**

```yaml
# Add to automation actions
- action: rest_command.log_to_sheets
  data:
    message: '{{ new_message }}'
    timestamp: '{{ now().isoformat() }}'
```

**To InfluxDB (for Grafana dashboards):**

```yaml
# In configuration.yaml
influxdb:
  host: your_influxdb_host
  database: home_assistant
  include:
    entities:
      - counter.golf_messages_sent
      - input_text.golf_message_1
```

---

## Notifications & Alerts

### Daily Summary Notification

Create an automation to send daily summaries:

```yaml
alias: Golf Message Daily Summary
trigger:
  - platform: time
    at: "20:00:00"
action:
  - action: notify.mobile_app
    data:
      title: "📊 Daily Message Report"
      message: |
        Golf messages today: {{ states('sensor.golf_messages_today') }}
        Total sent: {{ states('counter.golf_messages_sent') }}
        Success rate: {{ states('sensor.golf_message_success_rate') }}%

        Latest message: {{ states('input_text.golf_message_1').split(': ', 1)[1] }}
```

### Weekly Report

```yaml
alias: Golf Message Weekly Report
trigger:
  - platform: time
    at: "09:00:00"
  - platform: template
    value_template: "{{ now().weekday() == 6 }}"  # Sunday
action:
  - action: notify.mobile_app
    data:
      title: "📈 Weekly Message Report"
      message: |
        Messages this week: {{ states('sensor.golf_messages_this_week') }}
        Total all-time: {{ states('counter.golf_messages_sent') }}
        Success rate: {{ states('sensor.golf_message_success_rate') }}%
        Average length: {{ states('sensor.golf_avg_message_length') }} chars
```

### Failure Alert

```yaml
alias: Golf Message Failure Alert
trigger:
  - platform: state
    entity_id: counter.golf_messages_failed
action:
  - action: notify.mobile_app
    data:
      title: "⚠️ Message Send Failed"
      message: |
        Golf message failed to send!
        Time: {{ now().strftime('%H:%M') }}
        Total failures: {{ states('counter.golf_messages_failed') }}
      data:
        priority: high
        tag: message_failure
```

---

## Advanced Analytics

### Create a Comprehensive Stats Dashboard

```yaml
type: vertical-stack
cards:
  - type: markdown
    title: 📊 Golf Message Analytics
    content: |
      ## Performance Metrics

      **All-Time Stats**
      - Total Sent: {{ states('counter.golf_messages_sent') }}
      - Success Rate: {{ states('sensor.golf_message_success_rate') }}%
      - Failed: {{ states('counter.golf_messages_failed') }}

      **Recent Activity**
      - Today: {{ states('sensor.golf_messages_today') }}
      - This Week: {{ states('sensor.golf_messages_this_week') }}
      - This Month: {{ states('sensor.golf_messages_this_month') }}

      **Message Quality**
      - Avg Length: {{ states('sensor.golf_avg_message_length') }} chars
      - Last Sent: {{ states('sensor.golf_last_message_time') }}
      - Next Send: {{ states('sensor.golf_next_message') }}

  - type: history-graph
    title: Activity Trend
    hours_to_show: 168
    entities:
      - counter.golf_messages_sent

  - type: gauge
    entity: sensor.golf_message_success_rate
    name: Success Rate
    min: 0
    max: 100
    severity:
      green: 95
      yellow: 85
      red: 0
```

### Message Length Distribution

```yaml
template:
  - sensor:
      - name: "Golf Message Length Category"
        unique_id: golf_length_category
        state: >
          {% set latest = states('input_text.golf_message_1') %}
          {% if latest %}
            {% set length = latest | length %}
            {% if length < 40 %}
              Short
            {% elif length < 80 %}
              Medium
            {% else %}
              Long
            {% endif %}
          {% else %}
            Unknown
          {% endif %}
        icon: mdi:format-size
```

### Consistency Score

Track how consistently messages are being sent:

```yaml
template:
  - sensor:
      - name: "Golf Message Consistency"
        unique_id: golf_consistency
        state: >
          {% set expected = 7 %}  # Expected 7 messages in history
          {% set history = [
            states('input_text.golf_message_1'),
            states('input_text.golf_message_2'),
            states('input_text.golf_message_3'),
            states('input_text.golf_message_4'),
            states('input_text.golf_message_5'),
            states('input_text.golf_message_6'),
            states('input_text.golf_message_7')
          ] %}
          {% set actual = history | select() | list | length %}
          {{ ((actual / expected) * 100) | round(0) }}
        unit_of_measurement: "%"
        icon: mdi:chart-line
```

---

## Best Practices

### 1. Start Simple
Begin with just a counter, then add sensors as needed.

### 2. Use Meaningful Names
Name your sensors and counters clearly (e.g., `golf_messages_sent` not `counter_1`).

### 3. Set Up Alerts
Configure failure notifications so you know when something breaks.

### 4. Review Regularly
Check your stats weekly to ensure everything is working properly.

### 5. Archive Old Data
Use the recorder configuration to keep historical data for analysis.

### 6. Test Thoroughly
Test your tracking by manually triggering automations and verifying counters increment.

### 7. Document Your Setup
Keep notes on what each sensor tracks and how it's calculated.

---

## Troubleshooting

### Counter Not Incrementing
- Verify counter entity exists in Helpers
- Check automation YAML includes counter.increment action
- Ensure automation is triggering (check traces)
- Look for errors in Home Assistant logs

### Template Sensors Showing "Unknown"
- Check entity IDs are correct
- Verify template syntax (use Developer Tools → Template)
- Ensure source entities have valid states
- Reload templates after changes

### Statistics Not Recording
- Enable entities in recorder configuration
- Check database isn't full
- Verify purge_keep_days is set appropriately
- Restart Home Assistant after recorder changes

### Notifications Not Sending
- Verify notify service is configured
- Check mobile app is connected
- Test notifications from Developer Tools
- Review automation traces for errors

---

## Next Steps

- **[DASHBOARD.md](DASHBOARD.md)** - Create beautiful dashboards to visualize your data
- **[EXAMPLES.md](EXAMPLES.md)** - See complete configuration examples
- **[README.md](README.md)** - Return to main documentation

---

For more help, visit the [GitHub repository](https://github.com/oneofthegeeks/ai-sms-automation-blueprint) or open an issue.

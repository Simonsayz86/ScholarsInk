# Night Lights Color Notification Automation

## Overview
This Home Assistant script flashes night lights with specific colors to provide visual notifications for different events (doorbell, security alerts, package deliveries, reminders, and warnings).

## Script Details
- **Script Name**: `night_lights_color_notification`
- **Location**: Home Assistant → Settings → Automations & Scenes → Scripts
- **Status**: ✅ Created and Tested (January 2025)

## Features
- Color-coded notifications for different event types
- Flexible location targeting (all lights, specific areas, or individual rooms)
- Non-intrusive 3-flash pattern with smooth transitions
- Easy to integrate with automations, Node-RED, or service calls

## Input Parameters

### notification_type
Defines the color of the notification flash:
- `doorbell` - Cyan blue [0, 150, 255]
- `security` - Red [255, 0, 0]
- `package` - Orange [255, 165, 0]
- `reminder` - Purple [128, 0, 255]
- `warning` - Yellow [255, 255, 0]
- Default: White [255, 255, 255]

### target_lights
Defines which lights to flash:
- `all` - All night lights (downstairs hallway, master bedroom entry, mud room, upstairs hallway)
- `entrance` - Front entrance lights (downstairs hallway, upstairs hallway)
- `bedroom` - Master bedroom entry light only
- `mud` - Mud room night light only
- Default: `all`

## Usage Examples

### Manual Test
```yaml
service: script.night_lights_color_notification
data:
  notification_type: doorbell
  target_lights: all
```

### In an Automation
```yaml
automation:
  - alias: "Doorbell Ring Visual Notification"
    trigger:
      - platform: state
        entity_id: binary_sensor.front_doorbell
        to: 'on'
    action:
      - service: script.night_lights_color_notification
        data:
          notification_type: doorbell
          target_lights: entrance
```

### Node-RED Integration
Use the "Call Service" node with:
- **Domain**: script
- **Service**: night_lights_color_notification
- **Data**: 
```json
{
  "notification_type": "package",
  "target_lights": "all"
}
```

## Technical Specifications

### Flash Pattern
- **Repeat Count**: 3 times
- **On Duration**: 500ms
- **Off Duration**: 500ms
- **Transition**: 0.3 seconds (smooth fade)
- **Brightness**: Maximum (255)
- **Total Duration**: ~3 seconds

### Targeted Lights
- `light.downstairs_hallway_night_light`
- `light.master_bedroom_entry_night_light`
- `light.mud_room_night_light`
- `light.upstairs_hallway_night_light`

## Integration Recommendations

### 1. Doorbell Notifications
- Trigger: Ring doorbell or Nest doorbell press
- Color: Cyan blue
- Lights: Entrance or all

### 2. Security Alerts
- Trigger: Motion detected in away mode, door/window sensors
- Color: Red
- Lights: All

### 3. Package Delivery
- Trigger: Package detected via camera or delivery notification
- Color: Orange
- Lights: Entrance

### 4. Calendar Reminders
- Trigger: 15 minutes before calendar event
- Color: Purple
- Lights: Bedroom or all

### 5. System Warnings
- Trigger: Low battery, device offline, leak detected
- Color: Yellow
- Lights: All

## Future Enhancements
- [ ] Add support for custom RGB colors
- [ ] Variable flash count (1-10)
- [ ] Configurable flash speed
- [ ] Integration with Alexa announcements
- [ ] Add "emergency" mode with rapid red flashing
- [ ] Save and restore previous light states

## Testing
Script was successfully tested on January 2025 with:
- Notification type: doorbell
- Target lights: all
- Result: All 4 night lights flashed cyan blue 3 times as expected

## Notes
- This script works best when night lights are already on
- If lights are off, they will flash briefly then turn back off
- The script does not currently save/restore previous light colors
- Transition time of 0.3s provides smooth fading without being too slow

## Related Files
- [Smart Home Device Master List](SMART_HOME_DEVICE_MASTER_LIST.md) - Complete inventory of all devices

---
*Created: January 2025*  
*Last Updated: January 2025*  
*Version: 1.0*

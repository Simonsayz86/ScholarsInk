# SMART HOME DEVICE MASTER LIST
## Complete System Documentation - January 2026

---

## EXECUTIVE SUMMARY

This document provides a complete inventory and taxonomy of all smart home devices, integrations, and automation systems deployed at the residence. The system comprises 104+ active devices across 12+ integrations, managed through Home Assistant (192.168.1.119:8123) with supporting platforms including SmartThings, Lutron Caseta, Ring, and various IoT protocols.

**Network Infrastructure:**
- Primary HA Server: 192.168.1.119:8123 (previously 192.168.1.82:8123)
- UGREEN NAS: 192.168.1.119:9999
- Router: Luxul (192.168.1.254)
- Zigbee2MQTT: 192.168.1.119:8080
- Homebridge: 192.168.1.119:8581
- Node-RED: Integrated via HA

---

## DEVICE LABELING TAXONOMY (41 LABELS)

### Room-Type Labels (7)
- RT_LivingRoom
- RT_Kitchen  
- RT_Bedroom
- RT_Bathroom
- RT_Hallway
- RT_Garage
- RT_Outdoor

### Device-Type Labels (9)
- DT_Light
- DT_Switch
- DT_Sensor
- DT_Lock
- DT_Camera
- DT_Fan
- DT_Cover
- DT_Remote
- DT_MediaPlayer

### Function-Based Labels (6)
- FN_Occupancy
- FN_Security
- FN_Climate
- FN_Entertainment
- FN_Energy
- FN_Cleaning

### Integration/Protocol Labels (10)
- INT_Ring
- INT_LutronCaseta
- INT_MobileApp
- INT_Forecast
- INT_Backup
- INT_Sun
- INT_SmartThings
- INT_NodeRED
- INT_Broadlink
- INT_Roomba

### Platform/Service Labels (5)
- PL_Core (HA Core)
- PL_Cloud (Cloud-based)
- PL_Local (Local network)
- PL_AddOn (HA Add-ons)
- PL_ThirdParty

### Location/Zone Labels (3)
- Z_Home
- Z_Upstairs
- Z_Downstairs

---

## INTEGRATION BREAKDOWN

### Ring Integration (2 devices)
**Platform:** Cloud  
**Purpose:** Security cameras and doorbells  
**Status:** Active  
**Devices:**
- Front Door Camera
- Front Door Motion Sensor

### Lutron Caseta Integration (63 devices)
**Platform:** Local (Lutron Bridge)  
**Purpose:** Lighting control, switches, dimmers, fans  
**Status:** Active - Primary lighting system  
**Device Distribution:**
- Lights: ~45
- Switches: ~12
- Fans: ~6

**Areas Covered:** All rooms

### SmartThings Integration (28 devices)
**Platform:** Cloud (Samsung)  
**Purpose:** Mixed IoT devices, sensors, locks, outlets  
**Status:** Active  
**Device Types:**
- Motion sensors
- Door/window sensors
- Smart locks
- Temperature sensors
- Smart outlets
- Media players (Roku)

**Integration Method:** SmartThings hub → Home Assistant

### Mobile App Integration (1 device)
**Platform:** Core  
**Purpose:** Phone presence detection and sensors  
**Device:** Pixel 8 Pro  
**Sensors Exposed:** Location, battery, connectivity status

### Weather Forecast Integration (1 device)
**Platform:** Cloud (Met.no)  
**Purpose:** Local weather data  
**Status:** Active

### Backup Integration (1 device)
**Platform:** Add-on  
**Purpose:** System backup monitoring  
**Status:** Active

### Sun Integration (1 device)
**Platform:** Core  
**Purpose:** Sunrise/sunset automation triggers  
**Status:** Active

### Node-RED Integration (2 devices)
**Platform:** Third-party  
**Purpose:** Advanced automation logic  
**Status:** Active  
**Exposed Entities:**
- Status sensor
- Master automation switch

### Broadlink Integration (2 devices)
**Platform:** Local  
**Purpose:** IR/RF remote control  
**Status:** Active  
**Devices:**
- Living Room IR Remote
- AC Power Control

### Roomba Integration (1 device)
**Platform:** Third-party (iRobot)  
**Purpose:** Automated vacuum  
**Status:** Active  
**Model:** Roomba (Living Room)

### Roku Integration (1 device)
**Platform:** Third-party  
**Purpose:** Media streaming  
**Integration:** Via SmartThings  
**Location:** Living Room

### System Monitor Integration (1 device)
**Platform:** Core  
**Purpose:** HA server health monitoring  
**Metrics:** CPU, memory, disk usage

---

## AUTOMATION SYSTEM

### Key Automations Documented:

**1. Phase 11: Evening Warm Night Lights**
- Triggers: Motion detection
- Actions: Warm lighting activation
- Recently Modified: Removed Master Bedroom Entry Light (Jan 2026)

**2. Security Monitoring**
- Ring camera integration
- Motion sensor triggers
- Lock state monitoring

**3. Climate Control**
- Fan automation based on temperature
- Thermostat schedules

**4. Occupancy-Based Lighting**
- Motion sensor triggers
- Time-of-day adjustments
- Zone-based control

---

## NETWORK TOPOLOGY

### Core Infrastructure
```
Internet
  |
  └─ Luxul Router (192.168.1.254)
      |
      ├─ Home Assistant Server (192.168.1.119:8123)
      │   ├─ Zigbee2MQTT (192.168.1.119:8080)
      │   ├─ Homebridge (192.168.1.119:8581)
      │   └─ Node-RED (integrated)
      |
      ├─ UGREEN NAS (192.168.1.119:9999)
      |
      ├─ Lutron Caseta Bridge (local)
      |
      ├─ SmartThings Hub (cloud)
      |
      └─ Ring Devices (cloud)
```

### Recent Network Changes (Jan 2026)
- Sharp Aquos TV added via ethernet
- Luxul switch integration updated
- NAS drive ethernet connection relocated
- No Home Assistant configuration changes required for NAS move

---

## DEPLOYMENT AUTOMATION SCRIPTS

### Available Python Scripts:

**1. export_entities_to_csv.py**
- **Purpose:** Export all HA entities to CSV with auto-labeled taxonomy
- **Requirements:** websockets, requests, asyncio
- **Output:** entities_labels.csv
- **Configuration:** Pre-configured for 192.168.1.82:8123

**2. bulk_apply_labels.py**
- **Purpose:** Bulk-apply 41-label taxonomy to all entities
- **Method:** WebSocket API entity registry updates
- **Input:** entities_labels.csv
- **Features:** Progress reporting, error handling, rollback support

### Git-Based Configuration Management:
```bash
cd /config
git init
git add configuration.yaml customize.yaml
git commit -m "Initial HA config with device-label metadata"
git remote add origin git@github.com:USER/home-assistant-config.git
git push -u origin main
```

### Validation Commands:
```bash
cd /config
ha core check  # Validate configuration
ha core restart  # Apply changes
```

### Rollback Procedure:
```bash
cd /config
git log --oneline  # Find last good commit
git reset --hard <commit-hash>
ha core restart
```

---

## HOME ASSISTANT LABEL IMPLEMENTATION

### Phase A: UI Label Creation
**Location:** Settings → Organization → Labels  
**Action:** Create all 41 labels manually in HA interface  
**Status:** Ready for deployment

### Phase B: YAML Metadata Schema
**File:** `/config/customize.yaml`  
**Structure:**
```yaml
homeassistant:
  customize: !include customize.yaml
```

**Per-Entity Pattern:**
```yaml
entity_id:
  friendly_name: "Device Name"
  room_type: RT_LivingRoom
  device_type: DT_Light
  function_labels:
    - FN_Entertainment
    - FN_Energy
  integration_label: INT_LutronCaseta
  platform_label: PL_Local
  zone_label: Z_Downstairs
```

### Phase C: Bulk Application via WebSocket API
**Method:** Python script using config/entity_registry/update  
**Authentication:** Long-lived access token  
**Progress:** Live console output with success/error tracking

---

## DEVICE ORGANIZATION BEST PRACTICES

### Naming Conventions:
- Use descriptive, location-based names
- Include room/zone prefix for easy filtering
- Example: "Living Room Main Lights" vs "Light 1"

### Area Assignment:
- All devices assigned to appropriate HA Areas
- Areas map to Room-Type labels
- Enables voice control via "turn off all lights in kitchen"

### Label Application Strategy:
- Minimum 2-4 labels per device
- Always include: room_type, device_type, integration, platform
- Add function labels based on primary use case
- Zone label for multi-floor management

### Automation Organization:
- Use descriptive names with phase numbers
- Document trigger conditions and actions
- Track modifications in git commits
- Test with "Check Configuration" before restart

---

## MAINTENANCE PROCEDURES

### Regular Tasks:

**Weekly:**
- Review automation logs
- Check device battery levels
- Verify backup completion

**Monthly:**
- Update HA core and integrations
- Review and optimize automations
- Clean up unused entities
- Git commit configuration changes

**Quarterly:**
- Full system backup to NAS
- Review and update device labels
- Network security audit
- Documentation updates

### Troubleshooting Commands:
```bash
# Check HA logs
ha core logs

# Restart specific integration
ha integration reload smartthings

# Check system resources
ha system info

# Database maintenance
ha recorder purge --days 30
```

---

## EXPANSION ROADMAP

### Planned Additions:
- Matter protocol integration
- Additional Zigbee devices
- Energy monitoring system
- Advanced presence detection
- Multi-room audio system

### Integration Candidates:
- Scrypted (HomeKit Secure Video)
- ESPHome devices
- Additional Ring cameras
- Smart blinds/shades
- Irrigation control

---

## REFERENCE LINKS

### Home Assistant Resources:
- Official Docs: https://www.home-assistant.io/docs/
- Community Forum: https://community.home-assistant.io/
- Integration Docs: https://www.home-assistant.io/integrations/

### Perplexity Smart Home Space:
- Implementation Guides: Comprehensive device labeling taxonomy
- Python Automation Scripts: Export and bulk-apply utilities
- Network Documentation: Infrastructure change logs

### Git Repositories:
- ScholarsInk: https://github.com/Simonsayz86/Scholarsink (ready for HA config)

---

## APPENDIX: QUICK REFERENCE

### Network Addresses:
```
Home Assistant: http://192.168.1.119:8123
UGREEN NAS: http://192.168.1.119:9999  
Zigbee2MQTT: http://192.168.1.119:8080
Homebridge: http://192.168.1.119:8581
Router Admin: http://192.168.1.254
```

### API Authentication:
```
Long-Lived Token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiI4OTg5NjcxMzkyYzg0N2ZlOGQzNzA2NmJkNTQ0MmMxZiIsImlhdCI6MTc2NTAwNjM4MSwiZXhwIjoyMDgwMzY2MzgxfQ.7fhprwUnch9_8thp2syjbraIdZuzis6uWgqpA08ZkLI
(Valid through 2036)
```

### Integration Count Summary:

| Integration | Device Count | Platform |
|------------|-------------|----------|
| Lutron Caseta | 63 | Local |
| SmartThings | 28 | Cloud |
| Ring | 2 | Cloud |
| Node-RED | 2 | Local |
| Broadlink | 2 | Local |
| Mobile App | 1 | Core |
| Forecast | 1 | Cloud |
| Backup | 1 | Add-on |
| Sun | 1 | Core |
| Roomba | 1 | Cloud |
| Roku | 1 | Cloud |
| System Monitor | 1 | Core |
| **TOTAL** | **104** | **Mixed** |

---

## DOCUMENT VERSION CONTROL

**Version:** 1.0  
**Created:** January 3, 2026  
**Last Updated:** January 3, 2026  
**Author:** System Administrator  
**Status:** Production

**Change Log:**
- 2026-01-03: Initial comprehensive documentation
- 2026-01-03: Added 41-label taxonomy
- 2026-01-03: Included Python automation scripts
- 2026-01-03: Documented network topology and recent changes

---

**END OF DOCUMENT**

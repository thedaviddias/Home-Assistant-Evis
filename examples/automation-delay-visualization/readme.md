# Automation Delay Visualization

While adding some presence based automation and parametrization for them, like delays for turning off devices and lights, for testing purposes I wanted to see some visualization how the delay progresses. But I ended up liking so much that I fine tuned it to be part of my dashboards.
![](automation-visualization.gif)

## Helpers

For each automation to visualize, I used 3 helpers

* input_boolean - to turn the automation on/off (in Node-RED)
* input_number - in secconds to define the delay, when to turn off the device, after no presence in the room.
* timer - timer is used to visualize the delay.

## Dashboard Visualization

![](automation-visualization-2.gif)

For the visualization I used [timer-bar-card](https://github.com/rianadon/timer-bar-card) with a grid and mushroom entity cards which show the input_boolean and input_number helpers.

```YAML
type: custom:stack-in-card
mode: vertical
cards:
  - type: grid
    square: false
    columns: 2
    cards:
      - type: custom:mushroom-entity-card
        entity: input_boolean.office_automation_presense_lights
        name: Presence Lights
        icon: mdi:robot
        card_mod:
          style: |
            ha-card {
              z-index: 5;
              border: 0px;
            }
      - type: custom:mushroom-entity-card
        entity: input_number.office_automation_presence_lights_delay
        name: Lights Off Delay
        card_mod:
          style: |
            ha-card {
              z-index: 5;
              border: 0px;
            }
  - type: custom:timer-bar-card
    entities:
      - timer.office_automation_presence_lights_timer
    invert: true
    bar_height: 2px
    bar_foreground: orange
    bar_background: '#111'
    layout: full_row
    text_width: 0px
    bar_width: 80%
    card_mod:
      style: |
        ha-card {
          height: 4px;
          margin-top: -40px;
          margin-bottom: -25px;
          padding: 0px;
          z-index: 1;
          border: 0px;
        }

```

## Automation (Node-RED)

![](automation-timer.png)

Here is the JSON in compact [node-red-automation-compact.json](node-red-automation-compact.json) and below as formatted

```JSON
[
    {
        "id": "e4cefc106cda6406",
        "type": "server-state-changed",
        "z": "8817f468bc7f98b2",
        "name": "Office Motion Sensor",
        "server": "dba636d5.d235b8",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [
            {
                "property": "name",
                "value": ""
            },
            {
                "property": "icon",
                "value": ""
            }
        ],
        "entityidfilter": "binary_sensor.office_motion_sensor_occupancy",
        "entityidfiltertype": "exact",
        "outputinitially": false,
        "state_type": "str",
        "haltifstate": "",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "outputs": 1,
        "output_only_on_state_change": true,
        "for": 0,
        "forType": "num",
        "forUnits": "minutes",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "eventData"
            },
            {
                "property": "topic",
                "propertyType": "msg",
                "value": "",
                "valueType": "triggerId"
            }
        ],
        "x": 170,
        "y": 260,
        "wires": [
            [
                "b4c2a37a84fce73c"
            ]
        ]
    },
    {
        "id": "b4c2a37a84fce73c",
        "type": "api-current-state",
        "z": "8817f468bc7f98b2",
        "name": "Automation On?",
        "server": "dba636d5.d235b8",
        "version": 3,
        "outputs": 2,
        "halt_if": "on",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "input_boolean.office_automation_presense_lights",
        "state_type": "str",
        "blockInputOverrides": false,
        "outputProperties": [],
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "override_topic": false,
        "state_location": "payload",
        "override_payload": "msg",
        "entity_location": "data",
        "override_data": "msg",
        "x": 400,
        "y": 260,
        "wires": [
            [
                "2ed1d762c80d7f13"
            ],
            []
        ]
    },
    {
        "id": "2ed1d762c80d7f13",
        "type": "switch",
        "z": "8817f468bc7f98b2",
        "name": "",
        "property": "payload",
        "propertyType": "msg",
        "rules": [
            {
                "t": "eq",
                "v": "on",
                "vt": "str"
            },
            {
                "t": "eq",
                "v": "off",
                "vt": "str"
            }
        ],
        "checkall": "true",
        "repair": false,
        "outputs": 2,
        "x": 610,
        "y": 260,
        "wires": [
            [
                "5494de738fc044b4",
                "e85c5f2fc15514ed"
            ],
            [
                "f2a1ad600d24f52d"
            ]
        ]
    },
    {
        "id": "5494de738fc044b4",
        "type": "time-range-switch",
        "z": "8817f468bc7f98b2",
        "name": "",
        "lat": "",
        "lon": "",
        "startTime": "22:30",
        "endTime": "09:00",
        "startOffset": 0,
        "endOffset": 0,
        "x": 830,
        "y": 200,
        "wires": [
            [
                "161761aa14dfbdeb"
            ],
            [
                "f99fe55652428ccd"
            ]
        ]
    },
    {
        "id": "e85c5f2fc15514ed",
        "type": "api-call-service",
        "z": "8817f468bc7f98b2",
        "name": "",
        "server": "dba636d5.d235b8",
        "version": 5,
        "debugenabled": true,
        "domain": "timer",
        "service": "cancel",
        "areaId": [],
        "deviceId": [],
        "entityId": [
            "timer.office_automation_presence_lights_timer"
        ],
        "data": "",
        "dataType": "jsonata",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 830,
        "y": 300,
        "wires": [
            [
                "09e072677c06aaf0"
            ]
        ]
    },
    {
        "id": "f2a1ad600d24f52d",
        "type": "api-current-state",
        "z": "8817f468bc7f98b2",
        "name": "Automation Delay",
        "server": "dba636d5.d235b8",
        "version": 3,
        "outputs": 1,
        "halt_if": "",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "input_number.office_automation_presence_lights_delay",
        "state_type": "num",
        "blockInputOverrides": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "entity"
            }
        ],
        "for": 0,
        "forType": "num",
        "forUnits": "minutes",
        "x": 710,
        "y": 360,
        "wires": [
            [
                "f8bdee5637aa6eb9"
            ]
        ]
    },
    {
        "id": "161761aa14dfbdeb",
        "type": "api-call-service",
        "z": "8817f468bc7f98b2",
        "name": "Turn ON Monitor Light 1",
        "server": "dba636d5.d235b8",
        "version": 5,
        "debugenabled": false,
        "domain": "light",
        "service": "turn_on",
        "areaId": [],
        "deviceId": [],
        "entityId": [
            "light.office_monitor_light_1"
        ],
        "data": "",
        "dataType": "jsonata",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1090,
        "y": 160,
        "wires": [
            []
        ]
    },
    {
        "id": "f99fe55652428ccd",
        "type": "api-call-service",
        "z": "8817f468bc7f98b2",
        "name": "Turn ON Monitor Light 1 & 2",
        "server": "dba636d5.d235b8",
        "version": 5,
        "debugenabled": false,
        "domain": "light",
        "service": "turn_on",
        "areaId": [],
        "deviceId": [],
        "entityId": [
            "light.office_monitor_light_1",
            "light.office_monitor_light_2"
        ],
        "data": "",
        "dataType": "jsonata",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1100,
        "y": 240,
        "wires": [
            []
        ]
    },
    {
        "id": "09e072677c06aaf0",
        "type": "trigger",
        "z": "8817f468bc7f98b2",
        "name": "Delay",
        "op1": "",
        "op2": "0",
        "op1type": "nul",
        "op2type": "str",
        "duration": "1",
        "extend": true,
        "overrideDelay": true,
        "units": "s",
        "reset": "on",
        "bytopic": "all",
        "topic": "topic",
        "outputs": 1,
        "x": 1030,
        "y": 300,
        "wires": [
            [
                "b2c55f7aabb2d2bc"
            ]
        ]
    },
    {
        "id": "f8bdee5637aa6eb9",
        "type": "function",
        "z": "8817f468bc7f98b2",
        "name": "",
        "func": "var seconds = msg.payload;\nvar delay = seconds * 1000;\n// HH:MM:SS\n// var timer = new Date(seconds * 1000).toISOString().substring(11, 16)\n// MM:SS\nvar timer = new Date(delay).toISOString().slice(11, 19)\n\nmsg.delay = delay;\nmsg.timer = timer\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 920,
        "y": 360,
        "wires": [
            [
                "09e072677c06aaf0",
                "49ef78247fba5c80"
            ]
        ]
    },
    {
        "id": "b2c55f7aabb2d2bc",
        "type": "api-call-service",
        "z": "8817f468bc7f98b2",
        "name": "Turn OFF Monitor Light 1 & 2",
        "server": "dba636d5.d235b8",
        "version": 5,
        "debugenabled": false,
        "domain": "light",
        "service": "turn_off",
        "areaId": [],
        "deviceId": [],
        "entityId": [
            "light.office_monitor_light_1",
            "light.office_monitor_light_2"
        ],
        "data": "",
        "dataType": "jsonata",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1220,
        "y": 360,
        "wires": [
            []
        ]
    },
    {
        "id": "49ef78247fba5c80",
        "type": "api-call-service",
        "z": "8817f468bc7f98b2",
        "name": "",
        "server": "dba636d5.d235b8",
        "version": 5,
        "debugenabled": true,
        "domain": "timer",
        "service": "start",
        "areaId": [],
        "deviceId": [],
        "entityId": [
            "timer.office_automation_presence_lights_timer"
        ],
        "data": "{\"duration\":timer}",
        "dataType": "jsonata",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1040,
        "y": 420,
        "wires": [
            []
        ]
    },
    {
        "id": "dba636d5.d235b8",
        "type": "server",
        "name": "Home Assistant",
        "version": 5,
        "addon": true,
        "rejectUnauthorizedCerts": true,
        "ha_boolean": "y|yes|true|on|home|open",
        "connectionDelay": true,
        "cacheJson": true,
        "heartbeat": false,
        "heartbeatInterval": 30,
        "areaSelector": "friendlyName",
        "deviceSelector": "friendlyName",
        "entitySelector": "friendlyName",
        "statusSeparator": "at: ",
        "statusYear": "hidden",
        "statusMonth": "short",
        "statusDay": "numeric",
        "statusHourCycle": "h23",
        "statusTimeFormat": "h:m",
        "enableGlobalContextStore": true
    }
]
```

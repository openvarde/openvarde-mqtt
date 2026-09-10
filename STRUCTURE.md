# OpenVarde MQTT Structure

This document describes the MQTT topic structure used by OpenVarde services and devices.

The goal is to keep the MQTT namespace predictable, readable and easy to extend as new sensors, services and nodes are added.

## Topic structure

OpenVarde topics follow this general structure:

```text
openvarde/<node>/<service>/<type>
```

Where:

* `openvarde` — root namespace for the project
* `<node>` — the device or node publishing the data
* `<service>` — the sensor, service or subsystem
* `<type>` — the type of message or data

Example:

```text
openvarde/varde01/gps/position
```

## Current topic tree

```text
openvarde/
└── varde01/
    ├── system/
    │   ├── status
    │   └── heartbeat
    │
    └── gps/
        ├── position
        ├── status
        └── alert
```

Additional nodes use the same structure:

```text
openvarde/varde02/...
openvarde/varde03/...
```

## GPS

### Position

```text
openvarde/varde01/gps/position
```

Example payload:

```json
{
  "lat": 59.787981,
  "lon": 5.161365,
  "alt": 19.2,
  "satellites": 7,
  "hdop": 1.0,
  "timestamp": "2026-09-11T00:00:00Z"
}
```

### Status

```text
openvarde/varde01/gps/status
```

Example:

```json
{
  "fix": true,
  "mode": 3,
  "satellites": 7
}
```

### Alert

```text
openvarde/varde01/gps/alert
```

Used when the GPS monitoring service detects an abnormal condition such as unexpected position deviation or loss of a valid fix.

Example:

```json
{
  "type": "position_deviation",
  "severity": "warning",
  "distance": 125.4,
  "timestamp": "2026-09-11T00:00:00Z"
}
```

## System

### Status

```text
openvarde/varde01/system/status
```

Used to indicate the current state of the node.

Example:

```json
{
  "status": "online"
}
```

### Heartbeat

```text
openvarde/varde01/system/heartbeat
```

Periodic message indicating that the node and MQTT connection are alive.

Example:

```json
{
  "timestamp": "2026-09-11T00:00:00Z"
}
```

## Conventions

Topic names should:

* use lowercase characters
* avoid spaces
* use `/` to separate hierarchy levels
* use stable node identifiers such as `varde01`
* describe what the message represents rather than which application consumes it

Applications should subscribe only to the parts of the hierarchy they require.

Examples:

```text
openvarde/varde01/#          # Everything from varde01
openvarde/+/gps/#            # GPS data from all nodes
openvarde/+/gps/alert        # GPS alerts from all nodes
openvarde/+/system/status    # System status from all nodes
```

## Future topics

The namespace is intended to allow additional OpenVarde modules without changing the basic structure.

For example:

```text
openvarde/varde01/environment/temperature
openvarde/varde01/environment/humidity

openvarde/varde01/power/battery
openvarde/varde01/power/voltage

openvarde/varde01/network/status

openvarde/varde01/weather/pressure

openvarde/varde01/radio/status
```

Not all topics listed here are necessarily implemented. This document should be updated as MQTT interfaces are added or changed.

# API Documentation

## Overview
The backend exposes REST endpoints for driver monitoring, recognition, and profile management. All endpoints use JSON responses and accept multipart form data for image uploads.

## Endpoints

### `GET /`
**Purpose:** Health check

Response:
```json
{
  "message": "Smart Vehicle AI Backend Running"
}
```

### `POST /detect-face`
**Purpose:** Analyze a driver frame and return telemetry

Request:
- multipart form field `file`: image file

Response schema: `TelemetryResponse`

Example response:
```json
{
  "driver": {
    "faceDetected": true,
    "faceCount": 1,
    "isDrowsy": false,
    "isYawning": false,
    "isTalking": false,
    "fatigueLevel": "Low",
    "safetyScore": 100,
    "phoneDetected": false,
    "warningCount": 0,
    "emergencyMode": false,
    "recommendedAction": "Continue Driving",
    "attentionStatus": "Focused",
    "headDirection": "Center",
    "lookingAway": false,
    "attentionScore": 96,
    "blinkRate": 12,
    "gazeStability": 92
  },
  "vision": {
    "trackingState": "Locked",
    "meshEnabled": true,
    "meshConfidence": 0.95,
    "pipelineStatus": "Operational",
    "fps": 30,
    "latency": 42
  },
  "vehicle": {
    "riskLevel": "Low",
    "safetyMode": "Monitoring",
    "assistState": "Active"
  },
  "events": [
    {
      "type": "Driver Focus Stable",
      "severity": "monitoring"
    }
  ]
}
```

### `POST /register-driver`
**Purpose:** Register a new driver profile with face embedding and preferences

Request fields (multipart/form-data):
- `name`: string
- `driving_style`: string
- `ac_temperature`: string
- `ambient_mode`: string
- `seat_position`: string
- `assistant_voice`: string
- `file`: face image

Response:
```json
{
  "success": true,
  "message": "Name registered"
}
```

### `POST /recognize-driver`
**Purpose:** Identify a driver from a face image

Request:
- multipart form field `file`: image file

Response:
```json
{
  "matched": true,
  "driver": "John Doe",
  "confidence": 0.92
}
```

### `DELETE /clear-drivers`
**Purpose:** Remove all stored driver profiles

Response:
```json
{
  "success": true,
  "message": "All driver profiles cleared"
}
```

## Telemetry Fields
### `driver`
- `faceDetected`: face was detected in the frame
- `faceCount`: number of faces detected
- `isDrowsy`: drowsiness state
- `isYawning`: yawning detected
- `isTalking`: talking detected
- `fatigueLevel`: low/medium/high
- `safetyScore`: 0-100 safety score
- `phoneDetected`: phone usage detected
- `warningCount`: number of warning stages
- `emergencyMode`: emergency intervention active
- `recommendedAction`: recommended action text
- `attentionStatus`: driver attention state
- `headDirection`: left/right/center
- `lookingAway`: driver is looking away
- `attentionScore`: attention percentile
- `blinkRate`: estimated blinks per minute
- `gazeStability`: percent of center gaze

### `vision`
- `trackingState`: Locked or Lost
- `meshEnabled`: whether face mesh is active
- `meshConfidence`: estimated mesh quality
- `pipelineStatus`: pipeline health
- `fps`: frames per second
- `latency`: processing latency in milliseconds

### `vehicle`
- `riskLevel`: Critical/Warning/Low
- `safetyMode`: Protection/Monitoring
- `assistState`: Intervention/Active

### `events`
- `type`: event name
- `severity`: critical/warning/info/monitoring

## Error Responses
- When no face is detected during recognition or registration, the API returns a message indicating failure.
- For `/recognize-driver`, unknown driver results in `matched: false` and a descriptive message.

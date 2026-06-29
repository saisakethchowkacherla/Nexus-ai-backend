# Backend Documentation

## Overview
This document describes the current backend implementation for the NEXUS AI system. It covers architecture, services, routes, models, detection pipelines, telemetry, event generation, recognition, phone detection, and safety logic.

## Folder Structure
- `app/main.py` - FastAPI application and driver recognition routes
- `app/api/routes/detection.py` - `POST /detect-face` endpoint
- `app/services/face_detection_service.py` - core detection and telemetry pipeline
- `app/services/phone_detection_service.py` - YOLO phone detection service
- `app/services/event_service.py` - AI event generation and cooldown logic
- `app/models/telemetry_models.py` - Pydantic response models
- `database.py` - SQLite driver profile storage and schema
- `face_engine.py` - InsightFace face embedding extraction
- `run.py` - local server startup helper

## FastAPI Architecture
The backend uses FastAPI to expose REST endpoints. CORS is enabled for `http://localhost:5173` to support frontend communication. The API is organized as:
- `app.include_router(detection_router)` to register detection routes
- `/` health check
- `/detect-face` telemetry and driver state
- `/register-driver` driver enrollment
- `/recognize-driver` driver identification
- `/clear-drivers` profile management

## Services
### Face Detection Service
- Uses OpenCV Haar cascade for initial face detection
- Uses MediaPipe FaceMesh for 468 landmark extraction
- Computes drowsiness, attention, gaze stability, head direction, blink rate, fatigue level, safety score, and recommended action
- Detects yawning and talking from mouth landmark distance
- Detects phone usage by passing the camera frame to YOLO

### Phone Detection Service
- Loads YOLOv8 model from `yolov8n.pt`
- Scans each frame for the `cell phone` label
- Returns `True` if any phone is detected

### Event Service
- Builds AI events based on driver state flags
- Supports cooldowns for repeated event types
- Event types include drowsiness, emergency intervention, driver distraction, missing driver, unstable attention, phone usage, and stable focus

### Recognition Service
- Extracts embeddings using InsightFace FaceAnalysis
- Stores embeddings as JSON strings in SQLite
- Matches new images against stored driver embeddings with cosine similarity
- Recognizes driver when similarity exceeds 0.45

## Routes
### `GET /`
Returns a health check message.

### `POST /detect-face`
Accepts an uploaded image file and returns `TelemetryResponse` with driver, vision, vehicle, and event details.

### `POST /register-driver`
Accepts driver metadata plus a face image. Stores the name and serialized embedding with preferences.

### `POST /recognize-driver`
Accepts a face image and identifies the best matching driver from the stored profiles.

### `DELETE /clear-drivers`
Deletes all rows from the `drivers` SQLite table.

## Models
### `DriverTelemetry`
- `faceDetected: bool`
- `faceCount: int`
- `isDrowsy: bool`
- `isYawning: bool`
- `isTalking: bool`
- `fatigueLevel: str`
- `safetyScore: int`
- `phoneDetected: bool`
- `warningCount: int`
- `emergencyMode: bool`
- `recommendedAction: str`
- `attentionStatus: str`
- `headDirection: str`
- `lookingAway: bool`
- `attentionScore: int`
- `blinkRate: int`
- `gazeStability: int`

### `VisionTelemetry`
- `trackingState: str`
- `meshEnabled: bool`
- `meshConfidence: float`
- `pipelineStatus: str`
- `fps: int`
- `latency: int`

### `VehicleTelemetry`
- `riskLevel: str`
- `safetyMode: str`
- `assistState: str`

### `AIEvent`
- `type: str`
- `severity: str`

### `TelemetryResponse`
Contains `driver`, `vision`, `vehicle`, and `events`.

## Detection Pipeline
1. Decode uploaded image bytes to OpenCV BGR image
2. Detect face rectangles with Haar cascade
3. Convert image to RGB and process with MediaPipe FaceMesh
4. Extract landmarks for eyes, nose, mouth, and face edges
5. Detect yawning, talking, head pose, and gaze
6. Compute fatigue and safety scores
7. Call event generator and return structured telemetry

## Telemetry Generation
Telemetry is calculated per frame and includes:
- `fps`
- `latency`
- `blinkRate`
- `gazeStability`
- `attentionScore`
- `safetyScore`
- `riskLevel`
- `safetyMode`
- `assistState`

## Event Generation
Events are generated for:
- Drowsiness Detected
- Emergency Intervention
- Yawning Detected
- Talking Detected
- Driver Distracted
- Driver Not Detected
- Unstable Attention
- Phone Usage Detected
- Driver Focus Stable

Cooldowns prevent alert flooding for repeated conditions.

## Face Recognition
Driver embeddings are generated from the first detected face in an image. Embeddings persist in SQLite and are matched with cosine similarity. Unknown faces return `matched: false` when similarity is below 0.45.

## Phone Detection
Phone usage detection is implemented with YOLOv8 and triggers safety penalties and events when a cell phone is present.

## Safety and Emergency Logic
Safety logic includes:
- reducing safety score for drowsiness, yawning, talking, looking away, missing face, and phone detection
- emergency mode activation when warning counters reach the defined threshold
- recommended action is `Pull Over` when emergency mode is active
- `riskLevel`, `safetyMode`, and `assistState` reflect current driver state

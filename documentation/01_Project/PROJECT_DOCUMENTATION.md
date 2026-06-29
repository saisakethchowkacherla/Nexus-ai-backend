# Backend Project Documentation

## Backend Workflow
The backend pipeline processes driver video frames, analyzes driver state, generates telemetry, and exposes results through REST endpoints. The workflow is:
1. Client uploads a frame to `/detect-face`
2. Image is decoded and faces are detected
3. MediaPipe extracts facial landmarks
4. Driver attention and drowsiness are computed
5. Phone usage is detected with YOLO
6. Safety scores and emergency status are calculated
7. Events are generated
8. Pydantic response is returned to the client

## AI Pipeline
- `OpenCV Haar cascade` identifies face regions
- `MediaPipe FaceMesh` extracts 468 landmarks
- `InsightFace FaceAnalysis` generates normalized embeddings
- `YOLOv8` identifies phone objects in the frame
- Decision engine computes attention, fatigue, and risk
- Event engine applies cooldowns and severity labels

## Emergency Workflow
- Driver state is continuously evaluated
- `warningCount` increases with drowsiness duration
- `emergencyMode` becomes true when warning thresholds are met
- `recommendedAction` updates to `Pull Over`
- Events such as `Emergency Intervention` are emitted

## Telemetry Workflow
Telemetry is packaged in `TelemetryResponse` and includes three categories:
- `driver` state
- `vision` pipeline health
- `vehicle` safety summary

Telemetry supports frontend dashboards and analytics.

## Evaluation Workflow
Evaluation scripts run against the dataset folders in `dataset/`:
- `face_recognition` validates stored driver matching
- `head_pose` validates head direction classification
- `drowsiness` validates eye closure, yawning, and fatigue labels
- `distraction` validates looking away, phone usage, and talking detection
- `lighting_conditions` validates face detection across lighting conditions

Results are written to `evaluation/results/metrics.json` and CSV files.

## Deployment Workflow
1. Install dependencies from `requirements.txt`
2. Place `yolov8n.pt` in the repository root
3. Start the service with `python run.py`
4. The backend serves requests on port `8000`
5. Integrate with frontend at `http://localhost:5173`
6. In production, run with Uvicorn workers or containerize the application


---

# Final Backend Workflow

The backend now follows a modular processing architecture.

## Processing Pipeline

Camera Input

↓

OpenCV Face Detection

↓

MediaPipe Face Mesh

↓

Eye Tracking

↓

Blink Detection

↓

Head Pose Estimation

↓

Mouth Analysis
(Yawning & Talking)

↓

YOLO Phone Detection

↓

Driver Recognition

↓

Attention Engine

↓

Fatigue Engine

↓

Safety Score Engine

↓

Emergency Decision Engine

↓

Telemetry Response

↓

Frontend Dashboard

## Decision Engine

The decision engine combines multiple AI outputs into a single driver safety assessment.

Current decision factors include:

- Face detected
- Driver recognized
- Eyes closed
- Yawning
- Talking
- Phone usage
- Looking away
- Head direction
- Blink rate
- Fatigue level
- Safety score
- Warning counter
- Emergency mode
- Recommended action

The backend exposes all these values through a unified telemetry response consumed by the frontend dashboard.
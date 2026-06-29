# Model Pipeline

## Overview
The backend model pipeline combines OpenCV, MediaPipe, InsightFace, and YOLOv8. It performs face detection, landmark extraction, driver recognition, phone detection, and safety scoring.

## OpenCV Pipeline
- Input image bytes are decoded to OpenCV BGR format
- The face cascade classifier `haarcascade_frontalface_default.xml` detects face rectangles
- A grayscale image is used for fast frontal face detection
- Detection result determines whether the driver is present

## MediaPipe Pipeline
- Converts the BGR image to RGB
- Uses `mp.solutions.face_mesh.FaceMesh` with `refine_landmarks=True`
- Extracts 468 landmarks for the first detected face
- Landmarks used in:
  - eye openness detection
  - mouth opening and yawning detection
  - head direction and gaze analysis
  - blink and gaze stability measurements

## InsightFace Pipeline
- Implemented in `face_engine.py`
- `FaceAnalysis` loads on CPUExecutionProvider
- Processes the input image to return detected faces
- Extracts 512-dimensional face embeddings from the first face
- Embeddings are serialized as JSON and stored in SQLite

## YOLO Phone Detection Pipeline
- Implemented in `app/services/phone_detection_service.py`
- Loads `yolov8n.pt` at startup
- Runs inference on each input frame
- Scans result boxes for the label `cell phone`
- Returns `True` when phone usage is detected

## Decision Engine
- Combines driver state flags into a safety decision
- Input signals include:
  - `is_drowsy`
  - `is_yawning`
  - `is_talking`
  - `looking_away`
  - `phone_detected`
  - `face_detected`
  - `gaze_stability`
- Warning counter logic updates based on drowsiness duration
- Emergency mode becomes active when thresholds are exceeded

## Safety Engine
- Computes `fatigueLevel` from yawning and drowsiness
- Computes `safetyScore` by subtracting penalties for:
  - drowsiness
  - yawning
  - talking
  - looking away
  - missing face
  - phone usage
- Clamps score between 0 and 100
- Determines recommended action: `Continue Driving` or `Pull Over`
- Sets vehicle risk state and assist mode based on detected conditions

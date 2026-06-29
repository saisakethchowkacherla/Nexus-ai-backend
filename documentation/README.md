# NEXUS AI Backend

## Project Overview
NEXUS AI Backend is the Python service layer for a smart vehicle driver monitoring system. It provides real-time driver state analysis, face recognition, phone usage detection, and emergency escalation through a FastAPI REST API.

## Backend Features
- Real-time driver monitoring using MediaPipe Face Mesh
- Face recognition with InsightFace embeddings
- Phone usage detection with YOLOv8
- Drowsiness, distraction, and yawning detection
- Safety score, fatigue estimation, and warning counters
- Emergency mode and recommended action support
- Driver profile registration and recognition
- Telemetry response structure for live dashboards
- SQLite driver profile storage
- Evaluation scripts for accuracy and dataset benchmarking

## Tech Stack
- Python 3.10+
- FastAPI
- Uvicorn
- OpenCV
- MediaPipe
- InsightFace
- ultralytics YOLOv8
- SQLite
- NumPy

## Folder Structure
- `app/` - FastAPI application, API routes, services, models
- `database.py` - SQLite connection and driver table schema
- `face_engine.py` - InsightFace embedding extraction
- `run.py` - Local server startup script
- `evaluation/` - Evaluation scripts and metric results
- `dataset/` - Image dataset for evaluation
- `yolov8n.pt` - YOLO phone detection model weights

## Installation
1. Clone the repository.
2. Create and activate a Python virtual environment.
3. Install dependencies:

```bash
pip install -r requirements.txt
```

## Running Locally
Start the backend service with:

```bash
python run.py
```

The API will be available at `http://0.0.0.0:8000`.

## API Endpoints
- `GET /` - Health check
- `POST /detect-face` - Driver frame analysis and telemetry
- `POST /register-driver` - Register new driver profile
- `POST /recognize-driver` - Recognize stored driver from image
- `DELETE /clear-drivers` - Clear all driver profiles

## Evaluation
Use the evaluation scripts to validate backend AI performance:
- `evaluation/evaluate_face_recognition.py`
- `evaluation/evaluate_head_pose.py`
- `evaluation/evaluate_drowsiness.py`
- `evaluation/evaluate_distraction.py`
- `evaluation/evaluate_lighting.py`
- `evaluation/generate_charts.py`
- `evaluation/generate_csv.py`
- `evaluation/generate_report.py`

## Deployment
Deploy the backend on any host that supports Python and FastAPI. For production, use an ASGI server such as Uvicorn or Gunicorn with workers.

## Future Scope
- Vehicle CAN integration
- WebSocket telemetry streaming
- Cloud deployment with autoscaling
- Mobile companion support
- Advanced fatigue and emotion detection

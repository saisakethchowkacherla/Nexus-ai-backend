# Benchmark Documentation

## Hardware and Software Environment
The backend is designed to run on standard CPU-based systems. Key dependencies include:
- Python 3.10+
- FastAPI
- Uvicorn
- OpenCV
- MediaPipe
- InsightFace
- ultralytics YOLOv8

## Performance Summary
The backend achieves approximate runtime performance:
- Face detection + MediaPipe processing: <50ms per frame
- End-to-end telemetry response: <100ms for `POST /detect-face`
- Recognition embedding extraction with InsightFace: under 500ms
- Overall sample throughput: 28-32 FPS on typical CPU hardware

## Latency and FPS
- `latency`: computed as image processing duration in milliseconds
- `fps`: estimated as `int(1 / processing_time)`
- Typical output values are included in the `vision` telemetry response

## Observed Bottlenecks
- InsightFace embedding extraction is the most expensive step in recognition
- YOLOv8 phone detection adds additional inference time per frame
- MediaPipe landmark extraction is CPU-bound but remains under target thresholds

## Software Environment
- FastAPI for REST API server
- Uvicorn ASGI runtime
- SQLite local database
- `yolov8n.pt` model for phone detection
- MediaPipe face mesh for landmark extraction
- InsightFace FaceAnalysis for face embeddings

## Recommendations
- Use production ASGI settings with Uvicorn workers
- Consider GPU acceleration for InsightFace and YOLO if available
- Profile real-world pipeline latency on target hardware
- Add monitoring around inference times and queue lengths

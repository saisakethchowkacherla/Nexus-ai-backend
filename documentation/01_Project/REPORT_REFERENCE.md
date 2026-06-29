# Report Reference

## Backend Report Summary
The backend supports the NEXUS AI driver monitoring system with the following capabilities:
- Real-time face detection and analysis
- Driver recognition with InsightFace embeddings
- Phone usage detection using YOLOv8
- Drowsiness, distraction, and yawning detection
- Safety score and emergency mode activation
- Structured telemetry response for frontend dashboards
- SQLite profile storage and profile management APIs

## Important Files
- `app/main.py` - API entrypoint and route definitions
- `app/services/face_detection_service.py` - core telemetry and detection logic
- `app/services/phone_detection_service.py` - YOLO phone detection
- `app/services/event_service.py` - event generation logic
- `app/models/telemetry_models.py` - response schemas
- `database.py` - driver storage schema
- `face_engine.py` - InsightFace embedding extraction
- `evaluation/` - evaluation scripts and metrics
- `dataset/` - labeled evaluation images

## Report Topics
- Backend architecture and data flow
- AI model pipelines and inference steps
- API endpoints and response schemas
- Dataset organization and evaluation methodology
- Evaluation results and benchmark summaries
- Design decisions and trade-offs
- Future scope and planned improvements

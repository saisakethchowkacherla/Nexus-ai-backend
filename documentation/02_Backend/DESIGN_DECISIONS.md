# Design Decisions

## Why FastAPI
FastAPI was chosen for its high-performance async REST capabilities, automatic OpenAPI docs, and strong developer ergonomics. It enables clean route definition and easy integration with Pydantic models.

## Why MediaPipe
MediaPipe FaceMesh provides fast and robust 468-point facial landmark detection on CPU. It enables detailed eye, mouth, and head pose analysis without requiring GPU acceleration.

## Why InsightFace
InsightFace is used for driver recognition because it produces reliable 512-dimensional face embeddings and supports CPU execution. It is well-suited for matching driver profiles from a small on-device database.

## Why YOLO only for phone detection
YOLOv8 is used specifically for detecting cell phones because object detection is the right tool for physical object presence. Using YOLO for only phone detection keeps the pipeline focused and avoids overloading the face analysis path.

## Why polling style API
The backend is designed as a request-response REST API. This simplifies integration with the frontend and allows telemetry to be generated on demand for each frame without holding persistent socket state.

## Why current backend architecture
The architecture keeps responsibilities separated:
- `app/main.py` handles route registration and profile APIs
- `face_detection_service.py` handles vision analytics and telemetry
- `event_service.py` handles alert creation logic
- `phone_detection_service.py` handles phone object detection
- `face_engine.py` handles driver embedding extraction
- `database.py` manages SQLite storage

This structure is maintainable, easy to extend, and conforms to standard Python modular design.

## Performance Trade-offs
- OpenCV Haar cascade is used for fast face detection instead of heavier deep models
- Only the first detected face is processed to reduce compute
- Event cooldowns prevent repeated processing of the same alert
- YOLO phone detection is run per frame, which adds compute but improves safety accuracy

## Future Migration Possibilities
- Migrate to WebSockets for live telemetry streaming
- Replace SQLite with a more scalable database for larger deployments
- Add GPU acceleration for InsightFace and YOLO inference
- Move from polling to event-driven telemetry ingestion
- Add a separate model service for recognition and detection inference

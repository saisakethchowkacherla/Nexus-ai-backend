# Presentation Notes

## Objectives
- Showcase backend capabilities for real-time driver monitoring
- Explain how the system detects drowsiness, distraction, and phone usage
- Describe driver recognition and safety scoring
- Present emergency intervention and recommended action logic
- Highlight evaluation results and areas for improvement

## Architecture
- FastAPI backend with modular services
- OpenCV + MediaPipe for driver state analysis
- InsightFace for face recognition
- YOLOv8 for phone detection
- SQLite for driver profile storage

## AI Workflow
1. Frame upload to `/detect-face`
2. Face detection and landmark extraction
3. Drowsiness and attention scoring
4. Phone detection and safety scoring
5. Event generation
6. Telemetry response returned to frontend

## Backend Features
- Driver recognition and registration
- Real-time face and attention analysis
- Phone usage and talking detection
- Fatigue level estimation and safety score
- Emergency mode and pull over recommendation
- Structured telemetry for dashboard consumption

## Evaluation
- Face recognition: 100% on current dataset
- Lighting conditions: 100% detection rate
- Head pose: 83.33% overall
- Distraction: phone usage 82.61%, looking away 31.25%
- Drowsiness: yawning 44.44%, fatigued 38.89%

## Limitations
- Fatigue and yawning detection need tuning
- Looking-away detection needs more dataset coverage
- Head pose classification is weaker for forward-facing examples
- Real-world data may differ from evaluation dataset

## Future Scope
- Add emotion detection and road context awareness
- Move to WebSockets for live telemetry
- Support vehicle CAN data and mobile apps
- Add GPU acceleration and scalable databases

## Key Messages
- The backend provides a complete safety telemetry API
- It supports both recognition and real-time driver state monitoring
- The current implementation is strong in recognition and lighting robustness
- The system still needs refinement in fatigue and distraction edge cases

---

## Final Demonstration Flow

Driver Enters Vehicle

↓

Face Recognition

↓

Load Driver Preferences

↓

Continuous Driver Monitoring

↓

Telemetry Dashboard

↓

Warning Level 1

↓

Warning Level 2

↓

Emergency Overlay

↓

Parking Assist Activation

↓

Recommended Action

↓

Driver Recovery

↓

Continue Monitoring

The complete demonstration showcases the full AI safety lifecycle from driver recognition to emergency intervention.
# Dataset Documentation

## Organization
The backend dataset is organized by evaluation category.

```
dataset/
├── distraction/
│   ├── focused/
│   ├── looking_away/
│   ├── phone_usage/
│   └── talking/
├── drowsiness/
│   ├── eyes_closed/
│   ├── eyes_open/
│   ├── fatigued/
│   └── yawning/
├── face_recognition/
│   ├── ram/
│   ├── saketh/
│   └── unknown_driver/
├── head_pose/
│   ├── down/
│   ├── forward/
│   ├── left/
│   └── right/
└── lighting_conditions/
    ├── daylight/
    ├── indoor/
    ├── low_light/
    └── night/
```

## Image Categories
- `distraction` evaluates looking away, phone usage, and talking detection.
- `drowsiness` evaluates eye closure, yawning, and fatigue state.
- `face_recognition` evaluates driver recognition accuracy for known and unknown drivers.
- `head_pose` evaluates left/right/center classification.
- `lighting_conditions` evaluates face detection robustness under different lighting.

## Evaluation Datasets
Each evaluation script uses its matching dataset folder:
- `evaluation/evaluate_face_recognition.py` → `dataset/face_recognition`
- `evaluation/evaluate_head_pose.py` → `dataset/head_pose`
- `evaluation/evaluate_drowsiness.py` → `dataset/drowsiness`
- `evaluation/evaluate_distraction.py` → `dataset/distraction`
- `evaluation/evaluate_lighting.py` → `dataset/lighting_conditions`

## Dataset Statistics
Based on current evaluation results, the dataset provides:
- full support for known driver recognition and unknown driver detection
- head pose cases for forward, left, and right orientations
- drowsiness examples for closed eyes, yawning, fatigued, and open eyes
- distraction cases with phone, talking, looking away, and focused driving
- lighting scenarios covering daylight, indoor, low light, and night

## Notes
- `unknown_driver` images are evaluated separately to verify recognition rejection.
- The dataset structure is designed for quick evaluation of backend detection modules.

# Results Documentation

## Evaluation Summary
The backend evaluation results are stored in `evaluation/results/metrics.json` and corresponding CSV files. Current results indicate:
- Face recognition: 100.0% overall
- Lighting detection: 100.0% overall
- Head pose: 83.33% overall
- Distraction overall varies by category, with phone usage at 82.61%
- Drowsiness detection has lower accuracy for fatigued and yawning cases

## Accuracy Tables
### Face Recognition
- `ram`: 100.0%
- `saketh`: 100.0%
- `unknown_driver`: 100.0%
- `overall`: 100.0%

### Drowsiness
- `eyes_open`: 100.0%
- `eyes_closed`: 100.0%
- `fatigued`: 38.89%
- `yawning`: 44.44%

### Distraction
- `focused`: 100.0%
- `looking_away`: 31.25%
- `phone_usage`: 82.61%
- `talking`: 100.0%

### Head Pose
- `forward`: 75.0%
- `left`: 95.0%
- `right`: 80.0%
- `overall`: 83.33%

### Lighting
- `daylight`: 100.0%
- `indoor`: 100.0%
- `low_light`: 100.0%
- `night`: 100.0%
- `overall`: 100.0%

## Observations
- Face recognition is performing strongly for the current known profiles.
- Lighting conditions are handled robustly by the detection pipeline.
- Drowsiness categories for fatigued and yawning need improvement.
- Looking-away detection is the most fragile category in distraction evaluation.
- Head pose classification performs best for left-facing and right-facing examples.

## Strengths
- High accuracy for face recognition and lighting variance
- Stable performance for talking and focused distraction cases
- Clear structured telemetry output for downstream systems

## Limitations
- Current backend metrics expose areas needing tuning in fatigue and yawning
- Head pose evaluation is less accurate for forward-facing images
- Distraction detection is reliable for some categories but weak for looking-away
- Event generation may require threshold adjustment based on real-world data

## Research Conclusions
The backend is strong in recognition and lighting robustness, while fatigue and advanced distraction detection still require refinement. Future tuning should focus on yawning, fatigued state detection, and head pose stability for center-facing images.

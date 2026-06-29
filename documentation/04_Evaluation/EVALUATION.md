# Evaluation Documentation

## Evaluation Methodology
The backend includes standalone Python scripts for AI evaluation. Each script loads images from a dataset folder, sends them through `process_driver_frame`, and compares the output with expected class labels.

## Evaluation Scripts
- `evaluation/evaluate_face_recognition.py` - measures driver matching accuracy against stored driver embeddings
- `evaluation/evaluate_head_pose.py` - measures head direction classification for forward, left, and right images
- `evaluation/evaluate_drowsiness.py` - measures drowsiness detection, yawning detection, and fatigue estimation
- `evaluation/evaluate_distraction.py` - measures distraction detection, phone usage detection, and talking detection
- `evaluation/evaluate_lighting.py` - measures face detection robustness under different lighting conditions

## Metrics
### Current evaluation metrics from `evaluation/results/metrics.json`
- Face recognition: 100.0% overall
- Drowsiness:
  - eyes_open: 100.0%
  - eyes_closed: 100.0%
  - fatigued: 38.89%
  - yawning: 44.44%
- Distraction:
  - focused: 100.0%
  - looking_away: 31.25%
  - phone_usage: 82.61%
  - talking: 100.0%
- Head pose:
  - forward: 75.0%
  - left: 95.0%
  - right: 80.0%
  - overall: 83.33%
- Lighting conditions: 100.0% overall

## Charts and Reports
- `evaluation/generate_charts.py` generates visualization charts from the results
- `evaluation/generate_csv.py` converts the JSON metrics to CSV files
- `evaluation/generate_report.py` produces a report summary
- Evaluation output files are stored in `evaluation/results/`

## Benchmark Support
The evaluation folder is designed to support benchmarking of accuracy, dataset coverage, and model robustness across categories.

## Performance Observations
- Face recognition is stable for the current stored profiles
- Lighting conditions show robust face detection across day and night scenarios
- Drowsiness yawning and fatigue detection require further tuning for higher accuracy
- Distraction detection has strong performance on talking and phone usage, but looking-away detection is lower in the current dataset

## Limitations
- Current metrics are dataset-dependent and may not generalize
- `fatigued` and `yawning` detection is currently under 50% on the evaluation dataset
- Head pose accuracy is lower for forward-facing cases compared to left/right
- Phone detection depends on YOLOv8 performance and dataset quality

# Smoothed Person Detection with YOLO11 and OpenCV

A Python computer-vision demonstration that runs YOLO11 person detection/tracking on a webcam or video file, applies configurable confidence and size filters, and delays drawing a track until its ID has persisted across multiple frames.

This is a portfolio/learning project. The default thresholds favor larger, closer detections and may reject distant people; accuracy and speed have not been benchmarked on a labeled dataset.

## Features

- Ultralytics YOLO11 nano model (\`yolo11n.pt\`)
- Person-class filtering
- Confidence threshold
- Tracker IDs via Ultralytics tracking
- Three-frame persistence before rendering a track
- Optional Gaussian blur before inference
- MP4 output for processed frames
- Local webcam or video-file input

## Workflow

\`\`\`text
Webcam or input video
  -> optional Gaussian blur
  -> YOLO11 tracking
  -> person/confidence filter
  -> minimum box size and area filter
  -> persistence check
  -> annotated MP4
\`\`\`

## Requirements

- Python 3.10 or newer recommended
- Webcam for live capture, or a local video file
- Desktop environment with OpenCV GUI support for the default preview

## Install

\`\`\`bash
git clone https://github.com/nyalaman2085/smoothed-yolo11-person-detector.git
cd smoothed-yolo11-person-detector
python -m venv .venv
# macOS / Linux
source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
\`\`\`

The model weights are downloaded by Ultralytics when needed and when network access is available.

## Run

### Webcam

\`\`\`bash
python detector.py
\`\`\`

Press \`q\` in the preview window to stop. The default output is \`output_smoothed.mp4\`.

### Video file

\`\`\`bash
python -c "from detector import run_detector; run_detector('input.mp4', 'output_smoothed.mp4')"
\`\`\`

Use a different output filename for each run if you want to keep prior results.

## Configuration

Edit the constants in \`detector.py\`:

| Setting | Default | Effect |
|---|---:|---|
| \`MODEL_PATH\` | \`yolo11n.pt\` | Model weights path |
| \`THRESHOLD\` | \`0.65\` | Minimum confidence |
| \`PERSISTENCE_FRAMES\` | \`3\` | Consecutive track sightings required |
| Minimum width | 80 px | Reject smaller boxes |
| Minimum height | 120 px | Reject shorter boxes |
| Minimum frame-area ratio | 3% | Reject boxes occupying less of the frame |

The minimum-size and area filters are resolution- and distance-sensitive. Tune them using representative videos and document the trade-off between missed detections and false positives.

## Validation before making performance claims

Test varied lighting, distance, motion, and occlusion. Record precision/recall against manually labeled frames, ID switches, false positives, missed detections, and frames per second. The repository does not claim measured accuracy or production readiness without this evaluation.

## Limitations

- Uses pretrained general-purpose weights; no custom training dataset is included.
- The default filters may reject people who appear small or far away.
- Track IDs can change after occlusion or tracker resets.
- Gaussian blur may suppress some image details; compare blurred and unblurred runs rather than assuming it improves results.
- OpenCV preview windows are unsuitable for a headless server without an explicit no-GUI mode.
- This demo is not a production surveillance, safety, or occupancy system.

## Technical summary

Python · OpenCV · Ultralytics YOLO11 · object tracking · bounding-box filtering · video processing

## License

Add a license file before inviting reuse or contributions. Check the licenses for the model and dependencies separately.

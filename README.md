# Yalla Play! Python AI judge

The second way to connect the AI judge: process the video on a laptop and import a signed report into the app.

## Install

```
pip install opencv-python numpy
pip install ultralytics      # optional, football only: YOLOv8n person + ball detection with ByteTrack
```

Install ffmpeg too (https://ffmpeg.org). The judge uses it to read the sound track for whistles and the GO beep. Without it, the video is still judged but sound is skipped.

## Run

1. In the app, open the AI judge step, add the video, and mark the goals / lanes / tray spots.
2. Open **Use the Python AI judge instead**, note the judge code (e.g. `YP-ABCD-1234`) and download `yalla_config.json`.
3. Run:

```
python yalla_judge.py football match.mp4 --config yalla_config.json --key YP-ABCD-1234
python yalla_judge.py football match.mp4 --config yalla_config.json --key YP-ABCD-1234 --yolo
python yalla_judge.py dhow race1.mp4 --config yalla_config.json --key YP-ABCD-1234
python yalla_judge.py karamah turn1.mp4 --config yalla_config.json --key YP-ABCD-1234
```

4. Import the `*_yalla_report.json` it writes, using the same step in the app.

## How the app knows the report is genuine

The report's `payload` is signed with HMAC-SHA256 using the judge code as the key. The app recomputes the signature and refuses the report if a single character was changed. It also compares the SHA-256 fingerprint of the video in the report with the video loaded in the app.


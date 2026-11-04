# Plan for "Measure It"

**Goal:** Build an end‑to‑end computer‑vision pipeline that, given a single photo of a physical object, reports the object's real width and height in millimetres.

## 8‑hour breakdown (high‑level)

| Phase | Estimated time | Tasks |
|-------|----------------|-------|
| **1. Planning & Repo Init** | 30‑45 min | - Pick object & reference<br>- Print & measure calibration board<br>- Create GitHub repo, `dev-test01` branch, folder structure<br>- Write & commit `docs/Plan.md`, `docs/Objectives.md`, `docs/DefinitionOfDone.md`, and dataset split list (no code yet) |
| **2. Camera Calibration** | 1 h | - Lock camera focus/zoom/resolution<br>- Capture ≥20 checkerboard (or ChArUco) images<br>- Implement `calibration/calibrate.py` (OpenCV `calibrateCamera`)
- Save `camera_params.json` with matrix, distortion coeffs, reprojection error |
| **3. Data Collection & Labelling** | 1.5‑2 h | - Capture ≥70 images of target object + reference using calibrated camera<br>- Undistort images, store in `dataset/images/`
- Annotate object & reference masks (CVAT/Label Studio) → COCO JSON in `dataset/annotations/`
- Create fixed 70/20/10 train/val/test split file (`dataset/split_list.json`) |
| **4. Model Selection & Training** | 1.5‑2 h | - Choose an allowed segmentation architecture (e.g., U‑Net with ResNet‑34 backbone via `segmentation_models.pytorch`)
- Write `models/train.py` with reproducible seeds
- Train on train split, tune on val, evaluate on test (IoU, mAP@0.5, mAP@0.5:0.95, precision, recall) |
| **5. Inference & Measurement Engine** | 1.5 h | - `inference/predict.py`:
  1. Load camera params → undistort image
  2. Run segmentation → masks for object & reference
  3. Compute scaling factor using known reference size → convert object bounding box to mm
  4. Handle missing object/reference with clear refusal messages
- Produce mask overlay with width, height, confidence |
| **6. Ground‑Truth Verification** | 1 h | - Measure ≥10 test instances with caliper
- Compare raw vs. undistorted images, report MAE & MAPE
- Stress‑test a breaking condition (e.g., camera tilt) and log error degradation |
| **7. Documentation & PR** | 45 min | - Finish `README.md` (setup, hardware, dataset summary, model SHA‑256)
- Update `docs/DefinitionOfDone.md` with evidence links
- Open PR from `dev-test01` → `main` |
| **8. Loom Video (Gatekeeper)** | 30 min | - Record 5‑min video covering live measurement, code walk‑through, and the four technical questions (K1‑K4) |

## Decision Record (to be filled later)
- **Rejected approaches:** Ultralytics YOLO (license & client restriction), Roboflow hosted models (cloud‑only, prohibited).
- **Cost of rejection:** Lost potential speed gains, but ensured full open‑source compliance and reproducibility.
- **Future considerations:** If time permits, experiment with DeepLabV3+ for higher IoU.

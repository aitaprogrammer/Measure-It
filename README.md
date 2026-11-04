# Measure It

A repository for building, calibrating, and evaluating measurement tooling — from data
collection and model training to inference and downstream measurement pipelines.

## Project Layout

```
.
├── calibration/     # Sensor / device calibration routines and reference data
├── dataset/         # Raw and processed datasets used for training & eval
├── models/          # Trained model weights, checkpoints, and model configs
├── inference/       # Serving / inference code and entrypoints
├── measurement/     # Downstream measurement utilities and analysis scripts
├── docs/            # Project documentation, notes, and design docs
├── README.md        # This file
└── requirements.txt # Python dependencies
```

## Getting Started

```bash
pip install -r requirements.txt
```

See [docs/](docs/) for detailed notes on each subsystem.

## License

Add license information here.
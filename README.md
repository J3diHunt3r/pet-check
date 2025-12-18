# pet-check

A Flask-based backend API for pet identification and recognition using machine learning.

## Features

- Pet detection using YOLOv5
- Dog face recognition using DogFaceNet-inspired architecture
- Snout feature extraction and matching
- Firebase integration for data storage

## Setup

1. Install dependencies:
```bash
pip install -r requirements.txt
```

2. Run the setup script (optional, for ML enhancements):
```bash
python setup_ml_enhancements.py
```

3. Configure Firebase credentials:
   - Place your `firebase-credentials.json` in the project root
   - Note: This file is gitignored for security

4. Start the server:
```bash
python app.py
```

## Model Files

**Important:** The application does NOT require local model files to be committed to git. All models are loaded automatically:

- **YOLOv5**: Automatically downloaded from PyTorch Hub on first run
- **DogFaceNet**: Model architecture is created from scratch using pre-trained ResNet50 weights (downloaded automatically)
- **SSD MobileNet**: Not used by the application

The `models/` directory is gitignored. If you have local model files, they will remain on your machine but won't be tracked by git.

## API Endpoints

- `POST /scanFace` - Scan and extract face features
- `POST /storeSnout` - Store pet snout data
- `POST /identifyPet` - Identify a pet from an image

## Requirements

- Python 3.8+
- See `requirements.txt` for full dependency list

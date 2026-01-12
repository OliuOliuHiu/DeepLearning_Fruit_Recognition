# DLBA Fruit & Veggie Classifier

FastAPI + React app that classifies images into fruit or vegetable, stores predictions in MongoDB, and shows them in a dashboard.

---
## Overview

- **Backend**: FastAPI + TensorFlow model loader (supports fruit/vegetable tagging, history, analytics).
- **Frontend**: React + Vite UI for single/batch uploads, history, and analytics.
- **Database**: MongoDB (local container by default).
- **Docker**: everything starts with `docker compose up`.

---
## System Architecture

Below is a full-stack System Architecture: 

![System Architecture](system_architecture.png)

> User uploads an image from the front-end → The FastAPI back-end (running inside Docker) receives and processes the image → CNN models (TensorFlow – MobileNetV2) inside the container are invoked to generate predictions → The prediction result is stored in MongoDB (also containerized) → The front-end fetches and visualizes results and analytics.
---
## Project structure
```
dlba/
├── back-end/
│   ├── main.py                 # FastAPI routes
│   ├── model.py                # Model loading + inference helpers
│   ├── database.py             # MongoDB utilities
│   ├── requirements.txt
│   └── scripts/
│       └── download_model.py   # Optional helper to fetch model weights
│
├── front-end/
│   ├── src/
│   │   ├── App.tsx
│   │   ├── config.ts           # Reads API base URL from env
│   │   ├── components/         # UI building blocks
│   │   └── pages/              # Predict, Batch upload, History, Dashboard…
│   ├── package.json
│   └── Dockerfile
│
├── model/
│   ├── .gitkeep                # placeholder – add your .h5 + .labels.txt here
│   └── (your_model_files)
│
├── docker-compose.yml
├── fruit_vegetable_recognition_train_model.ipynb
├── link_dataset_model.txt      # link model h5 and data used for train test model
├── README.md
└── REBUILD_DOCKER.md
```
---
## Run everything with Docker Compose
```bash
# 1. Clone the repository
git clone https://github.com/OliuOliuHiu/DeepLearning_Fruits_Vegetables_Recognition.git
cd dlba

# 2. Ensure model files are present in ./model (see section above)

# 3. Build and start all services
docker compose up --build
```
---
## Prerequisites
- Docker Engine 24+ and Docker Compose v2
- Git
- TensorFlow `.h5` model (not stored in the repo)
- Optional: accompanying `.labels.txt` file for friendly class names
---
## Prepare the model assets
1. Place your trained model inside `model/` (same folder as `docker-compose.yml`):
   ```
   model/
     └── fruit_classifier_mobilenetv2.h5
   ```
2. (Optional but recommended) add a label file so predictions display real names:
   ```
   model/
     ├── fruit_classifier_mobilenetv2.h5
     └── fruit_classifier_mobilenetv2.labels.txt  # one label per line, in model order
   ```
3. The repo keeps `model/.gitkeep` so the folder exists even without weights.

> **Need to download the model automatically?**  
> Provide a `MODEL_URL` when deploying and run `python back-end/scripts/download_model.py` during build.
---

# 🛡️ Visual Detection of Deceptive Clickbait UI Elements in Websites (Clickbait Detector)

## 1. Project Overview

Modern websites increasingly use visually deceptive UI elements — fake download buttons, misleading pop-ups, and system-like security messages — to manipulate users into unintended interactions. Existing browser-level protections rely on URL analysis, HTML inspection, or rule-based blocking, which fail to detect deceptions rooted in **visual appearance and layout manipulation**.

This project builds a **computer vision-driven pipeline** that:
1. Captures a screenshot of any webpage through a Chrome browser extension.
2. Preprocesses and analyzes the screenshot using deep learning.
3. Classifies the page and highlights deceptive UI elements in real-time.

> **Key Differentiator:** The system detects deception through **visual features only** (color contrast, spatial layout, button geometry, brand impersonation) — not through URL or text analysis.

## 2. Key Features

- **Chrome Extension Integration**: Real-time screenshot capture and on-page visual alerts.
- **Hybrid Architecture**: Combines Spatial Deep Learning (YOLOv8) with Classical Computer Vision Validation (OpenCV).
- **Expanded ROI Analysis**: Measures edge density to differentiate messy ad spaces from clean UI padding.
- **Page-Level Color Outliers**: Uses K-Means Clustering to flag buttons with massive Euclidean color outliers in LAB color space.
- **Internal Complexity**: Uses Harris Corner Detection to count corners inside buttons (fake buttons pack high complexity).
- **Contrast & Saturation Validation**: Employs CLAHE / HSV to measure local contrast and vividness.

## 3. Tech Stack

- **Browser Extension**: JavaScript, Chrome API, HTML/CSS.
- **Backend Server**: Python, Flask, Flask-CORS.
- **Deep Learning Frameworks**: PyTorch, Torchvision, Ultralytics (YOLOv8), timm.
- **Computer Vision**: OpenCV, Albumentations, Pillow.
- **Data & Analysis**: NumPy, Scikit-Learn, Matplotlib, Seaborn, Jupyter.
- **Testing**: Pytest.

## 4. System Architecture

![System Architecture](diagram.png)

## 5. How It Works

This project uses a two-stage pipeline to detect deceptive UI:

1. **Spatial Deep Learning (YOLOv8)**
   - Scans the webpage screenshot to identify the structural boundaries of all UI elements (Buttons, Banners).
   
2. **Classical Computer Vision Validation (OpenCV)**
   - Filters the detected elements to separate *Real* elements from *Fake/Clickbait* elements using pure mathematical Image Processing algorithms.

## 6. Project Folder Structure

```
min_vision_project/
├── CLAUDE.md              # Detailed design and phase planning
├── README.md              # Project documentation
├── requirements.txt       # Python dependencies
├── data/                  # Raw and processed datasets
├── extension/             # Chrome browser extension code
├── models/                # Trained YOLOv8 models & checkpoints
├── notebooks/             # Data exploration and training notebooks
├── scripts/               # Standalone utility scripts (data cleaning, etc.)
├── src/                   # Core pipeline code (preprocessing, server, training)
└── tests/                 # Unit and integration tests
```

## 7. Installation & Setup Guide

### Prerequisites
- Python 3.10+
- Google Chrome

### Backend Setup
1. Clone the repository and navigate into it.
2. Create and activate a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
4. Start the Flask backend:
   ```bash
   python -m src.server.app
   ```
   *(Note: Ensure the Flask server is running on `http://localhost:5000`. For local testing without a trained checkpoint, the server will start with an untrained model.)*

### Extension Setup
1. In Chrome, open Extensions (`chrome://extensions`).
2. Enable **Developer mode** (top right toggle).
3. Click **Load unpacked** and select the `extension/` folder from the project root.

## Screenshots / Demo

![Project Website Preview](website1.png)

## 8. Usage

1. Open any web page in Chrome.
2. Click the Clickbait Detector extension icon.
3. Click **Re-scan This Page** in the popup. The extension captures a screenshot and sends it to the backend.
4. Results are shown in the popup (label, confidence). If the page is detected as `phishing` or `clickbait`, an in-page alert banner will appear and deceptive elements will be highlighted.

## 9. API Endpoints

- `POST /analyze`: Receives a base64 encoded screenshot and URL from the Chrome extension, processes the image through the YOLOv8 + OpenCV pipeline, and returns bounding boxes, predicted label (`legitimate`, `phishing`, `clickbait`), and confidence score.

## 10. Training & Evaluation

**Train Model:**
```bash
python -m src.training.train_yolo
```

**Get Evaluation Metrics:**
```bash
python -m src.training.evaluate_yolo
```

## 11. Testing

Run the test suite using pytest:
```bash
python -m pytest tests/ -v
```

## 12. Contributors

- M.P.S. Koshala (EG/2021/4617)
- W.A.L.N. Wanigasooriya (EG/2021/4848)
- D.M.D.P. Dassanayaka (EG/2021/4456)
- K.N.Y. Kumara (EG/2021/4624)

**Course:** EE7204 / EC7205 — Image Processing & Computer Vision  
**Department:** Electrical and Information Engineering, University of Ruhuna  

## 13. Acknowledgements

- Dataset: [Kaggle Phishing Sites Screenshot](https://www.kaggle.com/datasets/zackyzac/phishing-sites-screenshot/data)
- YOLO Dataset Annotations: [Roboflow Workspace](https://app.roboflow.com/join/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ3b3Jrc3BhY2VJZCI6Im5BbTB4Vno5Qm9XWkpWc0t0eWwzZmg3SmVSdDEiLCJyb2xlIjoib3duZXIiLCJpbnZpdGVyIjoibmltZXNoYXlhc2l0aEBnbWFpbC5jb20iLCJpYXQiOjE3ODA1NjE5NTh9.NJ1vhemCN0rZWt6RHJvbJvHM-WqmnTViryljDIbsyQc)
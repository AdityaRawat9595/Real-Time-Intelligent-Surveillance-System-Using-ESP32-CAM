# Real-Time Intelligent Surveillance System Using ESP32-CAM

This repository contains the implementation of a **hybrid, two-tier face recognition pipeline** designed for edge-based surveillance. By leveraging the low-cost **ESP32-CAM** for data acquisition and a host-side hybrid algorithm (Fisherface + FaceNet), the system achieves high accuracy without requiring expensive GPUs or cloud-based processing.

---

## Project Overview
The system migrates intelligence to the edge, providing a scalable, low-maintenance solution for smart homes, offices, and campuses. It is designed to overcome the limitations of traditional CCTV systems by offering real-time identification and automated responses.

### Key Features
* **Two-Tier Recognition:** Uses Fisherface for lightning-fast initial passes and FaceNet for deep-level verification.
* **Hardware Efficiency:** Optimized for the **ESP32-CAM**, an inexpensive microcontroller that integrates an OV2640 image sensor and Wi-Fi.
* **Real-Time Feedback:** Overlays bounding boxes, recognized names, and confidence scores directly onto the live stream.
* **Privacy-First:** Edge processing allows raw images to be discarded locally after feature extraction.

---

## System Architecture
The system follows a strategic multi-stage processing pipeline:

1.  **Face Detection (Haar Cascade):** Operates as a "pre-filter" to locate faces in the video frame with high speed and low memory usage.
2.  **Tier 1 - Fisherface (Fast Checker):** A classical algorithm using Linear Discriminant Analysis (LDA) that provides results in under one millisecond on a standard CPU.
3.  **Tier 2 - FaceNet (Deep Thinker):** A modern deep-learning model that generates 128-dimensional embeddings to handle difficult lighting or angled faces.

---

## Performance Metrics
The hybrid approach ensures the system stays responsive while maintaining high reliability:
* **Accuracy**: 95% – 97%.
* **Precision**: 93% – 95%.
* **Recall**: 91% – 94%.
* **Combined Latency**: Stays below 150 ms, making it suitable for interactive systems.

---

## Installation & Setup

### 1. Hardware Requirements
* **ESP32-CAM Module**: Primary data acquisition hardware.
* **USB-to-Serial Adapter**: Required to program and upload code to the module.
* **Host System**: A computer (e.g., MacBook M2 or equivalent) to run the recognition pipeline.

### 2. Software Prerequisites
* **Arduino IDE**: Used to load the camera firmware.
* **Python 3**: Core language for the recognition logic.
* **Key Libraries**: OpenCV, Keras-FaceNet, TensorFlow, and Scikit-learn.

### 3. Setup Steps
1.  **Flash ESP32-CAM**: Upload the firmware via Arduino IDE to configure the module as a Wi-Fi IP Camera.
2.  **Capture Dataset**: Collect images for each class (e.g., Vihan, Mansi, Aditya) and a diverse "Unknown" class to reduce false positives.
3.  **Train Model**: Train the Fisherface classifier and generate pre-computed FaceNet embeddings.
4.  **Run Recognition**: Start the Python script to stream the MJPEG feed from the ESP32 IP address and perform real-time identification.

---

## Decision Logic
The system applies a specific decision hierarchy:
* **Check FaceNet First**: If cosine similarity is $\geq 0.50$, the match is accepted immediately.
* **Fall back to Fisherface**: If FaceNet is below 0.50, the system accepts a match if the Fisherface distance is $\leq 5500$.
* **Confidence Floor**: To avoid errors, if the overall certainty is below 65%, the person is labeled as "Unknown".

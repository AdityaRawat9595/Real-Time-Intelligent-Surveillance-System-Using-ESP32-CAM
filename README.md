# Real-Time Intelligent Surveillance System Using ESP32-CAM

[cite_start]This repository contains the implementation of a **hybrid, two-tier face recognition pipeline** designed for edge-based surveillance[cite: 2, 68]. [cite_start]By leveraging the low-cost **ESP32-CAM** for data acquisition and a host-side hybrid algorithm (Fisherface + FaceNet), the system achieves high accuracy without requiring expensive GPUs or cloud-based processing[cite: 176, 181, 401, 406].

---

## Project Overview
[cite_start]The system migrates intelligence to the edge, providing a scalable, low-maintenance solution for smart homes, offices, and campuses[cite: 101, 104]. [cite_start]It is designed to overcome the limitations of traditional CCTV systems by offering real-time identification and automated responses[cite: 105, 107].

### Key Features
* [cite_start]**Two-Tier Recognition:** Uses Fisherface for lightning-fast initial passes and FaceNet for deep-level verification[cite: 62, 137].
* [cite_start]**Hardware Efficiency:** Optimized for the **ESP32-CAM**, an inexpensive microcontroller that integrates an OV2640 image sensor and Wi-Fi[cite: 102, 122].
* [cite_start]**Real-Time Feedback:** Overlays bounding boxes, recognized names, and confidence scores directly onto the live stream[cite: 232, 296].
* [cite_start]**Privacy-First:** Edge processing allows raw images to be discarded locally after feature extraction[cite: 112, 363].

---

## System Architecture
[cite_start]The system follows a strategic multi-stage processing pipeline[cite: 791]:

1.  [cite_start]**Face Detection (Haar Cascade):** Operates as a "pre-filter" to locate faces in the video frame with high speed and low memory usage[cite: 696, 709].
2.  [cite_start]**Tier 1 - Fisherface (Fast Checker):** A classical algorithm using Linear Discriminant Analysis (LDA) that provides results in under one millisecond on a standard CPU[cite: 139, 142, 197].
3.  [cite_start]**Tier 2 - FaceNet (Deep Thinker):** A modern deep-learning model that generates 128-dimensional embeddings to handle difficult lighting or angled faces[cite: 65, 147, 150].

---

## Performance Metrics
[cite_start]The hybrid approach ensures the system stays responsive while maintaining high reliability[cite: 68, 203]:
* [cite_start]**Accuracy**: 95% – 97%[cite: 905].
* [cite_start]**Precision**: 93% – 95%[cite: 905].
* [cite_start]**Recall**: 91% – 94%[cite: 905].
* [cite_start]**Combined Latency**: Stays below 150 ms, making it suitable for interactive systems[cite: 402, 994].

---

## Installation & Setup

### 1. Hardware Requirements
* [cite_start]**ESP32-CAM Module**: Primary data acquisition hardware[cite: 256, 257].
* [cite_start]**USB-to-Serial Adapter**: Required to program and upload code to the module[cite: 633].
* [cite_start]**Host System**: A computer (e.g., MacBook M2 or equivalent) to run the recognition pipeline[cite: 447, 522].

### 2. Software Prerequisites
* [cite_start]**Arduino IDE**: Used to load the camera firmware[cite: 437, 443].
* [cite_start]**Python 3**: Core language for the recognition logic[cite: 453, 454].
* [cite_start]**Key Libraries**: OpenCV, Keras-FaceNet, TensorFlow, and Scikit-learn[cite: 463, 471, 479, 495].

### 3. Setup Steps
1.  [cite_start]**Flash ESP32-CAM**: Upload the firmware via Arduino IDE to configure the module as a Wi-Fi IP Camera[cite: 443].
2.  [cite_start]**Capture Dataset**: Collect images for each class (e.g., Vihan, Mansi, Aditya) and a diverse "Unknown" class to reduce false positives[cite: 529, 530, 575].
3.  [cite_start]**Train Model**: Train the Fisherface classifier and generate pre-computed FaceNet embeddings[cite: 668, 670, 946].
4.  [cite_start]**Run Recognition**: Start the Python script to stream the MJPEG feed from the ESP32 IP address and perform real-time identification[cite: 640, 643].

---

## Decision Logic
[cite_start]The system applies a specific decision hierarchy[cite: 152]:
* [cite_start]**Check FaceNet First**: If cosine similarity is $\geq 0.50$, the match is accepted immediately[cite: 153].
* [cite_start]**Fall back to Fisherface**: If FaceNet is below 0.50, the system accepts a match if the Fisherface distance is $\leq 5500$[cite: 154, 155].
* [cite_start]**Confidence Floor**: To avoid errors, if the overall certainty is below 65%, the person is labeled as "Unknown"[cite: 156].

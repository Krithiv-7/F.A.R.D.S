# ✅ F.A.R.D.S - Project Task Checklist

A comprehensive checklist to track every step of development for the Fire Analysis and Response Drone System (F.A.R.D.S).

---

## 📁 Project Setup

- [ ] Finalize tech stack and toolchains
- [ ] Assign roles to team members

---

## 🧭 Planning & Research

- [ ] Study existing drone-based fire detection systems
- [ ] Identify legal and airspace regulations (DGCA/FAA)
- [ ] Define core modules and responsibilities
- [ ] List components and estimate budget
- [ ] Define testing environment (lab + field test plans)
- [ ] Prepare detailed timeline and milestones

---

## 🛠️ Hardware Setup

### Drone Frame & Assembly

- [ ] Choose and procure drone frame (quadcopter recommended)
- [ ] Mount:
  - [ ] ESCs
  - [ ] Propellers
  - [ ] Motors
  - [ ] Battery
  - [ ] Landing gear

### Sensors & Payload

- [ ] GPS module setup and wiring
- [ ] Mount & configure FLIR thermal camera
- [ ] Add IR flame sensor(s)
- [ ] Optional: Add LIDAR for 3D mapping
- [ ] Mount Raspberry Pi / Jetson Nano as onboard compute module
- [ ] Attach 4G/LTE or WiFi communication module

### Flight Controller Setup

- [ ] Install PX4 / ArduPilot firmware
- [ ] Calibrate sensors (IMU, compass, GPS)
- [ ] Test basic remote/manual controls
- [ ] Test basic takeoff → hover → land routine
- [ ] Test waypoint navigation

---

## 🧠 AI & Detection Module

### Data Collection & Preprocessing

- [ ] Collect training data of fire images and thermal readings
- [ ] Label datasets (bounding boxes, masks if needed)
- [ ] Preprocess thermal image data for model input

### Model Training

- [ ] Choose baseline model (YOLOv8 / TensorFlow + OpenCV)
- [ ] Train fire classification model
- [ ] Train fire area estimation or segmentation model
- [ ] Save and export trained weights

### Onboard Inference

- [ ] Set up inference script on Raspberry Pi / Jetson
- [ ] Optimize model for edge deployment (TensorRT / TFLite)
- [ ] Run real-time fire detection from onboard video feed
- [ ] Test accuracy vs false positives

---

## 🔥 Fire Magnitude & Area Analysis

- [ ] Measure fire area using pixel-to-distance conversion
- [ ] Estimate temperature from thermal camera values
- [ ] Create scoring logic (Low / Medium / High intensity)
- [ ] Create bounding map of fire spread direction
- [ ] Suggest best starting point for suppression (e.g., NW edge)

---

## 📏 Resource Estimation Module

- [ ] Design logic to calculate:
  - [ ] Number of extinguishers needed
  - [ ] Water volume (liters)
  - [ ] Foam/Chemical suppressant units
- [ ] Define thresholds for resource assignment based on fire magnitude
- [ ] Test estimation logic with known fire profiles
- [ ] Export estimated values to JSON format

---

## 📡 Communication Module

- [ ] Set up 4G/LTE or WiFi connection
- [ ] Integrate MQTT or REST API transmission
- [ ] Configure endpoint to receive data
- [ ] Send:
  - [ ] Fire report (JSON)
  - [ ] Coordinates
  - [ ] Images
  - [ ] Strategy recommendation
- [ ] Add fallback if offline (log locally and send later)

---

## 🖥️ Dashboard & Control Panel

### Frontend

- [ ] Create interface using React or Flutter
- [ ] Display:
  - [ ] Live feed (or snapshot image)
  - [ ] Fire location on map
  - [ ] Estimated resources
  - [ ] Fire spread direction
  - [ ] Suggested suppression path
- [ ] Include status panel for:
  - [ ] Battery
  - [ ] Altitude
  - [ ] GPS location
  - [ ] Connectivity status

### Backend

- [ ] Set up server (Node.js / Flask / Firebase)
- [ ] Receive incoming data from drone
- [ ] Store logs (Fire reports, status, telemetry)
- [ ] Provide API for dashboard frontend

---

## 🔬 Testing & Simulation

- [ ] Create test plan for detection module
- [ ] Test drone in controlled environment (printed fire images / heat pads)
- [ ] Perform altitude vs accuracy tests
- [ ] Evaluate estimation correctness
- [ ] Perform latency and connectivity tests
- [ ] Validate real-time communication with server
- [ ] Conduct a complete simulated mission

---

## 📝 Final Deliverables

- [ ] Final working drone with all modules
- [ ] Dashboard with real-time data
- [ ] Documentation:
  - [ ] Project overview
  - [ ] Setup guide
  - [ ] API reference
  - [ ] Architecture diagram
- [ ] Record a project demo video
- [ ] Submit final report / presentation

---

## 🧹 Bonus & Optional Tasks

- [ ] Add support for swarm drones (future scope)
- [ ] Add payload module (mini extinguisher/foam sprayer)
- [ ] Add auto-return-to-base feature
- [ ] Optimize for solar-powered or extended flight

---

*Built by students, for disaster response innovation.* 🚁🔥

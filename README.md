# 🔥 Fire Analysis and Response Drone System (F.A.R.D.S)

F.A.R.D.S (Fire Analysis and Response Drone System) is a modular, open-source platform that integrates autonomous drones, AI-based fire detection, and a centralized command dashboard to detect, analyze, and respond to fire incidents—especially in remote or high-risk areas. Designed for both research and real-world deployment, F.A.R.D.S offers a scalable system for environmental monitoring, disaster response, and smart city integration.

---

## 📌 Key Modules

### 1. 🚁 Drone Firmware
- Based on PX4 / ArduPilot.
- Custom waypoint-based navigation.
- Logs telemetry and mission data.

### 2. 🔍 Fire Detection AI
- Deep learning model (YOLO/ResNet based) to detect wildfires from aerial images.
- Capable of bounding-box detection, heat-map generation, and spread estimation.
- Real-time inference ready for edge devices (Jetson Nano, Raspberry Pi).

### 3. 📊 Mission Control Dashboard
- Web-based dashboard (Next.js + Tailwind CSS).
- Live location tracking and drone telemetry.
- Fire alerts, image analysis, and mission control interface.

### 4. 🧠 Backend API
- Built with FastAPI.
- Bridges dashboard, drone, and AI modules.
- Handles data storage, mission history, fire prediction services, and more.

### 5. 🧰 Hardware Design
- CAD models for drone components.
- Circuit schematics and custom payload integration (thermal camera, GPS, etc.).
- Full list of tested components with sourcing info.

---

## 🧠 Use Cases

- **Forest Fire Monitoring**
- **Agricultural Risk Surveillance**
- **Disaster Management**
- **Smart City Fire Prevention**
- **Environmental Research**

---

## 📁 Project Structure

```bash
FARDS-Drone-System/
├── drone-firmware/             # PX4 configs, flight paths, logs
├── fire-detection-ai/          # AI model, dataset, inference code
├── mission-control-dashboard/  # Web dashboard (frontend)
├── backend-api/                # FastAPI backend server
├── hardware-design/            # CAD, components, schematics
├── docs/                       # Overview, research, usage
├── tests/                      # Unit + integration tests
├── README.md                   # This file
├── CASE.md                     # Use case document
├── PLANS.md                    # Implementation plans
├── TASKS.md                    # Development checklist
├── CONTRIBUTING.md             # For open source contributors
└── .gitignore, LICENSE, etc.
```

---

## ⚙️ Installation & Setup

### 🔧 Prerequisites
- Python 3.10+
- Node.js 18+
- Docker (optional for local dev)
- PX4/ArduPilot setup (for firmware flashing)
- GPU (for local model training)

---

## 📦 Datasets & Models

- Dataset folder includes sample training data.
- Pretrained models available in `fire-detection-ai/model/`.
- You can train your own model using:
```bash
python scripts/preprocess.py
python scripts/train_model.py
```

---

## 📡 Drone Integration

- Use `drone-firmware/README.md` for setting up firmware and telemetry.
- Tested with Pixhawk + GPS + ESP8266 for MAVLink telemetry.
- Includes flight path examples and automated waypoint missions.

---

## 🧪 Testing

- Unit tests under `tests/unit/`
- Integration tests for backend and AI inference
- Test logs from field missions available in `drone-firmware/test-logs/`

```bash
pytest tests/
```

---

## 📚 Documentation

- System flow: [`docs/overview.md`](docs/overview.md)
- Team & contributors: [`docs/team.md`](docs/team.md)
- Research sources: [`docs/research.md`](docs/research.md)

---

## 🤝 Contributing

We welcome contributions from developers, drone enthusiasts, AI researchers, and students. See [`CONTRIBUTING.md`](CONTRIBUTING.md) for guidelines.

---

## 🛡️ License

This project is licensed under the MIT License. See the `LICENSE` file for details.

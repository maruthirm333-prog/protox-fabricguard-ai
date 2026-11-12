# PROTOX FabricGuard AI 🧵

**AI-based automatic fabric defect inspection system**

A camera looks at a piece of fabric, an AI model decides whether it is **Normal**, **Broken** or **Scratch**, and the result is shown on an OLED screen and a web dashboard.

> 🚧 **Work in progress: early prototype.** This is **not** a commercially validated inspection machine. Camera capture is not yet stable and the AI model is still being improved. See [Current status](#current-status).

---

## What it detects

| Category | Meaning |
|----------|---------|
| **NORMAL** | No visible defect |
| **BROKEN** | Broken yarn or weave |
| **SCRATCH** | Scratch-like surface defect |

**Main objective:** capture fabric images, analyse them with an AI model, identify the defect category and display the inspection result on an OLED screen.

---

## How it works

```
        FABRIC SAMPLE
   (normal / broken / scratch)
              │
              ▼
     ESP32-CAM + OV3660
      captures the image
              │
              ▼   Wi-Fi: JPEG image
   LAPTOP: PYTHON AI SERVER
  receives + preprocesses image
              │
              ▼
    CNN DEEP-LEARNING MODEL
  predicts the defect category
              │
              ▼
        PREDICTION RESULT
 NORMAL / BROKEN / SCRATCH + confidence
              │
              ▼
   ESP32 OLED  +  WEB DASHBOARD
```

More detail: [docs/architecture.md](docs/architecture.md)

---

## Current status

| Area | Status |
|------|--------|
| OLED wiring and display test | ✅ Done: shows the FabricGuard startup message |
| PSRAM detection | ✅ Done |
| Camera initialisation | ✅ Done |
| Stable image capture | 🔧 In progress: fixing repeated frame-buffer overflow (FB-OVF) |
| AITEX dataset preparation | ✅ Image tiles prepared for the first experiments |
| Initial binary CNN (AITEX tiles) | ⚠️ Trained: about 85.96% validation accuracy, but the confusion matrix showed weak defect detection |
| Genuine three-class dataset | ⬜ Needed: real, correctly labelled fabric photos |
| Three-class model | ⬜ Not trained yet |
| Laptop server, ESP32-to-laptop link, OLED result display | ⬜ Pending integration testing |
| Web dashboard | 🔧 Basic interface exists in the firmware; live testing pending |

---

## Hardware

| Component | Purpose |
|-----------|---------|
| ESP32-CAM | Main camera controller |
| OV3660 | Captures fabric images |
| ESP32-WROOM development board | Development and programming support |
| 0.96-inch OLED (SSD1306) | Displays the inspection result |
| Wi-Fi | Communicates with the laptop |
| Power supply | Powers the hardware |

**OLED wiring** (pins configured in the current FabricGuard code):

| OLED | ESP32-CAM |
|------|-----------|
| VCC | 3.3V |
| GND | GND |
| SDA | GPIO 14 |
| SCL | GPIO 15 |

The camera pin mapping must also match the actual camera board. Details: [hardware/README.md](hardware/README.md)

---

## Software

- Python 3.13
- TensorFlow 2.21
- OpenCV
- NumPy
- CNN (Convolutional Neural Network)
- FastAPI or the current Python HTTP server, depending on the final server code
- Arduino firmware for the ESP32-CAM

The trained model is planned to be saved as `fabricguard_model.keras`.

---

## ESP32-CAM firmware (current behaviour)

1. Initialise the Serial Monitor
2. Initialise the OLED
3. Initialise the OV3660 camera
4. Configure grayscale image capture
5. Capture an image
6. Convert the captured frame to JPEG
7. Connect to Wi-Fi
8. Send the JPEG to the laptop server
9. Receive the AI prediction
10. Display the result on the OLED

| Camera setting | Value |
|----------------|-------|
| Image format | Grayscale |
| Resolution | QVGA (320 × 240), currently being tested |
| Frame buffer | 1 |
| Frame location | PSRAM when available |
| Capture mode | `CAMERA_GRAB_WHEN_EMPTY` |

**Current problem:** repeated frame-buffer overflow (FB-OVF). The next hardware/software milestone is stable camera capture without it.

---

## Laptop AI server

The laptop does the AI processing. It:

- receives the image through an HTTP POST
- decodes the JPEG with OpenCV
- preprocesses the image
- loads the trained TensorFlow model
- predicts the fabric category
- returns the prediction and confidence to the ESP32-CAM

Example response format:

```
NORMAL|96
BROKEN|89
SCRATCH|93
```

> These are **illustrative responses, not measured model results.**

---

## Web dashboard

The firmware already contains a basic web interface showing:

- live camera image
- current prediction
- confidence percentage
- inspection frame count
- AI server connection status

---

## Dataset

| Category | Images planned |
|----------|---------------:|
| Normal | 50 |
| Broken | 50 |
| Scratch | 50 |
| **Total** | **150** |

The AITEX textile-defect dataset was also downloaded, and image tiles were prepared for the first experiments.

> ⚠️ **Important:** the 50-image PDFs generated so far contain **synthetic, AI-generated texture samples**. They are **not** verified real fabric photographs. Reliable training needs **genuine, correctly labelled fabric photos**, and synthetic samples must never be presented as real production data.

No dataset files are included in this repository yet. See [docs/dataset-notes.md](docs/dataset-notes.md).

---

## Roadmap

| Phase | Focus | Status |
|-------|-------|--------|
| 1 | Hardware | Partially completed |
| 2 | AI development | Needs improvement |
| 3 | Integration | Pending |
| 4 | Final product | Pending |
| 5 | GitHub | Documentation published; source code, diagrams and sample images to be added |

Full checklist: [docs/roadmap.md](docs/roadmap.md)

---

## Final product goals

1. Automatically capture fabric images
2. Identify normal and defective fabric
3. Distinguish broken and scratch defects
4. Display the result on the OLED
5. Show inspection information on a laptop dashboard
6. Store inspection results for later analysis

> **Important:** the current prototype is not yet a commercially validated inspection machine. Reliable three-class performance, real-world testing and stable hardware are needed before any claim of production readiness.

---

## Repository layout

**In this repository now:**

```
protox-fabricguard-ai/
├── README.md
├── .gitignore
├── docs/
│   ├── architecture.md
│   ├── roadmap.md
│   └── dataset-notes.md
└── hardware/
    └── README.md
```

**Planned, added as the real project files are organised** (nothing here is placeholder code):

```
├── firmware/ESP32_CAM/FabricGuard.ino
├── ai/            (training, evaluation, test prediction, server, saved model)
├── dataset/       (normal / broken / scratch)
├── hardware/      (circuit diagram, component list)
├── dashboard/
└── requirements.txt
```

### Before committing anything new

- No passwords, Wi-Fi credentials or API keys
- No virtual environments
- Check the size and licence of any dataset before adding it
- Label synthetic images clearly as synthetic

---

## About PROTO X

FabricGuard AI is a [PROTO X](https://protox-engineering.netlify.app/) project, an engineering project platform in Hassan, Karnataka, India. More: [protox-website](https://github.com/maruthirm333-prog/protox-website)

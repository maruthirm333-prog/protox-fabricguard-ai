# Architecture

## Workflow

```
FABRIC SAMPLE (normal / broken / scratch)
      │
      ▼
ESP32-CAM + OV3660            captures the fabric image
      │
      ▼  Wi-Fi: JPEG image
LAPTOP: PYTHON AI SERVER      receives the image and preprocesses it
      │
      ▼
CNN DEEP-LEARNING MODEL       predicts the defect category
      │
      ▼
PREDICTION RESULT             NORMAL / BROKEN / SCRATCH + confidence
      │
      ▼
ESP32 OLED  +  WEB DASHBOARD  displays the inspection result
```

## 1. ESP32-CAM firmware

What the current Arduino firmware does:

1. Initialise the Serial Monitor
2. Initialise the OLED
3. Initialise the OV3660 camera
4. Configure grayscale image capture
5. Capture an image
6. Convert the captured frame into JPEG
7. Connect to Wi-Fi
8. Send the JPEG to the laptop server
9. Receive the AI prediction
10. Display the result on the OLED

Current camera settings:

| Setting | Value |
|---------|-------|
| Image format | Grayscale |
| Resolution | QVGA (320 × 240), currently being tested |
| Frame buffer | 1 |
| Frame location | PSRAM when available |
| Capture mode | `CAMERA_GRAB_WHEN_EMPTY` |

**Known issue:** repeated frame-buffer overflow (FB-OVF) is being troubleshot. Stable capture without it is the next hardware/software milestone.

## 2. Laptop AI server

The laptop is the main AI processing unit. Its responsibilities:

- Receive the image through an HTTP POST
- Decode the JPEG using OpenCV
- Preprocess the image
- Load the trained TensorFlow model
- Predict the fabric category
- Return the prediction and confidence to the ESP32-CAM

Response format (illustrative, **not measured results**):

```
NORMAL|96
BROKEN|89
SCRATCH|93
```

The server is either FastAPI or the current Python HTTP server implementation, depending on the final server code.

## 3. AI model

Technology: Python 3.13, TensorFlow 2.21, OpenCV, NumPy, CNN.

Training process:

1. Collect and organise images
2. Preprocess and resize images
3. Divide data into training and validation sets
4. Build the CNN architecture
5. Train the model
6. Evaluate prediction accuracy and individual class performance
7. Save the trained model as `fabricguard_model.keras`

**Trained so far:** an initial **binary** CNN using AITEX tiles. Validation accuracy was approximately 85.96%, but the confusion matrix showed weak defect detection. The three-class model (Normal / Broken / Scratch) still needs to be improved and evaluated.

## 4. Display layer

- **OLED (0.96-inch SSD1306):** local inspection result. The display test passed (it shows the FabricGuard startup message).
- **Web dashboard:** the firmware already has a basic web interface with the live camera image, current prediction, confidence percentage, inspection frame count and AI server connection status.

## Open issues

- Frame-buffer overflow (FB-OVF) in camera capture
- No genuine, correctly labelled three-class dataset yet
- Weak defect detection in the initial binary model
- Camera pin mapping must match the actual camera board
- Not yet tested on unseen real fabric

# Sign Language Translator (Windows)

This project implements a real-time sign language recognition system on Windows using computer vision (MediaPipe), deep learning (TensorFlow with LSTM), and grammar correction (LanguageTool). It recognizes hand signs from webcam input, translates them into text, and optionally corrects the grammar of the resulting sentences.

## Prerequisites

- **Python 3.8 or higher**: Download and install from [python.org](https://www.python.org/downloads/). Ensure "Add Python to PATH" is checked during installation.
- A webcam or camera device connected to your Windows computer.
- Internet connection (for initial dependency installation and grammar correction API).

## Setup Instructions

### Step 1: Create and Activate a Virtual Environment

A virtual environment isolates project dependencies from your system-wide Python installation.

1. Open a Command Prompt or PowerShell:
   - Press `Win + R`, type `cmd` (or `powershell`), and press Enter.
2. Head in your root folder
3. Create a virtual environment named `venv`:
   ```
   python -m venv venv
   ```
4. Activate the virtual environment:
   ```
   venv\Scripts\activate
   ```
   You should see `(venv)` appear before your prompt, e.g., `(venv) C:\...`.

### Step 2: Install Required Dependencies

1. Run:
   ```
   pip install -r requirements.txt
   ```

### Step 3: Verify Installation
Check that all packages installed correctly:
```
pip list
```
Ensure the versions meet or exceed the minimum requirements listed above.

## Running the Project

The project includes three scripts that must be run in order: `data_collection.py`, `model.py`, and `main.py`. Keep the virtual environment activated (`(venv)` visible in your prompt) for all steps.

### 1. Collect Data (`data_collection.py`)
This script captures video from your webcam, extracts hand landmarks using MediaPipe, and saves them as `.npy` files in the `data` directory.

#### Instructions:
1. Run the script:
   ```
   python data_collection.py
   ```
2. A "Camera" window will open, showing your webcam feed with instructions.
3. For each action (e.g., `a`, `b`):
   - The script pauses and displays "Press 'Space' when you are ready."
   - Position your hand to perform the sign.
   - Press the `Space` key to record 10 frames for each of the 30 sequences per action.
4. Repeat for all actions. Data will be saved in `data\<action>\<sequence>\<frame>.npy`.
5. Close the window by clicking the 'X' when finished.

#### Notes:
- If you see "Cannot access camera," ensure your webcam is connected and not in use by another application.
- Run as administrator if `keyboard` module access is denied: right-click Command Prompt, select "Run as administrator," then activate the environment and run the script.

### 2. Train the Model (`model.py`)
This script loads the collected data, trains an LSTM neural network, and saves the model as `my_model.h5`.

#### Instructions:
1. Run the script:
   ```
   python model.py
   ```
2. The script will:
   - Load `.npy` files from the `data` directory.
   - Train the model for 100 epochs (this may take several minutes depending on your CPU/GPU).
   - Save the trained model as `my_model.h5`.
   - Display the test accuracy after training.
3. Wait for training to finish. You’ll see progress bars for each epoch.

#### Notes:
- If a `FileNotFoundError` occurs (e.g., `data\a\0\0.npy`), ensure `data_collection.py` completed successfully and the `data` directory is populated.
- Low accuracy (<50%) may indicate insufficient data; re-run `data_collection.py` with more varied samples.

### 3. Run the Real-Time Translator (`main.py`)
This script uses the trained model to recognize signs in real-time, builds sentences, and applies grammar correction if desired.

#### Instructions:
1. Run the script:
   ```
   python main.py
   ```
2. A "Camera" window will open, showing your webcam feed with hand landmarks.
3. Perform signs (e.g., `a`, `b`):
   - The script collects 10 frames to predict a sign.
   - If confidence exceeds 90%, the sign is added to the sentence, displayed at the bottom.
4. Controls:
   - Press `Space` to clear the sentence and reset.
   - Press `Enter` to correct the grammar of the current sentence.
5. Close the window by clicking the 'X' to exit.

#### Notes:
- Ensure `my_model.h5` exists in the project directory from `model.py`.
- If no text appears or predictions are off, check the model’s accuracy from `model.py`.

## Troubleshooting

- **Camera Not Working**: Verify webcam connection in Device Manager (`Win + X > Device Manager > Cameras`). Try `cv2.VideoCapture(1)` in `data_collection.py` and `main.py` if `0` fails.
- **Permission Errors**: Run Command Prompt as administrator for `keyboard` module access.
- **Installation Issues**: Update `pip` with `pip install --upgrade pip` and retry installation.
- **Low Accuracy**: Collect more data or adjust the model in `model.py` (e.g., increase epochs or LSTM units).

## Project Structure
```
Sign-Language-Translator-main/
├── data/               # Directory for collected data (created by data_collection.py)
├── my_model.h5         # Trained model file (created by model.py)
├── data_collection.py  # Script to collect training data
├── model.py            # Script to train the model
├── main.py             # Script for real-time sign recognition
├── my_functions.py     # Helper functions (assumed provided)
└── README.md           # This file
```

## Dependencies
- `numpy>=1.21.0`: Numerical operations.
- `mediapipe>=0.10.0`: Hand landmark detection.
- `opencv-python>=4.5.0`: Video capture and image processing.
- `keyboard>=0.13.5`: Keyboard input detection.
- `tensorflow>=2.12.0`: Neural network training and inference.
- `language-tool-python>=2.7.0`: Grammar correction.
- `scikit-learn>=1.0.0`: Data splitting and metrics.

---

This README is streamlined for Windows users, with clear instructions and common troubleshooting tips. Let me know if you’d like to refine it further!
# 🤖 Hiwonder LeArm Robotic Arm Controller

A standalone Windows controller and responsive browser dashboard for the **Hiwonder LeArm** robotic arm over **Bluetooth Low Energy (BLE)**. Features real-time manual joint positioning, automated multi-step movement routines, synchronized browser-synthesized audio, and text-to-speech interaction.

---

## 🚀 Quick Start (No Python Required)

1. Download **`LeArm_Controller.exe`** from the [Releases](https://github.com/hue09/LeArm-Controller/releases/latest) section.
2. Power on your LeArm robot with the 7.4V/8.4V power supply or battery.
3. Double-click `LeArm_Controller.exe`.
4. The application scans nearby Bluetooth devices, pairs automatically with the arm, and launches the web dashboard at `http://127.0.0.1:8080`.

---

## 🕹️ What's Inside the Controller App

The application consists of three integrated layers: an **Interactive Web Dashboard**, an **Automated Action & Audio Engine**, and a **Bluetooth LE Hardware Bridge**.

### 1. Manual Servo Sliders & Fine-Tuning
Each of the 6 digital bus servos features individual range clamping to prevent mechanical binding, along with quick single-step nudge buttons:

| Joint | Servo ID | Pulse Range (μs) | Description & Controls |
| :--- | :---: | :---: | :--- |
| **Claw** | Servo 1 | `1150` – `2450` | Gripper open/close; includes `+20 Close` and `-20 Open` nudge buttons |
| **Wrist Pitch** | Servo 2 | `800` – `2200` | End-effector vertical up/down tilt |
| **Wrist Roll** | Servo 3 | `700` – `2300` | End-effector axial rotation |
| **Elbow** | Servo 4 | `800` – `2250` | Forearm extension and flexion |
| **Shoulder** | Servo 5 | `800` – `2200` | Main arm height and forward/backward reach |
| **Base Turntable** | Servo 6 | `705` – `2500` | 360° horizontal rotation; includes `← Left (-30)` and `Right (+30) →` buttons |

---

### 2. Pre-Programmed Action Routines
Automated kinematic routines executed directly via the UI button grid:

* **🔄 Auto Pick & Drop:** Executes a complete pick-and-place operation:
  * Opens the claw and reaches down to the designated object position.
  * Clamps the payload and lifts to intermediate clearance height.
  * Rotates the base table across to the target drop zone.
  * Releases the gripper, chirps confirmation buzzer tones, and returns to center.
* **👋 Say Hi (Single Wave):** Raises the forearm, tilts the wrist forward, sounds a dual-beep buzzer chime, performs 3 quick waves, and speaks:  
  > *"Hello! Nice to meet you."*
* **👋 Say Bye:** Extends the arm high with a downward wrist angle, executes 3 broad, deliberate farewell waves, sounds descending tones, speaks, and centers:  
  > *"Goodbye! Have a great day!"*
* **▶ Repeat Hi / ⏹ Stop Hi:** Initiates a continuous greeting presentation loop. Waves repeatedly while periodically announcing:  
  > *"Hi, nice to meet you, hope you enjoy the show."*
* **🎵 Dance with Music / ⏹ Stop Dance:** Drives dynamic coordinated movements across all 6 axes while synchronizing physical motion to a live synthesized beat.
* **📐 Calibrated Center:** Immediately resets all servos to the calibrated rest pose (`S6:1912`, `S5:1510`, `S4:1496`, `S3:1451`, `S2:1731`, `S1:1790`) and silences active loops.

---

### 3. Integrated Audio & Speech Synthesis
* **Web Audio API Chiptune Synth:** Synthesizes an 8-bit dance soundtrack directly in the browser—generating live kick drums and an 8-note melodic lead without external audio files.
* **HTML5 SpeechSynthesis Engine:** Generates natural voice feedback for greetings, cycle statuses, and farewells through system speakers.
* **Hardware Buzzer Pulses:** Transmits serial `0x0B` pulse commands to sound the robot's physical onboard buzzer during actions.

---

### 4. Background Server & Bluetooth LE Bridge
* **Zero-Config Auto Discovery:** Leverages `BleakScanner` on startup to automatically discover and link to any nearby broadcasting `LeArm` or `Hiwonder` robot without manually configuring MAC addresses.
* **Dynamic Handle Resolution:** Traverses GATT services at runtime to identify active writable characteristics (`write` / `write-without-response`), preventing connection dropouts.
* **Local HTTP Micro-Server:** Embedded Python HTTP daemon hosting the web control interface on port `8080`.

---

## 🛠️ Running from Source (Developers)

If modifying or running the Python script directly:

```bash
# Clone the repository
git clone [https://github.com/hue09/LeArm-Controller.git](https://github.com/hue09/LeArm-Controller.git)
cd LeArm-Controller

# Install requirements
pip install bleak pyinstaller

# Run the controller
python learm_gui.py

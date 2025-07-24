  # Phase 2: Companion Computer Setup (The Brain)

  **Objective:** To prepare the Raspberry Pi to act as the onboard "brain," capable of running Python scripts, accessing a camera, and installing the necessary AI and drone control libraries.

  ---

  ### Part A: Hardware Setup

  This phase is done on a workbench, not on the drone.

  | Component | Recommendation | Purpose |
  | :--- | :--- | :--- |
  | **Companion Computer**| Raspberry Pi 4 Model B (4GB or 8GB) | Provides the necessary processing power for AI and video analysis. |
  | **Storage** | High-Speed 32GB (or larger) Class 10/U3 MicroSD Card | Fast storage is crucial for OS performance and writing video/log files. |
  | **Power Supply** | Official Raspberry Pi USB-C Power Supply | Provides stable power during the setup and software installation process. |
  | **Camera Module** | Raspberry Pi Camera Module v2 or similar | This will be the Pi's "eye" for visual data capture and AI analysis. |

  ---

  ### Part B: OS Installation & Initial Setup

  1.  **Download Raspberry Pi Imager:** Go to the official Raspberry Pi website and download the "Raspberry Pi Imager" for your computer (Windows/Mac/Linux).
  2.  **Flash the OS:**
      * Insert the microSD card into your computer.
      * Open Raspberry Pi Imager.
      * Choose "Raspberry Pi OS (64-bit)" for best performance.
      * In the advanced settings (cog icon), pre-configure a hostname (e.g., `fards-pi`), enable SSH, and set your Wi-Fi credentials. This allows "headless" setup (no monitor/keyboard needed).
      * Select your microSD card and click "Write".
  3.  **First Boot:**
      * Eject the microSD card and insert it into the Raspberry Pi.
      * Power on the Pi. Wait a few minutes for it to boot and connect to your Wi-Fi.
      * Find the Pi's IP address from your router's admin page.
      * Connect to the Pi from your computer's terminal using SSH: `ssh your_username@<pi_ip_address>`.

  ---

  ### Part C: Software Installation & Configuration

  Run these commands in the SSH terminal connected to your Pi.

  1.  **Update System Packages:** Always start with this.
      ```bash
      sudo apt update
      sudo apt full-upgrade -y
      ```
  2.  **Install Essential Libraries:**
      ```bash
      sudo apt install -y python3-pip git build-essential libopenblas-dev libatlas-base-dev
      ```
  3.  **Install Python Packages:** Install the core libraries for drone control, image processing, and AI.
      ```bash
      pip3 install dronekit opencv-python tensorflow
      ```
  4.  **Enable Camera Interface:**
      * Run `sudo raspi-config`.
      * Navigate to `Interface Options` -> `Legacy Camera` and select `Enable`.
      * Reboot the Pi when prompted.

  ---

  ### Phase 2 Milestone: System Readiness Check

  * **Physical Check:** Physically connect the camera module ribbon cable to the Pi's camera port.
  * **Camera Test:** Run the following command in the terminal to take a test picture.
    ```bash
    libcamera-still -o test-image.jpg
    ```
    If a `test-image.jpg` file is created, the camera is working correctly.
  * **Python Script Test:** Create a simple python file `test_script.py` with the content `import cv2; import tensorflow; import dronekit; print("All libraries imported successfully!")` and run it with `python3 test_script.py`.

  **Once the Raspberry Pi can successfully boot, connect to Wi-Fi, take a picture, and run the test script without import errors, Phase 2 is complete.** You now have a functional brain, ready to be connected to the drone's body.

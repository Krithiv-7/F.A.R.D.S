  # Phase 5: Sensor Integration & Data Capture

  **Objective:** To connect the visual and thermal cameras to the Raspberry Pi and write scripts to successfully capture and save data from them.

  ---

  ### Part A: Hardware Connection

  This part involves connecting the camera modules to the Raspberry Pi mounted on the drone.

  1.  **Visual Camera (4K):**
      * **Component:** Raspberry Pi Camera Module 3 (or similar).
      * **Connection:** Power down the Pi. Gently lift the plastic clip on the Pi's CSI (Camera Serial Interface) port. Insert the camera's ribbon cable, ensuring the blue tab faces the USB ports. Press the clip down firmly to secure it.
  2.  **Thermal Camera (FLIR):**
      * **Component:** FLIR Lepton 3.5 with a PureThermal 2/3 Breakout Board.
      * **Connection:** The PureThermal board makes this easy. It connects directly to the Raspberry Pi via a standard USB-C to USB-A cable. This is simpler and more reliable than using GPIO for the thermal camera.
  3.  **Mounting:** Securely mount both cameras to the drone's frame, ensuring they have a clear, unobstructed view downwards and slightly forwards. They should be protected from vibrations as much as possible.

  ---

  ### Part B: Visual Data Capture (4K Camera)

  We will use the modern `picamera2` library, which is the standard for recent Raspberry Pi OS versions.

  1.  **Install the Library:** SSH into the Pi and run:
      ```bash
      sudo apt update
      sudo apt install -y python3-picamera2
      ```
  2.  **Create Capture Script:** Create a file named `visual_capture.py` with the following code:
      ```python
      from picamera2 import Picamera2, Preview
      import time

      # Initialize the camera
      picam2 = Picamera2()

      # Configure for a still image
      camera_config = picam2.create_still_configuration(main={"size": (1920, 1080)}, lores={"size": (640, 480)}, display="lores")
      picam2.configure(camera_config)

      # Start the camera preview (optional, useful for testing with a monitor)
      # picam2.start_preview(Preview.QTGL)

      # Start the camera
      picam2.start()
      time.sleep(2) # Allow time for camera to initialize

      # Capture and save a still image
      image_filename = "test_visual.jpg"
      picam2.capture_file(image_filename)
      print(f"Image saved to {image_filename}")

      # Stop the camera
      picam2.stop()
      # picam2.stop_preview()
      ```
  3.  **Run the Script:** Execute `python3 visual_capture.py`. It should save a high-resolution image named `test_visual.jpg` in the current directory.

  ---

  ### Part C: Thermal Data Capture (FLIR Camera)

  This requires a library to interact with the PureThermal board.

  1.  **Install Dependencies:**
      ```bash
      sudo apt install -y libusb-1.0-0-dev
      pip3 install pyusb
      ```
  2.  **Install `pylepton` library:**
      ```bash
      # Clone the library repository
      git clone [https://github.com/groupgets/pylepton.git](https://github.com/groupgets/pylepton.git)
      cd pylepton/
      # Install it
      sudo python3 setup.py install
      cd ..
      ```
  3.  **Create Thermal Capture Script:** Create a file named `thermal_capture.py`:
      ```python
      import numpy as np
      import cv2
      from pylepton import Lepton

      try:
          with Lepton() as l:
              # Retrieve a frame from the thermal camera
              a, _ = l.capture()
              
              # Normalize the data for display
              cv2.normalize(a, a, 0, 65535, cv2.NORM_MINMAX)
              np.right_shift(a, 8, a)
              
              # Convert to an 8-bit grayscale image
              img = np.uint8(a)
              
              # Apply a color map for visualization
              heatmap_img = cv2.applyColorMap(img, cv2.COLORMAP_INFERNO)
              
              # Save the resulting heatmap
              image_filename = "test_thermal_heatmap.jpg"
              cv2.imwrite(image_filename, heatmap_img)
              print(f"Thermal heatmap saved to {image_filename}")

      except Exception as e:
          print(f"Error capturing from thermal camera: {e}")
          print("Is the PureThermal board connected via USB?")

      ```
  4.  **Run the Script:** Execute `python3 thermal_capture.py`. It should save a colorized heatmap image named `test_thermal_heatmap.jpg`.

  ---

  ### Phase 5 Milestone: Successful Data Capture

  **Once you can successfully run both `visual_capture.py` and `thermal_capture.py` on the drone-mounted Pi and generate both a standard JPG image and a thermal heatmap image, Phase 5 is complete.**

  The drone now has its senses and can perceive the world in both the visual and thermal spectrums, ready for the AI to interpret this data.

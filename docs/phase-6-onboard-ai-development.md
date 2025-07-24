# Phase 6: Onboard AI Development & Testing

  **Objective:** To train a basic fire detection model, convert it for use on the Raspberry Pi, and create a script that uses the drone's camera to classify images in real-time.

  ---

  ### Part A: Data Collection & Preparation (On a Desktop Computer)

  A good AI model needs good data. This part is done on your main computer, not the Pi.

  1.  **Find a Dataset:** You need thousands of images. A great starting point is to search for "fire and smoke image datasets" on platforms like Kaggle or Roboflow. You need two main categories of images:
      * **`fire`:** Images containing flames, wildfires, bonfires, etc.
      * **`no_fire` (or `neutral`):** Images of forests, fields, and urban areas *without* any fire. This is crucial to teach the model what is *not* a fire.
  2.  **Organize the Dataset:** Create a folder structure like this:
      ```
      dataset/
      ├── train/
      │   ├── fire/
      │   │   ├── fire_image_001.jpg
      │   │   └── ...
      │   └── no_fire/
      │       ├── no_fire_image_001.jpg
      │       └── ...
      └── validation/
          ├── fire/
          │   ├── fire_image_val_001.jpg
          │   └── ...
          └── no_fire/
              ├── no_fire_image_val_001.jpg
              └── ...
      ```
      Split your data roughly 80% for `train` and 20% for `validation`.

  ---

  ### Part B: Model Training (On a Desktop Computer)

  We will use TensorFlow and Keras to build and train a simple Convolutional Neural Network (CNN).

  1.  **Setup Python Environment:** Ensure you have TensorFlow installed on your desktop (`pip install tensorflow`).
  2.  **Create Training Script:** Create a file named `train_fire_model.py`. This script will:
      * Load the images from your `dataset` directory.
      * Define a simple CNN model architecture (e.g., a few convolutional layers, max pooling, and dense layers).
      * Compile the model with an optimizer (like `adam`) and a loss function (`binary_crossentropy`).
      * Train the model on your dataset for a set number of epochs.
      * Save the final trained model to a file (e.g., `fire_detection_model.h5`).

  ---

  ### Part C: Model Conversion to TensorFlow Lite

  The full `.h5` model is too large and slow for the Raspberry Pi. We must convert it to the optimized TensorFlow Lite (`.tflite`) format.

  1.  **Create Conversion Script:** Create a file named `convert_to_tflite.py` on your desktop. This script will:
      * Load the saved model (`fire_detection_model.h5`).
      * Use the `TFLiteConverter` to convert the model.
      * Save the converted model to a new file: `fire_model.tflite`.
  2.  **Transfer to Pi:** Copy the `fire_model.tflite` file from your desktop to the Raspberry Pi using a tool like `scp` or a USB drive.

  ---

  ### Part D: Real-Time Inference on the Raspberry Pi

  This script will use the `.tflite` model to classify images captured live from the Pi's camera.

  1.  **Install TFLite Runtime:** On the Raspberry Pi, the full TensorFlow library is heavy. We'll install only the interpreter.
      ```bash
      # This command might vary slightly based on your Pi's OS version and Python version.
      # Check the official TensorFlow website for the correct wheel file.
      pip3 install tflite-runtime
      ```
  2.  **Create Inference Script:** On the Pi, create a file named `live_fire_detection.py`. This script will:
      * Import the `picamera2` and `tflite_runtime` libraries.
      * Load the `fire_model.tflite` model and allocate its tensors.
      * Start the camera.
      * In a loop:
          * Capture an image from the camera.
          * Pre-process the image to match the model's input requirements (e.g., resize to 224x224, normalize pixel values).
          * Feed the image to the TFLite interpreter.
          * Get the prediction result (a probability score between 0 and 1).
          * Print the result: "Fire Detected!" or "No Fire Detected" based on the score.

  ---

  ### Phase 6 Milestone: Live Fire Detection

  **The goal is to run `live_fire_detection.py` on the Raspberry Pi and have it correctly classify images in real-time.**

  * Point the drone's camera at a picture or video of a fire on a monitor. The script should print "Fire Detected!".
  * Point the camera at a normal scene (e.g., a plant, a wall). The script should print "No Fire Detected".

  **Once the Pi can reliably distinguish between these two scenarios using the live camera feed, Phase 6 is complete.** The drone is no longer just seeing; it is starting to understand.

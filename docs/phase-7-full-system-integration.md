# Phase 7: Full System Integration & Mission Logic

  **Objective:** To combine the flight control, sensor capture, and AI inference modules into a single master script that can execute a complete, autonomous fire-scouting mission.

  ---

  ### Part A: The Master Mission Logic

  This phase revolves around creating one primary Python script on the Raspberry Pi that orchestrates the entire drone operation from start to finish.

  The logical flow of the mission will be:
  1.  **Initialization:** The script starts, connects to the Pixhawk via DroneKit, and initializes the camera and the TFLite AI model.
  2.  **Receive Mission:** The script gets the target GPS coordinates for the search area (for now, this can be hardcoded).
  3.  **Takeoff & Transit:** The script commands the drone to arm, take off to a safe search altitude (e.g., 20 meters), and fly to the starting corner of the search grid.
  4.  **Execute Search Pattern:** The drone autonomously flies a grid pattern (a "lawnmower" pattern) over the target area.
  5.  **Scan & Analyze (The Core Loop):** While flying the grid, the script continuously:
      * Captures an image from the camera.
      * Processes the image and feeds it to the AI model.
      * If the model detects fire with a high confidence score (e.g., > 95%):
          * It immediately records the drone's current GPS coordinates and altitude.
          * It saves the corresponding image for evidence.
          * (For now, it will print a "FIRE DETECTED AT [coordinates]" message).
  6.  **Mission Completion:** Once the search grid is fully covered, the script commands the drone to enter "Return to Launch" (RTL) mode.
  7.  **Shutdown:** After the drone has landed and disarmed, the script closes all connections and saves a mission log file.

  ---

  ### Part B: The Master Script (`master_mission.py`)

  This script will be a significant piece of code that brings together functions and logic from all previous phases.

  **Key Components of the Script:**

  * **Imports:** It will import `dronekit`, `picamera2`, `tflite_runtime`, `numpy`, `cv2`, and `time`.
  * **Functions:**
      * `connect_to_vehicle()`: Establishes the connection to the Pixhawk.
      * `load_ai_model()`: Loads the `fire_model.tflite` and prepares the interpreter.
      * `arm_and_takeoff()`: The same function from Phase 4.
      * `fly_grid_pattern(coordinates_list)`: A new, more advanced function that takes a list of GPS waypoints and commands the drone to fly to each one in sequence.
      * `analyze_image(image)`: Takes a captured image, preprocesses it, runs inference, and returns the result ("fire" or "no_fire") and the confidence score.
  * **Main Loop:** The main part of the script will execute the mission logic described in Part A, calling the functions above in the correct order.

  ---

  ### Part C: Integration Testing (Safety is Paramount)

  1.  **Ground Test (Propellers OFF):**
      * Run the `master_mission.py` script on the ground with propellers removed.
      * Verify that the script connects to the vehicle, initializes the camera, and loads the AI model.
      * Point the camera at a picture of fire and confirm the "FIRE DETECTED" message appears in the console.
      * Check that the script attempts to send flight commands (you can see this in the console output).
  2.  **First Integrated Flight Test:**
      * **Location:** A very large, open field, completely clear of obstacles and people.
      * **Target:** Place a clear, visible "fire" target on the ground for the drone to find (e.g., a large orange or red tarp).
      * **Execution:** Place the drone, stand far back with the RC transmitter ready, and run the `master_mission.py` script.
      * **Observation:** Watch carefully to ensure the drone follows the flight plan, and monitor the console output (via SSH on a laptop) to see if it detects the target. Be ready to take manual control at any moment.

  ---

  ### Phase 7 Milestone: The First "Intelligent" Mission

  **Once the drone can autonomously execute the full mission—take off, fly a search pattern, correctly identify the simulated fire target on the ground, log its location, and safely return to launch—Phase 7 is complete.**

  This is the "proof of concept" for the entire F.A.R.D.S. system. You have successfully integrated all core components into a working prototype.

# Phase 1: Core Drone Assembly & Basic Flight (The Body)

  **Objective:** To assemble a quadcopter, install the core electronics, and achieve a stable, manually controlled flight.

  ---

  ### Part A: Component Selection & Purchase

  Before you build, you need the parts. Here is a recommended list for a robust F.A.R.D.S. platform. You can find these components at online hobby stores or electronics suppliers in India.

  | Component | Recommendation | Why? |
  | :--- | :--- | :--- |
  | **Drone Frame** | DJI F450 or a generic 450mm glass fiber frame | The 450mm size is the "sweet spot": large enough to carry the payload (Pi, cameras) but still agile. |
  | **Flight Controller** | Pixhawk 4 or Holybro Pixhawk 6C | These are powerful, reliable, and well-supported by the ArduPilot/PX4 community. This is the drone's "muscle." |
  | **Motors** | 4x Brushless Motors (e.g., 920kV - 1000kV) | The 'kV' rating determines speed. 920kV is a good balance of thrust and efficiency for this frame size. |
  | **ESCs** | 4x 30A Electronic Speed Controllers | Must be able to handle the current your motors will draw. 30A provides a safe margin. |
  | **Propellers** | 9-inch to 10-inch (e.g., 1045 or 9450) | Match the propellers to your motor and frame size for optimal performance. Get several spare pairs. |
  | **Battery** | 3S or 4S LiPo Battery (e.g., 5200mAh 4S) | A 4S battery provides more power. 5200mAh offers a good balance of flight time and weight. |
  | **RC Transmitter & Receiver**| FlySky FS-i6 or a similar 6+ channel system | You need at least 6 channels for standard flight modes. This is your manual control link. |
  | **GPS Module** | M8N GPS Module | Essential for all autonomous functions. The M8N is a reliable standard. |
  | **Power Module** | APM/Pixhawk Power Module | This measures battery voltage and current, which is critical for failsafes and telemetry. |

  ---

  ### Part B: Assembly & Wiring

  Follow these steps methodically. A clean build is a reliable build.

  1.  **Frame Assembly:** Assemble the main drone frame according to its manual. Don't fully tighten all the screws yet.
  2.  **Motor Mounting:** Mount the four motors onto the arms of the frame. Ensure they are secure. Note the rotation direction for each motor (two will be clockwise (CW), two counter-clockwise (CCW)).
  3.  **ESC Installation:** Secure one ESC to each arm, usually with zip ties. The three thick wires from the ESC will connect to the three wires on the motor. The order doesn't matter yet.
  4.  **Power Distribution:** Solder the main power leads of all four ESCs to the Power Distribution Board (PDB) or the frame's integrated PDB. Solder the Power Module's input leads to the PDB as well. **Crucially, double-check your polarity (+ to +, - to -).**
  5.  **Flight Controller Mounting:** Mount the Pixhawk in the center of the frame on its anti-vibration foam pad. The arrow on the Pixhawk case **must** point directly to the front of the drone.
  6.  **Connect Components to Pixhawk:**
      * Connect the Power Module's output to the Pixhawk's `POWER` port.
      * Connect the GPS module to the `GPS` port. Mount the GPS antenna on a mast, away from other electronics.
      * Connect the RC receiver to the `RC IN` port.
      * Connect the signal wires from the four ESCs to the `MAIN OUT` ports 1 through 4 on the Pixhawk. The order is critical and defined by the ArduPilot/PX4 documentation.

  ---

  ### Part C: Firmware & Calibration (The Digital Setup)

  This part is done on your computer.

  1.  **Install Ground Control Software:** Download and install **Mission Planner** on your Windows PC. (If using Mac/Linux, use QGroundControl).
  2.  **Flash Firmware:** Connect the Pixhawk to your computer via USB. **Do not have the main drone battery plugged in.** Open Mission Planner, go to the "Setup" tab, and click "Install Firmware." Choose the latest stable "ArduCopter" version.
  3.  **Calibrate Everything:** This is the most important step. Follow the wizards in Mission Planner to calibrate:
      * **Accelerometer:** You'll be asked to place the drone on all six of its sides (level, on its right side, left side, nose up, nose down, upside down).
      * **Compass:** Hold the drone and rotate it through all axes. Do this away from large metal objects.
      * **Radio Control:** Calibrate your RC transmitter sticks and switches.
      * **ESC Calibration:** This synchronizes the ESCs. Follow the "All-at-once" calibration guide in the ArduPilot documentation. This step ensures all motors spin up together.
  4.  **Check Motor Direction:** With propellers **OFF**, arm the drone in Mission Planner and slightly raise the throttle. Check that each motor spins in the correct direction as per the ArduCopter diagram. If a motor spins the wrong way, swap any two of the three thick wires between the motor and its ESC.

  ---

  ### Phase 1 Milestone: First Flight

  * **Pre-flight Check:** Go to an open field, far from people. Put the propellers on, ensuring the correct CW/CCW props are on the correct motors.
  * **Test Flight:** Place the drone on the ground. Stand back. Arm the drone using your RC transmitter.
  * **Goal:** Gently increase the throttle until the drone becomes light and lifts off. Practice hovering a meter off the ground. Test its response to your pitch, roll, and yaw commands. Land gently.

  **Once you have achieved a stable, controlled manual hover, Phase 1 is complete.** You have successfully built the "Body" of F.A.R.D.S.

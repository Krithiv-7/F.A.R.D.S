  # Phase 3: Connecting Brain to Muscle (The Nervous System)

  **Objective:** To establish a reliable communication link between the Raspberry Pi (the brain) and the Pixhawk Flight Controller (the muscle), enabling the Pi to receive live data from the drone.

  ---

  ### Part A: Physical Connection & Power

  This part involves careful wiring. **Ensure the main drone battery is disconnected before you begin.**

  1.  **Mount the Raspberry Pi:** Securely mount the Raspberry Pi onto the drone's frame. Use standoffs to prevent electrical shorts and ensure it's stable.
  2.  **Powering the Pi:** The Pi needs a stable 5V power source. **Do not power it from the Pixhawk's USB port.**
      * **Component:** Use a 5V UBEC (Universal Battery Eliminator Circuit). This efficiently steps down the voltage from your main LiPo battery.
      * **Wiring:** Solder the UBEC's input wires to your Power Distribution Board (PDB), alongside the ESCs. Connect the UBEC's 5V output to the Raspberry Pi's 5V and GND GPIO pins. This ensures both the Pi and the Pixhawk share a common ground, which is essential for serial communication.
  3.  **Serial Port Connection:** This is the data link.
      * **Cable:** You need a 4-pin serial cable.
      * **Pixhawk Port:** Locate the `TELEM2` (or `SERIAL2`) port on your Pixhawk.
      * **Raspberry Pi Pins:** Locate the GPIO pins on the Raspberry Pi. You will use `GND`, `TXD` (GPIO 14), and `RXD` (GPIO 15).
      * **Wiring (Crucial):**
          * Pixhawk `GND` -> Raspberry Pi `GND` (Pin 6)
          * Pixhawk `TX` (Transmit) -> Raspberry Pi `RXD` (Receive - Pin 10)
          * Pixhawk `RX` (Receive) -> Raspberry Pi `TXD` (Transmit - Pin 8)
          * *Do not connect the VCC (power) pin between the two devices.*

  ---

  ### Part B: Software Configuration

  Both devices need to be told to talk to each other over their serial ports.

  1.  **Configure Raspberry Pi:**
      * SSH into your Raspberry Pi.
      * Run `sudo raspi-config`.
      * Go to `Interface Options` -> `Serial Port`.
      * When asked "Would you like a login shell to be accessible over serial?", select **`No`**.
      * When asked "Would you like the serial port hardware to be enabled?", select **`Yes`**.
      * Reboot the Pi. This frees up the hardware serial port (`/dev/ttyAMA0`) for our use.

  2.  **Configure Pixhawk (via Mission Planner):**
      * Disconnect the serial cable from the Pi for now.
      * Connect the Pixhawk to your computer via USB and open Mission Planner.
      * Go to the `CONFIG` -> `Full Parameter List` tab.
      * In the search box, find the parameter `SERIAL2_PROTOCOL`. Set it to `2` (which means MAVLink 2).
      * Find the parameter `SERIAL2_BAUD`. Set it to `57` (which corresponds to 57600 baud). This is a reliable speed for this connection.
      * Click "Write Params" to save the changes to the Pixhawk.

  ---

  ### Part C: The Connection Test Script

  This script will run on the Raspberry Pi to confirm the connection is live.

  1.  **Reconnect the serial cable** between the Pi and the Pixhawk.
  2.  **Power up the entire drone system:** Connect the main LiPo battery. The drone, Pixhawk, and Raspberry Pi should all power on.
  3.  **SSH into the Pi.**
  4.  **Create the Script:** Create a new file named `connection_test.py` and add the following Python code:
      ```python
      from dronekit import connect, VehicleMode
      import time

      #-- Set the connection string
      #-- For Raspberry Pi, the serial port is likely /dev/ttyAMA0
      connection_string = '/dev/ttyAMA0'
      baud_rate = 57600

      #-- Connect to the vehicle
      print(f"Connecting to vehicle on: {connection_string} at {baud_rate} baud...")
      vehicle = connect(connection_string, baud=baud_rate, wait_ready=True)

      print("Connection Successful!")

      #-- Print some vehicle attributes (state)
      print("Get some vehicle attribute values:")
      print(f" GPS: {vehicle.gps_0}")
      print(f" Battery: {vehicle.battery}")
      print(f" Last Heartbeat: {vehicle.last_heartbeat}")
      print(f" Is Armable?: {vehicle.is_armable}")
      print(f" System status: {vehicle.system_status.state}")
      print(f" Mode: {vehicle.mode.name}")

      #-- Close connection
      vehicle.close()
      print("Connection closed.")
      ```
  5.  **Run the script:** In the Pi's terminal, run `python3 connection_test.py`.

  ---

  ### Phase 3 Milestone: Live Telemetry

  **The goal is to see live data from the drone printed in your terminal.** If the script runs and successfully prints the GPS status, Battery voltage, and other attributes without errors, Phase 3 is complete.

  You have successfully built the drone's nervous system and proven that the brain can listen to the body.
